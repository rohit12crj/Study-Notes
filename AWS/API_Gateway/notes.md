✅ Videos
- https://www.youtube.com/watch?v=jcibXVFiFek  --> Rahul

---
✅ Difference between POST , PUT & PATCH
- POST --> Create --> Used to create a new resource
- PUT --> Replace --> Used to replace an entire resource (or create if it doesn’t exist)
- PATCH --> Partial Update --> Used to update part of a resource

---
✅ API Gateway Throttling --> Used to Protect API Gateway
- youtube.com/watch?v=sCXejDwPGY4
- Rate Limit vs Burst Limit
- <img width="523" height="425" alt="image" src="https://github.com/user-attachments/assets/54badf55-4301-478c-8215-89d8d0f2ce07" />

---
✅ How will u secure API Gateway ?
- Authorizer ( 2 types --> Lambda & Cognito )
- API Keys

---
✅ Proxy vs Non Proxy Integration 
- In proxy integration, API Gateway forwards the full request to Lambda with minimal processing, whereas in non-proxy integration, API Gateway transforms both request and response using mapping templates, giving more control over the API contract.

---
✅ Validators ?

---
✅ What are the types of APIs in API Gateway?
- REST API – Full-featured, supports advanced features.
- HTTP API – Lightweight, cheaper, lower latency.
- WebSocket API – For real-time communication (chat apps, live dashboards).

---
✅ What is the difference between REST API and HTTP API?
- <img width="604" height="378" alt="image" src="https://github.com/user-attachments/assets/73bbc786-3cd1-4048-9efe-11083263ace0" />
- https://www.youtube.com/watch?v=O8RKpHQt6l4

---
4) What is a Stage in API Gateway?

A Stage is a named reference to a deployment.
Example:

dev

qa

prod

Each stage can have:

Stage variables

Logging

Throttling settings

5) What is Deployment in API Gateway?

A deployment is a snapshot of the API configuration.
You must deploy API changes to a stage for them to become active.


6) What authentication mechanisms are supported?

IAM authentication

Lambda Authorizer (Custom Authorizer)

Cognito User Pools

API Keys

JWT (for HTTP API)

8) What is a Lambda Authorizer?

A Lambda function that validates tokens (like JWT) and returns an IAM policy to allow/deny access.

9) What is a Resource Policy?

A JSON policy attached to API Gateway to control:

Which IPs can access

Which AWS accounts can invoke

10) What integration types are available?

Lambda integration

HTTP integration

AWS Service integration

Mock integration

VPC Link (for private resources)

11) What is VPC Link?

VPC Link allows API Gateway to connect to private resources inside a VPC (like NLB).

12) How does API Gateway integrate with Lambda?

Flow:
Client → API Gateway → Lambda → Response → API Gateway → Client

It can use:

Proxy integration (recommended)

Non-proxy integration

🔹 Monitoring & Logging
13) How do you monitor API Gateway?

CloudWatch Metrics

CloudWatch Logs

X-Ray tracing

Important metrics:

4XXError

5XXError

Latency

IntegrationLatency

14) How do you enable logging?

Enable execution logging

Enable access logging

Configure CloudWatch log group


15) What is throttling in API Gateway?

API Gateway limits requests using:

Rate limit (requests per second)

Burst limit

Helps prevent:

DDoS

Backend overload

16) What are usage plans?

Usage plans allow:

Rate limiting

Quotas

API key management


17) How does caching work in API Gateway?

Enable cache at stage level

Set TTL

Improves performance

Reduces backend calls

18) How to make API Gateway private?

Create Private API

Use VPC Endpoint (Interface endpoint)

Attach resource policy

19) How do you implement versioning?

Use stages (v1, v2)

Use base path mapping in custom domain

20) How would you design a production-ready API Gateway setup?

Best Practices:

Use custom domain

Enable WAF

Enable throttling

Enable logging

Use Cognito/IAM auth

Use caching

CI/CD deployment


21) Your API is returning 502 error. What will you check?

Lambda logs

Integration configuration

Timeout settings

VPC/NLB health

Mapping templates

22) How to reduce latency?

Use HTTP API

Enable caching

Reduce Lambda cold start

Use provisioned concurrency

23) How do you implement rate limiting per customer?

Create usage plan

Assign API key

Attach throttling rules

