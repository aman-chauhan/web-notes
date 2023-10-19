# Web Notes
A Modern App for Notes on the Web


## Services

### Application Services

- User Service
- Note Service
- Topic Service
- Feed Service
- API Service
- UI Service

### Infrastructure Services

- RPC Service
- Message Broker Service
- Reverse Proxy Service


## APIs

- [ ] POST /api/login

- [ ] POST /api/user
- [ ] GET /api/user/<uuid>
- [ ] GET /api/user/<uuid>/history
- [ ] UPDATE /api/user/<uuid>
- [ ] DELETE /api/user/<uuid>
- [ ] POST /api/user/<uuid>/follow
- [ ] POST /api/user/<uuid>/unfollow

- [ ] POST /api/note
- [ ] GET /api/note
- [ ] GET /api/note/<uuid>
- [ ] GET /api/note/<uuid>/history
- [ ] UPDATE /api/note/<uuid>
- [ ] DELETE /api/note/<uuid>
- [ ] POST /api/note/<uuid>/like
- [ ] POST /api/note/<uuid>/unlike

- [ ] POST /api/note/<uuid>/comment
- [ ] POST /api/note/<uuid>/comment/<uuid>
- [ ] GET /api/note/<uuid>/comment
- [ ] GET /api/note/<uuid>/comment/<uuid>
- [ ] UPDATE /api/note/<uuid>/comment/<uuid>
- [ ] DELETE /api/note/<uuid>/comment/<uuid>

- [ ] GET /api/topic
- [ ] GET /api/topic/<uuid>

- [ ] GET /api/feed


## ER Diagram

```mermaid
erDiagram
    User {
        string uuid PK
        string username
        string fname
        string lname
        string email
        string password
        string salt
    }

    Note {
        string uuid PK
        string user_uuid FK
        datetime created_at
        string title
        string body
        bool public
    }

    NoteHistory {
        string note_uuid FK,PK
        datetime updated_at PK
        string title
        string body
        bool public
        bool deleted
    }

    Like {
        string user_uuid FK,PK
        string note_uuid FK,PK
        datetime liked_at
    }

    LikeHistory {
        string user_uuid FK,PK
        string note_uuid FK,PK
        datetime updated_at PK
        bool unliked
    }

    Comment {
        string user_uuid FK,PK
        string note_uuid FK,PK
        string comment_uuid PK
        string parent_uuid
        datetime commented_at
        string body
    }

    CommentHistory {
        string comment_uuid FK,PK
        datetime updated_at PK
        string body
        bool deleted
    }

    Topic {
        string uuid PK
        string name
        datetime created_at
    }

    NoteTopic {
        string note_uuid FK,PK
        string topic_uuid FK,PK
    }

    NoteTopicHistory {
        string note_uuid FK,PK
        string topic_uuid FK,PK
        datetime updated_at PK
        bool removed
    }

    TopicSubscription {
        string user_uuid FK,PK
        string topic_uuid FK,PK
        datetime subscribed_at
    }

    TopicSubscriptionHistory {
        string user_uuid FK,PK
        string topic_uuid FK,PK
        datetime updated_at PK
        bool unsubscribed
    }

    UserFollow {
        string user_uuid FK,PK
        string following_uuid FK,PK
        datetime followed_at
    }

    UserFollowHistory {
        string user_uuid FK,PK
        string following_uuid FK,PK
        datetime updated_at PK
        bool unfollowed
    }

    User |o--o{ Note : "create"
    NoteHistory }o--|| Note : "update"
    User ||--o{ Like : "like"
    Like }o--|| Note : "like"
    LikeHistory }o--|| Like : "update"
    User ||--o{ Comment : "comment"
    Comment }o--|| Note : "comment"
    Comment |o--o{ Comment : "subcomment"
    CommentHistory }o--|| Comment : "update"
    Topic ||--o{ NoteTopic : "has"
    NoteTopic }o--|| Note : "has"
    NoteTopicHistory }o--|| NoteTopic : "update"
    User ||--o{ TopicSubscription : "subscribe"
    TopicSubscription }o--|| Topic : "subscribe"
    TopicSubscriptionHistory }o--|| TopicSubscription : "update"
    User ||--o{ UserFollow : "follows"
    UserFollow }o--|| User : "follows"
    UserFollow ||--o{ UserFollowHistory : "update"
```
