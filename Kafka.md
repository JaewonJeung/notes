# Kafka

In a nutshell, a horizontally distributed queues with pubs/subs on either ends.
Often used for non-functional requirements in [[Delivery framework]].
"[[Batch write]]" is also a technique used along with Kafka in the worker/consumer side

## How it works
- Each queue is called `partition` (and each partition can have replicas for backup). These are immutable sequence of appended messages
- They are logically grouped by `topics`. 50kb per topic
- `Brokers` are servers that hold the partitions
- Each `consumer group` contains machines that process/subscribe to a specific topic
- Each consumer will read the message based on the latest offset, and once it's finished with its job, it'll commit the next offset.

## Usage examples
1. Message queue
    - Async processing (YT transcoding jobs, web crawler)
    - In-order message processing (Ticketmaster waiting queue)
    - General producer/consumer decoupling for independent scaling (LC online judge)
2. Data stream
    - Need to process a lot of real time data (click aggregator)
    - Stream of messages consumed by multiple consumers simultaneously (Messenger or live commenting system)

## Things to keep in mind
- Scalability
    - Aim for < 1MB per message (key-val). NO BLOBS 
    - Each broker limit estimates are 1TB data and 10k messages per sec
    - To scale up, add more brokers (some services auto manage scaling); choose good partition key
    - Dealing with hot partitions:
        - if no requirement for ordering, just don't have partition keys. It'll distribute the messages among the partitions
        - Compound key. AdID:{1-10}. AdID:{userID}. No ordering among the splitted partitions. i.e., no ordering across 1-10 partitions
        - Backpressure. Slow down the producer
- Fault tolerance & durability
    - replica/follower partitions for each partiion
- Errors & retries
    - There's no native support for retries. Need to implement this manually
    - One pattern is to have a topic for retries and send it to that partition for the first failure, and then if that doesn't work, to dead letter queue (DLQ)
    - `SQS` supports exponential backoff out of the box, so consider using this
- Performance for a lot of real-time events
    - Things that can help even more
        - Producer sends messages in batches
        - Producer compresses (gzip) messages before sending
- Retention policies (minimizing storage)
    - retention by time
    - retention by bytes