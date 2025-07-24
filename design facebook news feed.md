# design facebook news feed

Assume we're handling uni-directional "follow" relationships

## Func
- users should be able to create posts
- " should be able to follow people
- " should be able to see the posts (text,photo,vid) of the people they follow, in chronological order

## Func OOS
- users reacting/commenting on other posts
- users signing up
- users 
- " should be able to search people
- " should be able to see the posts of a specific person they search
- ad

## Non Func
- System should be highly available & low latency. PACELC: AP+L
- Create posts can be eventually available, < 10 sec
- Post feed can also be eventually available < 10 sec
- The system should be able to handle blob storage for posts
- Seen feed should be loaded quickly < 100ms
- New feed should be loaded quickly < 500ms
- DAU 1B

## Non Func OOS
- Auth/author

## Core entities
- Users
- Posts
- Follow

## API
- POST /v1/post/create -> status
body {
    post_body
    attachments
    tags
}

- PUT /v1/follow -> status
body {
    username
}

- GET /v1/feed?timestamp={} -> posts[]

## Design & satisfying non-func 
Post
- id
- user_id
- text_body
- attachment_blob_uri

User
- id
- ...

Follow
- id
- follower_id
- followee_id

## Things learned
- Solving the problem of getting the feed of a user efficiently
    - "Infinite scroll" with pagination [[fb-live-comments]] explains it better
    - This is solved by keeping track of the timestamp of the oldest post the user has seen and have that as a cursor
    - When there's a GET request, build results using Follow and Post, cache like 500 of them into Redis
    - User sends the timestamp of the oldest post they've seen
- users with many follows. Solving the problem of fan-out (one request leading to many more processings) 
    - ! shift in thinking from computing the feed resulting on WRITE than READ
    - Have a separate feed table with partition key of userID and the val of postIDs[]. This may be a case where DyDB could be useful
- Users with many followers. The previous solution could slow down writes too much for people with many followers since it would mean the post update should go to a lot of users in the feed table
    - When a post is made,
        1. post service updates the post table
        2. post service sends the request to post queue
        3. post queue popped by workers
        4. workers can update the feed table
    - To optimize this even further, for accounts that have ridiculous number of followers rather than fanning out to 100m+ followers, we can put a flag on those accounts so that it doesn't go to the queue. 
        - when a user GETs its feed, we can use the precomputed feed table + for the flagged accounts, we can just normally get the post based on the timestamp
    - Another problem here is a hot key problem in which the shard (even in dydb) that has a user with lots of followers will get a lot of hits after a post. Even if we were to put Redis in front, the cluster is still going to have massive load
        - Having a redundant multiple instances of cache LB by client could mitigate. 
        - In dydb case, DAX solves this problem 

https://www.hellointerview.com/learn/system-design/problem-breakdowns/fb-news-feed