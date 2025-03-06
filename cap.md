# cap

## Definition
In a distributed computing system, we can guarantee only two of the followings:
- Consistency - Every node has the latest data, so every read op receives the most recent write 
- Availablity - Every request recieves _a_ response, whether it is the latest data or not
- Partition tolerance - Store data in a distributed manner so that the system as a whole continues to operate even with arbitrary partitioning due to intra-system comm issue (e.g. network or node failure). You get to decide what to do when a partition does occur
    - ! Partion tolerance is necessary since network or node failures are bound to happen, and we don't wanna completely lose the data
    - This is almost the whole reason why we have distributed systems

## CP 
- Waiting for a response from the partitioned node might result in a timeout error
- CP is a good choice if your business needs require atomic reads and writes. Like Google Docs, but even Google Docs is moving to AP with "conflict resolution" as a way to solve consistency issue

## AP 
- Responses return the most readily available version of the data available on any node, which might not be the latest. Writes might take some time to propagate when the partition is resolved.
- AP is a good choice if the business needs very high availability since downtime/high latency is costly and can allow for *eventual consistency*. Gmail/Facebook

## Why can't we achieve all three?
- First, understand that partion tolerance is a must and is a time-consuming operation because you need to get all nodes to be on the same updated state
- Then if we add consistency into it, then what happens is for every write, we need to wait until all the nodes are updated. In this case, while the update is propagating, we can't serve the clients
- Now if we instead add availability, then this means that after a write op, a read op might occur while the propagation is WIP. But you have to serve with acceptable latency! So you serve the potentially outdated data

## Consistency Patterns
How to sync nodes
- Weak consistency
    - Write, but reads may or may not see it (best effort)
- Eventual consistency 
    - Write, and reads will eventually see it. Data replicated async. Works well in highly available systems
- Strong consistency
    - Write, reads will see it. Data replicated sync. Works well in systems that need transactions
    - e.g. File systems, [[RDBMS]]

- Techniques
    - Backups
    - Master-slave
    - Multi-master
    - 2 Phase Commit
    - Paxos

## Availbility Patterns
- Fail-over
    - Active-passive
        - Heartbeats sent between active and passive server. Passive server takes over when heartbeat interrupted 
        - Length of downtime determined by whether the passive server is hot or cold
    - Active-active
        - Both servers are managing traffic. Spread load
- Replication
    - [[master-slave replication]]
    - [[master-master replication]]

- Availability in parallel vs sequence
    - If component A and B where avail(A) = avail(B) = 99.9%. If they're placed sequentially, the availability is avail*avail, 99.8%
    - If "" . If they're placed in parallel, it becomes an OR, so the availability is 1-(unavail*unavail) = 99.9999%