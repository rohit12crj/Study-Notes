#### Cloudwatch

✅ How will u send custom cloudwatch logs from EC2 ?
- To send custom logs from EC2 to CloudWatch, I would install the CloudWatch Agent, configure it with the log file paths and log group name , attach an IAM role with necessary permissions, and start the agent so it continuously streams logs to CloudWatch.

---

✅ How will u send custom cloudwatch logs from on prem VM ?
- setup DX or VPN from on prem to AWS Cloud
- Since its On prem VPN , use STS Assume role . no need to use keys & secrets --> check this part from ChatGPT
- in IAM use least privelege 
- Install cloudwatch agent
- configure clouwatch agent with file path & log group name
- start agent 

---

✅ Get notified when CloudWatch Agent on an on-prem VM stops working

---

✅ is cloudwatch agent push or pull based ?
- CloudWatch Agent is push-based. It collects logs and metrics locally and pushes them to CloudWatch.

---

✅  for short live batch jobs is cloudwatch agent not useful then ? --> check with chatGPT

---
---
---

#### Cloudtrail

✅ what do u need to do if u want to store logs in cloudtrail for more than 90 days 
- To retain CloudTrail logs for more than 90 days, we need to create a trail and configure it to deliver logs to an S3 bucket, where we can define lifecycle policies for long-term storage
