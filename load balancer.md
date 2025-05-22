# load balancer

## Load balancer algorithms
- Random
- Least loaded
- Session/cookies
- Round robin or weighted round robin
- Layer 4 (Transport)
- Layer 7 (Application)

## Benefits
- SSL termination
- Session persistent if the webapp doesn't keep sessions itself
- Helps horizontal scaling
- Eliminates SPOF

## Caveats
- While it is used to prevent SPOF, the LB can also be a SPOF
    - This means that we need more than 1 LB nodes. Increases complexities

## Answers
0. In front of what do you need LBs? How do you represent this in interviews?
In front of every service that processes same requests with multiple machines.
This can be redundant in interviews. 
Either you omit LB from the design and just mention that the service is horizontally scaled, or put LB right after API Gateway

1. What is a load balancer and why is it essential in modern distributed systems?
A load balancer distributes incoming network traffic across multiple backend servers. This distribution helps ensure that no single server becomes a bottleneck, enhancing system reliability, performance, and scalability.

2. Describe different load balancing algorithms and their use cases.
- Round Robin: Distributes requests sequentially across servers.
- Least Connections: Sends traffic to the server with the fewest active connections.
- IP Hash: Routes requests based on client IP, which can be useful for session persistence.
- Weighted Round Robin/Least Connections: Assigns different weights to servers based on capacity, directing more traffic to more powerful servers.

3. What are the differences between Layer 4 and Layer 7 load balancing? When to use each?
- Layer 4 (Transport Layer): Operates at the TCP/UDP level and makes routing decisions based on IP address and port numbers without inspecting the actual content of the traffic. Useful for persistent connections like websockets 
- Layer 7 (Application Layer): Operates at the HTTP/HTTPS level and can make routing decisions based on content, headers, cookies, or URLs, which allows for more granular control. Everything except persistent connections

4. How do load balancers ensure high availability and handle failover?
They regularly perform health checks on backend servers to determine their availability. If a server fails, the load balancer automatically reroutes traffic to healthy servers using strategies like active-passive or active-active configurations, ensuring continuous service.

5. What role does DNS play in load balancing?
DNS can be used for basic load balancing by resolving a domain name to multiple IP addresses (round-robin DNS). However, DNS-based load balancing is less dynamic because it typically doesn't account for real-time server health or load conditions.

6. How do load balancers implement session persistence (sticky sessions)?
Session persistence ensures that a client’s requests are consistently routed to the same backend server. This can be achieved using methods such as cookie-based persistence or source IP hashing, which is important for applications that store session-specific data on a single server.

7. What are the trade-offs between hardware and software load balancers?
- Hardware Load Balancers: Often offer high performance and reliability with dedicated appliances, but they can be costly and less flexible for scaling and updates.
- Software Load Balancers: Generally more cost-effective and easier to scale or update, but may have performance limitations compared to specialized hardware under very high loads.

8. How can security be integrated into a load balancing setup?
Security measures can include SSL/TLS termination at the load balancer, DDoS protection, the use of web application firewalls (WAFs), and applying network-level security policies such as IP whitelisting or blacklisting.

9. How do load balancers scale to handle increasing traffic?
Load balancers scale either vertically (by enhancing the capacity of existing servers) or horizontally (by adding more load balancing nodes). Modern systems often use auto-scaling techniques to dynamically adjust capacity based on traffic patterns.

10. What are common methods to monitor and troubleshoot load balancers?
Monitoring involves collecting metrics like request rates, response times, error rates, and health check results. Troubleshooting might use logs, real-time monitoring dashboards, and tracing tools to identify issues, verify configurations, and ensure that traffic is being correctly distributed.

