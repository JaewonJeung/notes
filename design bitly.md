Bit.ly is a URL shortening service that converts long URLs into shorter, manageable links. It also provides analytics for the shortened URLs.

Functional:
- receives a url and returns a shortened url
- 1:1 mapping of the url (no collision). Querying same input url returns the same output url
- Ability to retrieve the url even in the future
- Provide analytics


non-functional
- AP (?? how to guarantee 1:1 mapping? Dist lock?
- Analytics latency should be low
- url shortening latency must be low


Core entities
- Input URL
- Output URL
- Analytics?


API
- POST /v1/shorten -> shortened url
    - body {
        - input_url
    - }
- GET (REDIRECT) /v1/urls/:shorted_url -> original_url
- GET /v1/analytics -> analytics json
- GET /v1//analytics/aggregate/:timestamp_choice -> aggregate_analytics


Diagram:

INIT
Client <-> API Gateway/LB (note that every component needs LB) <-> url shortening service (repl) <-> dydb 
 														 ^—> analytics service (repl) ————^
postgres
- origin_url
- shortened_url

																						———> Redis url cache
																						v
																		   v—- url shortening service (repl)      <-> 	dydb
																			     ^|
Client <-> API Gateway/LB (note that every component needs LB. Handles RL as well) —> Queue (for bursts) 
														Queue (same Q impl as above)												|	
 														 ^—> analytics service (repl) <-> Analytics cache <——- analytics cronjob <————
postgres
- origin_url
- shortened_url

URL Retrieval cache (redis)
- shortened_url: origina_url

analytics cache (redis might be an overkill):
- analytics for past 10 min
- analytics for past 30 min
- analytics for past 60 min
- …


Things to note in this problem
- Redirect types (301 vs 302)
- Asymmetric operation (more read vs write)
    - scaling primary server separating read and write ops. Scaling independently
- HDD vs SSD vs Memcache speed difference
    - 
    Memory access time: ~100 nanoseconds (0.0001 ms)
    SSD access time: ~0.1 milliseconds
    HDD access time: ~10 milliseconds
- Ensuring unique url

https://www.hellointerview.com/learn/system-design/problem-breakdowns/bitly