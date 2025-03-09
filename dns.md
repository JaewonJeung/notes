# dns

Converts domain name to IP address

## Types of routing
### Latency-based
- Used in case you want a client from region A to be routed to your load balancer (LB) in region B or C based on the latency of (A-B vs. A-C). 
### geolocation-based
- Used in case you want to serve traffic based on the geographic location of the clients. You might want all queries from Europe to be routed to Franfurt region

## Attacks
- ? DNS cache poisoning
- ? DNS hijacking
- ? DNS spoofing
- ? DNS amplification
- ? DNS rebinding
- ? DNS tunneling