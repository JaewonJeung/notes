# design gopuff

Functional
1. users should be able to query available items in their area
2. users should be able to order items

Non-functional
- users should be able to query local DC items quickly 
- users should have the guarantee that once they are in the checkout process, the items are locked (consistency)

Core entities
- User
- Item
- DC
- Inventory
- Order

API
GET /v1/availbility?lat={lat}&long={long} -> ItemMetadata[]
POST /v1/order -> OrderDetails | Failure
- body {
    - lat
    - long
    - items[]
}

Design

# Things to note 
- Separating the system that solely does checking of the nearby DCs taking road conditions and practical speed into matter
    - In this, for speed, we can implement a two-stage architecture where 
        0. Load/sync the DC info onto the service once in a while since the DCs are not that many and doesn't change change
        1. Once we get the location request, we do a radius check to filter out the ones that are too far
        2. Once we filtered down, then use the distance-checking service

- Ordering items with strong consistency
    - RDBMS transactions. (SERIALIZIBLE or FOR UPDATE)

- Item availability query speedup 
    - Redis cache, cache-aside
    - and/or DB sharding (speedup) + read async replica (resiliency + speedup)
        - When sharding, we can have multiple DB instances
        - There is a few replica strategies, and we're gonna have it for each shard
        - One leader node can be used for read+write (ordering with transactions) and for query-only ops, we can use the async read replicas since it can take time for leader->follower data syncing, but for data reads for query, we can tolerate a bit of data off-sync

https://www.hellointerview.com/learn/system-design/problem-breakdowns/gopuff