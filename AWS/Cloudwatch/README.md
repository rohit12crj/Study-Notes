How to get email notifications if errors are there in cloudwatch logs 

Your application writes logs to Amazon CloudWatch Logs

A Metric Filter scans log entries for patterns like ERROR, Exception, 5xx

Matching logs increment a custom CloudWatch metric

A CloudWatch Alarm monitors that metric

Alarm sends notification via Amazon SNS (email, SMS, Slack, etc.)


Beginner Scenarios
Q1. Your EC2 instance CPU is spiking to 100% randomly. How do you detect and alert on this?
Answer: Use CloudWatch Metrics + Alarms:

EC2 automatically publishes CPUUtilization metric to CloudWatch every 1 minute (with detailed monitoring) or 5 minutes (basic)
Create a CloudWatch Alarm:

bashaws cloudwatch put-metric-alarm \
  --alarm-name high-cpu \
  --metric-name CPUUtilization \
  --namespace AWS/EC2 \
  --dimensions Name=InstanceId,Value=i-1234567890 \
  --statistic Average \
  --period 300 \
  --threshold 80 \
  --comparison-operator GreaterThanThreshold \
  --evaluation-periods 2 \
  --alarm-actions arn:aws:sns:us-east-1:123:my-topic

Alarm triggers SNS → email/SMS/PagerDuty
Enable detailed monitoring on EC2 for 1-minute granularity
Also set up alarms for MemoryUtilization, DiskReadOps using CloudWatch Agent


Q2. Your application logs are on EC2. How do you centralize and monitor them in CloudWatch?
Answer: Install and configure the CloudWatch Agent:

Install agent on EC2:

bashsudo yum install amazon-cloudwatch-agent

Configure /opt/aws/amazon-cloudwatch-agent/etc/amazon-cloudwatch-agent.json:

json{
  "logs": {
    "logs_collected": {
      "files": {
        "collect_list": [{
          "file_path": "/var/log/app/application.log",
          "log_group_name": "/myapp/prod",
          "log_stream_name": "{instance_id}"
        }]
      }
    }
  }
}

Start the agent
Logs appear in CloudWatch Logs — searchable, filterable, alertable


EC2 IAM role needs CloudWatchAgentServerPolicy
Works for EC2, on-premises, and containers


Q3. How do you view Lambda function errors and logs in CloudWatch?
Answer: Lambda automatically sends logs to CloudWatch Logs:

Log group: /aws/lambda/{function-name}
Each invocation creates a log stream
Logs include: START, END, REPORT (duration, memory used, billed duration)
Add custom logs in code:

pythonimport logging
logger = logging.getLogger()
logger.setLevel(logging.INFO)
logger.info("Processing order: %s", order_id)
Key Lambda metrics in CloudWatch:
MetricDescriptionErrorsNumber of failed invocationsDurationExecution timeThrottlesRequests rejected due to concurrency limitConcurrentExecutionsSimultaneous running instancesInvocationsTotal calls

Set alarms on Errors and Throttles for production monitoring


🟠 Intermediate Scenarios
Q4. You need to monitor memory usage of EC2 instances. CloudWatch doesn't show it by default. How do you fix it?
Answer: Memory is an OS-level metric — not accessible by AWS hypervisor. Use the CloudWatch Agent:

Install CloudWatch Agent on EC2
Add memory config:

json{
  "metrics": {
    "metrics_collected": {
      "mem": {
        "measurement": ["mem_used_percent"],
        "metrics_collection_interval": 60
      },
      "disk": {
        "measurement": ["disk_used_percent"],
        "resources": ["/"],
        "metrics_collection_interval": 60
      }
    }
  }
}

Metrics appear under CWAgent namespace in CloudWatch
Create alarms on mem_used_percent > 85%


Same approach for disk usage, swap, processes
For ECS/EKS — use Container Insights for container-level memory metrics


Q5. How do you create a dashboard showing health of your entire application stack?
Answer: Use CloudWatch Dashboards:

Create a dashboard combining metrics from multiple services:

bashaws cloudwatch put-dashboard \
  --dashboard-name AppHealth \
  --dashboard-body file://dashboard.json
Include widgets for:

EC2: CPUUtilization, NetworkIn/Out
RDS: DatabaseConnections, ReadLatency, WriteLatency
ALB: RequestCount, TargetResponseTime, HTTPCode_ELB_5XX_Count
Lambda: Errors, Duration, Throttles
SQS: ApproximateNumberOfMessagesVisible
Custom app metrics: response times, order counts, error rates
Dashboards auto-refresh every 1/5/10/15/30/60 minutes
Share dashboards with stakeholders — no AWS login needed via snapshot sharing
Use automatic dashboards — pre-built per service (EC2, Lambda, RDS)


Q6. How do you detect and alert when your application is throwing too many errors in logs?
Answer: Use CloudWatch Logs Metric Filters:

Create a Metric Filter on your log group:

bashaws logs put-metric-filter \
  --log-group-name /myapp/prod \
  --filter-name ErrorCount \
  --filter-pattern "ERROR" \
  --metric-transformations \
    metricName=ApplicationErrors,metricNamespace=MyApp,metricValue=1
```
2. Every log line containing "ERROR" increments the `ApplicationErrors` metric
3. Create a **CloudWatch Alarm** on `ApplicationErrors > 10` in 5 minutes
4. Alarm → SNS → PagerDuty/email

**Advanced filter patterns:**
```
# JSON logs
{ $.level = "ERROR" }

# Specific error type
{ $.errorCode = "500" && $.path = "/api/payment" }

# Multiple conditions
?ERROR ?Exception ?FATAL

Q7. Your team wants to be alerted when AWS bill exceeds $500. How do you set this up?
Answer: Use CloudWatch Billing Alarms:

Enable billing alerts in AWS account settings (Billing preferences)
Billing metrics are only in us-east-1 region
Create alarm:

bashaws cloudwatch put-metric-alarm \
  --alarm-name billing-alert-500 \
  --metric-name EstimatedCharges \
  --namespace AWS/Billing \
  --dimensions Name=Currency,Value=USD \
  --statistic Maximum \
  --period 86400 \
  --threshold 500 \
  --comparison-operator GreaterThanThreshold \
  --evaluation-periods 1 \
  --alarm-actions arn:aws:sns:us-east-1:123:billing-alerts

Also set up AWS Budgets for more granular control:

Budget by service, linked account, tag
Forecast-based alerts (e.g., alert when projected to exceed $1000)




🔴 Advanced Scenarios
Q8. How do you publish custom business metrics (e.g., orders per minute) to CloudWatch?
Answer: Use CloudWatch PutMetricData API:
pythonimport boto3
cloudwatch = boto3.client('cloudwatch')

cloudwatch.put_metric_data(
    Namespace='MyApp/Orders',
    MetricData=[{
        'MetricName': 'OrdersProcessed',
        'Value': 42,
        'Unit': 'Count',
        'Dimensions': [
            {'Name': 'Environment', 'Value': 'prod'},
            {'Name': 'Region', 'Value': 'us-east-1'}
        ]
    }]
)

Call this from your app code, Lambda, or a background job
Create dashboards and alarms on custom metrics just like AWS metrics
Use high-resolution metrics (1-second granularity) for critical real-time monitoring:

Set StorageResolution: 1 in PutMetricData
Standard metrics: 1-minute granularity


Dimensions allow slicing metrics by environment, region, service, etc.
Metrics retained: 3 hours (1s), 15 days (1m), 63 days (5m), 15 months (1h)


Q9. How do you automatically remediate an issue when a CloudWatch alarm fires?
Answer: Use CloudWatch Alarms + Systems Manager Automation or Lambda:
Option 1 — EC2 Auto Recovery:
bashaws cloudwatch put-metric-alarm \
  --alarm-name ec2-auto-recover \
  --metric-name StatusCheckFailed_System \
  --namespace AWS/EC2 \
  --alarm-actions arn:aws:automate:us-east-1:ec2:recover

Automatically recovers EC2 instance on system failure

Option 2 — Lambda Remediation:

Alarm → SNS → Lambda
Lambda reads alarm details and takes action:

Restart ECS task
Scale out Auto Scaling Group
Restart RDS instance
Block an IP in WAF



Option 3 — SSM Automation:

Alarm → SSM Automation runbook
Pre-built runbooks: restart EC2, run diagnostics, patch instances
Full audit trail in Systems Manager


Q10. How do you implement distributed tracing across microservices using CloudWatch?
Answer: Use AWS X-Ray with CloudWatch ServiceLens:

Instrument each service with X-Ray SDK:

pythonfrom aws_xray_sdk.core import xray_recorder, patch_all
patch_all()  # Auto-instruments AWS SDK, requests, etc.

Enable Active Tracing on Lambda, API Gateway, ECS
CloudWatch ServiceLens provides:

Service Map — visual topology of all services
End-to-end trace across API Gateway → Lambda → DynamoDB → SNS
Latency breakdown per service hop
Error rate per service


CloudWatch Synthetics — proactively monitor:

Create Canary scripts that simulate user behavior (login, checkout)
Run every minute — alerts if user journey breaks
Catches issues before real users notice


CloudWatch RUM (Real User Monitoring):

JavaScript snippet in frontend
Captures real user performance, errors, browser metrics
Correlates with backend X-Ray traces




Q11. How do you handle a situation where CloudWatch Logs are generating huge costs?
Answer: Common causes and fixes:
Diagnose:
bash# Find which log groups are largest
aws logs describe-log-groups \
  --query 'logGroups[*].[logGroupName,storedBytes]' \
  --output table
Cost reduction strategies:

Set Log Retention — by default logs are kept forever:

bashaws logs put-retention-policy \
  --log-group-name /myapp/prod \
  --retention-in-days 30

Reduce log verbosity — change log level from DEBUG to WARN/ERROR in production
Filter before ingestion — use CloudWatch Agent filters to drop noisy logs
Export to S3 — move old logs to S3 (much cheaper) using Export Task or Subscriptions
Use S3 + Athena instead of CloudWatch Logs Insights for historical analysis
Compress logs — enable compression in CloudWatch Agent config
Lambda logging — set LOG_LEVEL=ERROR in production environment variables


Q12. How do you set up anomaly detection for your application metrics?
Answer: Use CloudWatch Anomaly Detection:

CloudWatch uses ML to learn the normal pattern of a metric (time of day, day of week seasonality)
Creates a dynamic expected band instead of a fixed threshold
Alert when metric goes outside the band

bashaws cloudwatch put-anomaly-detector \
  --namespace AWS/ApplicationELB \
  --metric-name TargetResponseTime \
  --stat Average
Create alarm based on anomaly band:
bashaws cloudwatch put-metric-alarm \
  --alarm-name response-time-anomaly \
  --metric-name TargetResponseTime \
  --namespace AWS/ApplicationELB \
  --comparison-operator GreaterThanUpperThreshold \
  --threshold-metric-id anomaly_detection_band \
  --treat-missing-data notBreaching
```

**Benefits over static thresholds:**
- No need to manually set thresholds
- Handles traffic spikes (Black Friday) automatically
- Learns seasonal patterns (night vs day traffic)
- Reduces false positives significantly

---

**Q13. How do you stream CloudWatch Logs to a third-party tool like Splunk or Elasticsearch?**

**Answer:** Use **CloudWatch Logs Subscriptions**:

**Option 1 — Kinesis Data Firehose (recommended for high volume):**
```
CloudWatch Logs → Subscription Filter → Kinesis Firehose → Splunk/S3/OpenSearch
```
- Real-time streaming, handles high throughput
- Built-in transformation via Lambda in the pipeline

**Option 2 — Lambda:**
```
CloudWatch Logs → Subscription Filter → Lambda → Splunk/HTTP endpoint

More flexible, direct API calls to any destination
Good for low-to-medium volume

Option 3 — CloudWatch Logs to OpenSearch:

Native integration — one-click setup in console
Good for ELK-style log analysis without leaving AWS

Setup subscription filter:
bashaws logs put-subscription-filter \
  --log-group-name /myapp/prod \
  --filter-name AllLogs \
  --filter-pattern "" \
  --destination-arn arn:aws:firehose:us-east-1:123:deliverystream/splunk

<img width="578" height="428" alt="image" src="https://github.com/user-attachments/assets/5bea538a-5651-4250-bac3-fea6cc2bd826" />

