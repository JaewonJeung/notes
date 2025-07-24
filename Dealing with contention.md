# Common scenarios
- Online auction
- Event booking
- Ride sharing dispatch
- Inventory system

# Single node DB case
If the system needs strong consistency during high-contention, do your best to go with single DB
- Pessimistic Locking
    - Use transactions. But transaction itself doesn't prevent other transactions from reading the same data concurrently 
    - Can acquire exclusive lock using FOR UPDATE 
- Isolation level
    - SERIALIZABLE, transactions appear to run one after another but also detects conflicts on its own (the requester need to retry)
    - Much more expensive than explicit locks since the DB needs to track all potential conflicts 
- Optimistic Concurrency Control (OCC)
    - Unlike pessimistic, it assumes conflicts are rare and detects after they occur
    - Include version number with data (or certain data can serve as that like checking once again when updating). When updating, specify both new value and the expected version
    - The req that sees contention can retry
    - Makes sense when conflicts are rare

# Multiple node DB case
- Two phase commit
    - Guaranteeing atomicity across multiple systems
    - "Prepare" for multiple DB. Then "Commit" for multiple DB. A log file tracks the state
    - Expensive and fragile. 
- Saga pattern
- [[Distributed lock]]
    - Used instead of coordinating complex transactions
    - Redis with TTL. SET command
    - Can also use database column. adding columns and lock. Doesn't need extra infra but may be slower than cache ops and need to implement the cleanup logic urself


# Edge case
- What if there's a need for strong consistency on a hot resource, requests in a dedicated queue would handle the ordering. 