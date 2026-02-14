# Cloudwatch

How to get email notifications if errors are there in cloudwatch logs 

Your application writes logs to Amazon CloudWatch Logs

A Metric Filter scans log entries for patterns like ERROR, Exception, 5xx

Matching logs increment a custom CloudWatch metric

A CloudWatch Alarm monitors that metric

Alarm sends notification via Amazon SNS (email, SMS, Slack, etc.)
