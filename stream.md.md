# stream.md

## The need for it
- Large amount of real-time data processing
    - ingesting high volume of user-engagement data (like, comments, shares). Analytics dashboard
- Event sourcing
    - snapshots of the state of the application 
    - Auditability. roll back. Finance--each transaction can be an event
- publish-subscribe pattern 
    - real-time chat app; each stream would be a room 


## Concrete
[[Kafka]]