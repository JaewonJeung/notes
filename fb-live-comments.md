# fb-live-comments

Handling infinite scroll:
pagination technique
- cursor technique: 
    - Something like GET /comments/:liveVideoId?cursor={last_comment_id}&pageSize=10
    - where the cursor is an indexed field. 
    - this is way more efficient and reliable than the offset method since we know exactly where to resume instead of having to do offset calculation and linear scanning of the comments
    - the actual logic can be something like "KeyConditionExpression": "liveVideoId = :liveVideoId AND commentId < :cursor"; with a limit of pageSize