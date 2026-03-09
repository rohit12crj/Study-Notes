✅ what FinOps rules in conjunction with AWS Service Catalogue were u using ?
- In our project we implemented FinOps practices using AWS Service Catalog to control cloud costs and enforce governance.
- Developers could provision infrastructure only through approved Service Catalog products backed by CloudFormation templates. 

---

✅Common AWS cost-saving options (FinOps best practices)
- Right Sizing – Reduce over-provisioned EC2/RDS instances to smaller sizes.
- Reserved Instances (RI) – Commit to 1 or 3 years for up to 70% cost savings.
- Savings Plans – Flexible alternative to RI for EC2, Lambda, and Fargate.
- Spot Instances – Use unused AWS capacity at 70–90% discount for batch or stateless workloads.
- Auto Scaling – Scale resources up/down based on demand to avoid idle capacity.
- Stop Non-Production Resources – Automatically stop dev/test EC2 or RDS after working hours.
- Use S3 Lifecycle Policies – Move old data to cheaper storage classes (Standard → IA → Glacier).
- Delete Unused Resources – Remove unattached EBS volumes, unused Elastic IPs, idle load balancers.
- Use Graviton Instances – ARM-based EC2 instances that are ~20–40% cheaper.
- Use Smaller Storage Volumes – Optimize EBS/RDS storage sizes and IOPS.
- Use Auto Scaling for ECS/EKS – Avoid running idle containers.
- CloudWatch Log Retention – Set log retention period to avoid long-term storage costs.
- Use AWS Budgets & Cost Explorer – Monitor and alert on unexpected spending.
- Use Compute Spot in ECS/EKS – Run container workloads on cheaper spot capacity.
- Use Data Transfer Optimization – Keep services in the same AZ/VPC to reduce data transfer charges
- Use valkey cache instead of redis cache
  
