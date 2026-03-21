✅ Other Notable AWS Services

- Opensearch , Opensearch Pipelines ( With Grok Parameter ) , Index Lifecycle Policy
- Dynamo Db , DynamoDb Streams , DAX 
- Athena
- Quicksight
- Redshift
- Glue --> check Data Engineering Folder 
- MSK ( Managed Streaming for Kafka ) --> check Data Engineering Folder 
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
