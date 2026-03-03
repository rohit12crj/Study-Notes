✅ Scenario 1: DDoS Protection

Your e-commerce site is getting flooded with HTTP requests from thousands of IPs during a flash sale. How would you use AWS WAF to mitigate this?


- Create a **Rate-Based Rule** in WAF to limit requests per IP (e.g., 2000 requests per 5 minutes)
- Combine WAF with AWS Shield Advanced for volumetric DDoS protection
- Use IP reputation managed rule groups (AWS Managed Rules) to auto-block known bad actors
- Front the application with CloudFront + WAF so traffic is filtered at the edge before hitting origin
- Enable WAF logging to Kinesis Firehose → S3 for post-incident analysis



✅ Scenario 2: SQL Injection Attack

Your RDS-backed application is being targeted with SQL injection attempts via form fields. How do you stop this with WAF?


- Attach a WAF Web ACL to the ALB in front of your app
- Enable the AWS Managed Rule Group: AWSManagedRulesSQLiRuleSet — it covers UNION-based, blind SQLi, etc.
- Add a custom rule to inspect query strings, body, and URI for SQLi patterns
- Set the rule action to Block and enable Amazon CloudWatch metrics for alerting
- Use WAF sampled requests to validate what's being caught before switching from Count → Block


Scenario 3: Geo-Blocking

Your SaaS application is only licensed for use in the US and Canada. How do you restrict access geographically?

- In WAF Web ACL, create a Geographic Match Rule
- Set condition: if country is NOT in [US, CA] → Block
- Attach the Web ACL to CloudFront distribution for global enforcement at the edge
- Return a custom 403 response body explaining regional restrictions
- For B2B clients with known IPs outside US/CA, add an IP Set allow rule with higher priority to override the geo-block


Scenario 4: Bot Management

You notice scrapers are harvesting product prices from your site, impacting performance. How do you handle this?

- Enable AWS WAF Bot Control managed rule group (targeted mode for advanced bot detection)
- It uses browser fingerprinting, behavioral analysis, and CAPTCHA challenges
- Create rules to allow legitimate bots (Googlebot, etc.) via IP Set or label matching
- Use CAPTCHA action on suspicious user agents instead of hard blocking — reduces false positives
- For pricing pages specifically, scope the rule to URI path /products/* to minimize latency impact elsewhere


Scenario 5: Rule Priority Conflict

You have an allow rule for your office IP and a block rule for a country that includes your office IP's country. Users in the office are getting blocked. What's wrong?

- This is a rule priority issue. WAF evaluates rules top-down by priority number (lower number = evaluated first)
- The block rule has higher priority (lower number) than the allow rule, so it fires first
- Fix: Move the IP allow rule to a lower priority number (e.g., priority 1) so it's evaluated before the geo-block rule (e.g., priority 10)
- Best practice: Always put explicit allow rules at the top, deny/block rules below


Scenario 6: False Positives

After enabling AWS Managed Rules, some legitimate API requests from your mobile app are being blocked. How do you troubleshoot?

- Switch the managed rule group action to Count mode (override to Count) temporarily
- Enable WAF full logging and send to CloudWatch Logs or S3
- Analyze logs to find which rule label is matching your legitimate traffic (e.g., awswaf:managed:aws:core-rule-set:CrossSiteScripting_Body)
- Use rule group override to set that specific rule to Count or exempt it
- Alternatively, create a scope-down statement so the rule only applies to web traffic, excluding your API path /api/v1/*


Scenario 7: Multi-Region Architecture

Your app runs in us-east-1 and eu-west-1 behind separate ALBs. How do you manage WAF consistently across both?


- WAF Web ACLs are regional (for ALB/API GW) — you must create one per region
- Use AWS Firewall Manager to centrally manage WAF policies across accounts and regions
- Define a Firewall Manager WAF policy and deploy it to both regions automatically
- Combine with AWS Organizations for multi-account enforcement
- For global enforcement with a single ACL, migrate traffic to CloudFront (which uses a global WAF ACL)


Scenario 8: API Gateway Protection

Your public REST API is being abused — attackers are fuzzing endpoints with malformed JSON payloads. How do you protect it?

- Attach a WAF Web ACL to the API Gateway stage
- Enable AWSManagedRulesCommonRuleSet which includes size restrictions and malformed body rules
- Add a custom rule with a size constraint on the body (e.g., block payloads > 10KB if not expected)
- Use a regex pattern set to block known fuzzing strings in headers/body
- Enable API Gateway throttling in addition to WAF for defense-in-depth


Scenario 9: Cost Optimization

Your WAF bill spiked due to high request volume. How do you optimize costs?

- WAF charges per Web ACL ($5/mo), rule ($1/rule/mo), and per million requests ($0.60)
- Consolidate rules into rule groups to reduce rule count
- Move high-traffic, low-risk paths (e.g., /static/*) to a scope-down statement so WAF rules don't evaluate them
- Use CloudFront in front — cache hits don't reach WAF inspection
- Review if Bot Control (premium) is needed everywhere or just on sensitive paths
- Use WAF logging sampling instead of full logging to reduce Firehose/S3 costs


Scenario 10: Zero-Day Vulnerability

A critical zero-day vulnerability is disclosed for a framework your app uses. You need a temporary fix before patching. What do you do?

- This is the virtual patching use case for WAF
- Quickly create a custom WAF rule that matches the specific exploit pattern (URI, header, or payload signature from the CVE)
- Set action to Block and deploy immediately — no application change needed
- Subscribe to AWS Threat Intelligence feeds and check if managed rules already cover the CVE
- Monitor via CloudWatch metrics + SNS alerts for rule hit rates
- Once the actual patch is deployed and tested, remove the virtual patch rule to avoid rule bloat


Key concepts to always mention: Web ACL, Rule priority, Managed Rule Groups, Scope-down statements, Count vs Block mode, Firewall Manager for multi-account, and WAF logging for observability.

what are Scope-down statements in aws waf

Instead of applying a rule to all incoming requests, you use a scope-down statement to apply the rule only to specific requests.

Suppose you create a rate-based rule (2000 requests per 5 min) but you only want it to apply to:

/login

/api/payment

Instead of applying rate limiting to all traffic, you:

Create a rate-based rule

Add a scope-down statement

Define condition:

URI path equals /login

Now the rate limit applies only to login requests, not the entire application.
