# design ticketmaster

## Fun
- search for events
- view events
- booking tickets

### fun OOS
- auth
- performers' side of view
- telling the client what the hottest thing is in the area

## non fun
- Query should be highly available and with low latency
- Booking should be atomic. That is, no double booking
- system should handle 1 mil / event query kind of scale
- There may be bursts
- the system is read-heavy

## non fun OOS
- GPDR
- PCI-DSS
- web-app security
- system backup

## core entities
- Users
- Event
- Ticket
- Performer
- Venue
- Order

## api

## design
- going over the apis for simple design first

## Things learned
- For the booking POST, the first implementation is using transaction and have available/booked field in the DB
    - Then for deep dive, we can implement distributed lock with TTL
    - ! It would still be good idea to have the transaction system in case the dist lock goes down and we still need to prevent double booking
    - ! Actually write down what the dist lock data would look like
    - ALTERNATIVELY, to not introduce another component, we can have a lock and lock expiration time on the DB row
- ! Remember to horizontally scale the actually services as well :)
- Speedups
    - Event VIEW heavy load mitigation
        - Caching: eventID:eventData. Taylor swift event. The name, venue, and description won't change often, so we can cache those easily
    - Event BOOKING heavy load mitigation
        - SSE for real-time seat update
        - Out of the box: Create an admin-enabled virtual queueing landing page that trickles people to the actual event page
    - Heavy SEARCH load mitigation (SQL "LIKE" is expensive)
        - For postgres, we can have full-text index
        - Alternatively, we can introduce [[elasticsearch]] w/ [[Change data capture (CDC)]] data syncing b/w DB <-> elasticsearch
        - Enable query result caching

https://www.hellointerview.com/learn/system-design/problem-breakdowns/ticketmaster