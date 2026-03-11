WAF operates at layer 7 , Shield operates at layer 4 & 7

---

✅  AWS Shield is a managed DDoS (Distributed Denial of Service) protection service that safeguards applications running on AWS. It provides always-on detection and automatic inline mitigations to minimize application downtime and latency.

- Two tiers: Shield Standard ( Free ) and Shield Advanced
- Protects against network/transport layer (L3/L4) and application layer (L7) attacks
- Integrated with CloudFront, Route 53, ELB, EC2, Global Accelerator

---

✅  Types of DDoS Attacks AWS Shield Protects Against

Volumetric Attacks (L3)
- Flood the network with massive traffic volume
- Examples: UDP floods, ICMP floods, reflection attacks (DNS, NTP amplification)

Protocol Attacks (L4)
- Exploit weaknesses in layer 3/4 protocols
- Examples: SYN floods, Ping of Death, fragmented packet attacks

Application Layer Attacks (L7)
- Target the application itself with seemingly legitimate requests
- Examples: HTTP floods, Slowloris, DNS query floods
- Harder to detect — requires deep packet inspection

---



---
---
---

✅ Common WAF Rules Used:

- Rate-based rules (limit requests per IP)
- IP set rules (block known malicious IPs)
- Geo-match rules (block traffic from specific countries)
- Managed rule groups (AWS Managed Rules)

---

✅ What is AWS WAF?
- AWS WAF (Web Application Firewall) is managed firewall service
- Operates at Layer 7 (Application Layer)
- Allows you to allow, block, or count requests based on rules
- Does not protect against L3/L4 DDoS — use Shield for that

---

<img width="515" height="353" alt="image" src="https://github.com/user-attachments/assets/1ad7bdf1-2f15-42e8-b8e6-ae1056bc1f64" />

---

✅ Core Components

Web ACL (Access Control List)

- Top-level WAF resource
- Contains ordered list of rules and rule groups
- Associated with one or more AWS resources
- Regional (for ALB, API GW) or Global (for CloudFront — must be in us-east-1)
- Default action: Allow or Block all requests not matching any rule

Rules
- Define conditions to inspect requests

Each rule has:
- Statement — what to inspect
- Action — what to do (Allow, Block, Count, CAPTCHA, Challenge)
- Priority — lower number = evaluated first

Rule Groups
- Reusable collection of rules
  
Three types:
- Managed Rule Groups (AWS or Marketplace)
- Custom Rule Groups (you create)
- IP Sets / Regex Pattern Sets (referenced by rules)



🔹 IP Sets

A list of IP addresses or CIDR ranges
Used in rules to allow/block specific IPs
Supports IPv4 and IPv6

🔹 Regex Pattern Sets

A collection of regex patterns
Used to match string patterns in request components


4. Rule Statements (Conditions)
WAF inspects various parts of HTTP requests:
Match Statements:
StatementInspectsIP Set MatchSource IP addressGeo MatchCountry of originString MatchSpecific strings in request partsRegex MatchRegex patterns in request partsSize ConstraintSize of request componentsSQL Injection MatchSQL injection patternsXSS MatchCross-site scripting patterns
Request Components You Can Inspect:

URI path
Query string
HTTP method
HTTP headers (individual or all)
HTTP body (first 8 KB by default, up to 64 KB)
Cookies
JSON body (with JSON parsing)
Single query parameter / all query parameters

Logical Statements (combine conditions):

AND — all conditions must match
OR — any condition must match
NOT — negates a condition

Rate-Based Statement:

Count requests from an IP over a 5-minute window
Trigger action when threshold is exceeded
Minimum rate: 100 requests per 5 minutes
Can aggregate by: IP, forwarded IP, HTTP method, query argument, URI path, header

Managed Rule Group Statement:

Reference AWS Managed or Marketplace rule groups

Label Match Statement:

Match labels added by previous rules in the same Web ACL
Enables multi-step rule logic


5. Rule Actions
ActionDescriptionAllowForward request to protected resourceBlockDrop request; return 403 by defaultCountCount the request but do not block (monitoring mode)CAPTCHAPresent CAPTCHA challenge to userChallengeSilent browser challenge (JavaScript-based)

Count is used for testing rules before enforcing them
CAPTCHA tokens are valid for a configurable duration (default: 300 seconds)
Challenge is silent — user doesn't see anything; bot detection via JS


6. AWS Managed Rule Groups
Pre-built rule groups maintained by AWS. Free with Shield Advanced; otherwise charged per rule group.
Core Rule Sets:
Rule GroupDescriptionAWSManagedRulesCommonRuleSetOWASP Top 10 — SQLi, XSS, LFI, RFI, etc.AWSManagedRulesAdminProtectionRuleSetProtects admin pagesAWSManagedRulesKnownBadInputsRuleSetBlocks known malicious inputs (Log4j, SSRF, etc.)AWSManagedRulesSQLiRuleSetAdvanced SQL injection protectionAWSManagedRulesLinuxRuleSetLinux-specific attacks (LFI, command injection)AWSManagedRulesUnixRuleSetUnix-specific attacksAWSManagedRulesWindowsRuleSetWindows-specific attacks (PowerShell injection)AWSManagedRulesPHPRuleSetPHP-specific vulnerabilitiesAWSManagedRulesWordPressRuleSetWordPress-specific attacksAWSManagedRulesAmazonIpReputationListBlocks known malicious AWS IPs, botsAWSManagedRulesAnonymousIpListBlocks VPNs, proxies, Tor exit nodesAWSManagedRulesBotControlRuleSetBot detection and managementAWSManagedRulesACFPRuleSetAccount creation fraud preventionAWSManagedRulesATPRuleSetAccount takeover prevention (credential stuffing)

7. AWS WAF Bot Control

Managed rule group specifically for bot traffic management
Two levels:

Common Bot Control — blocks common bots (scrapers, scanners, crawlers)
Targeted Bot Control — advanced detection (browser fingerprinting, behavioral analysis)


Can allow good bots (Googlebot, Bingbot) while blocking bad ones
Uses CAPTCHA and Challenge actions for suspicious bots


8. Account Takeover Prevention (ATP)

Monitors login endpoints for credential stuffing attacks
Detects: high volume of failed logins, stolen credential use, suspicious login patterns
Integrates with Cognito for native protection
Can add CAPTCHA challenges on suspicious login attempts
Tracks stolen credentials against breached credential databases


9. Account Creation Fraud Prevention (ACFP)

Monitors account registration endpoints
Detects fake account creation, bot signups, and fraud patterns
Inspects registration request components (email, phone, address)
Works with Cognito user pool sign-up flows


10. Labels and Label Namespaces

Rules can add labels to requests for downstream rules to inspect
Labels persist within the same Web ACL evaluation
Format: namespace:label (e.g., awswaf:managed:aws:bot-control:bot:verified)
Enables complex multi-rule logic without nested conditions
AWS Managed Rules automatically add labels you can match on


11. Rule Priority and Evaluation Order

Rules evaluated in ascending priority order (lowest number first)
First terminating action (Allow/Block/CAPTCHA/Challenge) stops evaluation
Count is non-terminating — evaluation continues
Web ACL default action applies if no rule matches
Rule groups are evaluated as a single unit at their priority level


12. WAF Capacity Units (WCU)

WAF uses WCU to measure rule complexity
Web ACL limit: 1,500 WCU (can be increased)
Different statements cost different WCUs:

Simple string match: 1 WCU
Regex: 3 WCU
SQL injection: 20 WCU
XSS: 40 WCU
Managed rule groups: each has a fixed WCU cost


Helps AWS manage performance at scale


13. Logging and Monitoring
WAF Logs

Full request logs sent to:

Amazon CloudWatch Logs
Amazon S3
Amazon Kinesis Data Firehose (then to S3, Redshift, OpenSearch)


Log includes: timestamp, action, matched rule, request details
Enable log redaction for sensitive fields (e.g., Authorization header)
Log all requests or only sampled requests

CloudWatch Metrics
MetricDescriptionAllowedRequestsCount of allowed requestsBlockedRequestsCount of blocked requestsCountedRequestsCount of counted requestsPassedRequestsRequests that passed rule group without match
WAF Sampled Requests

View up to 100 sample requests per rule in the console
Useful for debugging and rule tuning
Available for last 3 hours


14. WAF Pricing
ComponentCostWeb ACL$5/month per Web ACLRule$1/month per ruleRequests$0.60 per million requestsManaged Rule GroupVaries (AWS free with Shield Advanced)Bot Control (Common)$10/month + $1/million requestsBot Control (Targeted)$30/month + $1.50/million requestsATP Rule Group$20/month + $1/million login requestsACFP Rule Group$10/month + $0.50/million signup requests

15. WAF with AWS Firewall Manager

Centrally manage WAF policies across all accounts in AWS Organization
Automatically apply WAF rules to new resources as they're created
Enforce mandatory rules that account owners cannot remove
View compliance status across all accounts from a single dashboard
Requires: AWS Organizations + designated Firewall Manager admin account


16. WAF with Shield Advanced

Shield Advanced includes WAF at no additional cost for protected resources
SRT (Shield Response Team) can automatically create WAF rules during DDoS attacks
WAF handles L7 DDoS mitigation as part of Shield Advanced protection
Common auto-created rules during attacks: rate-based rules, IP reputation blocks


17. WAF with CloudFront

Web ACL must be created in us-east-1 for CloudFront (global scope)
Rules applied at 400+ edge locations globally
Can use geo-blocking to restrict access by country
Combine with Lambda@Edge for advanced request manipulation before WAF


18. Oversize Body Handling

WAF inspects the first 8 KB of request body by default (up to 64 KB configurable)
For bodies larger than the limit, you can configure:

Continue — inspect available portion, apply rules
Match — treat as matching (block if in blocking rule)
No match — treat as not matching (pass through)


JSON body parsing also applies size limits


19. CAPTCHA and Challenge Tokens

When WAF applies CAPTCHA/Challenge, it issues a token on success
Token stored as a cookie in the browser
Subsequent requests with valid token bypass CAPTCHA/Challenge
Token immunity time: configurable (default 300 sec for CAPTCHA, 300 sec for Challenge)
Challenge is silent (JS puzzle); CAPTCHA is visible (image puzzle)


20. Common Attack Protections
AttackWAF FeatureSQL InjectionSQLi match statement / AWSManagedRulesSQLiRuleSetXSSXSS match statement / AWSManagedRulesCommonRuleSetLog4ShellAWSManagedRulesKnownBadInputsRuleSetSSRFAWSManagedRulesKnownBadInputsRuleSetCredential StuffingATP Managed Rule GroupBot ScrapingBot Control Managed Rule GroupDDoS (L7)Rate-based rules + Shield AdvancedGeo RestrictionGeo match statementIP BlockingIP Set match statement

21. WAF Deployment Best Practices

Start rules in Count mode → analyze logs → switch to Block mode
Use AWS Managed Rules as baseline, add custom rules on top
Enable WAF logging to S3/CloudWatch for full visibility
Use rate-based rules to throttle abusive IPs
Set default action to Allow, let rules handle blocking (or Block all, allow selectively for restrictive apps)
Use Firewall Manager for org-wide consistent enforcement
Regularly review sampled requests and tune rules to reduce false positives
Enable Bot Control for publicly exposed APIs and web apps
Use labels for complex multi-step detection logic


22. Exam Tips / Gotchas

🔴 WAF is L7 only — does not protect against L3/L4 attacks (use Shield).
🔴 CloudFront WAF Web ACL must be in us-east-1 — common exam trap.
🔴 WAF cannot attach to NLB — only ALB, CloudFront, API GW, AppSync, Cognito.
🔴 Count action is non-terminating — evaluation continues to next rule.
🔴 Rate-based rules use a 5-minute sliding window.
🔴 Body inspection limited to 8 KB by default (max 64 KB).
🔴 Shield Advanced includes WAF free for protected resources.
🔴 Firewall Manager requires AWS Organizations to be enabled.
🔴 Geo-blocking is done via WAF, not Security Groups or NACLs.
🔴 Managed Rules can be overridden — set individual rules to Count to reduce false positives.
🔴 CAPTCHA = visible puzzle; Challenge = silent JS check.
🔴 ATP protects login pages; ACFP protects signup pages — don't mix them up.
🔴 WCU limit is 1,500 per Web ACL — complex rule sets can hit this limit.


That covers the full AWS WAF topic space. Let me know if you want combined notes across WAF + Shield + VPC for a full security revision session!

---


✅ Scenario 1: DDoS Protection

Your e-commerce site is getting flooded with HTTP requests from thousands of IPs during a flash sale. How would you use AWS WAF to mitigate this?


- Create a **Rate-Based Rule** in WAF to limit requests per IP (e.g., 2000 requests per 5 minutes)
- Combine WAF with AWS Shield Advanced for volumetric DDoS protection
- Use IP reputation managed rule groups (AWS Managed Rules) to auto-block known bad actors
- Front the application with CloudFront + WAF so traffic is filtered at the edge before hitting origin
- Enable WAF logging to Kinesis Firehose → S3 for post-incident analysis



### ✅ Scenario 2: SQL Injection Attack

Your RDS-backed application is being targeted with SQL injection attempts via form fields. How do you stop this with WAF?


- Attach a WAF Web ACL to the ALB in front of your app
- Enable the AWS Managed Rule Group: AWSManagedRulesSQLiRuleSet — it covers UNION-based, blind SQLi, etc.
- Add a custom rule to inspect query strings, body, and URI for SQLi patterns
- Set the rule action to Block and enable Amazon CloudWatch metrics for alerting
- Use WAF sampled requests to validate what's being caught before switching from Count → Block


### ✅ Scenario 3: Geo-Blocking

Your SaaS application is only licensed for use in the US and Canada. How do you restrict access geographically?

- In WAF Web ACL, create a Geographic Match Rule
- Set condition: if country is NOT in [US, CA] → Block
- Attach the Web ACL to CloudFront distribution for global enforcement at the edge
- Return a custom 403 response body explaining regional restrictions
- For B2B clients with known IPs outside US/CA, add an IP Set allow rule with higher priority to override the geo-block


### ✅ Scenario 4: Bot Management

You notice scrapers are harvesting product prices from your site, impacting performance. How do you handle this?

- Enable AWS WAF Bot Control managed rule group (targeted mode for advanced bot detection)
- It uses browser fingerprinting, behavioral analysis, and CAPTCHA challenges
- Create rules to allow legitimate bots (Googlebot, etc.) via IP Set or label matching
- Use CAPTCHA action on suspicious user agents instead of hard blocking — reduces false positives
- For pricing pages specifically, scope the rule to URI path /products/* to minimize latency impact elsewhere


### ✅ Scenario 5: Rule Priority Conflict

You have an allow rule for your office IP and a block rule for a country that includes your office IP's country. Users in the office are getting blocked. What's wrong?

- This is a rule priority issue. WAF evaluates rules top-down by priority number (lower number = evaluated first)
- The block rule has higher priority (lower number) than the allow rule, so it fires first
- Fix: Move the IP allow rule to a lower priority number (e.g., priority 1) so it's evaluated before the geo-block rule (e.g., priority 10)
- Best practice: Always put explicit allow rules at the top, deny/block rules below


### ✅ Scenario 6: False Positives

After enabling AWS Managed Rules, some legitimate API requests from your mobile app are being blocked. How do you troubleshoot?

- Switch the managed rule group action to Count mode (override to Count) temporarily
- Enable WAF full logging and send to CloudWatch Logs or S3
- Analyze logs to find which rule label is matching your legitimate traffic (e.g., awswaf:managed:aws:core-rule-set:CrossSiteScripting_Body)
- Use rule group override to set that specific rule to Count or exempt it
- Alternatively, create a scope-down statement so the rule only applies to web traffic, excluding your API path /api/v1/*


### ✅ Scenario 7: Multi-Region Architecture

Your app runs in us-east-1 and eu-west-1 behind separate ALBs. How do you manage WAF consistently across both?


- WAF Web ACLs are regional (for ALB/API GW) — you must create one per region
- Use AWS Firewall Manager to centrally manage WAF policies across accounts and regions
- Define a Firewall Manager WAF policy and deploy it to both regions automatically
- Combine with AWS Organizations for multi-account enforcement
- For global enforcement with a single ACL, migrate traffic to CloudFront (which uses a global WAF ACL)


### ✅ Scenario 8: API Gateway Protection

Your public REST API is being abused — attackers are fuzzing endpoints with malformed JSON payloads. How do you protect it?

- Attach a WAF Web ACL to the API Gateway stage
- Enable AWSManagedRulesCommonRuleSet which includes size restrictions and malformed body rules
- Add a custom rule with a size constraint on the body (e.g., block payloads > 10KB if not expected)
- Use a regex pattern set to block known fuzzing strings in headers/body
- Enable API Gateway throttling in addition to WAF for defense-in-depth


### ✅ Scenario 9: Cost Optimization

Your WAF bill spiked due to high request volume. How do you optimize costs?

- WAF charges per Web ACL ($5/mo), rule ($1/rule/mo), and per million requests ($0.60)
- Consolidate rules into rule groups to reduce rule count
- Move high-traffic, low-risk paths (e.g., /static/*) to a scope-down statement so WAF rules don't evaluate them
- Use CloudFront in front — cache hits don't reach WAF inspection
- Review if Bot Control (premium) is needed everywhere or just on sensitive paths
- Use WAF logging sampling instead of full logging to reduce Firehose/S3 costs


### ✅ Scenario 10: Zero-Day Vulnerability

A critical zero-day vulnerability is disclosed for a framework your app uses. You need a temporary fix before patching. What do you do?

- This is the virtual patching use case for WAF
- Quickly create a custom WAF rule that matches the specific exploit pattern (URI, header, or payload signature from the CVE)
- Set action to Block and deploy immediately — no application change needed
- Subscribe to AWS Threat Intelligence feeds and check if managed rules already cover the CVE
- Monitor via CloudWatch metrics + SNS alerts for rule hit rates
- Once the actual patch is deployed and tested, remove the virtual patch rule to avoid rule bloat


### ✅ Key concepts to always mention: Web ACL, Rule priority, Managed Rule Groups, Scope-down statements, Count vs Block mode, Firewall Manager for multi-account, and WAF logging for observability.

### ✅ in AWS WAF Default statement can be either block or allow

### ✅ what are Scope-down statements in aws waf

Instead of applying a rule to all incoming requests, you use a scope-down statement to apply the rule only to specific requests.

<img width="551" height="266" alt="image" src="https://github.com/user-attachments/assets/3d096d85-6d2e-4137-b9a2-7e77b25a8e9c" />

