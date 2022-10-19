# ppt redis 01

# Redis & Redisson

- chart

# introduction

We will talk about each construct in detail,

- the commands that can be performed and
- the performance implications.

Knowledge of what the data structures can do is critical to understand 
but successful use of Redis is also about

- when and
- how to use these data structures
- to solve specific domain problems and
- use cases.

To help join those dots we'll combine each of these data structures to solve a concrete problem, 
how to use Redis to create a reservation system for the 2020 Redis games in Tokyo. 

You'll see how we can do 
- inventory control, 
- state management, 
- seat reservation, 
- notifications 
- and number of other use cases with Redis.

## important concepts

- keys
  - naming
    - sales_summary
    - sales_summary:Football
    - .
    - user:{userSeqId} -> user:1000 -> 存 user 的資料
    - user:{userSeqId}:followers -> user:1000:followers -> 存 user 號碼 1000 的 followers
    - .
    - set inventory:100-meters-womens-final 1000
    - set inventory:4x100m-womens-final 1000
    - .
    - player:1000:credit-balance 
    - player:1000.wallet -> 100
    - player:1000.wallet -> empty
    - .
  - 好的命名規範，才能避免以後衝突
  - .
  - Whatever you choose, it's important that you have a consistent convention used throughout the team and the project.
  - This becomes critical when multiple teams, projects, or microservices will use the same Redis instance.
  - We want to avoid any key name clashes across projects, use cases, and domains.
  - .
- .

- db seq id -> uuid
- .

- 資料大小
  - key: 512MB
  - value: 512MB
  - 不代表 真的可以放 512MB
  - 要考慮 資料傳輸 / 資料儲存，以及所選擇的 redis data type
  - .
- .

## use cases

- Strings
  - can be numbers or plain text
  - 小數以下 17 位
  - 
  - .
- Hash -> Java HashMap
  - model a user from a SQL table
  - 可以動態 add / remove field
  - .
  - summary data
    - HINCRBY sales_summary total_sales 1
    - HINCRBY sales_summary total_sales -1
    - HMSET sales_summary total_sales 1721512 total_tickets_sold 56282
    - .
  - ticket availability
    - HSET event:judo availability:gold 7990
    - HSET event:judo availability:silver 2000
    - 旅行社 機票
    - 台鐵 搶票
    - .
  - Rate Limiting
    - hmset ep-20180210
      - "/pet/{petId}" 100
      - "/booking/{petId}" 100
  - .

<img alt="" src="./img/HASH-player.42.png" style="width: 800px">

<img alt="" src="./img/HASH-sales_summary.png" style="width: 800px">

- Lists -> Queues
  - LRANGE 0 9 in order to get the latest 10 posted items.
  - Another example is social news feed system: FB, Slack, Twitter, etc.
  - .
  - 可以插入中間任何位置
  - .
  - Sportfy playlist
    - .
  - Order list
    - LPUSH orders:4x100-womens-final jane:4 bill:8 charlie:6
    - LPUSH waitlist:basketball-mens-final brain:2
    - .
  - .
- Sets
  - SINTER tag:Java:posts tag:MySQL:posts tag:Redis:posts
  - search filter
  - .
- Sorted Sets
  - Stack Overflow rank the highest voted answers for each proposed question
  - leaderboard
  - .



- Strings
  - customer:1500 -> jane
  - .
- .
- Streams -> collecting data
  - .
- Pub/Sub -> messaging
  - .
- .




