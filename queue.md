# queue

## Purpose
- Handling bursty traffic to smooth out the load on the system
- Distributing work among multiple workers

## Caveats
- Don't introduce queue into sync workloads as this will most likely introduce latencies and won't be suitable for strong latency req (<500ms)