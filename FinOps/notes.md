what FinOps rules in conjunction with AWS Service Catalogue were u using ?
- In our project we implemented FinOps practices using AWS Service Catalog to control cloud costs and enforce governance.
- Developers could provision infrastructure only through approved Service Catalog products backed by CloudFormation templates. We enforced mandatory tagging for cost allocation, restricted instance types to prevent expensive resources, and configured AWS Budgets for cost alerts.
- We also implemented automated shutdown for non-production environments and lifecycle policies for S3 storage. These controls helped us improve cost visibility and reduce unnecessary cloud spending.

---

How will u send out mail to different AWS Account Owners about cost optimization rules within the AWS Services they are using ?
