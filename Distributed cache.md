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

## Cache write strategy
- Write through
    - Writing data to both the cache and the DB. Ensures consistency but slower write ops
- Write around
    - Write data only on DB. Minimizes cache pollution (some data may do a lot of writes but not be immediately read, and in this case, write-through will push out more important read-heavy cache) but increase data fetch time
- Write-back
    - Write data to cache and async write from cache to DB. Fast write and read but potential data loss if cache is not saved to disk