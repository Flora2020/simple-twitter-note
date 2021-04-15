## 概覽

#### 前台
| Feature                  | method & URL         | query params   |
| ------------------------ | ------- | ---------- |--------------- |
| 註冊                     | POST /signup          | |
| 登入                     | POST /login           | |
| 瀏覽所有 tweets           | GET /tweets           | userId, likedUserId |
| 新增一筆 tweet            | POST /tweets          | | 
| 瀏覽一筆 tweet            | GET /tweets/:id       | | 
| 刪除一筆 tweet            | DELETE /tweets/:id    | |
| 瀏覽所有 reply            | GET /replies          | userId , tweetId | 
| 新增一筆 reply            | POST /replies         | | 
| 刪除一筆 reply            | DELETE /replies/:id   | |
| 瀏覽十筆 users            | GET /users            | |
| 瀏覽目前的 user           | GET /users/current    | |
| 瀏覽一筆 user             | GET /users/:id        | | 
| 瀏覽帳戶設定              | GET /users/settings    | | 
| 修改帳戶設定              | PUT /users/settings    | |
| 瀏覽個人資料              | GET /users/profile     | | 
| 修改個人資料              | PUT /users/profile     | | 
| 新增一筆 like            | POST /likes/:tweetId   | |
| 刪除一筆 like            | DELETE /likes/:tweetId | |
| 瀏覽某位 user 的追隨者名單 | GET /followings/:id    | | 
| 新增一筆追隨              | POST /followings/:id   | |
| 刪除一筆追隨              | DELETE /followings/:id | |
| 瀏覽某位 user 的關注名單   | GET /followers/:id     | |

#### 後台
| Feature       | method & URL              | 
| ------------- | ------------------------- | 
| 登入           | POST /admin/login        |
| 瀏覽所有 tweets | GET /admin/tweets        | 
| 刪除一筆 tweets | DELETE /admin/tweets/:id |
| 瀏覽所有 users  | GET /admin/users         | 

---
## 詳情

### 註冊
**method & URL**
```
POST /signup
```
**Parameter**
| Params        | Require | Type          |
| ------------- | ------- | ------------- |
| account       | true    | string (50)   |
| name          | false   | string (50)   |
| email         | true    | string (50)   |
| password      | true    | string (8~20) |
| passwordCheck | true    | string (8~20) |
**Response**
Success
```
status code: 200
{
  "message": ["Done."]
}
```
Failure
```
status code: 400
{
  "message": ["Wrong email format.", "Name cannot be longer than 50 bytes."]
}
```

### 前台登入
**method & URL**
```
POST /login
```
**Parameter**
| Params       | Require | Type          |
| ------------ | ------- | ------------- |
| account      | true    | string (50)   |
| password     | true    | string (8-20) |
**Response**
Success
```
status code: 200
{
  "message": ["Done."],
  "token": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpZCI6MSwiaWF0IjoxNjE4MTA5MTg0fQ.Dw9GkhPP7DsJnqCaFHSOOBwxCtv5uZjX6iOm8_7799M",
  "user": {
    "id": "2",
    "account": "@user2",
    "name": "John Doe",
    "avatar": "https://via.placeholder.com/250",
    "isAdmin": false
  },
    "isAuthenticated": true
}
```
Failure
```
status code: 403
{
  "message": ["User only."]
}
```

### 瀏覽所有 tweet
**method & URL**
```
GET /tweets
```
**Parameter**
| Params      | Require | Type   | Feature   |
| ----------- | ------- | ------ | --------- |
|             |         |        | 瀏覽所有 tweets（依 createdAt 欄位 DESC 排序） |
| userId      | false   | number | 瀏覽某位使用者新增的所有 tweets （依 createdAt 欄位 DESC 排序）|
| likedUserId | false   | number | 瀏覽某位使用者按讚的所有 tweets（依按讚時間 DESC 排序）|

**Response**
Success （只列出一筆 tweets 示意）
```
status code: 200
{
  "message": ["Done."],
  "tweets": [
    {
      "id": "1",
      "content": "Accusantium quidem nisi eos nesciunt dolor nihil accusantium saepe.",
      "createdAt": "2021-04-07T15:19:56.000Z",
      "replyCount": "34", 
      "likeCount": "15",
      "isLiked": false,
      "user": {
        "id": "1",
        "account": "@root",
        "name": "Root",
        "avatar": "https://via.placeholder.com/250"
      }
    }
  ]
}
```
Failure
```
status code: 404
{
  "message": ["User not found."]
}
```
**未來優化方向**
每次只取出二十筆 tweets
| Params | Require | Type   | Feature             |
| ------ | ------- | ------ | ------------------- |
| page   | false   | number | 取得相應頁數的 tweets |

### 新增一筆 tweet
**method & URL**
```
POST /tweets
```
**Parameter**
| Params  | Require | Type         |
| ------- | ------- | ------------ |
| content | true    | string (140) |

**Response**
Success
```
status code: 200
{
  "message": ["Done."],
  "tweetId": "5"
}
```
Failure
```
status code: 400
{
  "message": ["Content cannot be longer than 140 bytes."]
}
```

### 瀏覽一筆 tweet
**method & URL**
```
GET /tweets/:id
```

**Response**
Success
```
status code: 200
{
  "message": ["Done."],
  "tweets": 
    {
      "id": "1",
      "content": "Accusantium quidem nisi eos nesciunt dolor nihil accusantium saepe.",
      "createdAt": "2021-04-07T15:19:56.000Z",
      "replyCount": "34", 
      "likeCount": "15",
      "isLiked": false,
      "user": {
        "id": "1",
        "account": "@root",
        "name": "Root",
        "avatar": "https://via.placeholder.com/250"
      }
    }
}
```
Failure
```
status code: 400
{
  "message": ["Wrong tweet id format."]
}
```

### 刪除一筆 tweet
**method & URL**
```
DELETE /tweets/:id
```

**Response**
Success
```
status code: 200
{
  "message": ["Done."]
}
```
Failure
```
status code: 404
{
  "message": ["Tweet not found."]
}
```

### 瀏覽所有 reply
**method & URL**
```
GET /replies
```
**Parameter**
| Params  | Require | Type   | Feature   |
| ------- | ------- | ------ | --------- |
|         |         |        | 瀏覽所有 reply（依 createdAt 欄位 DESC 排序） |
| userId  | false   | number | 瀏覽某位使用者新增的所有 reply （依 createdAt 欄位 DESC 排序）|
| tweetId | false   | number | 瀏覽某個 tweet 的所有 replies（依 createdAt 欄位 ASC 排序） |

**Response**
Success （只列出一筆 reply 示意）
```
status code: 200
{
  "message": ["Done."],
  "replies": [
    {
      "id": "1",
      "content": "Accusantium quidem nisi eos nesciunt dolor nihil accusantium saepe.",
      "createdAt": "2021-04-07T15:19:56.000Z",
      "tweetId": "1",
      "user": {
        "id": "1",
        "account": "@root",
        "name": "Root",
        "avatar": "https://via.placeholder.com/250"
      }
    }
  ]
}
```
Failure
```
status code: 404
{
  "message": ["Tweet not found."]
}
```
**未來優化方向**
每次只取出二十筆 reply
| Params | Require | Type   | Feature            |
| ------ | ------- | ------ | ------------------ |
| page   | false   | number | 取得相應頁數的 reply |

### 新增一筆 reply
**method & URL**
```
POST /replies
```
**Parameter**
| Params  | Require | Type         |
| ------- | ------- | ------------ |
| content | true    | string (140) |
| tweetId | true    | number       |

**Response**
Success
```
status code: 200
{
  "message": ["Done."],
  "tweetId": "1"
  "replyId": "6"
}
```
Failure
```
status code: 400
{
  "message": ["Content cannot be longer than 140 bytes."]
}
```
```
status code: 404
{
  "message": ["Tweet not found."]
}
```

### 刪除一筆 reply
**method & URL**
```
DELETE /replies/:id
```

**Response**
Success
```
status code: 200
{
  "message": ["Done."]
}
```
Failure
```
status code: 404
{
  "message": ["Reply not found."]
}
```

### 瀏覽十筆 user （依跟隨者數量 DSEC 排序）
**method & URL**
```
GET /users
```
**Response**
Success（只列出一筆 user 示意）
```
status code: 200
{
  "message": ["Done."],
  "users": [
    {
      "id": 1,
      "account": "@root",
      "name": "root",
      "avatar": "https://via.placeholder.com/250",
      "followerCount": "50",
      "isFollowed": false
    }
  ]
}
```
Failure
```
status code: 500
{
  "message": ["Quia ducimus commodi."]
}
```


### 瀏覽目前的 user
**method & URL**
```
GET /users/current
```
**Response**
Success
```
status code: 200
{
  "message": ["Done."],
  "currentUser": {
    "id": "1",
    "account": "@root",
    "name": "Root",
    "avatar": "https://via.placeholder.com/250",
    "isAdmin": true
  },
    "isAuthenticated": true
}
```
Failure

```
status code: 401
Unauthorized
```
```
status code: 500
{
  "message": ["TypeError: Cannot read property 'id' of nul"],
}
```

### 瀏覽一筆 user
**method & URL**
```
GET /users/:id
```
**Response**
Success
```
status code: 200
{
  "message": ["Done."],
  "user": {
    "id": "1",
    "account": "@root",
    "name": "Root",
    "biography": "Ea ad ea quisquam recusandae rerum. Vero commodi quia quo veritatis fugiat. Ut aut neque sed molestiae deserunt.",
    "avatar": "https://via.placeholder.com/250",
    "cover": "https://via.placeholder.com/250",
    "tweetCount": "14",
    "followingCount": "23",
    "followerCount": "13",
    "isFollowed": false
  }
}
```
Failure
```
status code: 404
{
  "message": ["User not found."],
}
```
**Example**
- userId 1 追隨 userId 2 和 userId 3，則 userId 1 的 followingCount 是 2
- userId 4 追隨 userId 1，則 userId 1 的 followerCount 是 1
### 瀏覽帳戶設定
**method & URL**
```
GET /users/settings
```
**Response**
Success
```
status code: 200
{
  "message": ["Done."],
  "settings": {
    "id": "1",
    "account": "@root",
    "name": "Root",
    "email": "root@example.com"
  }
}
```
Failure
```
status code: 500
{
  "message": ["Quia ducimus commodi."]
}
```

### 修改帳戶設定
**method & URL**
```
PUT /users/settings
```
**Parameter**
| Params        | Require | Type          |
| ------------- | ------- | ------------- |
| account       | true    | string (50)   |
| name          | false   | string (50)   |
| email         | true    | string (50)   |
| password      | true    | string (8-20) |
| passwordCheck | true    | string (8-20) |
**Response**
Success
```
status code: 200
{
  "message": ["Done."]
}
```
Failure
```
status code: 400
{
  "message": ["Password and confirm password does not match.", "This email is already taken."]
}
```

### 瀏覽個人資料
**method & URL**
```
GET /users/profile
```
**Response**
Success
```
status code: 200
{
  "message": ["Done."],
  "settings": {
    "id": "1",
    "name": "Root",
    "biography": "Quia ducimus commodi.",
    "avatar": "https://via.placeholder.com/250",
    "cover": "https://via.placeholder.com/250"
  }
}
```
Failure
```
status code: 500
{
  "message": ["Quia ducimus commodi."]
}
```

### 修改個人資料
**method & URL**
```
PUT /users/profile
```
**Parameter**
| Params    | Require | Type                     |
| --------- | ------- | ------------------------ |
| name      | false   | string (50)              |
| biography | false   | string (160)             |
| avatar    | false   | file (.jpg, .jpeg, .png) |
| cover     | false   | file (.jpg, .jpeg, .png) |
**Response**
Success
```
status code: 200
{
  "message": ["Done."]
}
```
Failure
```
status code: 400
{
  "message": ["Only jpg, jpeg, and png files are accepted."]
}
```

### 新增一筆 like
**method & URL**
```
POST /likes/:tweetId
```
**Response**
Success
```
status code: 200
{
  "message": ["Done."]
}
```
Failure
```
status code: 404
{
  "message": ["Tweet not found."]
}
```

### 刪除一筆 like
**method & URL**
```
DELETE /likes/:tweetId
```
**Response**
Success
```
status code: 200
{
  "message": ["Done."]
}
```
Failure
```
status code: 404
{
  "message": ["Tweet not found."]
}
```

### 瀏覽某位 user 的追隨者名單（依開始追隨的時間 DESC 排序）
**method & URL**
```
GET /followings/:id
```
**Response**
Success（只列出一筆 follower 示意）
```
status code: 200
{
  "message": ["Done."],
  followers: [
    {
      "id": "1",
      "account": "@root",
      "name": "Root",
      "avatar": "https://via.placeholder.com/250",
      "biography": "Est dolorem rerum nobis nam quia ad nihil voluptates officia. Molestiae eius voluptatem quo rem est.",
      "isFollowed": true
    }
  ]
}
```
Failure
```
status code: 404
{
  "message": ["Following user not found"]
}
```
**Example**
userId 1 正在追蹤 userId 2，則 `GET /followings/2` 的回傳值會包含 userId 1

### 新增一筆追隨
**method & URL**
```
POST /followings/:id
```
**Response**
Success
```
status code: 200
{
  "message": ["Done."]
}
```
Failure
```
status code: 404
{
  "message": ["The user you want to follow dose not exist."]
}
```

### 刪除一筆追隨
**method & URL**
```
DELETE /followings/:id
```
**Response**
Success
```
status code: 200
{
  "message": ["Done."]
}
```
Failure
```
status code: 404
{
  "message": ["You cannot unfollow a user who you are not following."]
}
```

### 瀏覽某位 user 的關注名單（依開始追隨的時間 DESC 排序）
**method & URL**
```
GET /followers/:id
```
**Response**
Success（只列出一筆 following 示意）
```
status code: 200
{
  "message": ["Done."],
  followings: [
    {
      "id": "1",
      "account": "@root",
      "name": "Root",
      "avatar": "https://via.placeholder.com/250",
      "biography": "",
      "isFollowed": true
    }
  ]
}
```
Failure
```
status code: 404
{
  "message": ["Follower user not found"]
}
```
**Example**
userId 2 正在追蹤 userId 1，則 `GET /followers/2` 的回傳值會包含 userId 1

---
### 後台登入
**method & URL**
```
POST /admin/login
```
**Parameter**
| Params        | Require | Type        |
| ------------- | ------- | ----------- |
| account       | true    | string (50) |
| password      | true    | string (50) |
**Response**
Success
```
status code: 200
{
  "message": ["Done."],
  "token": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpZCI6MSwiaWF0IjoxNjE4MTA5MTg0fQ.Dw9GkhPP7DsJnqCaFHSOOBwxCtv5uZjX6iOm8_7799M",
  "user": {
    "id": "1",
    "account": "@root",
    "name": "Root",
    "isAdmin": true
  },
    "isAuthenticated": true
}
```
Failure
```
status code: 403
{
  "message": ["Administrator only."]
}
```

### 後台瀏覽所有 tweet（依 createdAt 欄位 DESC 排序）
**method & URL**
```
GET /admin/tweets
```
**Response**
Success（只列出一筆 tweet 示意）
```
status code: 200
{
  "message": ["Done."],
  "tweets": [
    {
      "id": "1",
      "shortenContent": "Ducimus similique impedit eligendi tenetur dolorum",
      "createdAt": "2021-04-07T15:19:57.000Z",
      "user": {
        "id": "1",
        "account": "@root",
        "name": "Root",
        "avatar": "https://via.placeholder.com/250"
      }
    }
  ]
}
```
Failure
```
status code: 500
{
  "message": ["Ducimus similique impedit eligendi tenetur dolorum."]
}
```

### 後台刪除一筆 tweet
**method & URL**
```
DELETE /admin/tweets/:id
```
**Response**
Success
```
status code: 200
{
  "message": ["Done."]
}
```
Failure
```
status code: 400
{
  "message": ["Tweet not found."]
}
```

### 後台瀏覽所有 user（依推文數 DESC 排序）
**method & URL**
```
GET /admin/users
```
**Response**
Success（只列出一筆 user 示意）
```
status code: 200
{
  "message": ["Done."],
  "users": [
    {
      "id": "1",
      "account": "@root",
      "name": "Root",
      "avatar": "https://via.placeholder.com/250",
      "cover": "https://via.placeholder.com/250",
      "tweetCount": "45",
      "followingCount": "30",
      "followerCount": "40",
      "likedCount": "50"
    }
  ]
}
```
Failure
```
status code: 500
{
  "message": ["Ducimus similique impedit eligendi tenetur dolorum."]
}
```
**Example**
- userId 1 追隨 userId 2 和 userId 3，則 userId 1 的 followingCount 是 `2`
- userId 4 追隨 userId 1，則 userId 1 的 followerCount 是 `1`
- userId 1 有兩個 tweet，一個 tweet 得到 5 個 like，另一個 tweet 得到 3 個 like，則 userId 1 的 likedCount 是 5 + 3 = `8`
