✅ Implement caddy for URL redirection & reverse proxy , also use redis into it . Also explain how will u use API gateay with plans to sell it to 3rd party --> Used Terraform + ECS

#### Tech Stack ####


#### Project Overview 
I have an ecs cluster with 3 service
- caddy frontend --> which is a dashboard for editing caddy redirect rules , configuring proxy , viewing ssl certs
- caddy core --> which is the actual caddy image from dockerhub
- caddy backend --> backend logic

Also AWS RDS postgre sql along with efs ( to store ssl certs , caddyfile config )

my caddy core ECS Task is exposed via nlb , to redirect domain x.com to y.com i need to update cname of x.com to caddy core NLB DNS name

caddy frontend & backend tasks are exposed via alb , with 2 separate target group , based on 2 separate listeners 

 can u explain why this setup is used --> check AI tools for detailed explanation

---

✅ Gitlab to Github Action Migration . explain below points
-  500+ git repos migration from Gitlab to Github
-  Pipeline worflow conversion
-  Repo secrets & variables migration
-  Git commit history migration
-  Git LFS migration
-  Gitlab Users to Azure AD Migration in association with Github Teams 

---

✅ EKS Project + Devsecops Pipeline
- https://youtu.be/fuiTqI3noTo?si=BCNP34883wALOj4f --> Abhishek --> Tech Stack --> Terraform + EKS ( Used Load Balancer Service Type ) + Github Actions --> no observability
- <img width="614" height="457" alt="image" src="https://github.com/user-attachments/assets/e070ad78-d8c1-47f8-9bae-71489f7d2083" />
- https://www.udemy.com/course/ultimate-devops-project-with-resume-preparation/  --> Abhishek --> Tech Stack --> Terraform + EKS ( Used Ingress ) + + Github Actions --> no observability

---


✅ Splunk Logs to Opensearch Logs migration with opensearch pipelines with grok parameter & index alias with index lifecycle policy.  initially logs are coming to splunk via acquia server 

---

✅ On prem VM to AWS Cloud Migration
- Check migration folder inside AWS

---
✅ Acquia Cloud to AWS Cloud Migration

---
✅ AiOps Project
- https://www.youtube.com/watch?v=D36ZkJ-sKyQ  --> Run AI based DevSecOps on every Pull request | No coding
