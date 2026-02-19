4XX – Client Side Errors
🔴 400 – Bad Request

➡ Malformed request (invalid JSON, headers missing)

🔐 401 – Unauthorized

➡ Authentication failed (invalid/expired token)

⛔ 403 – Forbidden

➡ Access blocked

Very common in WAF (rule triggered, geo block, IP block)

❓ 404 – Not Found

➡ Wrong URL / route not configured

🚦 405 – Method Not Allowed

➡ HTTP method not supported (POST vs GET mismatch)

🚫 408 – Request Timeout

➡ Client didn’t send request in time

🚨 429 – Too Many Requests

➡ Rate limit exceeded (WAF or API throttling)


5XX – Server Side Errors
💥 500 – Internal Server Error

➡ Application crash / unhandled exception

🔄 502 – Bad Gateway

➡ Backend returned invalid response (ALB ↔ app issue)

🛑 503 – Service Unavailable

➡ No healthy targets / overloaded system

⏳ 504 – Gateway Timeout

➡ Backend too slow / timeout exceeded

How do you differentiate between WAF 403 vs Backend 403?

Scenario 1: You notice a spike in traffic — how do you identify and block malicious IPs using WAF?
Enable AWS WAF logging to CloudWatch or S3, analyze the logs to identify suspicious IPs or patterns, then create an IP set rule in WAF to block those IPs. You can also use AWS WAF Bot Control or rate-based rules to automatically block IPs exceeding a threshold (e.g., 2000 requests per 5 minutes).

Scenario 2: Your application is facing an SQL injection attack. How do you protect it?
Enable AWS Managed Rules — specifically the SQL database rule group — in your WAF Web ACL. This automatically inspects query strings, body, and headers for SQL injection patterns. Additionally, enable logging and set the rule action to Block rather than Count.

Scenario 3: You need to allow access only from specific countries. How do you do this in WAF?
Use a Geographic match rule in WAF. Create a rule that matches requests from allowed countries and set non-matching requests to Block. This is done at the Web ACL level and works with CloudFront, ALB, or API Gateway.

Scenario 4: How would you rate-limit an API to prevent abuse?
Create a rate-based rule in WAF. Set the request threshold (e.g., 1000 requests per 5 minutes per IP). WAF automatically tracks request counts and blocks IPs that exceed the limit. You can also scope the rule to specific URIs like /api/login to protect login endpoints.

Scenario 5: How do you protect against bot traffic?
Use AWS WAF Bot Control (managed rule group). It classifies bots as common or targeted. For common bots, you can block or CAPTCHA-challenge them. Combine with CAPTCHA action on suspicious rules to challenge users before allowing access.

Scenario 6: A legitimate user is being blocked by WAF. How do you troubleshoot?
Check WAF logs (CloudWatch Logs Insights or Athena on S3) and filter by the user's IP or request URI. Look at the terminatingRuleId to find which rule blocked the request. Then either tune the rule, add an IP set exception, or use a label match exception to allow that traffic.

Scenario 7: How do you apply different WAF rules to different environments (dev/prod)?
Create separate Web ACLs per environment. In prod, use Block mode; in dev/staging, use Count mode so rules log without blocking. Associate each Web ACL with its respective ALB, CloudFront, or API Gateway.

Scenario 8: How do you handle a zero-day vulnerability quickly?
AWS Managed Rules are updated automatically by AWS — no action needed on your part. For custom threats, you can create a custom rule quickly with a string match condition. AWS also offers WAF Security Automations (via AWS Solutions) that auto-update rules based on threat intelligence.

Scenario 9: Your application uses API Gateway — how do you attach WAF to it?
Create a Web ACL in WAF, then associate it with the API Gateway stage. In the API Gateway console, go to the stage settings and enable WAF, or use the WAF console to associate the Web ACL with the API Gateway ARN.

Scenario 10: How do you ensure WAF rules don't impact performance?
WAF rules are evaluated in priority order — put the most frequently matched (allow/deny) rules first to short-circuit evaluation. Use managed rules efficiently, avoid overly broad regex patterns, and monitor latency using CloudWatch metrics like AllowedRequests and BlockedRequests.

Bonus: WAF Capacity Units (WCUs)
Each Web ACL has a 5000 WCU limit. Managed rule groups consume a defined number of WCUs. Complex regex or size constraint rules use more WCUs. Always monitor WCU usage to avoid hitting limits.

