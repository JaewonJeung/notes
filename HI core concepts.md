# HI core concepts

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

#### Monitoring
- Infrastructure monitoring
    - cpu, mem, disk, network
- Service-level monitoring
    - "Is the service tip-worthy??"
    - latency, error rates, throughput
- Application-level monitoring
    - "Is the application doing its job?"
    - Num of users, active sessions, key business metrics



## Answers
Below is a set of concise answers to each question for active recall:

---

### Scaling
- **What is horizontal scaling and how does it differ from vertical scaling?**  
  **Answer:** Horizontal scaling means adding more machines or nodes to distribute the load, whereas vertical scaling means upgrading a single machine’s resources (CPU, RAM, etc.). Vertical scaling is simpler and can often handle growth up to a point, while horizontal scaling is more complex but offers greater expansion possibilities.
- **Why should you avoid jumping into horizontal scaling too quickly?**  
  **Answer:** Vertical scaling is usually sufficient for many cases, and horizontal scaling introduces additional complexity such as network communication, data partitioning, and state management. It’s best to start with vertical scaling and only move to horizontal scaling when the workload demands it.
- **What system-wide implications must be considered when implementing horizontal scaling?**  
  **Answer:** When scaling horizontally, you need to consider load balancing, state management (e.g., session persistence), data replication, consistency issues, and overall system coordination across distributed components.
---
### CAP Theorem
- **What does the CAP theorem state, and why is AP (Availability and Partition tolerance) often chosen as the default?**  
  **Answer:** The CAP theorem states that a distributed system can guarantee only two out of three properties: Consistency, Availability, and Partition tolerance. AP is often chosen as the default because many applications can tolerate eventual consistency while prioritizing availability and handling network partitions effectively.
- **In what scenarios is strong consistency (CP) critical?**  
  **Answer:** Strong consistency is crucial in systems where reading stale data could lead to severe consequences—such as inventory management, booking systems, and banking applications—where data accuracy is paramount.
- **How can a system incorporate multiple consistency models to meet different requirements?**  
  **Answer:** A system can mix models by, for example, using an AP approach for parts of the application where eventual consistency is acceptable (like product descriptions) and a CP approach for critical parts (such as inventory counts) to prevent issues like overselling.
---
### Locking
- **What factors should be considered when employing locks in a system?**  
  **Answer:** Key considerations include the granularity of the lock (finer granularity improves performance by reducing contention), the duration of the lock (shorter locks are preferable), and whether an optimistic concurrency approach could be used to reduce unnecessary locking.
- **How does lock granularity impact system performance?**  
  **Answer:** Finer-grained locks target smaller data sections, which minimizes contention among concurrent processes and improves overall system performance.
- **Why is it beneficial to keep locks as short as possible?**  
  **Answer:** Short locks reduce the waiting time for other processes, lower the risk of deadlocks, and enhance throughput by allowing faster release and acquisition of locks.
- **What is an optimistic concurrency strategy and when might you bypass traditional locking?**  
  **Answer:** An optimistic concurrency strategy involves performing operations without locking initially and then checking a version number or record state before committing changes. Locking is applied only if a conflict is detected, reducing overhead when contention is low.
---
### Indexing
- **Why is it important to invest in indexing upfront for fast reads?**  
  **Answer:** Effective indexing speeds up data retrieval by reducing the time complexity of searches. Properly planned indexes help ensure that read operations are efficient and responsive.
- **What are some specialized indexing techniques, and for what use cases are they typically applied?**  
  **Answer:**  
  - **Geospatial indexing:** Used in location-based services for spatial queries.  
  - **Vector databases:** Employed for high-dimensional data like similar images or documents.  
  - **Full-text indexing:** Enables efficient text searches in documents, tweets, or articles.
- **How does ElasticSearch function as a secondary index solution, and what trade-off does it introduce?**  
  **Answer:** ElasticSearch listens to changes via Change Data Capture from the primary database to maintain various indexes (full-text, geospatial, vector, etc.). This approach introduces additional latency and may temporarily cause data inconsistencies between the primary data store and the index.
---
### Communication Protocols
- **How does HTTPS support horizontal scaling of APIs?**  
  **Answer:** HTTPS is stateless, meaning each request is independent. This allows for easy distribution of incoming traffic across multiple servers via load balancers, enabling horizontal scaling without worrying about maintaining client state.
- **What are the benefits and use cases for long polling and server-sent events?**  
  **Answer:** Both long polling and server-sent events offer near real-time updates using simple HTTP mechanisms. They are less complex than websockets and are well-suited for scenarios where timely server-to-client updates are needed without full bidirectional communication.
- **How do websockets facilitate real-time bidirectional communication, and what challenges do they present?**  
  **Answer:** Websockets provide persistent, low-latency connections for real-time, bidirectional communication. However, maintaining many open connections can strain backend resources, often necessitating the use of a message broker to manage these connections more efficiently.
- **Why is it crucial to relegate state management to message brokers or databases when using stateful protocols?**  
  **Answer:** Centralizing state management in a message broker or database reduces complexity on individual servers and allows the system to scale horizontally while still handling stateful interactions with clients reliably.
---
### Security
- **What role does an API gateway play in handling authentication and authorization?**  
  **Answer:** An API gateway serves as the first point of contact, handling user authentication and authorization. It ensures that requests are verified and that only authorized access is allowed before forwarding them to the backend services.
---
### Monitoring
- **What metrics should be monitored at the infrastructure level?**  
  **Answer:** Infrastructure monitoring focuses on hardware and network metrics such as CPU usage, memory consumption, disk I/O, and network traffic.
- **What service-level indicators are important for assessing the health of a service?**  
  **Answer:** Key service-level metrics include latency, error rates, and throughput, which indicate whether the service is operating efficiently and meeting performance targets.
- **Which application-level metrics help determine if an application is fulfilling its intended purpose?**  
  **Answer:** Application-level monitoring tracks metrics like the number of users, active sessions, and key business metrics (such as transaction volumes or user engagement) to assess if the application is delivering value and performing as expected.