## When to use
- Need to scale the system and lower system latency at the same time
- Great for storing data that's expensive to compute or retrieve from a DB. 
- Can also reduce the number of DB queries for common queries 

## Some examples
- Saving aggregated metrics from multiple sources 
    - Analytics platform aggregating data from numerous sources and displaying them. Expensive
    - An async cronjob can do this and put the result in dist cache so that each user requesting the data doesn't involve redoing all of those
- Reducing number of DB queries to support concurrent users
    - User sessions often stored in a dist cache to reduce load on the DB
    - Session data can be stored in the cache so that the sytem processing it doesn't have to query DB for every user online and quickly retrieve data
- Speed up expensive queries
    - Some queries that useful can be complex to run each time
    - Say for each user, we want to show a list of posts from all the people people they follow. It requires a lot of joins. 
    - We can instead run the query once, store the results in a dist cache and retrieve the results

## Cache strategies
- `Cache aside`
    - No connections between cache and DB. App is the middleman
    - Flow
        1. App reaches out to cache
        2. If miss returned, app fetches from DB
        3. App updates cache
    - For read-heavy workflows 
    - Write is done through DB directly with TTL in cache (to ensure the data is fresh enough) and then "lazy load"
    - Can be combined with write around
- `Read through`
    - Cache sits in the middle. App <-> cache <-> DB
    - In case of cache miss, the cache is the one pulling data from DB
    - For read-heavy workflows
- `Write through`
    - Cache sits in the middle. App <-> cache <-> DB
    - Writing data to the cache which in turn to the DB (SYNC)
    - Ensures data consistency btw cache and DB but slower write ops
    - Paired with read-through
    - Can lead to cache pollution
- `Write around`
    - Write data only on DB. Minimizes cache pollution (some data may do a lot of writes but not be immediately read, and in this case, write-through will push out more important read-heavy cache) but increase data fetch time
    - Useful for lotsa write 
    - Can be combined with cache aside
- `Write-back`
    - Cache sits in the middle. App <-> cache <-> DB
    - Write data to cache, cache ACKs, and ASYNC write from cache to DB. 
    - Fast write (since app can do whatever after getting ack from cache) and read but potential data loss if cache is not saved to disk
    - 