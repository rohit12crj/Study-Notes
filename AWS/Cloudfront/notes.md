Your page loads in 80 ms in Australia but 600 ms in India. Same backend. Same code. What would you use to fix this?

Q1. Your static website hosted on S3 is slow for users in different countries. How do you fix it?
Answer: Put Amazon CloudFront in front of S3:

Create a CloudFront distribution with S3 as the origin
CloudFront caches content at 400+ edge locations globally
Users are served from the nearest edge — drastically reduces latency
Use Origin Access Control (OAC) to make S3 bucket private — only CloudFront can access it
Update S3 bucket policy to allow only CloudFront OAC principal
Point your domain to the CloudFront distribution URL


Q2. You updated files in S3 but users are still seeing old content. How do you fix it?
Answer: The old content is cached at edge locations. Two solutions:

Invalidation — force CloudFront to clear cached content:

bashaws cloudfront create-invalidation \
  --distribution-id ABCDEF123 \
  --paths "/images/*" "/index.html"
```
- **Versioned file names** — better long-term approach:
  - Rename files on update: `app.v2.js` instead of `app.js`
  - CloudFront treats it as a new object — no invalidation needed
  - Old files expire naturally via TTL

Note: Invalidations cost money after 1000 paths/month — versioning is cheaper at scale.

---

**Q3. How do you serve your website over HTTPS using CloudFront?**

**Answer:**
- Request a **free SSL/TLS certificate** from **AWS Certificate Manager (ACM)**
- ACM certificate must be in **us-east-1** region (CloudFront requirement)
- Attach the certificate to your CloudFront distribution
- Set **Viewer Protocol Policy** to `Redirect HTTP to HTTPS`
- Add your custom domain (e.g., `www.example.com`) as an **Alternate Domain Name (CNAME)** in CloudFront
- Create a **CNAME or Alias record** in Route 53 pointing to the CloudFront domain

---

## 🟠 Intermediate Scenarios

**Q4. You want different caching behavior for static assets vs API calls in the same CloudFront distribution. How?**

**Answer:** Use **Cache Behaviors** — you can define multiple rules based on URL path patterns:

| Path Pattern | Origin | TTL | Cache |
|---|---|---|---|
| `/api/*` | ALB / API Gateway | 0s | Disabled |
| `/images/*` | S3 | 86400s (1 day) | Enabled |
| `*.js, *.css` | S3 | 604800s (7 days) | Enabled |
| `/*` (default) | S3 | 3600s | Enabled |

- **API calls** — set TTL to 0, forward all headers/cookies
- **Static assets** — aggressive caching, no headers forwarded
- Rules are evaluated top-down, most specific path wins

---

**Q5. How do you restrict CloudFront content so only paying users can access it?**

**Answer:** Use **Signed URLs** or **Signed Cookies**:

- **Signed URLs** — for individual files (e.g., a specific video):
  - Generate a signed URL with expiry time using CloudFront key pair
  - URL is valid only for the specified duration and optionally IP
  - Good for: one-off downloads, email links

- **Signed Cookies** — for multiple files or entire sections:
  - Set 3 CloudFront cookies in the user's browser after authentication
  - All subsequent requests automatically include valid cookies
  - Good for: streaming platforms, premium content sections

- Backend (Lambda/EC2) generates and issues the signed URL/cookie after verifying user subscription

---

**Q6. Your API hosted behind CloudFront is returning stale data. How do you ensure dynamic content is never cached?**

**Answer:**
- Set **TTL to 0** in the Cache Behavior for `/api/*`
- Use **Cache Policy: CachingDisabled** (AWS managed policy)
- Or set response headers from your origin:
```
Cache-Control: no-cache, no-store, must-revalidate

Forward necessary headers/cookies/query strings so CloudFront passes them to origin
Use Origin Request Policy to control what CloudFront forwards upstream
CloudFront still provides benefits: DDoS protection, SSL termination, global routing


Q7. How do you block traffic from specific countries in CloudFront?
Answer: Use CloudFront Geo Restriction:

Allowlist — only allow specific countries
Blocklist — block specific countries

bashaws cloudfront update-distribution \
  --distribution-id ABCDEF \
  --restrictions GeoRestriction={RestrictionType=blacklist,Locations=[CN,RU]}
```
- For more granular control (block by IP, rate limit, custom rules) — use **AWS WAF** attached to CloudFront
- CloudFront passes `CloudFront-Viewer-Country` header to origin if you need country-aware logic

---

## 🔴 Advanced Scenarios

**Q8. How do you run custom logic at the edge (e.g., auth check, URL rewrite) with CloudFront?**

**Answer:** Two options — **CloudFront Functions** and **Lambda@Edge**:

| Feature | CloudFront Functions | Lambda@Edge |
|---|---|---|
| Latency | Sub-millisecond | Milliseconds |
| Runtime | JS only | Node.js, Python |
| Triggers | Viewer request/response | All 4 events |
| Max exec time | 1ms | 5s (viewer), 30s (origin) |
| Network access | No | Yes |
| Cost | Very cheap | More expensive |
| Use case | URL rewrites, header manipulation, simple auth | Complex auth, A/B testing, API calls |

**Common use cases:**
- **URL rewriting** → CloudFront Functions (e.g., `/blog/post-1` → `/blog/index.html`)
- **JWT auth at edge** → Lambda@Edge viewer request
- **A/B testing** → Lambda@Edge sets cookie and routes to different origins
- **Security headers** → CloudFront Functions on viewer response

---

**Q9. Design a multi-region active-active architecture using CloudFront.**

**Answer:**
```
User → CloudFront → Origin Group (Primary + Failover)
                    ├── ALB in us-east-1
                    └── ALB in eu-west-1

Use Origin Groups with failover criteria (on 5xx errors or timeout)
For active-active: use Lambda@Edge to route based on user's region:

Viewer from Europe → EU origin
Viewer from US → US origin


Use Route 53 latency-based routing to select the nearest ALB as origin
DynamoDB Global Tables or Aurora Global Database for data replication across regions
S3 Cross-Region Replication for static assets
Result: low latency globally + automatic failover


Q10. Your CloudFront distribution is being hit by a DDoS attack. How do you protect it?
Answer:

AWS Shield Standard — automatically enabled for all CloudFront distributions, protects against L3/L4 attacks (free)
AWS Shield Advanced — for sophisticated L7 attacks, 24/7 DDoS response team, cost protection
AWS WAF on CloudFront — create rules to:

Rate limit IPs (e.g., max 2000 req/5min per IP)
Block known bad IPs using IP Sets
Use Managed Rule Groups (AWS or marketplace) for OWASP Top 10
Block based on request patterns (SQL injection, XSS)


CloudFront itself absorbs volumetric attacks — its massive network capacity handles large floods
Enable Real-time logs → Kinesis → analyze attack patterns


Q11. How do you serve different content to mobile vs desktop users via CloudFront?
Answer:

CloudFront can forward the User-Agent header to origin — origin serves different content
Better approach: use CloudFront device detection headers:

CloudFront-Is-Mobile-Viewer: true
CloudFront-Is-Desktop-Viewer: true
CloudFront-Is-Tablet-Viewer: true


Enable these in Origin Request Policy
Origin (or Lambda@Edge) reads the header and returns appropriate response
For edge-based routing: Lambda@Edge reads device header and rewrites URL to device-specific content
Cache separately per device type by including device header in Cache Key


<img width="587" height="343" alt="image" src="https://github.com/user-attachments/assets/649c7287-1807-4b23-b4b4-ff084ba51a2c" />


Troubleshooting steps:

Low cache hit rate → check TTL settings, too many unique cache keys (query strings/cookies)
High origin latency → scale origin, enable keep-alive, check origin health
403 errors → check OAC/OAI config, S3 bucket policy, signed URL/cookie validity
5xx errors → origin is unhealthy, check ALB/EC2/Lambda behind it
Enable CloudFront Access Logs to S3 for detailed per-request analysis
Use CloudFront Real-Time Logs via Kinesis for live traffic analysis

<img width="604" height="378" alt="image" src="https://github.com/user-attachments/assets/3bd92e78-2395-4501-82f4-202b35c13e2c" />

