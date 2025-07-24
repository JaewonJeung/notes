# queue

## Purpose
- Handling bursty traffic to smooth out the load on the system
- Distributing work among multiple workers

## Caveats
- Don't introduce queue into sync workloads as this will most likely introduce latencies and won't be suitable for strong latency req (<500ms)
- Queues have at-least-once guarantee, which means that there may be a possibility of double enqueue. To handle this case, we can try to make the request idempotent by tracking/appending the request ID as well with UNIQUE feature in sql case