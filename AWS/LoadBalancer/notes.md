difference between alb , nlb & glbp

Q1. Your web application is getting too much traffic for a single EC2 instance. How do you scale it?
Answer: Use an Application Load Balancer (ALB) + Auto Scaling Group:

Deploy multiple EC2 instances behind an ALB
ALB distributes incoming requests across healthy instances
Auto Scaling Group automatically adds/removes instances based on CPU/traffic
Architecture:

Internet → Route 53 → ALB → Target Group → EC2 instances (ASG)

ALB performs health checks — removes unhealthy instances automatically
Instances in multiple AZs for high availability
ALB is highly available by default — spans multiple AZs


Q2. What are the three types of AWS Load Balancers and when do you use each?
Answer:
FeatureALBNLBGLBLayerL7 (HTTP/HTTPS)L4 (TCP/UDP/TLS)L3 (IP)RoutingPath, host, header, queryIP, port, protocolTransparent bump-in-wirePerformanceGoodUltra-low latency, millions RPSN/AUse caseWeb apps, microservices, APIsGaming, IoT, financial, real-timeFirewalls, IDS/IPS, deep packet inspectionWebSocketYesYesNoStatic IPNo (DNS-based)Yes (Elastic IP)NogRPCYesNoNo

ALB — default choice for web applications and REST/gRPC APIs
NLB — when you need static IP, extreme performance, or non-HTTP protocols
GLB — when routing traffic through third-party network appliances


Q3. How do you configure an ALB to route traffic to different services based on URL path?
Answer: Use ALB Path-Based Routing with multiple Target Groups:
/api/*     → Target Group: API servers (ECS)
/images/*  → Target Group: Image service (EC2)
/admin/*   → Target Group: Admin app (EC2)
/*         → Target Group: Frontend (EC2)
Setup:

Create separate Target Groups for each service
Add Listener Rules on ALB port 443:

bashaws elbv2 create-rule \
  --listener-arn arn:aws:elasticloadbalancing:... \
  --conditions Field=path-pattern,Values='/api/*' \
  --actions Type=forward,TargetGroupArn=arn:...:tg/api-tg \
  --priority 10

Rules evaluated by priority — lowest number first
Default rule (priority last) handles unmatched requests


🟠 Intermediate Scenarios
Q4. Your ALB is returning 504 Gateway Timeout errors. How do you troubleshoot?
Answer: 504 means ALB didn't get a response from the target in time. Steps:

Check target health — are instances healthy in the target group?
Check instance logs — is the app processing requests too slowly?
Check ALB idle timeout — default is 60 seconds:

If your app takes longer, increase it up to 4000 seconds
aws elbv2 modify-load-balancer-attributes --idle-timeout.timeout-seconds 120


Check security groups:

EC2 security group must allow inbound from ALB security group on app port


Check target response time in CloudWatch:

TargetResponseTime metric — shows how long targets take


Check connection draining — if targets are deregistering mid-request
Scale out — if targets are overloaded, add more instances


Q5. How do you enable HTTPS on an ALB and redirect HTTP to HTTPS?
Answer:
Step 1 — Get SSL certificate from ACM:
bashaws acm request-certificate \
  --domain-name www.example.com \
  --validation-method DNS
Step 2 — Add HTTPS listener on port 443:
bashaws elbv2 create-listener \
  --load-balancer-arn arn:... \
  --protocol HTTPS \
  --port 443 \
  --certificates CertificateArn=arn:aws:acm:... \
  --default-actions Type=forward,TargetGroupArn=arn:...
Step 3 — Add HTTP listener with redirect:
bashaws elbv2 create-listener \
  --load-balancer-arn arn:... \
  --protocol HTTP --port 80 \
  --default-actions \
    Type=redirect,RedirectConfig='{Protocol=HTTPS,Port=443,StatusCode=HTTP_301}'
```

- ALB handles SSL termination — backend EC2 receives plain HTTP
- For **end-to-end encryption** — use HTTPS on backend too

---

**Q6. How do you implement blue/green deployment using ALB?**

**Answer:** Use **ALB Weighted Target Groups**:
```
ALB Listener
  ├── Blue Target Group  (current v1) — weight 100
  └── Green Target Group (new v2)    — weight 0
Deployment steps:

Deploy new version to Green target group (separate EC2/ECS)
Shift small % of traffic to Green (canary test):

Blue: 90%, Green: 10%


Monitor errors, latency in CloudWatch
Gradually shift: 50/50 → 0/100
If issues — instantly flip back to Blue (weight 100/0)

bashaws elbv2 modify-rule \
  --rule-arn arn:... \
  --actions Type=forward,ForwardConfig='{
    TargetGroups=[
      {TargetGroupArn=arn:blue,Weight=90},
      {TargetGroupArn=arn:green,Weight=10}
    ]
  }'

AWS CodeDeploy automates this entire flow for ECS and EC2
For EKS — use AWS Load Balancer Controller with Ingress weights


Q7. Your application needs sticky sessions — users must always hit the same server. How do you configure this?
Answer: Enable Sticky Sessions (Session Affinity) on the Target Group:
bashaws elbv2 modify-target-group-attributes \
  --target-group-arn arn:... \
  --attributes \
    Key=stickiness.enabled,Value=true \
    Key=stickiness.type,Value=lb_cookie \
    Key=stickiness.lb_cookie.duration_seconds,Value=86400
```

**Two types:**
- **Load Balancer-generated cookie** (`AWSALB`) — ALB sets cookie, routes user to same target
- **Application-based cookie** — your app sets the cookie, ALB respects it

**Important considerations:**
- Sticky sessions can cause **uneven load distribution**
- If a target goes down, user session is lost — re-routed to new target
- **Better long-term solution** — store sessions in **ElastiCache Redis** and remove stickiness requirement entirely

---

**Q8. How do you protect your ALB from SQL injection and XSS attacks?**

**Answer:** Attach **AWS WAF** to the ALB:

1. Create a **WAF Web ACL**
2. Add **Managed Rule Groups**:
   - `AWSManagedRulesCommonRuleSet` — OWASP Top 10 (SQLi, XSS, etc.)
   - `AWSManagedRulesSQLiRuleSet` — SQL injection specific
   - `AWSManagedRulesKnownBadInputsRuleSet` — known attack patterns
3. Add **Rate limiting rule** — block IPs exceeding X requests/5min
4. Associate WAF Web ACL with ALB
5. Set action: **Block** (return 403) or **Count** (monitor first)

**Additional ALB security:**
- Enable **HTTPS only** — redirect all HTTP
- Use **Security Policy** — enforce TLS 1.2+ only
- Enable **Access Logs** → S3 for audit trail
- Place ALB in **public subnets**, EC2 in **private subnets**
- EC2 security group — only allow inbound from ALB security group

---

## 🔴 Advanced Scenarios

**Q9. How do you route traffic to different microservices using a single ALB?**

**Answer:** Use **ALB with multiple routing rules** — host-based + path-based:
```
api.example.com/users/*    → User Service Target Group
api.example.com/orders/*   → Order Service Target Group
api.example.com/payments/* → Payment Service Target Group
admin.example.com/*        → Admin Service Target Group
Host-based routing:
bashaws elbv2 create-rule \
  --conditions \
    Field=host-header,Values='admin.example.com' \
  --actions Type=forward,TargetGroupArn=arn:...:admin-tg
Combined host + path:
bash--conditions \
  '[{"Field":"host-header","Values":["api.example.com"]},
    {"Field":"path-pattern","Values":["/orders/*"]}]'
```

**Advanced routing:**
- Route by **HTTP header** — `X-Version: v2` → new target group
- Route by **query string** — `?region=eu` → EU target group
- Route by **source IP** — internal IPs → internal service
- One ALB can handle up to **100 rules** per listener

---

**Q10. How do you handle a scenario where your NLB targets are in different AWS accounts?**

**Answer:** Use **NLB with PrivateLink** for cross-account traffic:

**Architecture:**
```
Account A (Consumer)          Account B (Provider)
VPC-A → NLB Endpoint  ←→  PrivateLink ←→  NLB → Targets
```

**Setup in Provider Account (B):**
1. Create NLB in front of your service
2. Create **VPC Endpoint Service** pointing to the NLB
3. Whitelist Consumer Account A

**Setup in Consumer Account (A):**
1. Create **VPC Interface Endpoint** pointing to the endpoint service
2. Traffic flows privately — never over public internet

**Use cases:**
- SaaS providers exposing services to customers
- Shared services across business units in different accounts
- Marketplace services

**Benefits:**
- No VPC peering needed
- No route table changes
- Traffic stays within AWS network
- Consumer controls which endpoints they connect to

---

**Q11. Your ALB access logs show a lot of 502 errors. How do you diagnose and fix them?**

**Answer:** 502 Bad Gateway means **target returned an invalid response** to ALB.

**Common causes and fixes:**

| Cause | Fix |
|---|---|
| App crashed / not running | Restart app, check EC2/ECS health |
| App returned invalid HTTP response | Fix app code — ensure proper HTTP response format |
| Keep-alive timeout mismatch | App keep-alive timeout < ALB idle timeout — increase app timeout |
| Security group blocking | Allow ALB SG → EC2 SG on app port |
| Target deregistering | Increase deregistration delay if app needs time to finish requests |
| Out of memory / OOM kill | Scale up instance size, fix memory leaks |

**Diagnostic steps:**
1. Check **CloudWatch metrics** — `HTTPCode_Target_5XX_Count` spike
2. Enable **ALB Access Logs** → S3 — check `elb_status_code` vs `target_status_code`
3. Check **target health** in target group console
4. SSH to instance — check app logs, process status
5. Use **X-Ray** to trace failing requests end-to-end

---

**Q12. How do you set up a Global Load Balancer for multi-region traffic distribution?**

**Answer:** Use **AWS Global Accelerator** with regional ALBs/NLBs:
```
User (anywhere)
    ↓
AWS Global Accelerator (2 static Anycast IPs)
    ↓
┌───────────────┬───────────────┐
ALB us-east-1   ALB eu-west-1   ALB ap-southeast-1
    ↓               ↓               ↓
EC2/ECS         EC2/ECS         EC2/ECS
```

**Setup:**
1. Deploy your app stack (ALB + EC2) in multiple regions
2. Create **Global Accelerator**
3. Add **endpoint groups** per region with traffic dial (% of traffic)
4. Add regional ALBs as endpoints

**Benefits over Route 53:**
- Traffic enters AWS network at nearest **edge location** — reduced latency
- **Automatic failover** in under 30 seconds (vs DNS TTL with Route 53)
- **Static IPs** — no DNS propagation issues
- Health checks built-in — routes away from unhealthy regions

**Traffic control:**
- **Traffic dial** — send 20% to new region (canary)
- **Endpoint weights** — distribute within a region
- **Client affinity** — sticky routing by source IP

---

**Q13. How do you optimize ALB costs for a high-traffic application?**

**Answer:**

**Understand ALB pricing:**
- **LCU (Load Balancer Capacity Units)** — based on connections, requests, bandwidth, rule evaluations
- Fixed hourly charge + LCU charge

**Cost optimization:**
- **Consolidate ALBs** — use one ALB with multiple listeners/rules instead of multiple ALBs
- **Use path-based routing** — one ALB handles all microservices (avoid per-service ALB)
- **Reduce listener rules** — complex rule sets cost more LCUs
- **Offload SSL to ALB** — cheaper than processing on instances
- **CloudFront in front of ALB** — caches responses, reduces requests reaching ALB
- **Connection reuse** — enable HTTP keep-alive to reduce new connection overhead
- **Right-size target groups** — fewer, larger instances vs many small ones
- **Internal ALB for microservices** — internal ALBs cost less than internet-facing

---

## 💡 Quick Reference

| Feature | ALB Capability |
|---|---|
| **Path-based routing** | `/api/*` → different target group |
| **Host-based routing** | `api.example.com` vs `admin.example.com` |
| **Header-based routing** | Route by HTTP header value |
| **Weighted routing** | Blue/green, canary deployments |
| **Sticky sessions** | Cookie-based session affinity |
| **SSL termination** | HTTPS at ALB, HTTP to backend |
| **WAF integration** | Attach WAF for security rules |
| **Lambda targets** | ALB can forward to Lambda functions |
| **Authentication** | Native OIDC/Cognito auth at ALB level |
| **gRPC support** | Route gRPC traffic to microservices |
| **Access logs** | Detailed per-request logs to S3 |
| **Deletion protection** | Prevent accidental ALB deletion |

---

## 🔀 Choosing the Right Load Balancer
```
Need HTTP/HTTPS routing?          → ALB
Need ultra-low latency / TCP/UDP? → NLB
Need static IP on load balancer?  → NLB
Need to inspect packets / IDS?    → GLB
Need global multi-region routing? → Global Accelerator + ALB/NLB
Need CDN + caching?               → CloudFront + ALB
