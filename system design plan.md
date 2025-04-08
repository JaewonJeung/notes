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

## donnemartin
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
1. `Requirements` (5 min)
    - `Functional` ("Clients should be able to...")
        - Ask questions about the features as if you were talking to a client or PM
        - ! Identify and prioritize the top 3
    - `Non-functional` (System qualities. "The system should be...")
        - "System should be highly available", "The system should be able to scale to support 100M+ DAUs", "The system should be low latency, rendering feeds in under 200ms"
        - ! Important to have requirements in the context of the system and quantify if possible
        - Identify top 3 - 5 things among:
        - `PACELC Theorem`: Consistency vs Availability
        - `Environment Constraints`: System with limited memory or limited bandwidth (e.g. streaming video on 3G)?
        - `Scalability`: All systems should scale. But more specifically stuff like bursty traffic at a specific time/day? Does your system need to scale reads or writes more?
        - `Latency`: Specifically consider any requests that require meaningful computation. For example, low latency search when designing Yelp.
        - `Durability`: Data loss tolerance? social network vs. banking system 
        - `Compliance/Security`: Regulatory requirements. Data loss, access control, etc.
    - `Capacity` (Calculation/estimation)
        - Often unnecessary unless it's crucial for non-functional requirement
        - ! Tell them you'll skip calculation and do it while designing if necessary
        - https://www.hellointerview.com/blog/mastering-estimation
2. `Core entities` (2 min)
    - Identify and list the core entities of your system. Nouns and verbs. Small list.
    - Twitter example: "User" "Tweet" "Follow"
    - ! Think of the non-overlapping actors and what are necessary for the functional requirements
3. `API` (5 min)
    - Based on the functional req, define the contract between users and the system. Guides the high-level design
    - types
        - `REST API`: Basic CRUD. Bias towards this. Only nouns. No verbs
        - `GraphQL`: Use only if it's imperative for the client to fetch exactly the data they need (no over- or under- fetching)
        - `Wire protocol`: websockets, raw TCP
    - REST example:
        - POST /v1/tweets
            body: {
                "text": str
            }
          GET /v1/tweets/:tweetId -> Tweet
          POST /v1/follows/:userId
          GET /v1/feeds -> Tweet[]
        - Notice there's no userid in the tweet POST since it'll be identified using authentication token in req header
        - fyi, those things after : is called path parameter 
4. `Data flow` (Optional, 5 min)
    - If we're talking about data processing systems with many steps, helpful to list of sequence of actions/processes
    - Web crawler example:
    - For a web crawler, this might look like:
        1. Fetch seed URLs
        2. Parse HTML
        3. Extract URLs
        4. Store data
        5. Repeat
5. `High level diagramming` (10 - 15min) 
    - ! Primary goal: Satisfy functional requirements by satisfying the designed API
    - Don't overthink/over-complicate since you need to at least complete the system. Go one by one of the API endpoints
    - ! Be explicit about data flows. Starting from request and ending with response. When request reaches DB, doc relevant col/fields for the entity (Not too detailed. Wastes time.)
      - That is, for POST tweet, next to the DB, you can jot down
        - Tweet
          - id
          - userid
          - text
          - media: s3uri[]
    - Take notes of potential areas of complexity for the deep dives section
    - example diagram: ![alt text](image.png)
6. `Deep Dives` 
    - ! Harden design
        - Meeting non-functional req (go back and check each point)
        - Address edge case
        - Identifying & addressing issues & bottlenecks
  
### Core Concepts
#### Scaling
- Horizontal scaling
    - Note that vertical scaling is a lot less complex and machines can vertically scale to a surprising degree
- ! Don't leap too fast into horizontal scaling. Check if it's necessary
- ! If you do choose horizontal scaling, think about the implication on the rest of the system
- "Consistent hashing"

#### CAP
- AP should be the default choice. Only need strong consistency in systems where reading stale data is unacceptable
    - E.g. for strong consistency: inventory mgmt systems, booking systems, banking systems
- ! You can have more than one consistency models in your system. That is, you can provide product description with AP but CP inventory counts to prevent overselling

#### Locking
- Employing locks considerations
    - Granularity: Fine-grained locks for performance
    - Duration: Short locks as possible
    - Bypassing lock: Optimistic concurrent strategy. Locking only if necessary by employing a version manifest file and checking the read record version and depending on  

#### Indexing
- Work up front so that reads can be fast
- Specialized indexing
    - Geospatial: Location services
    - Vector DB: indexing high dimensional data (similar images, similar docs)
    - Full-text index: indexing text data. Searching for docs or searching for tweets
- ElasticSearch (a secondary index solution)
    - Supports full-text indexes, geospatial, and vector
    - Uses "Change Data Capture" to index DB and to listen on incoming changes coming from the DB
    - But this introduces a new source of latency, but it may be okay to be inconsistent 

#### Communication Protocols
- `HTTPS`: Stateless. Therefore you can scale the API horizontally by placing it behind a LB. Services SHOULDN'T assume state of the client
- `Long polling`; `Server Sent Events`: Near real-time updates from server to clients. Simplicity of HTTP and real-time update of websocket
- `websockets`: realtime bidirectional communication
    - Maintaining a lot of open connections like this can strain the backend. 
    - A common pattern for this instance is to insert a message broker between the client and the server. This way, connections are maintained by the broker, and the broker and communicate with the backend services
- ! Statefulness is major source of complexity. Relegate the state to a message broker or the DB. This way, your service can scale horizontally and still maintaining stateful communication with clients

#### Security
- Auth: "API gateway will handle auth and authorization"
- 

#### Monitoring

# Object design
https://python-patterns.guide