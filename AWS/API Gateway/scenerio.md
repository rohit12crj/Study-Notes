# AWS API Gateway - Scenario-Based Interview Questions & Answers

## Architecture & Design Scenarios

### Q1: Design a Multi-Tenant SaaS API with Per-Tenant Rate Limiting

**Scenario:** You're building a SaaS platform serving 1000+ customers. Each customer has different subscription tiers (Free: 100 req/day, Pro: 10,000 req/day, Enterprise: unlimited). How would you implement this with API Gateway?

**Answer:**

**Architecture:**
1. **REST API** (need usage plans and API keys)
2. **Custom Lambda Authorizer** to identify tenant and tier
3. **Usage Plans** for each tier with appropriate throttle/quota limits
4. **API Keys** per tenant associated with their usage plan

**Implementation:**
```
Request Flow:
Client → API Gateway → Lambda Authorizer → Check JWT/API Key 
→ Extract tenant_id and tier → Return IAM policy + context
→ API Gateway applies usage plan → Backend Lambda
```

**Detailed Setup:**
- Lambda Authorizer extracts tenant_id from JWT claims
- Add tenant_id to authorizer context: `context.tenant_id`
- Create 3 usage plans:
  - Free: 100 requests/day, 10 RPS, 20 burst
  - Pro: 10,000 requests/day, 100 RPS, 200 burst
  - Enterprise: No quota, 1000 RPS, 2000 burst
- Generate API key per tenant, associate with their usage plan
- Use stage variables to route to tenant-specific resources if needed
- CloudWatch custom metrics to track per-tenant usage

**Monitoring:**
- CloudWatch Logs with tenant_id from authorizer context
- Custom metrics: `PutMetricData` with tenant dimension
- Alarms per tier for quota approaching limits
- Dashboard showing usage by tenant and tier

**Alternative Approach for Finer Control:**
- Implement rate limiting in Lambda authorizer itself
- Use DynamoDB to track tenant usage with TTL
- More flexible but higher latency

---

### Q2: Your API is Getting 10,000 Requests per Second but Backend Can Only Handle 2,000 RPS

**Scenario:** You have an API Gateway in front of a legacy system that can only handle 2,000 RPS. During peak hours, you're receiving 10,000 RPS. How do you prevent the backend from being overwhelmed?

**Answer:**

**Immediate Solution:**

1. **Method-Level Throttling** at API Gateway:
```
Rate Limit: 2,000 RPS
Burst Limit: 4,000 (token bucket for spikes)
```

2. **Implement Caching** for read endpoints:
```
Cache Size: Start with 6.1 GB
TTL: 60-300 seconds (based on data freshness requirements)
Cache Key: Include relevant query parameters
```
If 70% of requests are GET operations and cache hit rate is 80%, effective backend load = 10,000 * 0.7 * 0.2 = 1,400 RPS

3. **Queue Non-Critical Requests**:
```
API Gateway → Lambda → SQS → Worker Lambda → Legacy System
Return 202 Accepted immediately
Process asynchronously
```

**Long-Term Solutions:**

4. **Backend Scaling**:
- Add read replicas for read-heavy workloads
- Implement connection pooling
- Use load balancer with health checks

5. **Request Prioritization**:
- Separate API endpoints for critical vs non-critical operations
- Higher throttle limits for critical endpoints
- Different usage plans for premium vs free users

6. **Client-Side Handling**:
- Return 429 with `Retry-After` header
- Implement exponential backoff in clients
- Add jitter to prevent thundering herd

**Monitoring:**
- CloudWatch alarm on backend 5XX errors
- IntegrationLatency metric to detect backend saturation
- Cache hit/miss ratio

---

### Q3: Migrate Monolith REST API to Microservices Without Downtime

**Scenario:** You have a monolithic API Gateway with 50+ endpoints backed by a single Lambda function (2000 lines). You need to break this into microservices. How do you migrate without downtime?

**Answer:**

**Migration Strategy: Strangler Fig Pattern**

**Phase 1: Preparation (Week 1-2)**
1. Identify logical service boundaries:
   - User Service: /users, /auth
   - Product Service: /products, /catalog
   - Order Service: /orders, /cart
   - Payment Service: /payments

2. Create new Lambda functions per service
3. Set up separate stages: `prod`, `migration-test`

**Phase 2: Parallel Testing (Week 3-4)**
1. Deploy new microservice Lambdas
2. Use stage variables to route to new services in `migration-test` stage:
```
Integration endpoint: ${stageVariables.userServiceUrl}
Prod stage variable: arn:aws:lambda:region:account:function:monolith
Migration stage variable: arn:aws:lambda:region:account:function:user-service
```

3. Route small % of traffic using canary deployment
4. Monitor both implementations in parallel

**Phase 3: Gradual Migration (Week 5-8)**
1. **Service-by-Service Migration**:
   - Week 5: Migrate User Service
     - Update stage variable in `prod` to point to new user-service Lambda
     - Deploy with 10% canary
     - Monitor metrics: latency, errors, success rate
     - Gradually increase to 100%
   
2. **Rollback Plan**:
   - Keep monolith Lambda deployed
   - If issues detected, update stage variable back to monolith
   - Zero downtime rollback

3. Repeat for each service (Product, Order, Payment)

**Phase 4: API Restructuring (Week 9-12)**
1. Create new HTTP APIs per microservice (cost optimization)
2. Use ALB with path-based routing as integration target
3. Implement API Gateway aggregation layer if needed

**Phase 5: Cleanup**
1. Deprecate old endpoints
2. Remove monolith Lambda after 30-day observation
3. Consolidate monitoring and alerting

**Key Considerations:**
- **Shared Database**: Migrate to separate databases per service gradually
- **Transactions**: Use Saga pattern with Step Functions for distributed transactions
- **Authentication**: Centralize in Lambda authorizer, share context with all services
- **Logging**: Use correlation ID (`$context.requestId`) across all services
- **Testing**: Contract testing between services

**Monitoring During Migration:**
- Dual-write and dual-read pattern to compare responses
- CloudWatch Logs Insights to compare monolith vs microservice latency
- X-Ray service map to visualize dependencies

---

### Q4: Design Real-Time Leaderboard System for Gaming Platform

**Scenario:** Build a real-time leaderboard for a mobile game with 1 million concurrent players. Players' scores update every few seconds. Top 100 players displayed on leaderboard. How would you design this?

**Answer:**

**Architecture:**

```
Mobile App → API Gateway (WebSocket) → Lambda → ElastiCache (Redis Sorted Sets)
                                      ↓
                                  DynamoDB (persistent storage)
                                      ↓
                                  EventBridge (score milestones)
```

**WebSocket API Setup:**

**Routes:**
1. `$connect` - Player connects, store connectionId in DynamoDB
2. `$disconnect` - Remove connectionId from DynamoDB
3. `updateScore` - Player submits new score
4. `getLeaderboard` - Fetch top 100
5. `$default` - Handle unknown messages

**Score Update Flow:**
```javascript
// updateScore route Lambda
1. Receive score from player
2. Validate score (anti-cheat checks)
3. Update Redis Sorted Set: ZADD leaderboard {score} {playerId}
4. Get player rank: ZREVRANK leaderboard {playerId}
5. Async write to DynamoDB for persistence
6. If rank changed significantly, broadcast to affected players
7. Trigger EventBridge event for milestone achievements
```

**Leaderboard Retrieval:**
```javascript
// getLeaderboard route Lambda
1. ZREVRANGE leaderboard 0 99 WITHSCORES
2. Batch get player details from DynamoDB
3. Return formatted leaderboard
4. Cache in API Gateway (cache enabled on route)
```

**Real-Time Updates:**
1. When player's rank changes, identify affected players
2. Get their connectionIds from DynamoDB
3. Use API Gateway Management API to push updates:
```javascript
const apiGateway = new ApiGatewayManagementApi({
  endpoint: process.env.WEBSOCKET_ENDPOINT
});

await apiGateway.postToConnection({
  ConnectionId: connectionId,
  Data: JSON.stringify({ type: 'leaderboard_update', data: newLeaderboard })
});
```

**Scalability Considerations:**

1. **Redis Cluster** for horizontal scaling
2. **DynamoDB on-demand** for variable write patterns
3. **Connection Management**:
   - Store connectionIds in DynamoDB with TTL
   - Clean up stale connections periodically
   - Use DynamoDB Streams to trigger cleanup

4. **Cost Optimization**:
   - Not all players need real-time updates
   - Only push updates to top 1000 players
   - Others use polling with caching
   - WebSocket charges: $1 per million messages + $0.25 per million connection minutes

5. **Global Leaderboards**:
   - Redis Global Datastore for multi-region
   - DynamoDB Global Tables
   - Route 53 latency-based routing

**Anti-Patterns to Avoid:**
- Don't broadcast every score update to all players (expensive)
- Don't use DynamoDB for real-time leaderboard queries (slow for top-N queries)
- Don't keep connections alive unnecessarily

**Monitoring:**
- WebSocket connection count
- Message delivery success rate
- Redis memory usage and eviction rate
- Lambda concurrency and errors
- Client reconnection rate

---

## Performance & Optimization Scenarios

### Q5: API Latency Increased from 100ms to 2000ms After Adding New Feature

**Scenario:** You added a new feature that enriches user data from a third-party API. Now p99 latency increased from 100ms to 2000ms. How do you troubleshoot and fix this?

**Answer:**

**Step 1: Identify the Bottleneck**

1. **Enable X-Ray Tracing** (if not already enabled):
```
View service map to identify slow components
Check trace details for individual requests
Look for Lambda initialization time vs execution time
```

2. **Check CloudWatch Metrics**:
```
Compare IntegrationLatency (backend time) vs Latency (total time)
If IntegrationLatency is high → backend issue
If (Latency - IntegrationLatency) is high → API Gateway overhead
```

3. **Review CloudWatch Logs**:
```
Filter logs for requests >1000ms
Check execution logs for the new Lambda function
Identify which third-party API call is slow
```

**Analysis Results:**
- X-Ray shows third-party API taking 1800ms
- Lambda execution time: 2000ms
- Third-party API has no SLA, unreliable

**Step 2: Immediate Fixes**

**Option A: Async Processing (Recommended)**
```
Synchronous Flow (Current - Slow):
API Gateway → Lambda → Third-party API (1800ms) → Response

Async Flow (New - Fast):
API Gateway → Lambda → SQS → Return 202 Accepted (50ms)
                          ↓
                    Worker Lambda → Third-party API → Update DynamoDB
                          ↓
                    EventBridge → Notify user via WebSocket/SNS
```

Benefits: API latency back to 100ms, better user experience

**Option B: Parallel Processing**
```javascript
// If third-party data is not critical for immediate response
async function handler(event) {
  // Get main data (fast)
  const mainDataPromise = getMainData(userId);
  
  // Trigger async enrichment (don't wait)
  invokeAsync('enrichment-lambda', { userId });
  
  // Return main data immediately
  const mainData = await mainDataPromise;
  return { statusCode: 200, body: JSON.stringify(mainData) };
}
```

**Option C: Caching with Stale-While-Revalidate**
```javascript
// Cache enriched data in DynamoDB with TTL
1. Check cache first (DynamoDB GetItem - 5ms)
2. If cache hit and fresh (< 5 min old) → return cached data
3. If cache hit but stale → return stale data + trigger background refresh
4. If cache miss → fetch from third-party (slow first request only)
```

**Step 3: Long-Term Solutions**

1. **Circuit Breaker Pattern**:
```javascript
// Fail fast if third-party API is down
if (thirdPartyFailureRate > 50%) {
  return cachedData || defaultData; // Don't wait for third-party
}
```

2. **Timeout & Fallback**:
```javascript
const timeout = 500; // ms
try {
  const result = await Promise.race([
    fetchThirdPartyData(),
    new Promise((_, reject) => setTimeout(() => reject(new Error('Timeout')), timeout))
  ]);
} catch (error) {
  // Use cached data or partial response
  return getCachedOrDefaultData();
}
```

3. **Connection Pooling**:
```javascript
// Reuse HTTP connections to third-party API
const https = require('https');
const agent = new https.Agent({ keepAlive: true, maxSockets: 50 });
```

4. **API Gateway Caching**:
- Enable caching for GET requests with third-party data
- TTL: 300 seconds
- Cache key includes user_id
- 90% cache hit rate → most requests serve from cache (10ms)

**Step 4: Monitoring & Alerting**

1. Set CloudWatch alarms:
   - p99 latency > 500ms
   - Third-party API timeout rate > 5%
   - Cache hit rate < 80%

2. Create dashboard:
   - Latency breakdown (API Gateway, Lambda, Third-party)
   - Success vs timeout vs error rates
   - Cache performance

**Outcome:**
- API latency: 100ms (cached) / 250ms (cache miss with async)
- User experience improved
- Resilient to third-party API failures

---

### Q6: API Costs Increased by 300% After Traffic Doubled. Optimize Without Degrading Performance.

**Scenario:** Your API traffic doubled from 100M to 200M requests/month. AWS bill increased from $1000 to $4000. Break down costs and optimize.

**Answer:**

**Step 1: Cost Analysis**

**Current Setup:**
- REST API: 200M requests × $3.50/M = $700
- Data transfer: 200M × 50KB avg × $0.09/GB = $900
- Lambda: 200M invocations × $0.20 per 1M = $40
- Lambda duration: 200M × 200ms × $0.0000166667/GB-sec = $665
- DynamoDB: $500
- CloudWatch: $300
- API Gateway Cache: 6.1GB × 730 hours × $0.02 = $890
- **Total: $3,995**

**Step 2: Optimization Strategy**

**1. Migrate to HTTP API** (Biggest Impact)
```
Current: REST API at $3.50/M
New: HTTP API at $1.00/M
Savings: 200M × ($3.50 - $1.00) = $500/month
```

Migration considerations:
- Remove API keys (use JWT authorizer instead)
- Remove request validators (validate in Lambda)
- Simplify CORS configuration
- Update client SDK if using API Gateway SDK generation

**2. Enable Compression**
```
Current: 50KB average response
With compression: 10KB average (80% reduction for JSON)
Data transfer savings: 200M × 40KB × $0.09/GB = $720/month
```

**3. Optimize Lambda**
```
Current: 512MB, 200ms avg
Analysis shows: Only using 256MB memory

New: 256MB, 150ms avg (CPU scales with memory)
Old cost: 200M × 200ms × 512MB × $0.0000166667 = $665
New cost: 200M × 150ms × 256MB × $0.0000166667 = $250
Savings: $415/month
```

**4. Implement Aggressive Caching**
```
Analysis: 60% of requests are GET /products (product catalog)
Product data changes every 5 minutes

Enable API Gateway caching:
Cache hit rate: 95%
Effective requests to Lambda: 200M × 40% + 200M × 60% × 5% = 86M

Lambda savings: (200M - 86M) × cost = $144/month
Cache cost: 6.1GB × 730 × $0.02 = $890/month
Net: -$746/month (cache costs more!)

Alternative: CloudFront caching (cheaper for high traffic)
CloudFront: $0.085/GB + request pricing
For 200M requests: ~$200/month
Savings vs API Gateway cache: $690/month
```

**5. Direct DynamoDB Integration** (Remove Lambda for Simple Reads)
```
Current: API Gateway → Lambda → DynamoDB
New: API Gateway → DynamoDB (using AWS service integration)

20% of requests are simple DynamoDB GetItem operations
Affected requests: 200M × 20% = 40M

Lambda savings: 40M × (invocation + duration) = $170/month
Added complexity: VTL mapping templates
```

**6. Batch Endpoints**
```
Current: Client makes 5 separate API calls to load dashboard
New: Single batch endpoint returns all data

Request reduction: 200M → 120M (40% reduction)
Savings across all components: 40% × $4000 = $1600/month

Implementation:
POST /api/batch
{
  "requests": [
    { "method": "GET", "path": "/users/123" },
    { "method": "GET", "path": "/orders/456" }
  ]
}
```

**7. Optimize DynamoDB**
```
Current: Provisioned capacity underutilized
New: On-demand pricing

Current: 50 RCU + 50 WCU = $50/month + overprovisioned to $500
New: On-demand based on actual usage = $200/month
Savings: $300/month
```

**Step 3: Implementation Plan**

**Month 1: Quick Wins (Low Risk)**
- Enable compression: $720 savings
- Optimize Lambda memory: $415 savings
- DynamoDB on-demand: $300 savings
- **Total Month 1: $1,435 savings**

**Month 2: Medium Effort**
- Migrate to HTTP API: $500 savings
- Implement batch endpoints: $1,600 savings
- **Total Month 2: $2,100 additional savings**

**Month 3: Advanced**
- CloudFront for caching: $690 savings
- Direct DynamoDB integration: $170 savings
- **Total Month 3: $860 additional savings**

**Final Results:**
- **Original cost: $4,000/month**
- **Optimized cost: $605/month**
- **Savings: $3,395/month (85% reduction)**
- **Performance: Improved (lower latency with CloudFront)**

**Monitoring:**
- Track cost per million requests metric
- Set budget alerts at $800/month
- Monthly cost review and optimization

---

## Security Scenarios

### Q7: Security Audit Found API Vulnerable to DDoS and Credential Stuffing

**Scenario:** Security team flagged your public API as vulnerable. Recent incidents: DDoS attack (50k RPS), credential stuffing attempts (100k failed login attempts in 1 hour). Implement comprehensive security.

**Answer:**

**Multi-Layer Security Implementation:**

**Layer 1: AWS WAF (Front-Line Defense)**

**1. Rate-Based Rule (DDoS Protection)**
```
Rule: Block IP if > 2,000 requests in 5 minutes
Action: Block for 1 hour
Scope: All endpoints except /health

Configuration:
{
  "RateLimitRule": {
    "RateLimit": 2000,
    "AggregateKeyType": "IP",
    "EvaluationWindowSec": 300
  }
}
```

**2. Geo-Blocking**
```
If business only operates in US, Canada, UK:
Block all other countries
Reduces attack surface by 70%
```

**3. IP Reputation List**
```
Use AWS Managed Rules:
- AWSManagedRulesAmazonIpReputationList (known malicious IPs)
- AWSManagedRulesAnonymousIpList (VPN, proxy, Tor exit nodes)
```

**4. Credential Stuffing Protection**
```
Custom Rule for /login endpoint:
- Rate limit: 5 failed attempts per IP per minute
- After 5 failures: CAPTCHA challenge
- After 10 failures: Block IP for 1 hour

Implementation:
COUNT mode for failed logins (check 401 status)
Use AWS WAF CAPTCHA action
```

**Layer 2: API Gateway Protection**

**1. Throttling Configuration**
```
Account-level: 10,000 RPS
Stage-level: 5,000 RPS (production)
Method-level limits:
  - POST /login: 100 RPS (authentication endpoint)
  - GET /products: 1,000 RPS (read-heavy)
  - POST /orders: 500 RPS (write operation)
```

**2. Resource Policy (IP Allowlisting for Admin)**
```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Principal": "*",
      "Action": "execute-api:Invoke",
      "Resource": "arn:aws:execute-api:region:account:api-id/*/*/*"
    },
    {
      "Effect": "Deny",
      "Principal": "*",
      "Action": "execute-api:Invoke",
      "Resource": "arn:aws:execute-api:region:account:api-id/*/*/admin/*",
      "Condition": {
        "NotIpAddress": {
          "aws:SourceIp": ["203.0.113.0/24", "198.51.100.0/24"]
        }
      }
    }
  ]
}
```

**Layer 3: Authentication Hardening**

**1. Multi-Factor Authentication (Cognito)**
```
Enable MFA for user pool:
- SMS or TOTP
- Mandatory for sensitive operations
- Adaptive authentication based on risk
```

**2. Advanced Cognito Security**
```
User Pool Settings:
- Password policy: Min 12 chars, complexity requirements
- Account takeover protection: Medium or High
- Advanced security features: Enable
- Compromised credentials check: Enable (checks against leaked passwords)
```

**3. Lambda Authorizer Enhancements**
```javascript
async function authorize(event) {
  const token = event.authorizationToken;
  
  // 1. Verify JWT signature and expiration
  const decoded = verifyToken(token);
  
  // 2. Check token in DynamoDB blacklist (for logout/revocation)
  const isBlacklisted = await checkTokenBlacklist(decoded.jti);
  if (isBlacklisted) throw new Error('Unauthorized');
  
  // 3. Check user account status
  const user = await getUserFromDB(decoded.sub);
  if (user.status === 'suspended') throw new Error('Unauthorized');
  
  // 4. Geo-fence check (if user normally logs in from US, flag login from China)
  const requestIP = event.requestContext.identity.sourceIp;
  const country = await getCountryFromIP(requestIP);
  if (country !== user.primaryCountry) {
    await sendSecurityAlert(user.email, country);
    // Optional: require step-up authentication
  }
  
  // 5. Rate limit per user (track in DynamoDB/ElastiCache)
  const requestCount = await incrementUserRequestCount(decoded.sub);
  if (requestCount > 1000) { // per minute
    throw new Error('Rate limit exceeded');
  }
  
  // 6. Return policy with least privilege
  return generatePolicy(decoded.sub, 'Allow', event.methodArn, decoded);
}
```

**Layer 4: Request Validation & Sanitization**

**1. API Gateway Request Validators**
```json
{
  "RequestValidator": {
    "validateRequestBody": true,
    "validateRequestParameters": true
  },
  "JsonSchema": {
    "type": "object",
    "properties": {
      "email": {
        "type": "string",
        "format": "email",
        "maxLength": 255
      },
      "password": {
        "type": "string",
        "minLength": 12,
        "maxLength": 128
      }
    },
    "required": ["email", "password"]
  }
}
```

**2. SQL Injection & XSS Protection (WAF)**
```
AWS Managed Rules:
- AWSManagedRulesKnownBadInputsRuleSet
- AWSManagedRulesSQLiRuleSet
- Add custom regex patterns for additional protection
```

**Layer 5: Monitoring & Incident Response**

**1. CloudWatch Alarms**
```
Critical Alarms:
- 4XXError rate > 50% (attack in progress)
- Request count spike > 3x baseline (DDoS)
- Failed auth attempts > 100/min (credential stuffing)
- IntegrationLatency > 5000ms (backend overload)
- WAF blocked requests > 1000/min

Actions:
- SNS notification to security team
- Lambda for automated response (block suspicious IPs)
- PagerDuty integration for critical issues
```

**2. Access Logging with Security Fields**
```json
{
  "requestId": "$context.requestId",
  "ip": "$context.identity.sourceIp",
  "userAgent": "$context.identity.userAgent",
  "requestTime": "$context.requestTime",
  "httpMethod": "$context.httpMethod",
  "resourcePath": "$context.resourcePath",
  "status": "$context.status",
  "protocol": "$context.protocol",
  "responseLength": "$context.responseLength",
  "wafResponse": "$context.wafResponseCode",
  "error": "$context.error.message"
}
```

**3. Security Information and Event Management (SIEM)**
```
Stream logs to:
- CloudWatch Logs Insights for queries
- S3 bucket for long-term storage
- Third-party SIEM (Splunk, Datadog)

Query examples:
- Failed login attempts by IP
- Geographic anomalies
- Unusual traffic patterns
```

**4. Automated Incident Response**
```
EventBridge Rule:
  WAF block count > 1000 in 5 min
  ↓
  Lambda Function:
    - Query WAF logs for attacking IPs
    - Add to IP set for extended block
    - Create security incident ticket
    - Send Slack notification
```

**Layer 6: Additional Hardening**

**1. Secrets Management**
```
- Store API keys in AWS Secrets Manager
- Rotate keys every 90 days
- Use IAM roles for service-to-service auth (no hardcoded keys)
- Never expose secrets in URL parameters
```

**2. TLS Configuration**
```
Custom Domain:
- TLS 1.2 minimum (TLS 1.3 preferred)
- Strong cipher suites only
- HSTS header: max-age=31536000; includeSubDomains
```

**3. Client Certificate (mTLS) for B2B**
```
For business partners:
- Require client certificates
- Store trusted CA certs in S3
- Validate in API Gateway
- Additional authentication layer
```

**4. Private API for Internal Services**
```
Admin APIs and internal services:
- Use Private API Gateway
- VPC Endpoint access only
- No public internet exposure
- Enhanced security for sensitive operations
```

**Layer 7: Compliance & Auditing**

**1. Enable AWS CloudTrail**
```
Log all API Gateway configuration changes
Track who created/modified/deleted APIs
Audit trail for compliance
```

**2. VPC Flow Logs**
```
For Private APIs:
Track all network traffic
Identify unusual patterns
Forensic analysis
```

**3. Regular Penetration Testing**
```
Quarterly security assessments
Automated vulnerability scanning
Bug bounty program
```

**Cost Summary:**
- WAF: ~$50/month (base) + $1/million requests
- Cognito: $0.0055 per MAU (first 50k free)
- Secrets Manager: $0.40 per secret/month + API calls
- CloudTrail: $2.00 per 100,000 events
- **Total additional cost: ~$200-500/month for comprehensive security**

**Security Metrics:**
- Attack detection rate: 99.9%
- False positive rate: <1%
- Time to detect: <1 minute
- Time to respond: <5 minutes (automated)
- Blocked malicious traffic: 2M+ requests/month

---

## Troubleshooting Scenarios

### Q8: API Returning Intermittent 502 Errors (5% of Requests)

**Scenario:** Production API started returning 502 Bad Gateway errors for 5% of requests. No recent deployments. How do you troubleshoot?

**Answer:**

**Step 1: Immediate Triage**

**1. Check CloudWatch Metrics (Last 1 Hour)**
```
- 5XXError count: Spiked to 5% (normally <0.1%)
- IntegrationLatency: Normal (100ms avg)
- Lambda errors: Slightly elevated
- Lambda concurrent executions: Normal
```

**2. Sample 502 Error Logs**
```
CloudWatch Logs Insights query:
fields @timestamp, @message, statusCode
| filter statusCode = 502
| sort @timestamp desc
| limit 20

Results show:
- "Execution failed due to configuration error: Malformed Lambda proxy response"
- Affects random requests, no pattern by endpoint
```

**Step 2: Root Cause Analysis**

**502 Error Causes:**
1. Malformed Lambda response
2. Lambda function timeout
3. Lambda out of memory
4. Backend returned invalid response format
5. Integration configuration error

**Investigation:**

**1. Check Lambda Function Logs**
```bash
aws logs tail /aws/lambda/my-function --since 1h --follow --filter-pattern "ERROR"
```

Findings:
- Occasional errors: "Cannot read property 'headers' of undefined"
- Happens when response object is not properly formatted

**2. Examine Lambda Code**
```javascript
// Bug found in error handling
exports.handler = async (event) => {
  try {
    const result = await processRequest(event);
    return {
      statusCode: 200,
      body: JSON.stringify(result)
    };
  } catch (error) {
    console.error(error);
    // BUG: Missing return statement for Lambda proxy format
    return error.message; // Should be: { statusCode: 500, body: error.message }
  }
};
```

**3. Verify Lambda Response Format**
```javascript
// Required format for Lambda proxy integration
{
  "statusCode": 200,
  "headers": {
    "Content-Type": "application/json"
  },
  "body": "{\"message\": \"success\"}", // Must be string
  "isBase64Encoded": false
}
```

**Root Cause Identified:**
- When Lambda throws exception, error handler returns malformed response
- API Gateway expects proper proxy response format
- Results in 502 Bad Gateway

**Step 3: Fix Implementation**

**Immediate Fix:**
```javascript
exports.handler = async (event) => {
  try {
    const result = await processRequest(event);
    return {
      statusCode: 200,
      headers: { 'Content-Type': 'application/json' },
      body: JSON.stringify(result)
    };
  } catch (error) {
    console.error('Lambda error:', error);
    
    // FIXED: Proper error response format
    return {
      statusCode: 500,
      headers: { 'Content-Type': 'application/json' },
      body: JSON.stringify({
        error: 'Internal Server Error',
        requestId: event.requestContext.requestId
      })
    };
  }
};
```

**Better Solution: Response Wrapper**
```javascript
// response-util.js
const createResponse = (statusCode, body, headers = {}) => ({
  statusCode,
  headers: {
    'Content-Type': 'application/json',
    'Access-Control-Allow-Origin': '*',
    ...headers
  },
  body: typeof body === 'string' ? body : JSON.stringify(body)
});

// Lambda handler
exports.handler = async (event) => {
  try {
    const result = await processRequest(event);
    return createResponse(200, result);
  } catch (error) {
    console.error('Error:', error);
    
    // Categorize errors
    if (error.name === 'ValidationError') {
      return createResponse(400, { error: error.message });
    }
    if (error.name === 'UnauthorizedError') {
      return createResponse(401, { error: 'Unauthorized' });
    }
    
    // Default 500 for unexpected errors
    return createResponse(500, { 
      error: 'Internal Server Error',
      requestId: event.requestContext.requestId 
    });
  }
};
```

**Step 4: Prevent Future Occurrences**

**1. Automated Testing**
```javascript
// Jest test to validate response format
test('Lambda returns valid API Gateway proxy response', async () => {
  const event = createMockEvent();
  const response = await handler(event);
  
  expect(response).toHaveProperty('statusCode');
  expect(response).toHaveProperty('body');
  expect(response).toHaveProperty('headers');
  expect(typeof response.body).toBe('string');
  expect(response.statusCode).toBeGreaterThanOrEqual(200);
  expect(response.statusCode).toBeLessThan(600);
});

// Test error scenarios
test('Lambda handles errors correctly', async () => {
  const event = createMockErrorEvent();
  const response = await handler(event);
  
  expect(response.statusCode).toBe(500);
  expect(JSON.parse(response.body)).toHaveProperty('error');
});
```

**2. CloudWatch Alarms**
```
Alarm: 5XXError > 1% of total requests
Threshold: 3 consecutive periods (15 minutes)
Action: SNS → PagerDuty → On-call engineer

Alarm: 502 Errors > 0
Threshold: 1 occurrence
Action: SNS → Slack channel
```

**3. Canary Deployment**
```
Before full deployment:
- Deploy to 10% traffic (canary)
- Monitor for 30 minutes
- Check 5XX error rate < 0.1%
- If errors detected, automatic rollback
- If stable, promote to 100%
```

**4. Lambda Powertools (Enhanced Logging)**
```javascript
const { Logger } = require('@aws-lambda-powertools/logger');
const logger = new Logger();

exports.handler = async (event) => {
  try {
    logger.info('Processing request', { event });
    const result = await processRequest(event);
    return createResponse(200, result);
  } catch (error) {
    logger.error('Lambda execution error', { error, event });
    return createResponse(500, { error: 'Internal Server Error' });
  }
};
```

**5. Integration Testing**
```javascript
// Test actual API Gateway integration
const AWS = require('aws-sdk');
const apiGateway = new AWS.APIGateway();

test('API Gateway integration returns valid response', async () => {
  const response = await apiGateway.testInvokeMethod({
    restApiId: 'api-id',
    resourceId: 'resource-id',
    httpMethod: 'POST',
    body: JSON.stringify({ test: 'data' })
  }).promise();
  
  expect(response.status).toBe(200);
  expect(response.body).toBeDefined();
});
```

**Step 5: Long-Term Monitoring**

**1. X-Ray Tracing**
```
Enable X-Ray:
- Trace all requests
- Identify malformed response patterns
- Alert on subsegment errors
- Annotate traces with response validation
```

**2. Structured Logging**
```javascript
// Log all responses for audit
logger.info('Response sent', {
  statusCode: response.statusCode,
  requestId: event.requestContext.requestId,
  duration: Date.now() - startTime,
  responseValid: validateResponse(response)
});
```

**3. Dashboard**
```
CloudWatch Dashboard metrics:
- Total requests
- 2XX, 4XX, 5XX breakdown
- 502 specific errors
- Lambda error rate
- Response time percentiles (p50, p95, p99)
```

**Outcome:**
- 502 errors reduced to 0%
- Automated testing prevents regression
- Better error handling and logging
- Proactive monitoring and alerting

---

### Q9: API Works Perfectly in Staging but Fails in Production

**Scenario:** Your API passes all tests in staging but fails in production with random timeout errors. Staging uses same Lambda code, same configuration. What could be wrong?

**Answer:**

**Common Causes of Staging/Production Discrepancies:**

**1. Concurrency & Scale Differences**

**Problem:** Production has 100x traffic, hits Lambda concurrency limits
```
Staging: 10 concurrent Lambda executions
Production: 1,000+ concurrent executions hitting account limit (default: 1,000)
```

**Investigation:**
```bash
# Check Lambda throttling metrics
aws cloudwatch get-metric-statistics \
  --namespace AWS/Lambda \
  --metric-name Throttles \
  --dimensions Name=FunctionName,Value=my-function \
  --start-time 2024-01-01T00:00:00Z \
  --end-time 2024-01-01T23:59:59Z \
  --period 300 \
  --statistics Sum
```

**Solution:**
```
1. Request concurrency limit increase from AWS
2. Set reserved concurrency per function
3. Use SQS for async processing to smooth traffic spikes
4. Enable provisioned concurrency for predictable load
```

**2. Database Connection Pool Exhaustion**

**Problem:** Staging has 1 concurrent request, production has 1000
```javascript
// Code works in staging (1 connection used)
// Fails in production (needs 1000 connections, DB only allows 100)

const mysql = require('mysql');
const connection = mysql.createConnection({
  host: process.env.DB_HOST,
  user: process.env.DB_USER,
  password: process.env.DB_PASSWORD
  // BUG: New connection created for each Lambda invocation
});

connection.connect();
// Execute query...
connection.end(); // Doesn't free immediately
```

**Investigation:**
```
RDS CloudWatch Metrics:
- DatabaseConnections: 100/100 (maxed out)
- High connection churn rate
- Lambda timeout errors correlate with connection saturation
```

**Solution:**
```javascript
// Use connection pooling with Lambda layers
const mysql = require('mysql2/promise');

// Create pool outside handler (reused across invocations)
const pool = mysql.createPool({
  host: process.env.DB_HOST,
  user: process.env.DB_USER,
  password: process.env.DB_PASSWORD,
  connectionLimit: 10, // Limit per Lambda instance
  waitForConnections: true,
  queueLimit: 0
});

exports.handler = async (event) => {
  const connection = await pool.getConnection();
  try {
    const [rows] = await connection.execute('SELECT * FROM users WHERE id = ?', [userId]);
    return createResponse(200, rows);
  } finally {
    connection.release(); // Return to pool
  }
};

// Or use RDS Proxy for connection pooling
```

**3. Environment-Specific Configuration**

**Problem:** Different stage variables or environment variables
```
Staging: TIMEOUT=3000ms
Production: TIMEOUT=300ms (typo, missing a zero)

Staging: DB_HOST=staging-db.us-east-1.rds.amazonaws.com
Production: DB_HOST=prod-db.us-west-2.rds.amazonaws.com (wrong region!)
```

**Investigation:**
```bash
# Compare stage variables
aws apigateway get-stage --rest-api-id xxx --stage-name staging
aws apigateway get-stage --rest-api-id xxx --stage-name production

# Compare Lambda environment variables
aws lambda get-function-configuration --function-name my-function:staging
aws lambda get-function-configuration --function-name my-function:production
```

**Solution:**
```
1. Use infrastructure as code (CloudFormation, Terraform)
2. Automated deployment pipeline validates configs
3. Separate parameter store paths per environment
4. Config validation in Lambda initialization
```

**4. Rate Limiting & Third-Party APIs**

**Problem:** Third-party API allows 100 RPS, staging only uses 1 RPS
```
Staging: 1 request/sec to third-party API → works fine
Production: 500 requests/sec to third-party API → rate limited (429 errors)
```

**Investigation:**
```
CloudWatch Logs:
"Third-party API returned 429 Too Many Requests"
Pattern: Happens only under high load
```

**Solution:**
```javascript
// Implement exponential backoff and retry
const retry = require('async-retry');

async function callThirdPartyAPI(data) {
  return await retry(
    async (bail) => {
      try {
        const response = await axios.post(THIRD_PARTY_URL, data);
        return response.data;
      } catch (error) {
        if (error.response?.status === 429) {
          const retryAfter = error.response.headers['retry-after'] || 1;
          await new Promise(resolve => setTimeout(resolve, retryAfter * 1000));
          throw error; // Retry
        }
        if (error.response?.status >= 400 && error.response?.status < 500) {
          bail(error); // Don't retry client errors
          return;
        }
        throw error; // Retry server errors
      }
    },
    {
      retries: 3,
      factor: 2,
      minTimeout: 1000,
      maxTimeout: 5000
    }
  );
}

// Or use SQS with controlled consumption rate
API Gateway → Lambda → SQS → Worker Lambda (rate-limited consumption)
```

**5. Cold Start Impact**

**Problem:** Staging keeps Lambda warm, production has variable traffic
```
Staging: Consistent traffic, Lambda always warm
Production: Burst traffic after quiet period → massive cold starts → timeouts
```

**Investigation:**
```
X-Ray traces show:
- Initialization time: 5000ms (cold start)
- Execution time: 200ms
- Total: 5200ms (API Gateway timeout at 29s not hit, but user expectation is <1s)

Lambda concurrent executions spike from 0 to 500 in 10 seconds
```

**Solution:**
```
1. Provisioned Concurrency (costly but effective)
   - Keeps minimum Lambdas warm
   - Cost: $0.0000041667 per GB-second
   
2. SnapStart (Java only)
   - Reduces cold starts by up to 90%
   
3. Keep Lambda warm (scheduled invocations)
   - EventBridge rule every 5 minutes
   - Invoke with { "warmup": true }
   - Lambda detects and returns early

4. Optimize cold start
   - Reduce deployment package size
   - Use Lambda layers for dependencies
   - Minimize initialization code
   - Use lighter runtime (Node.js vs Java)
```

**6. Security Group & Network Configuration**

**Problem:** Production VPC has stricter security group rules
```
Staging Lambda: Public subnet, can reach internet
Production Lambda: Private subnet, needs NAT Gateway for internet access

Error: Connection timeout to third-party API
```

**Investigation:**
```
VPC Flow Logs:
- Rejected connections from Lambda to internet
- Security group doesn't allow outbound HTTPS

Lambda is in private subnet but NAT Gateway is missing
```

**Solution:**
```
1. Add NAT Gateway to private subnet
2. Update route table: 0.0.0.0/0 → NAT Gateway
3. Security group outbound rules: Allow HTTPS (443)
4. Verify VPC endpoints for AWS services (S3, DynamoDB)
```

**7. Data Volume Differences**

**Problem:** Staging has small dataset, production has millions of records
```
Staging: Database query returns 10 results in 50ms
Production: Same query returns 100,000 results in 15 seconds → timeout
```

**Investigation:**
```
RDS Performance Insights:
- Query execution time: 15 seconds
- Missing index on production table
- Different data distribution
```

**Solution:**
```sql
-- Add missing index
CREATE INDEX idx_user_created ON users(created_at);

-- Add pagination
SELECT * FROM users WHERE created_at > ? ORDER BY created_at LIMIT 100;

-- Use cursor-based pagination for large datasets
```

**8. Monitoring & Alerting Differences**

**Root Cause:** Not properly monitoring production

**Investigation Approach:**
```
1. Enable X-Ray in production (same as staging)
2. Compare CloudWatch metrics side-by-side
3. Enable enhanced Lambda monitoring
4. Access logs with structured JSON format
5. Set up CloudWatch Insights queries

Query to find differences:
fields @timestamp, latency, statusCode
| filter statusCode >= 500
| stats count() by statusCode, bin(5m)
```

**Comprehensive Solution:**

**1. Parity Validation Script**
```bash
#!/bin/bash
# validate-env-parity.sh

# Compare Lambda configurations
STAGING_CONFIG=$(aws lambda get-function-configuration --function-name my-func:staging)
PROD_CONFIG=$(aws lambda get-function-configuration --function-name my-func:prod)

diff <(echo "$STAGING_CONFIG" | jq 'del(.FunctionArn, .LastModified)') \
     <(echo "$PROD_CONFIG" | jq 'del(.FunctionArn, .LastModified)')

# Compare API Gateway stages
STAGING_STAGE=$(aws apigateway get-stage --rest-api-id xxx --stage-name staging)
PROD_STAGE=$(aws apigateway get-stage --rest-api-id xxx --stage-name prod)

diff <(echo "$STAGING_STAGE" | jq '.variables') \
     <(echo "$PROD_STAGE" | jq '.variables')
```

**2. Production-Like Staging (Load Testing)**
```
Use Artillery or Locust to simulate production load in staging:

artillery quick --count 1000 --num 50 https://staging-api.example.com

Metrics to match production:
- Concurrent requests
- Request rate
- Data volume
- Geographic distribution
```

**3. Blue-Green Deployment with Validation**
```
1. Deploy to production-blue (isolated)
2. Route 1% traffic to blue
3. Monitor metrics for 30 minutes
4. Compare blue vs green metrics
5. If parity achieved, promote blue
6. If issues detected, rollback
```

**Final Checklist:**
- [ ] Concurrency limits appropriate for production scale
- [ ] Database connection pooling configured
- [ ] Environment variables validated
- [ ] Third-party API rate limits handled
- [ ] Cold start optimization implemented
- [ ] VPC/network configuration correct
- [ ] Indexes optimized for production data volume
- [ ] Monitoring and alerting parity with staging
- [ ] Load testing completed
- [ ] Gradual rollout strategy in place

---

## Cost & Compliance Scenarios

### Q10: Design HIPAA-Compliant API for Healthcare Data

**Scenario:** Building an API to handle Protected Health Information (PHI) for a healthcare platform. Must be HIPAA compliant. What are the architectural and operational requirements?

**Answer:**

**HIPAA Technical Safeguards Implementation:**

**1. Access Control (§164.312(a)(1))**

**Authentication**
```
- Use Cognito User Pool with MFA mandatory
- Password requirements: 12+ chars, complexity, rotation every 90 days
- Account lockout after 5 failed attempts
- Session timeout: 15 minutes of inactivity
- No shared credentials
```

**Authorization (Role-Based Access Control)**
```javascript
// Lambda Authorizer with strict RBAC
async function authorize(event) {
  const token = event.authorizationToken;
  const decoded = verifyJWT(token);
  
  // Check user role and permissions
  const user = await getUser(decoded.sub);
  const requestedResource = event.methodArn;
  
  // Minimum necessary access
  const permissions = {
    'doctor': ['read:patient', 'write:patient', 'read:records'],
    'nurse': ['read:patient', 'read:records'],
    'admin': ['read:users', 'write:users'],
    'patient': ['read:own-records']
  };
  
  const allowedActions = permissions[user.role] || [];
  const action = getActionFromArn(requestedResource);
  
  if (!allowedActions.includes(action)) {
    throw new Error('Unauthorized');
  }
  
  // Add user context for audit logging
  return generatePolicy(user.id, 'Allow', requestedResource, {
    userId: user.id,
    role: user.role,
    accessReason: decoded.purpose // Required for access
  });
}
```

**2. Audit Controls (§164.312(b))**

**Comprehensive Logging**
```json
// CloudWatch Access Logs (every API call)
{
  "timestamp": "$context.requestTime",
  "requestId": "$context.requestId",
  "userId": "$context.authorizer.userId",
  "userRole": "$context.authorizer.role",
  "sourceIP": "$context.identity.sourceIp",
  "userAgent": "$context.identity.userAgent",
  "httpMethod": "$context.httpMethod",
  "resourcePath": "$context.resourcePath",
  "requestBody": "$input.body", // Only for audit endpoints
  "status": "$context.status",
  "responseLength": "$context.responseLength",
  "latency": "$context.latency",
  "integrationLatency": "$context.integrationLatency",
  "errorMessage": "$context.error.message",
  "accessPurpose": "$context.authorizer.accessReason"
}
```

**Audit Log Requirements:**
- **Retention**: 6 years minimum (HIPAA requirement)
- **Immutability**: S3 with Object Lock (Compliance mode)
- **Integrity**: Use S3 Object Lock or blockchain for tamper-proof logs
- **Access**: Only security team can access, all accesses logged

**Implementation:**
```
API Gateway Access Logs → CloudWatch Logs → Kinesis Firehose → S3 (encrypted, Object Lock)
                                          ↓
                                    Real-time monitoring (Lambda)
```

**3. Encryption (§164.312(a)(2)(iv) & §164.312(e)(2)(ii))**

**Data in Transit**
```
- TLS 1.2 minimum (TLS 1.3 preferred)
- Custom domain with ACM certificate
- Strong cipher suites only
- HSTS header: max-age=31536000; includeSubDomains; preload
```

**Data at Rest**
```
DynamoDB: Enable encryption at rest (AWS managed or CMK)
S3: SSE-KMS with customer-managed keys
RDS: Encryption enabled with CMK
API Gateway Cache: Encryption enabled
Lambda environment variables: Encrypted with KMS

KMS Key Policy:
- Separate keys per environment (dev, staging, prod)
- Key rotation enabled (automatic annual rotation)
- Least privilege access
- All key usage logged in CloudTrail
```

**KMS Configuration:**
```javascript
// Encrypt PHI before storing
const AWS = require('aws-sdk');
const kms = new AWS.KMS();

async function encryptPHI(plaintext) {
  const params = {
    KeyId: process.env.KMS_KEY_ID,
    Plaintext: Buffer.from(JSON.stringify(plaintext)),
    EncryptionContext: {
      purpose: 'PHI-storage',
      dataType: 'patient-record'
    }
  };
  
  const { CiphertextBlob } = await kms.encrypt(params).promise();
  return CiphertextBlob.toString('base64');
}

async function decryptPHI(ciphertext) {
  const params = {
    CiphertextBlob: Buffer.from(ciphertext, 'base64'),
    EncryptionContext: {
      purpose: 'PHI-storage',
      dataType: 'patient-record'
    }
  };
  
  const { Plaintext } = await kms.decrypt(params).promise();
  return JSON.parse(Plaintext.toString());
}
```

**4. Transmission Security (§164.312(e)(1))**

**Private API Gateway**
```
- VPC Endpoint for API access
- No public internet exposure for PHI endpoints
- VPC endpoint policy restricting access
- VPC Flow Logs enabled
```

**Resource Policy:**
```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Deny",
      "Principal": "*",
      "Action": "execute-api:Invoke",
      "Resource": "execute-api:/*/*/*",
      "Condition": {
        "StringNotEquals": {
          "aws:sourceVpce": "vpce-xxxxxxxxx"
        }
      }
    },
    {
      "Effect": "Allow",
      "Principal": "*",
      "Action": "execute-api:Invoke",
      "Resource": "execute-api:/*/*/*"
    }
  ]
}
```

**5. Data Integrity (§164.312(c)(1))**

**Request Signing & Validation**
```javascript
// Verify request hasn't been tampered with
const crypto = require('crypto');

function verifyRequestSignature(event) {
  const signature = event.headers['X-Signature'];
  const timestamp = event.headers['X-Timestamp'];
  const body = event.body;
  
  // Prevent replay attacks (5-minute window)
  const now = Date.now();
  if (Math.abs(now - parseInt(timestamp)) > 300000) {
    throw new Error('Request expired');
  }
  
  // Verify signature
  const expectedSignature = crypto
    .createHmac('sha256', process.env.SIGNING_SECRET)
    .update(timestamp + body)
    .digest('hex');
  
  if (signature !== expectedSignature) {
    throw new Error('Invalid signature');
  }
}
```

**6. Person or Entity Authentication (§164.312(d))**

**Multi-Factor Authentication**
```
Cognito Configuration:
- SMS MFA or TOTP
- Mandatory for all users accessing PHI
- Step-up authentication for sensitive operations
- Device fingerprinting and risk-based auth
```

**Session Management**
```javascript
// Track sessions in DynamoDB
async function validateSession(userId, sessionId) {
  const session = await dynamoDB.get({
    TableName: 'Sessions',
    Key: { userId, sessionId }
  }).promise();
  
  if (!session.Item) {
    throw new Error('Invalid session');
  }
  
  // Check if session expired (15 min inactivity)
  const lastActivity = session.Item.lastActivity;
  if (Date.now() - lastActivity > 900000) {
    await terminateSession(userId, sessionId);
    throw new Error('Session expired');
  }
  
  // Update last activity
  await dynamoDB.update({
    TableName: 'Sessions',
    Key: { userId, sessionId },
    UpdateExpression: 'SET lastActivity = :now',
    ExpressionAttributeValues: { ':now': Date.now() }
  }).promise();
  
  return session.Item;
}
```

**7. Data Minimization & Purpose Limitation**

**Field-Level Access Control**
```javascript
// Return only necessary fields based on role
function filterPHI(patientRecord, userRole, purpose) {
  const rolePermissions = {
    'doctor': ['ssn', 'medicalHistory', 'diagnosis', 'prescriptions'],
    'nurse': ['medicalHistory', 'prescriptions'],
    'billing': ['ssn', 'insurance', 'billingHistory'],
    'patient': ['own-all-fields']
  };
  
  const allowedFields = rolePermissions[userRole] || [];
  
  // Filter record to include only allowed fields
  const filtered = {};
  for (const field of allowedFields) {
    if (patientRecord[field]) {
      filtered[field] = patientRecord[field];
    }
  }
  
  // Log access with purpose
  auditLog({
    userId,
    patientId: patientRecord.id,
    fieldsAccessed: Object.keys(filtered),
    purpose,
    timestamp: new Date()
  });
  
  return filtered;
}
```

**8. Breach Notification Readiness**

**Automated Breach Detection**
```javascript
// EventBridge rule for suspicious activity
{
  "source": ["aws.apigateway"],
  "detail-type": ["API Call via CloudTrail"],
  "detail": {
    "eventName": ["GetPatientRecord"],
    "responseElements": {
      "statusCode": [200]
    }
  }
}

// Lambda to detect anomalies
async function detectBreach(event) {
  const userId = event.detail.userIdentity.principalId;
  const patientId = event.detail.requestParameters.patientId;
  
  // Check for unusual access patterns
  const accessCount = await getAccessCount(userId, '1h');
  if (accessCount > 100) {
    await triggerSecurityAlert('Potential data breach - excessive access', {
      userId,
      accessCount,
      timeWindow: '1h'
    });
  }
  
  // Check for access outside normal hours
  const hour = new Date().getHours();
  const userSchedule = await getUserSchedule(userId);
  if (!userSchedule.workingHours.includes(hour)) {
    await triggerSecurityAlert('Off-hours access', {
      userId,
      patientId,
      time: new Date()
    });
  }
  
  // Check for geographic anomalies
  const sourceIP = event.detail.sourceIPAddress;
  const location = await getLocation(sourceIP);
  const userLocation = await getUserLocation(userId);
  if (distance(location, userLocation) > 100) { // miles
    await triggerSecurityAlert('Geographic anomaly', {
      userId,
      expectedLocation: userLocation,
      actualLocation: location
    });
  }
}
```

**Breach Response Plan**
```
1. Automated Detection → SecurityHub finding
2. Notification to security team (< 5 minutes)
3. Immediate investigation (< 1 hour)
4. If confirmed breach:
   - Isolate affected systems
   - Preserve evidence (logs)
   - Notify affected individuals (< 60 days)
   - Report to HHS if >500 individuals affected
```

**9. Business Associate Agreements (BAA)**

**AWS BAA**
```
- Sign BAA with AWS (required for HIPAA compliance)
- Covers: API Gateway, Lambda, DynamoDB, S3, CloudWatch, KMS
- Not all AWS services are HIPAA-eligible (check current list)
- Document all HIPAA-eligible services used
```

**Third-Party Services**
```
- BAA required for any third party processing PHI
- Audit third-party compliance annually
- Data Processing Addendum (DPA) in place
- Right to audit clause
```

**10. Disaster Recovery & Business Continuity**

**Backup Strategy**
```
- DynamoDB: Point-in-time recovery enabled
- S3: Versioning and Cross-Region Replication
- RDS: Automated backups (7-35 day retention)
- Backup encryption with KMS
- Test restore monthly
```

**Multi-Region Deployment**
```
Primary Region: us-east-1
DR Region: us-west-2

Route 53 Health Checks → Failover routing
DynamoDB Global Tables → Multi-region replication
S3 Cross-Region Replication → PHI backup
Lambda in both regions → Active-passive
```

**11. Compliance Monitoring**

**AWS Config Rules**
```
- api-gw-ssl-enabled
- encrypted-volumes
- s3-bucket-ssl-requests-only
- cloudtrail-enabled
- rds-storage-encrypted
- kms-cmk-not-scheduled-for-deletion
```

**Security Hub**
```
- Enable HIPAA Security Standard
- Automated compliance checks
- Findings aggregation
- Integration with incident response
```

**Regular Audits**
```
- Quarterly internal audits
- Annual external HIPAA audit
- Penetration testing (with proper authorization)
- Vulnerability scanning
- Review access logs for unauthorized access
```

**12. Employee Training & Access Management**

**Workforce Training**
```
- HIPAA training for all developers/operators
- Incident response drills
- Security awareness training
- Document training completion (compliance requirement)
```

**Access Reviews**
```
- Quarterly access reviews
- Remove terminated employees immediately
- Principle of least privilege
- Just-in-time access for elevated privileges
```

**Architecture Diagram:**
```
Internet
  ↓
Route 53 (Health Checks)
  ↓
CloudFront (optional, for static content only)
  ↓
Private API Gateway (VPC Endpoint)
  ↓
Lambda Authorizer (MFA check, RBAC, audit logging)
  ↓
Lambda Functions (encrypted environment variables)
  ↓
[DynamoDB (encrypted, PITR) | RDS (encrypted, Multi-AZ) | S3 (encrypted, versioned)]
  ↓
Audit Logs → S3 (Object Lock, 6-year retention)
  ↓
Security Monitoring (GuardDuty, SecurityHub, CloudWatch Alarms)
```

**Estimated Monthly Costs (for 1M API calls):**
- Private API Gateway: $3.50
- Lambda: $200
- DynamoDB (encrypted, on-demand): $400
- KMS: $50 (keys + API calls)
- CloudWatch Logs (6-year retention): $500
- S3 (audit logs, Object Lock): $100
- Security services (GuardDuty, SecurityHub): $150
- **Total: ~$1,400/month**

**Compliance Checklist:**
- [x] BAA with AWS
- [x] Encryption in transit (TLS 1.2+)
- [x] Encryption at rest (all data stores)
- [x] Access controls (authentication, authorization)
- [x] Audit logging (6-year retention, tamper-proof)
- [x] MFA enabled
- [x] Session management (15-min timeout)
- [x] Breach detection and notification plan
- [x] Disaster recovery and backups
- [x] Regular compliance audits
- [x] Employee training program
- [x] Data minimization (field-level access)
- [x] Monitoring and alerting
- [x] Incident response plan

---

**Key Takeaways:**

1. **Never store PHI in logs** - Use hashing or tokenization
2. **Minimum necessary access** - Field-level and role-based access control
3. **Audit everything** - Every access to PHI must be logged
4. **Encryption everywhere** - In transit and at rest, with key rotation
5. **Assume breach** - Have detection and response ready
6. **Regular testing** - Audits, penetration testing, DR drills
7. **Documentation** - Policies, procedures, training records
8. **BAAs** - With AWS and all third parties processing PHI
9. **Incident response** - <60 days notification requirement
10. **Continuous compliance** - Not a one-time effort

HIPAA compliance is ongoing - requires regular audits, monitoring, and updates as regulations evolve.

---

## Integration & Advanced Scenarios

### Q11: Implement Saga Pattern for Distributed Transaction Across Microservices

**Scenario:** You have an e-commerce order API that needs to coordinate across multiple services: Inventory (reserve items), Payment (charge card), Shipping (create label), Notification (send email). If any step fails, previous steps must be compensated/rolled back. Implement using API Gateway and Step Functions.

**Answer:**

**Architecture:**

```
Client
  ↓
API Gateway POST /orders
  ↓
Lambda (Order Orchestrator)
  ↓
Step Functions (Saga Workflow)
  ↓
[Inventory Service, Payment Service, Shipping Service, Notification Service]
  ↑
API Gateway (Service Endpoints)
```

**Step Functions State Machine (Saga Pattern):**

```json
{
  "Comment": "E-commerce order saga with compensation",
  "StartAt": "ReserveInventory",
  "States": {
    "ReserveInventory": {
      "Type": "Task",
      "Resource": "arn:aws:states:::apigateway:invoke",
      "Parameters": {
        "ApiEndpoint": "${InventoryAPIEndpoint}",
        "Method": "POST",
        "Path": "/inventory/reserve",
        "RequestBody.$": "$.orderItems",
        "AuthType": "IAM_ROLE"
      },
      "ResultPath": "$.reservationResult",
      "Catch": [
        {
          "ErrorEquals": ["States.ALL"],
          "ResultPath": "$.error",
          "Next": "ReservationFailed"
        }
      ],
      "Next": "ProcessPayment"
    },
    
    "ProcessPayment": {
      "Type": "Task",
      "Resource": "arn:aws:states:::apigateway:invoke",
      "Parameters": {
        "ApiEndpoint": "${PaymentAPIEndpoint}",
        "Method": "POST",
        "Path": "/payments/charge",
        "RequestBody": {
          "amount.$": "$.totalAmount",
          "customerId.$": "$.customerId",
          "orderId.$": "$.orderId"
        },
        "AuthType": "IAM_ROLE"
      },
      "ResultPath": "$.paymentResult",
      "Catch": [
        {
          "ErrorEquals": ["States.ALL"],
          "ResultPath": "$.error",
          "Next": "CompensateInventory"
        }
      ],
      "Next": "CreateShippingLabel"
    },
    
    "CreateShippingLabel": {
      "Type": "Task",
      "Resource": "arn:aws:states:::apigateway:invoke",
      "Parameters": {
        "ApiEndpoint": "${ShippingAPIEndpoint}",
        "Method": "POST",
        "Path": "/shipping/label",
        "RequestBody": {
          "address.$": "$.shippingAddress",
          "items.$": "$.orderItems",
          "orderId.$": "$.orderId"
        },
        "AuthType": "IAM_ROLE"
      },
      "ResultPath": "$.shippingResult",
      "Catch": [
        {
          "ErrorEquals": ["States.ALL"],
          "ResultPath": "$.error",
          "Next": "CompensatePayment"
        }
      ],
      "Next": "SendConfirmationEmail"
    },
    
    "SendConfirmationEmail": {
      "Type": "Task",
      "Resource": "arn:aws:states:::apigateway:invoke",
      "Parameters": {
        "ApiEndpoint": "${NotificationAPIEndpoint}",
        "Method": "POST",
        "Path": "/notifications/email",
        "RequestBody": {
          "email.$": "$.customerEmail",
          "orderId.$": "$.orderId",
          "trackingNumber.$": "$.shippingResult.trackingNumber"
        },
        "AuthType": "IAM_ROLE"
      },
      "ResultPath": "$.notificationResult",
      "Next": "OrderSuccess"
    },
    
    "OrderSuccess": {
      "Type": "Succeed"
    },
    
    "CompensatePayment": {
      "Type": "Task",
      "Resource": "arn:aws:states:::apigateway:invoke",
      "Parameters": {
        "ApiEndpoint": "${PaymentAPIEndpoint}",
        "Method": "POST",
        "Path": "/payments/refund",
        "RequestBody": {
          "transactionId.$": "$.paymentResult.transactionId",
          "amount.$": "$.totalAmount"
        },
        "AuthType": "IAM_ROLE"
      },
      "ResultPath": "$.refundResult",
      "Next": "CompensateInventory"
    },
    
    "CompensateInventory": {
      "Type": "Task",
      "Resource": "arn:aws:states:::apigateway:invoke",
      "Parameters": {
        "ApiEndpoint": "${InventoryAPIEndpoint}",
        "Method": "POST",
        "Path": "/inventory/release",
        "RequestBody": {
          "reservationId.$": "$.reservationResult.reservationId"
        },
        "AuthType": "IAM_ROLE"
      },
      "ResultPath": "$.releaseResult",
      "Next": "OrderFailed"
    },
    
    "ReservationFailed": {
      "Type": "Fail",
      "Error": "InventoryReservationFailed",
      "Cause": "Unable to reserve inventory"
    },
    
    "OrderFailed": {
      "Type": "Fail",
      "Error": "OrderProcessingFailed",
      "Cause": "Order failed and was compensated"
    }
  }
}
```

**Initial API Gateway Lambda (Order Orchestrator):**

```javascript
const AWS = require('aws-sdk');
const stepfunctions = new AWS.StepFunctions();

exports.handler = async (event) => {
  try {
    const order = JSON.parse(event.body);
    
    // Validate order
    if (!order.items || order.items.length === 0) {
      return {
        statusCode: 400,
        body: JSON.stringify({ error: 'Invalid order' })
      };
    }
    
    // Generate order ID
    const orderId = generateOrderId();
    
    // Start Step Functions execution
    const params = {
      stateMachineArn: process.env.SAGA_STATE_MACHINE_ARN,
      input: JSON.stringify({
        orderId,
        customerId: order.customerId,
        orderItems: order.items,
        totalAmount: calculateTotal(order.items),
        shippingAddress: order.shippingAddress,
        customerEmail: order.customerEmail
      }),
      name: `order-${orderId}-${Date.now()}`
    };
    
    const execution = await stepfunctions.startExecution(params).promise();
    
    // Return 202 Accepted (async processing)
    return {
      statusCode: 202,
      headers: {
        'Content-Type': 'application/json',
        'Location': `/orders/${orderId}`
      },
      body: JSON.stringify({
        orderId,
        status: 'processing',
        executionArn: execution.executionArn,
        message: 'Order is being processed'
      })
    };
    
  } catch (error) {
    console.error('Error starting saga:', error);
    return {
      statusCode: 500,
      body: JSON.stringify({ error: 'Internal server error' })
    };
  }
};
```

**Microservice APIs (via API Gateway):**

**Inventory Service:**
```javascript
// POST /inventory/reserve
exports.reserveInventory = async (event) => {
  const items = JSON.parse(event.body);
  
  try {
    // Check availability
    for (const item of items) {
      const available = await checkInventory(item.productId, item.quantity);
      if (!available) {
        return {
          statusCode: 409,
          body: JSON.stringify({
            error: 'InsufficientInventory',
            productId: item.productId
          })
        };
      }
    }
    
    // Reserve items (decrement available, increment reserved)
    const reservationId = generateReservationId();
    await dynamoDB.transactWrite({
      TransactItems: items.map(item => ({
        Update: {
          TableName: 'Inventory',
          Key: { productId: item.productId },
          UpdateExpression: 'SET reserved = reserved + :qty, available = available - :qty',
          ConditionExpression: 'available >= :qty',
          ExpressionAttributeValues: { ':qty': item.quantity }
        }
      }))
    }).promise();
    
    // Store reservation with TTL (auto-expire in 15 minutes if not confirmed)
    await dynamoDB.put({
      TableName: 'Reservations',
      Item: {
        reservationId,
        items,
        status: 'reserved',
        expiresAt: Date.now() + (15 * 60 * 1000),
        ttl: Math.floor(Date.now() / 1000) + (15 * 60)
      }
    }).promise();
    
    return {
      statusCode: 200,
      body: JSON.stringify({ reservationId, items })
    };
    
  } catch (error) {
    console.error('Reservation error:', error);
    return {
      statusCode: 500,
      body: JSON.stringify({ error: 'Reservation failed' })
    };
  }
};

// POST /inventory/release (Compensation)
exports.releaseInventory = async (event) => {
  const { reservationId } = JSON.parse(event.body);
  
  try {
    // Get reservation
    const reservation = await dynamoDB.get({
      TableName: 'Reservations',
      Key: { reservationId }
    }).promise();
    
    if (!reservation.Item) {
      return {
        statusCode: 404,
        body: JSON.stringify({ error: 'Reservation not found' })
      };
    }
    
    // Release items
    await dynamoDB.transactWrite({
      TransactItems: [
        ...reservation.Item.items.map(item => ({
          Update: {
            TableName: 'Inventory',
            Key: { productId: item.productId },
            UpdateExpression: 'SET reserved = reserved - :qty, available = available + :qty',
            ExpressionAttributeValues: { ':qty': item.quantity }
          }
        })),
        {
          Update: {
            TableName: 'Reservations',
            Key: { reservationId },
            UpdateExpression: 'SET #status = :status',
            ExpressionAttributeNames: { '#status': 'status' },
            ExpressionAttributeValues: { ':status': 'released' }
          }
        }
      ]
    }).promise();
    
    return {
      statusCode: 200,
      body: JSON.stringify({ reservationId, status: 'released' })
    };
    
  } catch (error) {
    console.error('Release error:', error);
    return {
      statusCode: 500,
      body: JSON.stringify({ error: 'Release failed' })
    };
  }
};
```

**Payment Service:**
```javascript
// POST /payments/charge
exports.chargePayment = async (event) => {
  const { amount, customerId, orderId } = JSON.parse(event.body);
  
  try {
    // Call payment provider (Stripe, etc.)
    const charge = await stripe.charges.create({
      amount: amount * 100, // cents
      currency: 'usd',
      customer: customerId,
      description: `Order ${orderId}`,
      metadata: { orderId }
    });
    
    // Store transaction
    await dynamoDB.put({
      TableName: 'Transactions',
      Item: {
        transactionId: charge.id,
        orderId,
        customerId,
        amount,
        status: 'charged',
        timestamp: Date.now()
      }
    }).promise();
    
    return {
      statusCode: 200,
      body: JSON.stringify({
        transactionId: charge.id,
        amount,
        status: 'success'
      })
    };
    
  } catch (error) {
    console.error('Payment error:', error);
    return {
      statusCode: 402,
      body: JSON.stringify({
        error: 'PaymentFailed',
        message: error.message
      })
    };
  }
};

// POST /payments/refund (Compensation)
exports.refundPayment = async (event) => {
  const { transactionId, amount } = JSON.parse(event.body);
  
  try {
    // Refund via payment provider
    const refund = await stripe.refunds.create({
      charge: transactionId,
      amount: amount * 100
    });
    
    // Update transaction status
    await dynamoDB.update({
      TableName: 'Transactions',
      Key: { transactionId },
      UpdateExpression: 'SET #status = :status, refundId = :refundId',
      ExpressionAttributeNames: { '#status': 'status' },
      ExpressionAttributeValues: {
        ':status': 'refunded',
        ':refundId': refund.id
      }
    }).promise();
    
    return {
      statusCode: 200,
      body: JSON.stringify({
        transactionId,
        refundId: refund.id,
        status: 'refunded'
      })
    };
    
  } catch (error) {
    console.error('Refund error:', error);
    return {
      statusCode: 500,
      body: JSON.stringify({ error: 'Refund failed' })
    };
  }
};
```

**Order Status Endpoint (Poll for Results):**

```javascript
// GET /orders/{orderId}
exports.getOrderStatus = async (event) => {
  const orderId = event.pathParameters.orderId;
  
  try {
    // Get order from DynamoDB
    const order = await dynamoDB.get({
      TableName: 'Orders',
      Key: { orderId }
    }).promise();
    
    if (!order.Item) {
      return {
        statusCode: 404,
        body: JSON.stringify({ error: 'Order not found' })
      };
    }
    
    // Get Step Functions execution status
    const execution = await stepfunctions.describeExecution({
      executionArn: order.Item.executionArn
    }).promise();
    
    return {
      statusCode: 200,
      body: JSON.stringify({
        orderId,
        status: execution.status, // RUNNING, SUCCEEDED, FAILED
        details: order.Item,
        lastUpdated: execution.stopDate || execution.startDate
      })
    };
    
  } catch (error) {
    console.error('Error getting order status:', error);
    return {
      statusCode: 500,
      body: JSON.stringify({ error: 'Internal server error' })
    };
  }
};
```

**EventBridge Integration for Real-Time Updates:**

```javascript
// EventBridge rule: Step Functions execution completed
{
  "source": ["aws.states"],
  "detail-type": ["Step Functions Execution Status Change"],
  "detail": {
    "status": ["SUCCEEDED", "FAILED", "ABORTED"],
    "stateMachineArn": ["${SagaStateMachineArn}"]
  }
}

// Lambda to handle completion
exports.handleOrderCompletion = async (event) => {
  const { executionArn, status, output } = event.detail;
  const result = JSON.parse(output);
  
  // Update order status in DynamoDB
  await dynamoDB.update({
    TableName: 'Orders',
    Key: { orderId: result.orderId },
    UpdateExpression: 'SET #status = :status, completedAt = :now',
    ExpressionAttributeNames: { '#status': 'status' },
    ExpressionAttributeValues: {
      ':status': status === 'SUCCEEDED' ? 'completed' : 'failed',
      ':now': Date.now()
    }
  }).promise();
  
  // Send WebSocket notification to client (if connected)
  if (result.connectionId) {
    await apiGatewayManagement.postToConnection({
      ConnectionId: result.connectionId,
      Data: JSON.stringify({
        type: 'order_update',
        orderId: result.orderId,
        status: status === 'SUCCEEDED' ? 'completed' : 'failed'
      })
    }).promise();
  }
};
```

**Idempotency Implementation:**

```javascript
// All service endpoints should be idempotent
exports.chargePayment = async (event) => {
  const { amount, customerId, orderId } = JSON.parse(event.body);
  const idempotencyKey = event.headers['Idempotency-Key'] || orderId;
  
  try {
    // Check if already processed
    const existing = await dynamoDB.get({
      TableName: 'IdempotencyKeys',
      Key: { idempotencyKey }
    }).promise();
    
    if (existing.Item) {
      // Return cached response
      return {
        statusCode: 200,
        body: existing.Item.response
      };
    }
    
    // Process payment
    const charge = await stripe.charges.create({
      amount: amount * 100,
      currency: 'usd',
      customer: customerId,
      idempotency_key: idempotencyKey
    });
    
    const response = {
      statusCode: 200,
      body: JSON.stringify({
        transactionId: charge.id,
        amount,
        status: 'success'
      })
    };
    
    // Store idempotency key with TTL (24 hours)
    await dynamoDB.put({
      TableName: 'IdempotencyKeys',
      Item: {
        idempotencyKey,
        response: response.body,
        ttl: Math.floor(Date.now() / 1000) + (24 * 60 * 60)
      }
    }).promise();
    
    return response;
    
  } catch (error) {
    console.error('Payment error:', error);
    return {
      statusCode: 402,
      body: JSON.stringify({ error: 'PaymentFailed' })
    };
  }
};
```

**Monitoring & Alerting:**

```
CloudWatch Metrics:
- Step Functions execution success/failure rate
- Average execution duration
- Compensation step invocations
- Individual service error rates

Alarms:
- Saga failure rate > 5%
- Payment compensation rate > 1%
- Execution duration > 30 seconds
```

**Benefits:**
- Distributed transaction consistency
- Automatic compensation on failure
- Async processing (fast API response)
- Visibility into each step
- Retry and error handling built-in
- Scalable and decoupled services

**Trade-offs:**
- Eventual consistency (not immediate)
- Compensation logic required
- Step Functions costs ($25 per million transitions)
- Increased complexity

---

This concludes the scenario-based questions. These cover:
- Multi-tenant architecture
- Performance troubleshooting
- Security hardening
- Migration strategies
- Real-world integration patterns
- HIPAA compliance
- Distributed transactions

Each scenario includes detailed implementation, code examples, and best practices for experienced professionals.
