For ops that take more than few seconds
Split API requests into two phases
- immediate ack
- background process

# Solution
- client <-> server
- server <-> DB (adds job to DB)
- server -> job queue (adds to queue)
- job queue <-> workers -> DB  (workers processing and status updates)

# Trade offs
pros
- immediate response
- independent scaling & fault isolation

cons
- complexity
- eventual consistency

# Implementation
## MQ
- SQS
- Kafka

## Workers
- normal servers
- serverless 
- container based


# Deep dives
- Handling worker failure
    - Another worker will retry
- Repeated failures of the job
    - Maybe there's a bug in the job. Then it'll be retried forever
    - You can add a DLQ such that after nth failure, the worker moves it to DLQ
- Preventing duplicate work
    - Impatient user clicks multiple times
    - Client side prevention
    - Server side prevention by constructing a unique identifier (something like userId+action+timestamp with a set granularity)
- Exploding queue
    - More jobs than usual
    - Employ backpressure. Reject new jobs (say system busy) when the queue is too deep
    - autoscale based on queue depth
- Mixed workloads
    - Certain work duration is longer than others
    - Separate queue by work type
