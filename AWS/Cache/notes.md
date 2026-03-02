Beginner Scenarios
Q1. Your web application is making repeated DB queries for the same data. How do you fix it?
Answer: Introduce a caching layer using Amazon ElastiCache:

Use ElastiCache for Redis or Memcached in front of your database
Application checks cache first (cache-aside / lazy loading pattern):

Check Redis for the key
If cache hit → return data
If cache miss → query DB, store result in Redis with a TTL, return data


Reduces DB load dramatically for read-heavy workloads


Q2. What's the difference between Redis and Memcached in ElastiCache? When do you use each?
Answer:

<img width="604" height="215" alt="image" src="https://github.com/user-attachments/assets/736d7b52-81bf-426f-8a12-653a66e8204e" />


Q3. How does CloudFront act as a cache?
Answer: Amazon CloudFront is a CDN (Content Delivery Network) that caches content at edge locations globally:

Static assets (images, CSS, JS) served from the nearest edge — reduces latency
Configured via Cache Behaviors with TTL settings
Cache key can include headers, query strings, cookies
Content invalidated using Invalidation API or via versioned URLs
Also caches API responses when used in front of API Gateway or ALB


🟠 Intermediate Scenarios
Q4. Your ElastiCache Redis node is running out of memory. How do you handle it?
Answer:

Scale up — move to a larger node type
Scale out — enable Redis Cluster Mode to shard data across multiple nodes
Set appropriate eviction policies:

allkeys-lru — evict least recently used keys (good for caching)
volatile-lru — evict LRU keys that have TTL set
noeviction — return error when full (for session stores)


Review TTLs — ensure you're not caching data indefinitely
Monitor CacheHits vs CacheMisses and FreeableMemory in CloudWatch


Q5. Users are seeing stale data after an update. How do you handle cache invalidation?
Answer: Cache invalidation is one of the hardest problems. Strategies:

TTL-based expiry — set a reasonable TTL (e.g., 60s), accept slight staleness
Write-through — update cache whenever DB is updated (always fresh, but more writes)
Event-driven invalidation — when data changes, publish an event → Lambda deletes/updates the cache key
Versioned cache keys — user:123:v2 instead of user:123 — new version = new key
Cache-aside with short TTL — simple and effective for most use cases


Q6. How do you cache API Gateway responses?
Answer: Enable API Gateway caching:

Enable per-stage in API Gateway settings
Set TTL (default 300s, max 3600s)
Cache key is based on the request URL + optional headers/query params
Clients can bypass cache using Cache-Control: max-age=0 header (if you allow it)
Good for read-heavy APIs with infrequently changing responses
For more control, put CloudFront in front of API Gateway for edge caching


Q7. Your application has a thundering herd problem — cache expires and thousands of requests hit the DB simultaneously. How do you fix it?
Answer: This is the cache stampede problem. Solutions:

Probabilistic early expiration (PER) — refresh cache slightly before it expires
Mutex/lock pattern — only one request populates the cache; others wait:

Use Redis SETNX to acquire a lock
Other requests serve stale data or wait briefly


Jitter on TTL — instead of TTL=300 for all keys, use TTL = 300 + random(0, 30) to spread expiry
Background refresh — a background job refreshes cache before expiry
Stale-while-revalidate — serve stale data immediately, refresh asynchronously


🔴 Advanced Scenarios
Q8. How do you implement session management at scale using ElastiCache?
Answer: Use ElastiCache for Redis as a centralized session store:

Store sessions as Redis hashes: session:{sessionId} → {userId, role, expiry}
Set TTL equal to session timeout (e.g., 30 minutes)
Every app server reads/writes to the same Redis cluster — no sticky sessions needed
Enable Multi-AZ with automatic failover for high availability
Use Redis AUTH + in-transit encryption (TLS) + at-rest encryption for security
This scales horizontally — add more app servers without session issues


Q9. Design a caching strategy for a leaderboard with millions of users.
Answer: Use Redis Sorted Sets (ZSET) — perfect for leaderboards:
ZADD leaderboard <score> <userId>
ZREVRANK leaderboard <userId>       → user's rank
ZREVRANGE leaderboard 0 9 WITHSCORES → top 10

Real-time updates — update score on every game event
Pagination — use ZREVRANGE with offset for top-N queries
User rank — ZREVRANK is O(log N) — very fast
Regional leaderboards — separate sorted sets per region
Persistence — enable Redis AOF to avoid losing scores on restart
TTL — for weekly/monthly leaderboards, expire the key at end of period


Q10. Your ElastiCache cluster is in a private VPC. Lambda can't connect to it. What do you do?
Answer:

Lambda must be deployed inside the same VPC as ElastiCache
Configure Lambda with the correct subnets (same as ElastiCache) and security group
ElastiCache security group must allow inbound on port 6379 (Redis) or 11211 (Memcached) from Lambda's security group
Ensure Lambda IAM role has AWSLambdaVPCAccessExecutionRole
Use ElastiCache connection pooling — Lambda reuses connections across warm invocations (initialize client outside handler)
Watch out for connection exhaustion — Lambda can spin up many concurrent instances, each opening a connection


Q11. How do you use DAX (DynamoDB Accelerator) and when should you prefer it over ElastiCache?
Answer: DAX is an in-memory cache purpose-built for DynamoDB:

Fully managed, no cache logic in application code
Sits between app and DynamoDB, API-compatible — just change the endpoint
Reduces read latency from milliseconds to microseconds
Supports item cache (GetItem) and query cache (Query/Scan)

Use DAX when:

You're already on DynamoDB and want zero-code caching
Need microsecond latency for DynamoDB reads
Don't want to manage cache invalidation logic

Use ElastiCache when:

Caching data from RDS, external APIs, or computed results
Need advanced data structures (sorted sets, pub/sub)
Multi-source caching beyond just DynamoDB


Q12. How do you monitor and troubleshoot cache performance in AWS?
<img width="555" height="281" alt="image" src="https://github.com/user-attachments/assets/cdf21e22-64c4-4a87-b954-ba768ec61266" />

Troubleshooting steps:

Low hit rate → review TTL, check if keys are set correctly
High evictions → memory too small or TTL too long, scale up/out
High latency → check network, node CPU, or connection count
Replication lag → scale up primary or reduce write volume

<img width="605" height="269" alt="image" src="https://github.com/user-attachments/assets/1f9df8e9-193e-4641-af40-a126f6cfff31" />
