## Client updates
- `Simple polling`
    - wait. poll. wait. poll
- `Long polling (usually skip this)`
    - poll. server holds onto the req and takes time to respond. poll immediately again
    - near real time with simple impl
    - cons. HTTP overhead, not good for freq updates due to round trips
- `SSE`
    - Instead of content-length, chunk. Basically turns HTTP into stream
    - More efficient than above. Auto reconnection using "last event ID". Still relatively simple to implement with some careful infra support
    - One-way comm. Browsers limiting # of concurrent conn per domain
    - Need full path infra support. e.g. the proxy/LB can't do buffered send 
- `WS`
    - Full duplex
    - Because of its stateful nature, it's common to pull out the WS component from the service and have a websocket service which sits in between LB and stateless services


## Server updates (push/pull)
- `Pulling` with Simple Polling
    - polling as trigger
    - Decoupling of the requester and the source of update (the DB as the middle man)
    - Not real time
- `Pushing` via consistent hashes (skip to pub/sub if no need for complex state)
    - The situation is that each each user is map to one of the servers (many to one)
        - This causes problem when user A in server 1 wants to talk to user B connected to server 2
    - Servers discovery by [[zookeeper]] or [[etcd]]
    - Using hash. User connects to a random server, and the server redirects the connection to the right server
    - A simple hash problems of scaling service up or down when the modulo N changes. Reconnection required for a lot of users
    - So use consistent hashing
        - Useful for persistent connection (WS/SSE) and the system needs to scale 
        - Minimal user disconnection
        - Still need a coordination service (zookeeper, etcd)
    - Better than pub/sub when each conn holds large states like Google Docs
- `Pushing` via pub/sub
    - Easier to impl
    - Have a pub/sub service like Redis
    - When client connects, register client with p/s server as a topic
    - When message is pushed to a topic, broadcast to subcribers of that topic