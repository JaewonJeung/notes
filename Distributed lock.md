## When to use
- Locking a resource (like concert ticket) mid-term long (min ~ 10 min)
- Different from ACID DB since the transactions they make is for very short term locking (quick updates)
- Some examples:
    - E-commerce checkout system
    - Ride-sharing matchmaking
    - Distributed cronjobs
    - Online auction bidding

## What tech
- Redis. Key-val store to lock and use the atomicity of the KV store to ensure only a single process can acquire the lock at a time
    - It also provides lock expiration time so that a lock doesn't stay like that forever if something were to happen