# design rate limiter

- First, rate limiter can be on both client and server side. For the common case, a client side rate limiter may suffice, but you should always have both
- Things to care about
    - limit requests with accuracy
    - low latency
    - high fault tolerance (rate limiter failure doesn't affect other components' functionality)
- Places to put the RL. Either in the service or the as a middleware. Need to even be conscious of language the server is written due to the processing speed. API Gateway is a famous middleware LB/RL

## Algorithms
- Token bucket algo
    - Two params: the size of the bucket and the token refill rate
    - Think of token as a coin to process a request. If token, process request. If no token, drop request
    - The bucket receives a number of tokens per a period of time to refill the allowance
    - Usually 1 bucket per API endpoint per user
        - If there's a post, follow, like endpoints, 3 buckets per user

- Leaking bucket algo
    - basically a queue
    - fixed bucket(queue) size. Requests are enqueued
    - Pros: request processed at a fixed rate. Stable outflow
    - Cons: in case of bursty traffic, a new set of requests would have to wait until the previous ones (potentially outdated) are going through

## Design
- High level, we need to keep track of the entity (user/IP) and a counter
- Redis has INCR (ctr) and EXPIRE (timeout)
- The rules can be fetched (from storage) and cached to a cache by a worker
- Based on the rule, the middleware can rate limit, by referring to Redis counter and either returning 429 too many requests or letting request go through to the service
- Let's make the rate limiter scalable. 
    - We have to be careful of data race issue
    - Have multiple instances of rate limiter, and they all point to the same Redis instance