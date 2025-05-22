# CDN

Distributing service spatially relative to end users
https://www.cloudflare.com/learning/cdn/what-is-a-cdn/

## Benefits of CDN
- Availability and performance
- Server load easing

## Types of CDNs
- Push CDN
    - When a new content is available, your server pushes it to all the CDNs 
- Pull CDN
    - When the first user requests the content, the content is pulled to the CDN and then to the user
    - The subsequent users will be able to enjoy the cached content
    - TTL (time to live) will determine how long the cache will stay

## Answers
1. What is a CDN and why is it important in modern system design?
A CDN (Content Delivery Network) is a distributed network of servers that cache and deliver web content (often time in conjuntions with blob storages) to users based on their geographic location. It minimizes latency, reduces load on origin servers, and improves overall reliability and scalability of web applications.

2. Explain the caching strategies used by CDNs.
CDNs typically use strategies such as Time-to-Live (TTL) to determine how long content remains cached, cache invalidation or purging to refresh outdated content, hierarchical caching with multiple layers (edge, regional, and central caches), and cache replacement policies like LRU (Least Recently Used) or LFU (Least Frequently Used) to optimize storage.

3. What are some common challenges in designing a CDN?
Common challenges include balancing content freshness with performance, achieving global scalability and low latency, strategically placing servers to cover all regions, implementing effective load balancing, and ensuring robust security against DDoS attacks and cache poisoning.

4. How does a CDN handle dynamic content differently from static content?
Static content (like images and CSS files) can be cached for long durations, whereas dynamic content, which is often personalized or frequently changing, may be handled using edge computing, dynamic site acceleration (DSA), or partial caching strategies to ensure timely and accurate delivery without compromising personalization.

5. How do CDNs ensure consistency and freshness of cached content?
Consistency is maintained through TTL settings, active cache invalidation or purging processes when content changes, and techniques like "stale-while-revalidate" where outdated content is served briefly while a fresh copy is fetched. Additionally, versioning URLs can help ensure users receive the latest content.

6. What are the trade-offs between global versus regional CDN deployments?
Global CDNs offer wide geographic coverage and low latency across continents but come with higher complexity and cost. Regional CDNs are easier to manage and can be optimized for a specific area, though they might not deliver the same performance benefits for users located outside the target region.

7. How do CDNs handle load balancing and failover?
Load balancing in CDNs is often managed via DNS-based techniques (like GeoDNS) or Anycast routing, which directs traffic to the nearest or healthiest server. Continuous health checks ensure that if one server or data center fails, traffic is rerouted to another operational node, ensuring high availability.

8. What metrics are used to evaluate CDN performance?
Key metrics include cache hit ratio (percentage of requests served by the cache), latency or response time, throughput (data served over time), error rates, and the ability to scale and handle peak traffic without degradation in performance.

9. How can you integrate a CDN into an existing web architecture?
Integration usually involves updating DNS settings to route traffic through the CDN, configuring which parts of the content should be cached versus delivered dynamically, implementing security measures such as SSL/TLS and DDoS protection, and setting up monitoring tools to track performance and make adjustments as needed.

10. What is the role of DNS in CDN routing?
DNS plays a crucial role in directing user requests to the most optimal edge server. It does this through techniques like GeoDNS, Anycast, and load-based routing, which consider geographic location, server health, and network conditions to minimize latency and balance the load effectively.