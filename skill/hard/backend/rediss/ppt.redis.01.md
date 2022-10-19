# ppt redis 01

# Redis & Redisson

- chart

## important concepts

- keys
  - naming
    - sales_summary
    - sales_summary:Football
  - .
- .

- db seq id -> uuid
- .

## use cases

- Lists -> Queues
  - LRANGE 0 9 in order to get the latest 10 posted items.
  - Another example is social news feed system
  - .
- Sets
  - SINTER tag:Java:posts tag:MySQL:posts tag:Redis:posts
  - search filter
  - .
- Sorted Sets
  - Stack Overflow rank the highest voted answers for each proposed question
  - leaderboard
  - .
- Hash -> Maps
  - model a user from a SQL table
  - summary data
    - HINCRBY sales_summary total_sales 1
    - HINCRBY sales_summary total_sales -1
    - HMSET sales_summary total_sales 1721512 total_tickets_sold 56282
    - .
  - .

<img alt="" src="./img/HASH-sales_summary.png" style="width: 800px">

- Strings
  - customer:1500 -> jane
  - .
- .
- Streams -> collecting data
  - .
- Pub/Sub -> messaging
  - .
- .




