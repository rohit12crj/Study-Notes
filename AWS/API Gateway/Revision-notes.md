## API Types

**REST API (v1)**
- Full-featured, supports API keys, usage plans, request validation
- Resource policies, custom authorizers, WAF integration
- More expensive, slightly higher latency
- Use for: Complex APIs needing throttling, API keys, SDK generation

**HTTP API (v2)**
- 71% cheaper, 60% faster than REST API
- Native JWT authorizer, simpler CORS
- No API keys, usage plans, or request validation
- Use for: Cost-sensitive, low-latency microservices

**WebSocket API**
- Bidirectional persistent connections
- Use for: Real-time chat, streaming, notifications

## Integration Types

**Lambda Proxy** - Entire request passed to Lambda, simple setup
**Lambda Custom** - Use VTL mapping templates for transformation
**HTTP Proxy** - Forward request to HTTP endpoint
**HTTP Custom** - Transform with mapping templates
**AWS Service** - Direct integration (DynamoDB, SQS, SNS, Step Functions) - no Lambda needed
**VPC Link** - Private integration with NLB/ALB in VPC
**Mock** - Return static response, useful for CORS OPTIONS

## Authentication & Authorization

**IAM Auth** - AWS SigV4, for service-to-service
**Cognito User Pool** - Built-in user auth, JWT validation, B2C apps
**Lambda Authorizer** - Custom logic, token or request-based, cached
**JWT Authorizer** - HTTP API only, native JWT validation, no Lambda
**Resource Policies** - IP whitelisting, VPC restrictions, cross-account

## Throttling

**Hierarchy**: Method-level > Stage-level > Account-level
**Default Account Limits**: 10,000 RPS, 5,000 burst
**Rate Limit**: Steady-state requests/second
**Burst Limit**: Token bucket for concurrent requests
**Throttled Response**: 429 Too Many Requests

## Caching

**TTL**: 0-3600 seconds
**Size**: 0.5GB to 237GB
**Cache Key**: Method + resource path + query strings + headers
**Invalidation**: Client sends `Cache-Control: max-age=0` header
**Best Practice**: Cache GET requests only, monitor hit/miss rates

## Usage Plans

- Combine API keys + throttle limits + quota limits
- Multiple plans per API (free, basic, premium)
- Quota: Daily/weekly/monthly request limits
- Used for monetization and client management

## Request/Response Transformation

**VTL (Velocity Template Language)** for mapping templates
**Context Variables**:
- `$context.requestId` - Unique request ID
- `$context.identity.sourceIp` - Client IP
- `$context.requestTime` - Timestamp
- `$context.authorizer.claims` - JWT claims
- `$input.path('$.field')` - Extract from JSON body
- `$input.params('paramName')` - Get parameters

## CORS

**Required Headers**:
- `Access-Control-Allow-Origin: *`
- `Access-Control-Allow-Methods: GET,POST,PUT,DELETE`
- `Access-Control-Allow-Headers: Content-Type,Authorization`

**Setup**: Enable CORS + Add OPTIONS method with mock integration
**HTTP API**: Simplified CORS configuration

## Stages & Deployment

**Stage** - Named reference to deployment (dev, staging, prod)
**Stage Variables** - Environment-specific values, use in Lambda aliases, endpoints
**Canary Deployment** - Route % traffic to new version, gradual rollout
**Best Practice**: Different AWS accounts for environments

## Monitoring

**Default Metrics**: Count, Latency, IntegrationLatency, 4XXError, 5XXError
**Logs**: Access logs (JSON format) + Execution logs
**X-Ray**: End-to-end tracing, service map, bottleneck identification
**Alarms**: Set on 4XX/5XX rates, p99 latency

## Security

**Layers**:
1. Resource policies (IP, VPC restrictions)
2. Authentication (IAM, Cognito, Lambda authorizer)
3. AWS WAF (SQL injection, XSS, rate limiting, geo-blocking)
4. TLS 1.2+ encryption
5. Secrets in Secrets Manager

**mTLS** - Client certificate authentication, requires custom domain
**Private API** - VPC endpoint only, no internet access

## Performance Optimization

- Use HTTP API (60% faster)
- Enable caching (cache hit = single-digit ms)
- Lambda provisioned concurrency (avoid cold starts)
- VPC Link for private integrations
- Regional endpoints (single region)
- Enable compression for large payloads
- Keep-alive connections to backend

## Cost Optimization

**Pricing (US East)**:
- REST API: $3.50/million requests
- HTTP API: $1.00/million requests (71% cheaper)
- WebSocket: $1.00/million messages + $0.25/million connection minutes

**Reduce Costs**:
- Use HTTP API instead of REST API
- Enable caching to reduce backend calls
- Direct AWS service integration (skip Lambda)
- Regional endpoints (not edge-optimized)
- Right-size cache capacity

## Common Patterns

**API Aggregation** - Single gateway → multiple backends
**BFF (Backend for Frontend)** - Separate APIs per client type
**Direct Service Integration** - API Gateway → DynamoDB/SQS (no Lambda)
**Event-Driven** - API Gateway → EventBridge → multiple consumers
**Saga Pattern** - API Gateway → Step Functions (orchestrate microservices)

## Limits & Quotas

**Hard Limits**:
- Payload size: 10MB
- Integration timeout: 29 seconds
- Header size: 10KB

**Adjustable**:
- Throttle rate: 10,000 RPS (default)
- Burst: 5,000 concurrent requests (default)
- Regional APIs: 600 per account
- Resources per API: 300

## Troubleshooting

**429 Too Many Requests** → Increase throttle limits, implement exponential backoff
**502 Bad Gateway** → Backend returned invalid response, check Lambda errors
**504 Gateway Timeout** → Integration timeout (>29s), use async patterns
**403 Forbidden** → Authorization failure, check IAM policies/authorizer
**CORS Errors** → Enable CORS, add OPTIONS method, verify headers
**High Latency** → Enable caching, optimize Lambda, check integration latency

## Endpoints

**Edge-Optimized** - CloudFront distribution, global users, higher cost
**Regional** - Same region as API, lower latency/cost for regional users
**Private** - VPC endpoint only, highest security

## VPC Link

**v1 (REST API)** - NLB only
**v2 (HTTP API)** - NLB, ALB, Cloud Map
**Use Case** - Secure private integration without internet exposure

## Custom Domains

- Requires ACM certificate
- Route 53 alias record
- Base path mapping for multiple APIs
- Regional or edge-optimized

## Request Validation

- Validate against JSON schema
- Check required parameters
- Return 400 errors early
- Reduce backend validation logic

## API Gateway Context

Available in mapping templates and authorizers:
- Request metadata: `$context.requestId`, `$context.httpMethod`
- Identity: `$context.identity.sourceIp`, `$context.identity.userAgent`
- Authorization: `$context.authorizer.*`
- Timing: `$context.requestTime`, `$context.requestTimeEpoch`
- Error: `$context.error.message`, `$context.error.messageString`

## Key Decisions

**REST vs HTTP API?**
- Need API keys, usage plans, request validation → REST
- Need lower cost and latency, simpler requirements → HTTP

**Integration Type?**
- Simple Lambda function → Lambda Proxy
- Need transformation → Lambda Custom or AWS Service Integration
- Legacy system → HTTP Custom with mapping templates
- Private resources → VPC Link

**Authentication?**
- Service-to-service → IAM
- User authentication → Cognito
- Custom logic → Lambda Authorizer
- Simple JWT → JWT Authorizer (HTTP API)

**Caching?**
- Read-heavy workloads → Yes
- Frequently changing data → No or low TTL
- Start with 0.5GB, scale based on hit rate

## Production Checklist

✓ Authentication/authorization configured  
✓ Throttling limits set  
✓ Caching enabled (if applicable)  
✓ CORS configured  
✓ AWS WAF enabled  
✓ CloudWatch logs + alarms  
✓ X-Ray tracing enabled  
✓ Custom domain with TLS 1.2+  
✓ Stage variables for environments  
✓ Request validation enabled  
✓ Canary deployment configured  
✓ Secrets in Secrets Manager  

## Interview Key Points

- Explain REST vs HTTP vs WebSocket use cases
- Throttling hierarchy: method > stage > account
- Lambda authorizer caching strategy
- CORS preflight workflow
- VPC Link for private integrations
- 29-second timeout limit and async solutions
- Cost optimization with HTTP API and caching
- Security layers: resource policies, auth, WAF, encryption
- Monitoring: CloudWatch metrics vs logs vs X-Ray
- Common errors and resolution (429, 502, 504, 403)
