# Approaches
1. Optimize read performance within your database
2. Scale your database horizontally
3. Add external caching layers

! Tip is to start sequentially as above and don't just jump right into complex caching strategy

# Optimize read performance within your database
## Indexing
- Adding index to columns you frequently query (scan), join, or sort

## Vertical scaling

## Denormalization
- Optimizing reads at the expense of writes
- Many joins lead to performance issues
- Sacrificing writes since you have to write to multiple locations

# Scale your database horizontally
## Read replicas (Lead-Follow replication)
- All writes go to primary but reads go to any replica
- Also provides redundancy
- Either synchronous repl or asynchronous repl
    - data consistency vs latency

## Sharding
- smaller datasets & dist load
- Functional sharding: split by business domain or features (posts, users, likes)


# Add external caching layers
## App-level caching
- Redis
- Useful for hot key problem. Spreading a single hot key across multiple cache entries

## CDN
- Only makes sense for resources that are accessed by multiple users, not user-specific data