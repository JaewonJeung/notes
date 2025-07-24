# Ad click aggregator

## Learnings
- Click dedup
    - If we have the guarantee that the user is going to be logged in,
        - Cache for ad_id:user_id with a TTL
        - before funneling the data into kafka, do a cache check if this is a duplicate/too early of a request 
    - But non-login click case is going to be the common case
        - To handle this, when the ad is placed, we can send an impression id and hmac sign it
        - When getting the ad click request, verify if impression id in cache, and then if not in cache, hmac verify and then send it along
- Analytics dashboard things
    - query cache
    - time granularities (day, week, month, year)
    - precompute service
- Allowing analytics dashboard to be almost real time (< 5 sec)
    -  [[stream processing framework]] and then funneling data to [[OLAP]]
- hot key problem
     - cache duplicate key
     - consistent hashing as mechansim for DB sharding
     - for kafka and flink, we can have a key + (numbered suffix) to distribute messages among multiple partiitons in a topic
