# vertical scaling and database choices
## Vertical scaling
- Initial choice

## DB chocies
- Different types of DBs. 
- Cassandra can be used for write heavy DB. read is lacking, though
- Reducing index overhead can also help

# sharding and partitioning
## Horizontal sharding
- hash-ish the input key and map to a server
- row based split

## Selecting a good partitioning key
- Important to share what partitioning key you're selecting. Something like the userID is a good key to spread evenly across the cluster

## Vertical partitioning
- Column based split
- Large table. Maybe split this into specialized tables

# Burst handling with queues and load shedding
## Queues
- Bursts. and autoscaling takes time
- Note that queues are only for short-lived bursts and not a way to patch a DB that can't handle steady load
- The queueing server only know that queueing is completed but need to go back to the DB again to check the status

## Load shedding
- The server checks what requests to drop instead of saving it to DB directly 
- Things like periodic location update data, you can drop one and just process the next one

# batching and multi-step reducers
## Batch write
- App layer
    - workers reading from kafka topic. Batch process and then store. Fault tolerant

## Hierarchical aggregation
- Intermediate processing
    - multiple batching processes. kafka -> worker -> kafka -> worker -> DB

# Deep dive
- How to handle resharding when adding more shards?
     - dual write. Write to both new and old. Read with preference for the new shard
- Hot key problem
    - split all keys for n number of shards
        - downside is we've blown up the storage
        - also slower read since the service has to reach out to all n nodes
    - split key dynamically
        - splitting the hot key into multiple sub keys based on whether the key is hot or not
            - way more complex