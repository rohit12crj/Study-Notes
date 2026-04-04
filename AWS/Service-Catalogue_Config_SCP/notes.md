✅ Videos
- https://youtu.be/ViFIojenFXs?si=nssY4KHlXP7EjNFH  --> Rahul

---
✅ 

| Service             | One Line                                      |
| ------------------- | --------------------------------------------- |
| AWS Service Catalog | Provides approved infrastructure templates    |
| AWS Config          | Tracks configuration changes and compliance   |
| SCP                 | Restricts maximum permissions across accounts |

---
✅ 

| Feature  | IAM Policy        | SCP                  |
| -------- | ----------------- | -------------------- |
| Scope    | User/Role         | Entire AWS account   |
| Purpose  | Grant permissions | Restrict permissions |
| Location | IAM               | AWS Organizations    |


---

<img width="233" height="224" alt="image" src="https://github.com/user-attachments/assets/478af721-39d0-42e8-bfe0-44aa8d0977c2" />

---

- Developers are launching EC2 instances manually with different AMIs and configurations, causing security and compliance issues. --> Use AWS Service Catalog.
- New project teams repeatedly ask for standard infrastructure. --> Create Service Catalog products
- Company security policy says S3 buckets should never be public. Detect & remediate .  AWS Config rule --> SSM Automation remediation
- Unencrypted EBS volumes are created. Detect & Fix . AWS Config rule --> SSM Automation remediation
- Company wants to allow AWS usage only in Mumbai and Singapore regions --> Use SCP
- Security team wants to ensure CloudTrail cannot be disabled --> Use SCP
- Developers launch expensive EC2 instances causing cost issues. --> Use SCP restricting instance types.
- Production resources must never be deleted accidentally --> Use SCP
  
---
✅  what SCP policies were u using
- no public s3 bucket
- no 0.0.0.0/0 allowed in sg ingress rule 






