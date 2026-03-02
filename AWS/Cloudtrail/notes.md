Beginner Scenarios
Q1. Someone deleted an S3 bucket in your AWS account. How do you find out who did it?
Answer: Use AWS CloudTrail:

CloudTrail logs all API calls made in your AWS account
Go to CloudTrail → Event History in the console
Filter by:

Event name: DeleteBucket
Resource type: S3
Time range: when the deletion occurred


The event record shows:

Who — IAM user/role ARN
When — exact timestamp
Where from — source IP address
What — bucket name, request parameters


For older events (>90 days), query CloudTrail Lake or check S3 logs if a trail was configured


Q2. What is CloudTrail and what does it record by default?
Answer: AWS CloudTrail is a governance, compliance, and auditing service that records AWS API activity.
By default (free, 90-day retention):

Management events — control plane operations (CreateEC2Instance, DeleteS3Bucket, AttachPolicy)
Available in Event History in the console

Not recorded by default (requires trail configuration):

Data events — S3 object-level (GetObject, PutObject), Lambda invocations, DynamoDB item-level
Insights events — detects unusual API activity patterns
Network activity events — VPC endpoint calls (newer feature)

Trail types:

Single-region trail — logs events in one region
Multi-region trail — logs all regions (recommended)
Logs delivered to S3 bucket, optionally to CloudWatch Logs and EventBridge


Q3. How do you ensure CloudTrail is enabled across all AWS regions and all future regions?
Answer: Create a multi-region trail:
bashaws cloudtrail create-trail \
  --name my-global-trail \
  --s3-bucket-name my-cloudtrail-bucket \
  --is-multi-region-trail \
  --include-global-service-events

--is-multi-region-trail — captures events from all current and future regions automatically
--include-global-service-events — includes IAM, STS, CloudFront (global services)
Enable using AWS Organizations trail to cover all accounts automatically
Use AWS Config rule cloud-trail-enabled to continuously check compliance


🟠 Intermediate Scenarios
Q4. You suspect an IAM user's credentials were compromised. How do you investigate using CloudTrail?
Answer: Step-by-step investigation:

Identify the user — check IAM Access Advisor, recent logins
Query CloudTrail for the user's activity:

Filter by userIdentity.userName = "compromised-user"
Look for unusual API calls, new IAM users created, policy changes


Check for lateral movement:

CreateUser, AttachUserPolicy, CreateAccessKey — privilege escalation
AssumeRole — moving to other roles
RunInstances, CreateBucket — resource creation (crypto mining, data exfil)


Check source IPs — unusual geographies or IPs
Use CloudTrail Lake or Athena for complex queries across large log volumes
Immediate response: disable access key, revoke sessions using aws iam delete-login-profile


Q5. How do you get real-time alerts when someone makes a change to a security group?
Answer: Use CloudTrail + CloudWatch Logs + Metric Filter + SNS:

Enable CloudTrail and send logs to CloudWatch Logs
Create a Metric Filter on the log group:

json{ ($.eventName = AuthorizeSecurityGroupIngress) || 
  ($.eventName = RevokeSecurityGroupIngress) ||
  ($.eventName = CreateSecurityGroup) ||
  ($.eventName = DeleteSecurityGroup) }

Create a CloudWatch Alarm on the metric (threshold ≥ 1)
Alarm triggers SNS topic → sends email/SMS/PagerDuty alert

Alternative (simpler): Use AWS Config rules + EventBridge:

EventBridge rule: source = aws.ec2, eventName = AuthorizeSecurityGroupIngress
Target: SNS, Lambda, or Slack webhook


Q6. Your CloudTrail logs are stored in S3. How do you query them efficiently?
Answer: Two approaches:
Option 1 — Amazon Athena:

Use CloudTrail's built-in "Create Athena table" feature
Queries CloudTrail logs directly in S3 using SQL:

sqlSELECT eventtime, eventname, useridentity.arn, sourceipaddress
FROM cloudtrail_logs
WHERE eventname = 'DeleteBucket'
  AND eventtime > '2024-01-01'
ORDER BY eventtime DESC;

Cost-effective — pay per query, no infrastructure

Option 2 — CloudTrail Lake:

Managed data lake purpose-built for CloudTrail
Retains events up to 7 years
Query using CloudTrail Lake SQL — no S3/Athena setup needed
Faster queries, better for security investigations
Supports cross-account event aggregation


Q7. How do you ensure CloudTrail logs haven't been tampered with?
Answer: Enable Log File Integrity Validation:
bashaws cloudtrail create-trail \
  --name secure-trail \
  --s3-bucket-name my-bucket \
  --enable-log-file-validation

CloudTrail creates a digest file every hour in S3
Digest contains SHA-256 hashes of all log files delivered in that period
Digest itself is signed using CloudTrail's private key
Validate using CLI:

bashaws cloudtrail validate-logs \
  --trail-arn arn:aws:cloudtrail:... \
  --start-time 2024-01-01

Returns whether each log file is valid, invalid, or deleted
Invalid/deleted file = tampered — triggers incident response


🔴 Advanced Scenarios
Q8. How do you centralize CloudTrail logs from 50 AWS accounts into one place?
Answer: Use AWS Organizations Trail:

Create a trail in the management (master) account with Organizations integration
Automatically applies to all member accounts — current and future
All logs delivered to a central S3 bucket in the security/logging account

bashaws cloudtrail create-trail \
  --name org-trail \
  --s3-bucket-name central-security-logs \
  --is-organization-trail \
  --is-multi-region-trail
```

**Best practice architecture:**
```
All AWS Accounts → CloudTrail → Central S3 (Log Archive Account)
                                      ↓
                              Athena / CloudTrail Lake
                                      ↓
                              Security Dashboard (QuickSight / SIEM)

Use S3 bucket policy to allow only CloudTrail service to write
Enable S3 Object Lock (WORM) to prevent log deletion
Send to CloudWatch Logs in security account for real-time alerting


Q9. How do you detect unusual or suspicious API activity automatically using CloudTrail?
Answer: Use CloudTrail Insights:

Automatically detects unusual write API activity patterns
Compares current activity against a 7-day baseline
Generates an Insights event when anomaly detected (e.g., spike in TerminateInstances calls)
Enable on a trail:

bashaws cloudtrail put-insight-selectors \
  --trail-name my-trail \
  --insight-selectors '[{"InsightType":"ApiCallRateInsight"},{"InsightType":"ApiErrorRateInsight"}]'
Combined security monitoring stack:

CloudTrail → raw API logs
CloudTrail Insights → anomaly detection
Amazon GuardDuty → threat detection (uses CloudTrail as a source)
AWS Security Hub → aggregates findings
EventBridge → routes alerts to SNS, Lambda, PagerDuty


Q10. A developer accidentally ran aws ec2 terminate-instances and killed production servers. How do you prevent this in the future using CloudTrail-based controls?
Answer: CloudTrail helps with detection, but prevention needs additional controls:
Prevention:

IAM policies — deny ec2:TerminateInstances on production instances using resource tags:

json{
  "Effect": "Deny",
  "Action": "ec2:TerminateInstances",
  "Resource": "*",
  "Condition": {
    "StringEquals": {"ec2:ResourceTag/Environment": "prod"}
  }
}

EC2 Termination Protection — enable on all production instances
Service Control Policies (SCPs) — restrict terminate at the org level for prod accounts
AWS Config rule — flag instances without termination protection

Detection & Response (CloudTrail-based):

EventBridge rule on TerminateInstances event → Lambda triggers SNS alert immediately
CloudTrail log shows who ran it, from which IP, at what time
Automated remediation — Lambda re-launches instance from AMI snapshot if tagged as prod


Q11. How do you use CloudTrail to audit S3 data access (who accessed which objects)?
Answer: Enable S3 Data Events in CloudTrail:
bashaws cloudtrail put-event-selectors \
  --trail-name my-trail \
  --event-selectors '[{
    "ReadWriteType": "All",
    "DataResources": [{
      "Type": "AWS::S3::Object",
      "Values": ["arn:aws:s3:::sensitive-bucket/"]
    }]
  }]'

Records: GetObject, PutObject, DeleteObject, HeadObject
Each log entry shows: who, what object, from which IP, when
Query with Athena for access reports:

sqlSELECT useridentity.arn, requestparameters, eventtime
FROM cloudtrail_logs
WHERE eventsource = 's3.amazonaws.com'
  AND requestparameters LIKE '%sensitive-bucket%'
  AND eventname = 'GetObject'
ORDER BY eventtime DESC;

Warning: Data events generate high volume — can significantly increase costs
Be selective — enable only for sensitive buckets


Q12. CloudTrail logs show API calls from an unknown IP. How do you respond?
Answer: Incident Response Steps:

Identify the scope — query CloudTrail for all actions from that IP:

sqlSELECT eventtime, eventname, useridentity.arn, requestparameters
FROM cloudtrail_logs
WHERE sourceipaddress = '1.2.3.4'
ORDER BY eventtime;

Assess the damage — look for:

New IAM users/keys created
Data accessed or exfiltrated (S3 GetObject)
Resources created (EC2, Lambda — possible crypto mining)
Permission escalations


Contain immediately:

Disable/delete compromised access keys
Revoke all active sessions: aws iam delete-login-profile
Attach an explicit Deny all policy to the compromised user/role
Block the IP in WAF / Security Group / NACL


Remediate:

Delete unauthorized resources
Rotate all credentials
Review and tighten IAM policies


Post-incident:

Enable GuardDuty for ongoing threat detection
Set up CloudTrail Insights for anomaly detection
Enable MFA on all IAM users

<img width="630" height="397" alt="image" src="https://github.com/user-attachments/assets/a9fbb669-59b9-476a-9e81-0898d921d492" />

