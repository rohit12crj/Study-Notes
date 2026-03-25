✅ Other Notable AWS Services

- Opensearch , Opensearch Pipelines ( With Grok Parameter ) , Index Lifecycle Policy
- Dynamo Db , DynamoDb Streams , DAX 
- Athena
- Quicksight
- Redshift
- ACM
- Glue --> check Data Engineering Folder 
- MSK ( Managed Streaming for Kafka ) --> check Data Engineering Folder
- EKS --> Check Kubernetes Folder
- Codepipeline
- Codedeploy
- Cognito ( User Pool , Identity Pool )
- SES ( Simple Email Service )
- Global Accelerator
- Budget & Cost Explorer
- Backup --> centrally manage & automate backup across various AWS services --> https://www.youtube.com/watch?v=6fvGbywTjsU&t=671s
- Organization
- AWS Control Tower --> automates  setup and governance of a secure multi-account AWS environment using a landing zone, guardrails, and automated account provisioning integrated with AWS Organizations.
- ECR - images Lifecycle Policy --> image versioning --> Image vulnerability Scanning --> need to generate ECR login password before pushing docker images in pipeline --> password can be genertaed while running the command specified in " view & push images " from ECR Console

---

✅ AWS Secure Landing Zone with Control Tower & Account Factory 
- https://tcsglobal.udemy.com/course/aws-secure-landing-zone-with-control-tower-and-aft/  --> not required but check for the terraform part if it is needed or not
- https://www.youtube.com/watch?v=ew16ZDoAals

---

#### Common Architecture Questions

---
✅ Explain security hub & security lake with respect to AWS 

---

✅ Difference between headers , querry string , cookies

---

✅ Difference between Forward Proxy & reverse proxy with examples 

https://youtu.be/CmYI2R2D2M0?si=Wg0v_FmG1_agkMIn --> Abhishek

---

✅ What are the common Reverse proxy used ? 
- Nginx
- Apache
- Caddy

---

✅ Explain Architecture flow
- Internet -> ALB -> Reverse Proxy -> ECS Tasks

<img width="912" height="336" alt="image" src="https://github.com/user-attachments/assets/daeea042-3628-42d1-9dfa-d623c6b1c2ad" />

---
| Feature           | Nginx                         | API Gateway                   | Load Balancer                   |
| ----------------- | ----------------------------- | ----------------------------- | ------------------------------- |
| Primary Role      | Reverse proxy + web server    | API management layer          | Traffic distribution            |
| Layer             | L7 (Application)              | L7 (Application)              | L4 (TCP) / L7                   |
| Routing           | Path-based, host-based        | Advanced API routing          | Basic (IP/port or simple rules) |
| Load Balancing    | ✅ Yes                         | ✅ Yes                         | ✅ Core function                 |
| Authentication    | ❌ Basic (manual config)       | ✅ Built-in (JWT, OAuth, etc.) | ❌ No                            |
| Rate Limiting     | ⚠️ Basic                      | ✅ Advanced                    | ❌ No                            |
| Caching           | ✅ Yes                         | ✅ Yes                         | ❌ No                            |
| SSL Termination   | ✅ Yes                         | ✅ Yes                         | ✅ Yes                           |
| Service Discovery | ❌ Limited                     | ✅ Yes                         | ❌ No                            |
| Observability     | ⚠️ Limited                    | ✅ Rich (metrics, tracing)     | ⚠️ Basic                        |
| Use Case          | Reverse proxy, static serving | Microservices API control     | Distribute traffic              |

---

✅ Why ALB & reverse Proxy are both used 
- ALB can handle basic application-level routing like path and host-based rules, but it lacks advanced capabilities such as URL rewriting, caching, custom authentication logic, and internal container routing.
- That’s why a reverse proxy like Nginx is often used alongside ALB to handle fine-grained application-level control.

<img width="247" height="215" alt="image" src="https://github.com/user-attachments/assets/8d2fb7d8-0058-478b-ada2-2d0822b2a593" />

<img width="290" height="113" alt="image" src="https://github.com/user-attachments/assets/76a8e2b4-0394-46e0-946d-d64f7346c506" />

---

✅ Different authentication techniques & when to use which with examples ?

---

✅ what are jump servers / bastion servers ? why do u need them ? How do u secure them ? Can we eliminate them using AWS Native services 

---

✅ How does webhooks works actually & why should we use them ?

---

✅ Difference between SSL , TLS & mTLS

---

✅ Difference between Rest API , HTTP API & GraphQL API

---
✅ Dual Connectivity from on prem to AWS to Azure 
- <img width="491" height="106" alt="image" src="https://github.com/user-attachments/assets/8099cae6-ba08-4cca-bf53-24388b37f41a" />

- "In a hybrid multi-cloud setup, OSPF is used for internal routing within on-prem, while BGP is used for external connectivity with AWS Direct Connect and Azure ExpressRoute. Routes are redistributed between OSPF and BGP at the edge router. Primary and secondary paths are controlled using BGP attributes like Local Preference and AS Path Prepending. In case of failure of the primary path (AWS), BGP automatically fails over to the secondary path (Azure)."

<img width="410" height="265" alt="image" src="https://github.com/user-attachments/assets/9ce85682-e0f6-4861-b7b3-cfcb8a65d4d4" />

---
✅ Nginx Full Course of 1 hr 
- https://youtu.be/9jZEfW8h5fQ?si=yt6wvKd30Btxn44d --> Abhishek

---
✅ explain HA for all aws services in your project ?

---
✅ RTO & RPO & what were these values in your project & how did u achieve this ?
- RTO --> The maximum acceptable time to restore a system after a failure. If RTO = 30 minutes, the system must be restored within 30 minutes after an outage.
- RPO --> The maximum acceptable amount of data loss measured in time. If RPO = 5 minutes, the system can only lose 5 minutes of data.
- In our project the RTO was around 10–15 minutes and RPO was less than 5 minutes.
- We achieved this using Multi-AZ architecture in AWS, where ECS services were deployed across multiple AZs behind an ALB.
- The database was RDS Multi-AZ, which provides synchronous replication and automatic failover within 1–2 minutes.
- We also configured automated backups, snapshots, and S3 cross-region replication for disaster recovery.
- Infrastructure was managed using Terraform, so in case of a disaster we could recreate the infrastructure quickly through our CI/CD pipelines.

---
✅ explain the various DR ( Disaster Recovery techniques ) & which one u used ?

