#### Cloudwatch

✅ How will u send custom cloudwatch logs from EC2 ?
- To send custom logs from EC2 to CloudWatch, I would install the CloudWatch Agent, configure it with the log file paths and log group details, attach an IAM role with necessary permissions, and start the agent so it continuously streams logs to CloudWatch.

---

✅ How will u send custom cloudwatch logs from on prem VM ?


---
---
---

#### Cloudtrail

✅ what do u need to do if u want to store logs in cloudtrail for more than 90 days 
- To retain CloudTrail logs for more than 90 days, we need to create a trail and configure it to deliver logs to an S3 bucket, where we can define lifecycle policies for long-term storage
