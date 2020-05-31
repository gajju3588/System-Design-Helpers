## Important Links
https://www.quora.com/What-are-the-best-practices-for-building-something-like-a-news-feed
https://www.slideshare.net/danmckinley/etsy-activity-feeds-architecture
http://highscalability.com/blog/2013/10/28/design-decisions-for-scaling-your-high-traffic-feeds.html


## My Notes

Three Subproblems
### Data Model
- Need to store user information, their relationship, activities. Need to optimize for read/write
- Two crucial Data : User-Feed, User-User. 
- Discussion over denormalization of data. storing feed content with feed id in user-feed tablee or store seperately
    - Data redundancy if denormalized
    - Data consistency: need to update multiple tables for any new update
    

### Feed Ranking
- Its not a chronological ranking
- Show in rank of probability of getting clicked of a post
- hide shown post in a rerank layer near front-end
- Closeness of friends, likeliness of page and person, Time decay of a story. All such features contribute to ranking


### Feed Publishing
- Problem when there is one user with millions of followers.
- Join to find all friends, get all posts for those friends. Expensive.
    - push : Update timeline of all the friends of a person whenever there is a new update. Read is faster, write is slower.
        - fanout on write
    - pull : Read all the post whenever user go to homepage. Read is slower, write is faster.
        - fanout on read
    - Need combination of both
        - disable push for high profile users.
        - limit fanout to only active friends.
