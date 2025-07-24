# system design plan

- Read through the System design topics 
    - donnemartin https://github.com/donnemartin/system-design-primer?tab=readme-ov-file#study-guide
    - hello interview https://www.hellointerview.com/learn/system-design/in-a-hurry/introduction
    - Alex Xu https://github.com/preslavmihaylov/booknotes/tree/master/system-design/system-design-interview
- Read through real world architectures techs
- Read through company engineering blogs 
- Review How to approach a system design and take notes to refer to 
- Work through System design questions with solutions
    - https://excalidraw.com/
- https://www.youtube.com/@jordanhasnolife5163/playlists
- Work through Object-oriented design questions with solutions

- https://github.com/madd86/awesome-system-design if needed

## donnemartin + HI Key Tech
- [[CAP]] Theorem
    - What is the context of this theorem and what does each three mean?
    - Which one of those is necessary and why?
    - When would you use each of the two combinations?
    - Why can't we achieve all three?
    - What are the three consistency patterns? Why would you use each and why wouldn't you?
    - What are the two availability patterns? 
    - Describe how you would calculate the availbility of two components serving in parallel, each with 99.9% avail
    - Explain what PACELC is
    - TODO review the consistency techniques and see if they fit in the place
    - Case study: How does s3 achieve so many 9s?
    - Case study: How does Google docs handle concurrency?
    - Case study: How do MMO games handle concurrency?
- [[DNS]]
    - Describe a scenario why you would want a latency-based routing
    - Describe a scenario why you would want a geolocation-based routing
    - TODO: attacks
- [[CDN]]
    - What is a CDN and why is it important in modern system design?
    - Explain the caching strategies used by CDNs
    - What are some common challenges in designing a CDN?
    - How does a CDN handle dynamic content differently from static content?
    - How do CDNs ensure consistency and freshness of cached content?
    - What are the trade-offs between global versus regional CDN deployments?
    - How do CDNs handle load balancing and failover?
    - What metrics are used to evaluate CDN performance?
    - How can you integrate a CDN into an existing web architecture?
    - What is the role of DNS in CDN routing?
- [[Load balancer]]
    - In front of what do you need LBs? How do you represent this in interviews?
    - Describe different load balancing algorithms and their use cases.
    - What are the differences between Layer 4 and Layer 7 load balancing? In what scenarios are each useful?
    - How do load balancers ensure high availability and handle failover?
    - What role does DNS play in load balancing?
    - How do load balancers implement session persistence (sticky sessions)?
    - What are the trade-offs between hardware and software load balancers?
    - How can security be integrated into a load balancing setup?
    - How do load balancers scale to handle increasing traffic?
    - What are common methods to monitor and troubleshoot load balancers?
- [[Application layer]]
    - 
- [[Storage]]
- [[API Gateway]]
    - What are some things API Gateways can handle?
- [[Queue]]
    - Two purposes of using queues?
    - When not to use queues?
- [[Kafka]]
- Database
    - [[Relational Database]]
        - What is SQL joins for and what's the downside?
        - What are indexes for and what's so nice about RDB indexing?
        - Explain RDBMS "transactions"
    - [[NoSQL Database]]
        - When might you choose a NoSQL database for your project?
        - What is an important caveat to remember when comparing NoSQL and Relational Database capabilities?
        - Describe the consistency models available in NoSQL databases
- Streams / Event Sourcing
- [[Distributed lock]]
    - Why can't we just use ACID property of a DB?
    - Some examples of scenarios where we need distributed lock
- [[Distributed cache]]

## HI
### Intro
- Interviews are about seeing your ability to navigate a complex problem 
- So break down into smaller problems
- The most common ways that candidates fail with problem navigation:
    Insufficiently exploring the problem and gathering requirements.
    Focusing on uninteresting/trivial aspects of the problem vs the most important ones.
    Getting stuck on a particular piece of the problem and not being able to move forward.
- The most common ways that candidates fail with high-level design:
    Not having a strong enough understanding of the core concepts to solve the problem.
    Ignoring scaling and performance considerations.
    "Spaghetti design" - a solution that is not well-structured and difficult to understand.
- The most common ways that candidates fail with technical excellence:
    Not knowing about available technologies.
    Not knowing how to apply those technologies to the problem at hand.
    Not recognizing common patterns and best practices.

### Delivery framework (sequence of steps for focused delivery)
[[Delivery framework]]

### Core Concepts Quiz
[[HI core concepts]]
Scaling
- What is horizontal scaling and how does it differ from vertical scaling?
- Why should you avoid jumping into horizontal scaling too quickly?
- What system-wide implications must be considered when implementing horizontal scaling?

CAP Theorem
- What does the CAP theorem state, and why is AP (Availability and Partition tolerance) often chosen as the default?
- In what scenarios is strong consistency (CP) critical?
- How can a system incorporate multiple consistency models to meet different requirements?

Locking
- What factors should be considered when employing locks in a system?
- How does lock granularity impact system performance?
- Why is it beneficial to keep locks as short as possible?
- What is an optimistic concurrency strategy and when might you bypass traditional locking?

Indexing
- Why is it important to invest in indexing upfront for fast reads?
- What are some specialized indexing techniques, and for what use cases are they typically applied?
- How does ElasticSearch function as a secondary index solution, and what trade-off does it introduce?

Communication Protocols
- How does HTTPS support horizontal scaling of APIs?
- What are the benefits and use cases for long polling and server-sent events?
- How do websockets facilitate real-time bidirectional communication, and what challenges do they present?
- Why is it crucial to relegate state management to message brokers or databases when using stateful protocols?

Security
- What role does an API gateway play in handling authentication and authorization?

Monitoring
- What metrics should be monitored at the infrastructure level?
- What service-level indicators are important for assessing the health of a service?
- Which application-level metrics help determine if an application is fulfilling its intended purpose?

# Object design
https://python-patterns.guide

# Design Pattern
- [[Real-time updates]]
- [[Dealing with contention]]
- [[scaling reads]]

# Pratice problems
- [[design bitly]]: url shortener
    - ! Back of the envelope calculation. IOPS for mem, SSD
- [[design gopuff]]: location two-stage arch, CP, ACID DB speedups+reliability
    - ! postgres are single instance nodes. Horizontal scaling is achieved by sharding & read-only replica
- [[design ticketmaster]]: transaction, distributed lock w/ TTL, payment processing, DB speedups, horizontal scaling, scalable string search w/ [[Change data capture (CDC)]]
    - ! It would still be good idea to have the transaction system in case the dist lock goes down and we still need to prevent double booking
    - ! Actually write down what the dist lock data would look like
    - ! Remember to mention horizontal scaling of the actual services in addition to the DB as well :)
- [[design facebook news feed]]: infinite scroll, fan-out problem, async workers, hot key problem
    - ! adjusting the prouduct in outlier instances like a user with 100k follows. FB limits num of friends to 5000
    - ! shift in thinking from computing the feed resulting on WRITE than READ
- [[design leetcode]]: isolation/security, leaderboard, long-running task, SQS usage
- [[fb-live-comments]]: infinite scroll

# Lower level design
- [[design rate limiter]]
- [[design key value store]]
- [[Ad Click aggregator]]
- [[Designing Ledger]]