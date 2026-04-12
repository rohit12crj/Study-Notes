✅ https://www.youtube.com/playlist?list=PLAdTNzDIZj_9c-o43WJ7yYBa5YEAkWiOv --> DevOps Shack Project Playlist

---
✅ Self Hosted Github Runners in EKS / ECS  OR Jenkins master & slave setup in ECS / EKS

---
✅ DR Project for 3 Tier web App 
- https://www.youtube.com/watch?v=WYCbczFIj3E&t=1869s
- <img width="949" height="461" alt="image" src="https://github.com/user-attachments/assets/fc0dbe03-84d3-4ce9-8f7c-8f6bb8e8712d" />
- <img width="910" height="448" alt="image" src="https://github.com/user-attachments/assets/fe5a4b00-5ac5-426a-94f4-f7bf58785c35" />
- <img width="937" height="440" alt="image" src="https://github.com/user-attachments/assets/2f117bf1-2d1a-4121-a2d3-118e198cc957" />
- <img width="875" height="442" alt="image" src="https://github.com/user-attachments/assets/dc9f5941-7f21-443c-9788-663ad974af93" />
- <img width="923" height="423" alt="image" src="https://github.com/user-attachments/assets/a555a41f-db0a-4f3b-a5b7-94b285691c91" />
- <img width="798" height="446" alt="image" src="https://github.com/user-attachments/assets/207dddf1-5a9c-448a-a554-76734223d930" />
- explain how you did Active / Passive Pilot Light for 3 Tier Web App .
- Explain Same region data replication for Primary Region = Frankfurt ( eu-central-1 )
- Explain Cross region Data Replication for Backup Region = us-east-1 ( N.Virginia ) for services like , RDS , EFS , Secret Manger , ECR images . Note --> ECS Cluster is created for both regions earlier beforehand
- what steps you would need to perform during a DR scenerio to shift traffic to Backup Region & make sure your app is running . how will u automate this ?
- what is cloud endure ? --> on prem to aws cloud DR 
- what was the actual RTO & RPO
- HA strategy means multi AZ deployment & DR strategy means multi Region deployment
- RTO --> The maximum acceptable time to restore a system after a failure. If RTO = 30 minutes, the system must be restored within 30 minutes after an outage.
- RPO --> The maximum acceptable amount of data loss measured in time. If RPO = 5 minutes, the system can only lose 5 minutes of data.
- In our project the RTO was around 10–15 minutes and RPO was less than 5 minutes.
- We achieved this using Multi-AZ architecture in AWS, where ECS services were deployed across multiple AZs behind an ALB.
- The database was RDS Multi-AZ, which provides synchronous replication and automatic failover within 1–2 minutes.
- We also configured automated backups, snapshots, and S3 cross-region replication for disaster recovery.
- Infrastructure was managed using Terraform, so in case of a disaster we could recreate the infrastructure quickly through our CI/CD pipelines.

---
✅ Terraform Vault Setup in EKS with HA architecture
- https://www.youtube.com/watch?v=rMG7XZqN0PM&list=PLAdTNzDIZj_9c-o43WJ7yYBa5YEAkWiOv&index=5  --> DevOps Shack
- https://www.youtube.com/watch?v=TeD8jDem2gs&t=835s  --> DevOps Shack
- https://www.youtube.com/playlist?list=PL7iMyoQPMtAP7XeXabzWnNKGkCex1C_3C --> Rahul ( Playlist )
- https://youtu.be/7xl4hY-W6Ts?si=lFPcBacoJWgNcr6G --> Abhishek

---
✅ Observability Project
- https://www.youtube.com/watch?v=n7WWme--U2I&t=204s --> Devops Shack
- https://www.youtube.com/watch?v=w2pnBa7eAbI&list=PLdpzxOOAlwvJUIfwmmVDoPYqXXUNbdBmb&index=7&t=1475s  --> Abhishek 

---
✅ 3 Tier Web App 

#### Tech Stack ####
- Iaac = Terraform
- Containerization Framework = ECS
- Storage = Postgre RDS + EFS + Secret Manager
- Devsecops Pipeline
- ⭐ Pipeline --> Github Actions
- ⭐ SCA --> Github Dependabot
- ⭐ SAST --> Github QL ( Code Quality ) + Secret Scanning
- ⭐ DAST --> not supported natively by Github . Used External tools like OWASP ZAP
- ⭐ Docker Image Storage & Scanning --> AWS ECR Advanced Scanning
- ⭐ Artifact Management --> Nexus Artifactory
- Observability Tools
- ⭐ metrics (what’s happening) --> 
- ⭐ logs (why it happened) --> 
- ⭐ traces (where it happened) -->

#### Project Overview 
- Implement caddy for URL redirection & reverse proxy , also use redis into it . Also explain how will u use API gateay with plans to sell it to 3rd party 

I have an ecs cluster with 3 service
- caddy frontend --> which is a dashboard for editing caddy redirect rules , configuring proxy , viewing ssl certs
- caddy core --> which is the actual caddy image from dockerhub
- caddy backend --> backend logic

Also AWS RDS postgre sql along with efs ( to store ssl certs , caddyfile config )

my caddy core ECS Task is exposed via nlb , to redirect domain x.com to y.com i need to update cname of x.com to caddy core NLB DNS name

caddy frontend & backend tasks are exposed via alb , with 2 separate target group , based on 2 separate listeners 

 can u explain why this setup is used --> check AI tools for detailed explanation

---
✅ Gitlab to Github Enterprise Repo Migration
- <img width="437" height="443" alt="image" src="https://github.com/user-attachments/assets/d9a54ced-a2b9-498b-90cd-880653d79232" />
- <img width="421" height="259" alt="image" src="https://github.com/user-attachments/assets/6f3723b3-7126-4c5f-a7c3-42fa62f25997" />
- <img width="518" height="366" alt="image" src="https://github.com/user-attachments/assets/df8fde13-8fe0-4583-8be4-2f43a447c70b" />
- <img width="431" height="302" alt="image" src="https://github.com/user-attachments/assets/68addc51-b331-486a-915c-f7f3e22a8abd" />
- <img width="430" height="445" alt="image" src="https://github.com/user-attachments/assets/acc4b3b6-f8ef-461b-a479-19a978655b3d" />
- <img width="450" height="343" alt="image" src="https://github.com/user-attachments/assets/38bac152-4cd0-4f63-bd0d-4eb3c500e698" />
- <img width="569" height="305" alt="image" src="https://github.com/user-attachments/assets/66655eb1-3336-4615-aff3-e1dc9638d94e" />

---
✅ EKS Project + Devsecops Pipeline

#### Tech Stack ####
- Iaac = Terraform
- Containerization Framework = EKS
- Devsecops Pipeline
- ⭐ Pipeline --> Github Actions
- ⭐ SCA --> Trivy
- ⭐ SAST --> Sonarqube
- ⭐ DAST --> OWASP ZAP
- ⭐ Docker Image Storage --> AWS ECR
- ⭐ Docker Image Scanning --> Trivy
- ⭐ Artifact Management --> Nexus Artifactory
- Observability Tools --> Prometheus , Grafana
 - https://youtu.be/fuiTqI3noTo?si=BCNP34883wALOj4f --> Abhishek --> Tech Stack --> Terraform + EKS ( Used Load Balancer Service Type ) + Github Actions --> no observability
- <img width="614" height="457" alt="image" src="https://github.com/user-attachments/assets/e070ad78-d8c1-47f8-9bae-71489f7d2083" />
- https://www.udemy.com/course/ultimate-devops-project-with-resume-preparation/  --> Abhishek --> Tech Stack --> Terraform + EKS ( Used Ingress ) + + Github Actions --> no observability

---
✅ Splunk to OpenSearch Log Migration  --> check Splunk_to_OpenSearch_Log_Migration.docx
- ⭐ Flow = Logs from acquia --> syslog ( from acquia side ) --> NLB --> fluentbit ( Running on ECS + EFS ) --> Writes to S3 --> S3 Object Notification -->  SQS --> opensearch ingestion pipeline ( grok parameter ) Polls SQS but reads from s3 --> opensearch index --> opensearch Dashboard  . Update the flow if required 
- ⭐ is opensearch pipeline poll based or push based ?
- ⭐ can syslog directly send the logs to sqs queue ?
- ⭐ difference between syslog & syslog-ng
- ⭐ Can syslog-ng be used in place of fluentbit ?
- ⭐ Use of s3 ? how will u delete the s3 log files which have been successfully processed by sqs
- ⭐ what type of SQS queue will u use ? FIFO or Standard ?
- ⭐ since you are using SQS FIFO , how will u handle message duplication ?
- ⭐ how does the fan out pattern works & on what triggers ?
- ⭐ how does messages from dlq goes for retyring ? Add Lambda for DLQ auto-retry
- ⭐ ECS Scale out patterns & on what triggers
- ⭐ give 2 scenerios where grok will help me in this case
- ⭐ in which case will u use sns before sqs step
- ⭐ what metrics will u use to monitor sqs queue ?
- ⭐ what happens if ecs container crashes ?
- ⭐ For fluentbit ECS Cluster , write docker file with EFS Volume Mounting
- ⭐ why NLB & not ALB ? what should be the NLB listener port no , protocol & target group rules
- ⭐ what is buffering & backpressure handling & how did u achieve this ?
- ⭐ what is sqs maxreceive count , visibility timeout  , message retention period set for your project
- ⭐ write opensearch Ingestion pipeline configuration ( need to add in doc file )
- ⭐ why sqs is being used ? can we omit it ? ( need to add in doc file )

---
✅ On prem VM to AWS Cloud Migration
- Check migration folder inside AWS

---
✅ Platform Migration ❌ not decided to be included in CV or not
- Acquia Cloud to AWS Cloud Migration 

---
✅ AiOps Project ❌ not decided to be included in CV or not
- https://www.youtube.com/watch?v=D36ZkJ-sKyQ  --> Run AI based DevSecOps on every Pull request | No coding 
