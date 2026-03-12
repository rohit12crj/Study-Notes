
<img width="368" height="317" alt="image" src="https://github.com/user-attachments/assets/2848eca1-f1bd-47ea-aa74-e52c352ecdbc" />

---

✅ Application Load Balancer (ALB)

Key Features
- Operates at Layer 7 — can inspect HTTP headers, path, query strings, host
- Supports HTTP, HTTPS, HTTP/2, WebSocket, gRPC
- Content-based routing (most important feature)
- Fixed response & redirect support
- Authenticates users via Cognito or OIDC

Routing Rules (Listener Rules)

Routes based on:
- Path — /api/* → Target Group A, /images/* → Target Group B
- Host header — api.example.com → TG-A, www.example.com → TG-B
- HTTP headers — custom headers
- Query strings — ?platform=mobile
- HTTP method — GET, POST etc.
- Source IP — CIDR-based

Target Types
- EC2 instances
- ECS tasks (containers)
- Lambda functions
- Private IP addresses

ALB - Good to know
- ALB has a fixed DNS name (no static IP) — use NLB if you need static IP
- X-Forwarded-For header — contains client's real IP (since ALB terminates connection)
- X-Forwarded-Port, X-Forwarded-Proto also added
- Supports slow start mode — gradually increases traffic to new targets
- Desync mitigation — protects against HTTP desync attacks

---

✅ Network Load Balancer (NLB)

Key Features
- Operates at Layer 4 — TCP, UDP, TLS
- Handles millions of requests per second with ultra-low latency (~100ms vs 400ms ALB)
- Static IP per AZ — one Elastic IP per AZ (useful for whitelisting)
- Preserves client source IP (no X-Forwarded-For needed)
- No security group on NLB itself — SGs on targets must allow NLB traffic

Target Types
- EC2 instances
- Private IP addresses
- ALB (NLB → ALB pattern for static IP + Layer 7 features)

Use Cases
- Gaming, IoT, financial trading (ultra-low latency)
- Static IP requirement
- Non-HTTP protocols (TCP, UDP)
- When client IP preservation is needed

---

✅ Listeners
- Process that checks for connection requests on a protocol + port
- Each LB must have at least one listener
- Listener has rules that define how to route requests
- Default rule is the fallback (no conditions)

---

✅ Target Groups
- Group of targets that receive traffic
- Each target group has its own health check settings
- Targets can be in multiple target groups
- Deregistration delay (connection draining) — time to complete in-flight requests before deregistering (default 300s, range 0–3600s)

---

✅ Health Checks
- Performed at target group level
- Protocols: HTTP, HTTPS, TCP
- Key settings: HealthCheckPath, HealthyThresholdCount, UnhealthyThresholdCount, Interval, Timeout
- Unhealthy targets are removed from rotation automatically

---

✅ Sticky Sessions (Session Affinity)
- Routes same client to same target for duration of session
- Uses a cookie
- Available on ALB and CLB
- NLB uses source IP-based persistence instead
- Downside: can cause uneven load distribution

Types:
- Duration-based cookie (AWSALB) — LB generates cookie, you set expiry
- Application-based cookie — your app generates cookie, custom name

---

✅ Cross-Zone Load Balancing
- Distributes traffic evenly across all registered targets in all AZs
- Without it: each LB node only routes to targets in its own AZ


<img width="473" height="95" alt="image" src="https://github.com/user-attachments/assets/9a75d862-aaf0-41da-955e-f453beb3c510" />

---

✅ SSL/TLS Termination
- LB terminates SSL → decrypts → forwards to targets (HTTP or re-encrypt)
- Uses X.509 certificates (managed via ACM — AWS Certificate Manager)
- Can upload your own certs

---

✅ SNI (Server Name Indication)
- Allows multiple SSL certs on one listener (multiple domains on one ALB/NLB)
- Client sends hostname in SSL handshake → LB selects correct cert
- Supported on ALB and NLB only — CLB supports only one cert

---

✅ HTTPS Listener
- Must specify default cert
- Can add cert list for SNI
- Security policy defines SSL protocols and ciphers

---

✅ Connection Draining / Deregistration Delay
- Time given to in-flight requests to complete when a target is deregistering or unhealthy
- ALB/NLB: called Deregistration Delay (default 300s)


---

✅ ALB Authentication
- Native support for user authentication

Integrates with:
- Amazon Cognito (user pools)
- OIDC providers (Google, Facebook, corporate IdP)
- Returns 401 or redirects to login page before forwarding to targets
- Great for internal apps without building auth logic yourself

---

✅ Security Groups
- ALB has its own SG — targets' SGs should allow traffic from ALB SG (not from internet)
- NLB has NO SG — targets must allow traffic from NLB's IP / client IPs

---

✅ WAF Integration
- AWS WAF can be attached to ALB only (not NLB/GWLB)

---

✅ Access Logs
- Disabled by default
- Stores detailed request info to S3
- Includes: client IP, request time, backend response, latency
- Useful for compliance and debugging
- No extra charge for logs, only S3 storage

---

✅ Monitoring
- CloudWatch Metrics: RequestCount, TargetResponseTime, HTTPCode_ELB_5XX, HealthyHostCount, UnHealthyHostCount
- Request Tracing — ALB injects X-Amzn-Trace-Id header
- CloudTrail — API call logging for LB config changes

---

✅ Auto Scaling Integration
- Load Balancing  works seamlessly with Auto Scaling Groups
- New instances automatically registered to target group on launch
- Terminated instances deregistered automatically
- Health check type can be set to ELB (instead of EC2) in ASG for app-level health checks

---

<img width="472" height="223" alt="image" src="https://github.com/user-attachments/assets/efe87273-fbf3-48de-8713-9bda8e2f49cc" />

---

✅ Key Exam Traps
- Need static IP on a load balancer → NLB (or NLB in front of ALB)
- Need to route based on URL path/host → ALB
- NLB preserves source IP, ALB does not (use X-Forwarded-For)
- SNI = multiple certs on one listener → ALB/NLB only
- ALB cannot have static IP directly — assign Elastic IP only to NLB
- WAF only attaches to ALB (not NLB)
- Cross-zone LB is always on for ALB (free), off by default for NLB (costs extra)
- Health checks at target group level, not listener level
- Deregistration delay = connection draining for in-flight requests

---

explain grace period

health check path can be applied only to ALB , in NLB there is no option to assign health check path
