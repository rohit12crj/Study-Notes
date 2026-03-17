
✅ When will us use Redis vs memcached

<img width="604" height="215" alt="image" src="https://github.com/user-attachments/assets/736d7b52-81bf-426f-8a12-653a66e8204e" />

| Use Case      | Redis | Memcached |
| ------------- | ----- | --------- |
| Sessions      | ✅     | ❌         |
| Leaderboards  | ✅     | ❌         |
| Rate limiting | ✅     | ❌         |
| Pub/Sub       | ✅     | ❌         |
| Simple cache  | ✅     | ✅         |
| Persistence   | ✅     | ❌         |

<img width="352" height="408" alt="image" src="https://github.com/user-attachments/assets/4a73e1a1-636d-4a17-a8e4-3fc2c9df9a5a" />

<img width="417" height="436" alt="image" src="https://github.com/user-attachments/assets/c94c2f92-fe7b-4c2f-aff2-9708586e53b2" />

---

✅ Explain different caching techniques  & when to use which ?

| Technique                      | How It Works                                           | Best Use Case             | Pros                                  | Cons                           | Example                       |
| ------------------------------ | ------------------------------------------------------ | ------------------------- | ------------------------------------- | ------------------------------ | ----------------------------- |
| **Cache-Aside (Lazy Loading)** | App checks cache → miss → fetch from DB → update cache | Read-heavy systems        | Simple, widely used, cost-efficient   | Stale data possible            | Product catalog, user profile |
| **Write-Through**              | Write goes to cache → cache writes to DB               | Strong consistency needed | Data always consistent                | Higher write latency           | User settings, banking data   |
| **Write-Back (Write-Behind)**  | Write to cache → DB updated asynchronously             | High write throughput     | Fast writes, reduces DB load          | Risk of data loss              | Analytics, logging            |
| **Read-Through**               | Cache fetches data from DB on miss automatically       | Simplify app logic        | Cleaner architecture                  | Less control in app            | Managed cache systems         |
| **Refresh-Ahead**              | Cache refreshes data before expiry                     | Frequently accessed data  | Low latency, avoids cache miss        | Extra overhead                 | Trending products             |
| **TTL-Based Caching**          | Data expires after fixed time                          | Non-critical freshness    | Simple, easy to implement             | Can serve stale data           | Weather, news                 |
| **Distributed Cache**          | Shared cache across multiple instances                 | Scalable systems          | Consistent across services            | Network overhead               | Redis cluster                 |
| **CDN / Edge Caching**         | Cache content closer to users globally                 | Static/global content     | Very low latency, reduces origin load | Limited to static/HTTP caching | Images, JS, CSS               |


<img width="566" height="340" alt="image" src="https://github.com/user-attachments/assets/e85f27d7-693b-47f7-9ff3-39a8046dabbb" />
