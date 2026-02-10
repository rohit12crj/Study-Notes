# API Gateway

# AWS API Gateway - Comprehensive Revision Notes
## For Experienced Professionals

---

## 1. API Gateway Types & Architecture

### REST API vs HTTP API vs WebSocket API

**REST API (v1)**
- Full-featured, mature API type
- Supports request/response transformations
- API keys, usage plans, request validation
- Resource policies, custom authorizers
- AWS WAF integration
- Higher latency (~few ms overhead)
- More expensive than HTTP APIs

**HTTP API (v2)**
- Simpler, faster, cheaper (up to 71% cost reduction)
- Lower latency (~60% faster)
- Limited features: no API keys, usage plans, or request validation
- Native JWT authorizer support
- CORS configuration is simpler
- Better for microservices and serverless backends
- Private integrations with VPC resources

**WebSocket API**
- Bidirectional, persistent connections
- Real-time applications (chat, streaming, notifications)
- Route selection expressions
- Connection management (connect, disconnect, message routes)
- Integration with Lambda, HTTP endpoints, AWS services

### When to Use Each Type
- **REST API**: Complex APIs requiring throttling, API keys, request validation, SDK generation
- **HTTP API**: Cost-sensitive, low-latency APIs with simple requirements
- **WebSocket API**: Real-time bidirectional communication

---

## 2. Integration Types

### Lambda Integration
**Lambda Proxy Integration (Recommended)**
- Request passed as-is to Lambda
- Lambda must return specific response format
- Simplest integration, minimal configuration
```json
{
  "statusCode": 200,
  "headers": {"Content-Type": "application/json"},
  "body": "{\"message\": \"success\"}"
}
```

**Lambda Custom Integration**
- Transform requests/responses using mapping templates
- More control over request/response format
- VTL (Velocity Template Language) for transformations

### HTTP Integration
**HTTP Proxy Integration**
- Forwards entire request to backend HTTP endpoint
- Minimal transformation
- Backend must handle API Gateway request format

**HTTP Custom Integration**
- Full control with mapping templates
- Transform requests/responses
- Useful for legacy systems

### AWS Service Integration
- Direct integration with AWS services (DynamoDB, SQS, SNS, Step Functions)
- No Lambda required
- Mapping templates required for request/response transformation
- Lower cost, reduced latency
- Example: API Gateway → DynamoDB PutItem

### VPC Link Integration
- Private integration with resources in VPC
- Network Load Balancer (NLB) or Application Load Balancer (ALB)
- Secure communication without exposing to internet
- REST API: VPC Link v1 (NLB only)
- HTTP API: VPC Link v2 (NLB, ALB, Cloud Map)

### Mock Integration
- Returns static response without backend
- Useful for API development, testing
- CORS preflight responses

---

## 3. Authentication & Authorization

### IAM Authentication
- AWS Signature v4 signing
- Use for service-to-service communication
- Leverage IAM roles and policies
- No additional cost

### Cognito User Pool Authorizer
- Built-in user authentication
- JWT token validation
- User management, MFA, social identity providers
- Best for B2C applications

### Lambda Authorizer (Custom Authorizer)
**Token-based**
- Bearer token in header
- Cached based on token
- Returns IAM policy

**Request-based**
- Evaluate headers, query strings, context
- Not cached by token
- More flexible

**Best Practices:**
- Enable caching (TTL: 300-3600 seconds)
- Return principal ID for logging
- Use environment variables for secrets
- Implement proper error handling

### JWT Authorizer (HTTP API only)
- Native JWT validation
- No Lambda required
- Configure issuer and audience
- Lower latency than Lambda authorizer

### Resource Policies
- Control API access at API level
- Allow/deny based on IP, VPC, AWS account
- Useful for cross-account access
- Whitelist/blacklist scenarios

---

## 4. Request/Response Transformation

### Mapping Templates (VTL)
```vtl
#set($inputRoot = $input.path('$'))
{
  "userId": "$input.params('userId')",
  "body": $input.json('$'),
  "timestamp": "$context.requestTime"
}
```

### Common Context Variables
- `$context.requestId`: Unique request ID
- `$context.identity.sourceIp`: Client IP
- `$context.authorizer.claims`: JWT claims
- `$context.error.message`: Error details
- `$context.requestTime`: Request timestamp

### Request Validation
- Validate request body against JSON schema
- Validate required parameters
- Reduce backend validation logic
- Return 400 errors early

---

## 5. Throttling & Usage Plans

### Throttling Levels (Priority Order)
1. **Method-level** throttling (highest priority)
2. **Stage-level** throttling
3. **Account-level** throttling (default: 10,000 RPS, 5,000 burst)

### Rate & Burst Limits
- **Rate limit**: Steady-state requests per second
- **Burst limit**: Maximum concurrent requests (token bucket algorithm)
- Throttled requests: 429 Too Many Requests

### Usage Plans
- **API keys**: Identify clients
- **Throttle limits**: Per API key
- **Quota limits**: Daily, weekly, monthly request limits
- Multiple usage plans per API
- Associate API keys with usage plans

### Best Practices
- Set conservative defaults, increase based on monitoring
- Use method-level throttling for critical endpoints
- Implement exponential backoff in clients
- Monitor CloudWatch metrics: Count, 4XXError, 5XXError, Latency

---

## 6. Caching

### API Gateway Cache
- **Cache TTL**: 0-3600 seconds
- **Cache capacity**: 0.5GB to 237GB
- **Cache key**: Method, resource path, query strings, headers
- Reduces backend calls, improves latency
- Cost: Per hour of cache provisioned

### Cache Configuration
**Cache Key Parameters**
- Query strings
- Headers
- Path parameters
- Integration headers

**Cache Settings**
- Require authorization for cache invalidation
- Encrypt cache data
- Per-method cache override

### Cache Invalidation
- Clients can invalidate with `Cache-Control: max-age=0` header
- Requires proper permissions
- Useful for testing, forced updates

### Best Practices
- Cache GET requests only
- Start with 0.5GB cache, scale based on hit rate
- Monitor `CacheHitCount` and `CacheMissCount`
- Use cache key parameters strategically
- Set appropriate TTL based on data volatility

---

## 7. CORS Configuration

### Preflight Requests (OPTIONS)
- Browser sends OPTIONS request before actual request
- API Gateway must respond with CORS headers
- Mock integration for OPTIONS method

### Required Headers
```
Access-Control-Allow-Origin: *
Access-Control-Allow-Methods: GET,POST,PUT,DELETE
Access-Control-Allow-Headers: Content-Type,Authorization
Access-Control-Max-Age: 86400
```

### REST API CORS
- Manual configuration via console or API
- Enable CORS on each method
- Configure response headers

### HTTP API CORS
- Simplified configuration
- Specify allowed origins, methods, headers
- Automatically handles preflight

---

## 8. Deployment & Stages

### Stages
- Named references to deployments
- Environment-specific configurations (dev, staging, prod)
- Each stage has unique invoke URL
- Stage variables for environment-specific values

### Stage Variables
- Environment variables at stage level
- Reference in integration endpoints, mapping templates
- Example: `${stageVariables.lambdaAlias}`
- Use for Lambda aliases, different backend endpoints

### Canary Deployments
- Route percentage of traffic to canary
- Test new version with production traffic
- Gradual rollout (0-100%)
- Monitor CloudWatch metrics
- Promote or rollback canary

### Deployment Best Practices
- Use stage variables for environment configuration
- Implement canary deployments for production
- Tag stages appropriately
- Enable CloudWatch logging per stage
- Use different AWS accounts for environments

---

## 9. Monitoring & Logging

### CloudWatch Metrics
**Default Metrics (No charge)**
- **Count**: Total API requests
- **IntegrationLatency**: Backend response time
- **Latency**: Total time (includes API Gateway overhead)
- **4XXError**: Client-side errors
- **5XXError**: Server-side errors

**Detailed Metrics (Charged)**
- **CacheHitCount**, **CacheMissCount**
- Per-method metrics

### CloudWatch Logs
**Execution Logs**
- Detailed request/response logs
- Integration calls, transformations
- Error traces
- Log levels: ERROR, INFO

**Access Logs**
- Structured logs in custom format
- JSON format recommended
- Include: requestId, ip, user, httpMethod, resourcePath, status, protocol, responseLength, latency

**Log Format Example**
```json
{
  "requestId": "$context.requestId",
  "ip": "$context.identity.sourceIp",
  "caller": "$context.identity.caller",
  "user": "$context.identity.user",
  "requestTime": "$context.requestTime",
  "httpMethod": "$context.httpMethod",
  "resourcePath": "$context.resourcePath",
  "status": "$context.status",
  "protocol": "$context.protocol",
  "responseLength": "$context.responseLength",
  "integrationLatency": "$context.integrationLatency",
  "latency": "$context.latency"
}
```

### X-Ray Tracing
- End-to-end request tracing
- Visualize service map
- Identify performance bottlenecks
- Trace Lambda, DynamoDB, other AWS services
- Enable at stage level

### Best Practices
- Enable access logs for production
- Use structured JSON logging
- Set up CloudWatch alarms: 4XX/5XX rates, latency p99
- Enable X-Ray for distributed tracing
- Use CloudWatch Logs Insights for analysis

---

## 10. Security Best Practices

### API Security Layers
1. **Resource policies**: IP whitelisting, VPC/VPCe restrictions
2. **Authentication**: IAM, Cognito, Lambda authorizer
3. **WAF**: SQL injection, XSS protection, rate limiting
4. **Encryption**: TLS in transit, encryption at rest (cache)
5. **Secrets**: Never expose in URLs or headers

### AWS WAF Integration
- Protect against common web exploits
- Rate-based rules (DDoS protection)
- Geo-blocking
- IP reputation lists
- Custom rules (SQL injection, XSS)

### Mutual TLS (mTLS)
- Client certificate authentication
- Domain name required (custom domain)
- Store trusted certificates in S3
- Strong authentication for B2B scenarios

### Private APIs
- Accessible only from VPC
- VPC endpoint (interface endpoint)
- Resource policies for VPC restriction
- No public internet access

### Client Certificates
- API Gateway generates client-side certificate
- Backend validates certificate
- Additional security layer

### Security Checklist
- [ ] Enable AWS WAF
- [ ] Use custom domain with TLS 1.2+
- [ ] Implement proper authentication
- [ ] Enable CloudWatch logging (encrypted)
- [ ] Use resource policies for IP restrictions
- [ ] Regular security audits with IAM Access Analyzer
- [ ] Enable X-Ray for anomaly detection
- [ ] Rotate API keys regularly
- [ ] Validate and sanitize all inputs

---

## 11. Performance Optimization

### Latency Optimization
**API Gateway Overhead**: ~few ms (REST API), ~60% faster (HTTP API)

**Strategies:**
1. Use HTTP API for lower latency
2. Enable caching (cache hit = ~single-digit ms)
3. Optimize Lambda cold starts (provisioned concurrency)
4. Use VPC Link for private integrations (avoid internet)
5. Reduce mapping template complexity
6. Enable compression for large payloads
7. Use regional API endpoints (for single-region)

### Connection Reuse
- Enable keep-alive connections to backend
- Lambda: Set connection timeout appropriately
- Reduces integration latency

### Payload Size
- Maximum payload: 10MB (REST API), 10MB (HTTP API)
- Use compression for large responses
- Consider pagination for large datasets
- Stream large files from S3 (presigned URLs)

### Lambda Integration Optimization
- **Provisioned Concurrency**: Eliminate cold starts
- **Lambda SnapStart**: Java cold start optimization
- **Lambda layers**: Reduce deployment package size
- **Environment variables**: Avoid initialization overhead

---

## 12. Cost Optimization

### Pricing Model (US East Region)
**REST API:**
- $3.50 per million requests
- Data transfer out charges
- Cache: $0.020/hour per GB

**HTTP API:**
- $1.00 per million requests (71% cheaper)
- Data transfer out charges

**WebSocket API:**
- $1.00 per million messages
- $0.25 per million connection minutes

### Cost Reduction Strategies
1. **Use HTTP API** instead of REST API where possible
2. **Enable caching** to reduce backend calls
3. **AWS service integration** (skip Lambda)
4. **Optimize payload size** to reduce data transfer
5. **Use regional endpoints** (not edge-optimized) for single-region
6. **Consolidate APIs** to reduce number of APIs
7. **Remove unused APIs** and stages
8. **Monitor usage** and right-size cache

### Edge-Optimized vs Regional vs Private
- **Edge-Optimized**: CloudFront distribution, global users, higher cost
- **Regional**: Single region, lower latency, lower cost
- **Private**: VPC-only access, no data transfer to internet

---

## 13. API Gateway Patterns

### 1. API Aggregation Pattern
- Single API Gateway → Multiple backend services
- Simplify client interaction
- Reduce client complexity
- Use mapping templates or Lambda

### 2. Request/Response Transformation
- Legacy system integration
- Format conversion (XML ↔ JSON)
- Data enrichment
- Header manipulation

### 3. Direct AWS Service Integration
- API Gateway → DynamoDB (CRUD operations)
- API Gateway → SQS (async processing)
- API Gateway → Step Functions (workflows)
- No Lambda required, lower cost/latency

### 4. Backend for Frontend (BFF)
- Separate APIs for web, mobile, IoT
- Optimized responses per client type
- Different authorization strategies

### 5. API Gateway + EventBridge
- API Gateway receives request
- EventBridge for event routing
- Decouple request handling from processing
- Multiple consumers for same event

### 6. GraphQL via Lambda
- REST API Gateway → Lambda (AppSync alternative)
- Custom GraphQL implementation
- Full control over schema and resolvers

### 7. Saga Pattern (Distributed Transactions)
- API Gateway → Step Functions
- Orchestrate microservices transactions
- Compensation logic for rollbacks

---

## 14. Troubleshooting Common Issues

### 429 Too Many Requests
**Cause:** Throttling limit exceeded
**Solution:**
- Increase throttle limits
- Implement client-side retry with exponential backoff
- Use usage plans to distribute limits
- Scale backend to handle load

### 502 Bad Gateway
**Cause:** Backend returned invalid response
**Solution:**
- Check Lambda function errors
- Verify integration response format
- Check backend timeout
- Review CloudWatch logs

### 504 Gateway Timeout
**Cause:** Integration timeout (29 seconds max)
**Solution:**
- Optimize backend processing time
- Use async patterns (SQS, Step Functions)
- Increase backend timeout (if < 29s)
- Consider breaking into smaller operations

### 403 Forbidden
**Cause:** Authorization failure
**Solution:**
- Verify authorizer configuration
- Check IAM policies
- Validate JWT token
- Review resource policies

### CORS Errors
**Cause:** Missing or incorrect CORS headers
**Solution:**
- Enable CORS in API Gateway
- Add OPTIONS method with mock integration
- Verify Access-Control-Allow-Origin header
- Check preflight request response

### High Latency
**Cause:** Multiple factors
**Solution:**
- Enable caching
- Optimize Lambda cold starts
- Check integration latency in CloudWatch
- Review mapping template complexity
- Use X-Ray for detailed trace

### Integration Latency Spikes
**Cause:** Backend performance issues
**Solution:**
- Scale backend (Lambda concurrency, database)
- Use provisioned concurrency
- Implement connection pooling
- Review backend logs

---

## 15. Advanced Topics

### Custom Domain Names
- User-friendly URLs (api.example.com)
- ACM certificate required
- Regional vs Edge-Optimized
- Base path mapping for multiple APIs
- Route 53 alias record

### API Gateway Endpoints
**Edge-Optimized (Default)**
- CloudFront distribution
- Global users, lowest latency
- Higher cost
- Deployed to us-east-1 CloudFront

**Regional**
- Same region as API
- Lower latency for regional users
- Lower cost
- Better control over distribution

**Private**
- VPC endpoint access only
- No internet exposure
- Highest security

### Binary Media Types
- Support for non-JSON payloads
- Images, PDFs, files
- Content-Type header matching
- Base64 encoding in Lambda proxy

### API Gateway Timeouts
- **Integration timeout**: 50ms to 29 seconds
- **Total timeout**: 29 seconds (hard limit)
- Async patterns for long operations

### Request Validators
- Validate request body, parameters, headers
- JSON schema validation
- Reduce backend validation
- Early error responses (400 Bad Request)

### SDK Generation
- Auto-generate client SDKs
- Java, JavaScript, Android, iOS, Ruby
- Includes authentication, serialization
- Useful for API consumers

### API Documentation
- OpenAPI 3.0 (Swagger) export/import
- Auto-generate documentation
- Developer portal integration
- Version management

### Multi-Region Active-Active
- Route 53 latency-based routing
- Multiple regional API deployments
- DynamoDB Global Tables for data
- Disaster recovery, low latency

---

## 16. Limits & Quotas (US East Region)

### Account-Level Limits (Adjustable)
- **Throttle rate**: 10,000 requests/second
- **Burst**: 5,000 concurrent requests
- **Regional APIs per account**: 600
- **Edge-optimized APIs per account**: 120

### API Limits
- **Resources per API**: 300
- **Stages per API**: 10
- **Methods per API**: 300
- **Integrations per API**: 300

### Request/Response Limits (Hard Limits)
- **Payload size**: 10 MB
- **Integration timeout**: 29 seconds
- **Header size**: 10 KB
- **Query string size**: 10 KB

### Cache Limits
- **Cache size**: 0.5 GB to 237 GB
- **Cache TTL**: 0 to 3600 seconds

### Other Limits
- **Custom authorizer timeout**: 30 seconds
- **Mapping template size**: 2 MB
- **Stage variables**: 64 KB total

---

## 17. Migration Strategies

### REST API to HTTP API
**Assess Compatibility:**
- Check features: API keys, usage plans, request validation not supported in HTTP API
- Evaluate Lambda authorizers (convert to JWT if possible)
- Review custom domain and TLS configuration

**Migration Steps:**
1. Create HTTP API with same resources/methods
2. Update Lambda functions (response format unchanged)
3. Configure JWT authorizers or Lambda authorizers
4. Test thoroughly in staging
5. Update DNS to point to HTTP API
6. Monitor metrics, errors
7. Deprecate REST API

### On-Premises to API Gateway
- Start with VPC Link for existing backends
- Gradually migrate to serverless (Lambda, DynamoDB)
- Use API Gateway for authentication/authorization
- Implement caching to reduce backend load

---

## 18. Real-World Scenarios

### Scenario 1: E-Commerce API
**Requirements:**
- Product catalog, cart, orders
- Public and authenticated APIs
- Rate limiting per customer tier
- Caching for product catalog
- PCI compliance for payments

**Solution:**
- REST API with Cognito authorizer
- Usage plans for different tiers (free, premium, enterprise)
- Caching for GET /products (TTL: 300s)
- Lambda proxy integration for business logic
- Direct DynamoDB integration for product reads
- Payment processing via Lambda → Stripe
- WAF for DDoS protection
- CloudWatch alarms for 5XX errors, latency

### Scenario 2: Real-Time Chat Application
**Requirements:**
- Bidirectional messaging
- Online presence
- Message history
- Scalable to millions

**Solution:**
- WebSocket API
- Routes: $connect, $disconnect, $default, sendMessage
- DynamoDB for connection IDs, message history
- Lambda for message routing
- ElastiCache for online presence
- EventBridge for fan-out to multiple recipients
- Connection management with TTL

### Scenario 3: Microservices Backend
**Requirements:**
- 15+ microservices
- Service-to-service auth
- Low latency, high throughput
- Cost-effective
- Private network only

**Solution:**
- HTTP API (lower cost, latency)
- Private API with VPC endpoints
- IAM authentication for service-to-service
- VPC Link to internal ALB
- Lambda for orchestration layer
- X-Ray for distributed tracing
- Separate stages per environment
- Blue-green deployment with canary

### Scenario 4: SaaS Multi-Tenant API
**Requirements:**
- Tenant isolation
- Per-tenant rate limiting
- Usage tracking and billing
- Multiple deployment regions

**Solution:**
- REST API with custom Lambda authorizer
- Tenant ID in JWT claims
- Usage plans with API keys per tenant
- Stage variables for tenant databases
- CloudWatch custom metrics for tenant usage
- Multi-region deployment with Route 53
- DynamoDB Global Tables for tenant data
- Quota enforcement in Lambda authorizer

---

## 19. Interview Preparation Points

### Key Concepts to Explain
1. **Difference between REST, HTTP, WebSocket APIs** - Features, use cases, pricing
2. **Integration types** - When to use proxy vs custom, AWS service integration benefits
3. **Throttling hierarchy** - Method > Stage > Account level
4. **Authentication mechanisms** - IAM vs Cognito vs Lambda authorizer trade-offs
5. **Caching strategy** - When to cache, cache key parameters, invalidation
6. **CORS** - Preflight, required headers, configuration differences
7. **VPC Link** - Private integrations, v1 vs v2
8. **Performance optimization** - HTTP API, caching, Lambda optimization
9. **Security layers** - Resource policies, WAF, mTLS, private APIs
10. **Troubleshooting** - Common error codes and resolution

### Design Questions to Prepare
- Design a scalable API for 1M+ users
- Implement rate limiting per customer tier
- Migrate monolith API to microservices
- Design real-time notification system
- Implement multi-region active-active API
- Cost optimization for high-traffic API
- Security hardening for financial API
- Design event-driven architecture with API Gateway

### Common Mistakes to Avoid
- Using REST API when HTTP API is sufficient
- Not enabling caching for read-heavy workloads
- Ignoring throttle limits and burst capacity
- Misconfiguring CORS
- Not monitoring integration latency separately
- Exposing sensitive data in URL parameters
- Not implementing proper error handling
- Overlooking request validation
- Not using stage variables for environments
- Forgetting about 29-second timeout limit

---

## 20. Checklist for Production APIs

### Pre-Deployment
- [ ] Authentication/authorization configured
- [ ] Request validation enabled
- [ ] CORS properly configured
- [ ] Throttling limits set appropriately
- [ ] Caching enabled for applicable endpoints
- [ ] Custom domain configured with ACM certificate
- [ ] Stage variables for environment-specific config
- [ ] API documentation generated

### Security
- [ ] AWS WAF enabled and rules configured
- [ ] Resource policies for IP restrictions
- [ ] TLS 1.2 or higher enforced
- [ ] API keys rotated regularly (if used)
- [ ] IAM least privilege policies
- [ ] Secrets in Secrets Manager/Parameter Store
- [ ] CloudWatch logs encrypted

### Monitoring & Alerting
- [ ] CloudWatch access logs enabled
- [ ] X-Ray tracing enabled
- [ ] CloudWatch alarms: 4XX/5XX rates, latency p99
- [ ] SNS notifications for critical alarms
- [ ] Dashboard for key metrics
- [ ] Log retention policy set

### Performance
- [ ] Payload sizes optimized
- [ ] Lambda provisioned concurrency (if needed)
- [ ] Regional endpoint (if single region)
- [ ] Connection reuse enabled
- [ ] Compression enabled for large payloads

### Deployment
- [ ] Canary deployment configured
- [ ] Rollback plan documented
- [ ] Blue-green deployment strategy
- [ ] Automated testing in staging
- [ ] Gradual traffic shift

### Maintenance
- [ ] Regular security patching
- [ ] API versioning strategy
- [ ] Deprecation policy
- [ ] Backup and disaster recovery plan
- [ ] Cost monitoring and optimization

---

## Quick Reference Commands

### AWS CLI - Common Operations

```bash
# Create REST API
aws apigateway create-rest-api --name "MyAPI" --description "My API"

# Create HTTP API
aws apigatewayv2 create-api --name "MyHTTPAPI" --protocol-type HTTP

# Deploy API
aws apigateway create-deployment --rest-api-id <api-id> --stage-name prod

# Create usage plan
aws apigateway create-usage-plan --name "Premium" --throttle rateLimit=1000,burstLimit=2000

# Enable caching
aws apigateway update-stage --rest-api-id <api-id> --stage-name prod --patch-operations op=replace,path=/cacheClusterEnabled,value=true

# Get API keys
aws apigateway get-api-keys

# Test invoke
aws apigateway test-invoke-method --rest-api-id <api-id> --resource-id <resource-id> --http-method GET

# Export API (OpenAPI)
aws apigateway get-export --rest-api-id <api-id> --stage-name prod --export-type swagger swagger.json

# CloudWatch logs
aws logs tail /aws/apigateway/<api-id> --follow

# Get metrics
aws cloudwatch get-metric-statistics --namespace AWS/ApiGateway --metric-name Count --dimensions Name=ApiName,Value=MyAPI --start-time 2024-01-01T00:00:00Z --end-time 2024-01-01T23:59:59Z --period 3600 --statistics Sum
```

---

## Resources for Deep Dive

### Official Documentation
- AWS API Gateway Developer Guide: https://docs.aws.amazon.com/apigateway/
- API Gateway REST API Reference: https://docs.aws.amazon.com/apigateway/latest/api/
- Best Practices: https://docs.aws.amazon.com/apigateway/latest/developerguide/best-practices.html

### Pricing
- https://aws.amazon.com/api-gateway/pricing/

### Workshops & Tutorials
- AWS API Gateway Workshop: https://catalog.workshops.aws/apigateway/
- Serverless Patterns Collection: https://serverlessland.com/patterns

### Monitoring Tools
- CloudWatch ServiceLens
- X-Ray Analytics
- AWS Cost Explorer

---

**Last Updated:** February 2026
**Author:** Comprehensive revision for experienced professionals
