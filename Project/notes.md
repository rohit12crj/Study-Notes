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
-  git repo migration from Gitlab to Github
-  Pipeline conversion
-  Repo secrets & varibales migration
-  Git commit history migration
-  Git LFS migration
-  Gitlab Users to Azure AD Migration in association with Github Teams 

---

✅ EKS Project + Devsecops Pipeline
- https://youtu.be/fuiTqI3noTo?si=BCNP34883wALOj4f --> Abhishek --> Tech Stack --> Terraform + EKS ( Used Load Balancer Service Type ) 
🔥 *Overview:*
→ A 3-tier blog platform (React + Node.js + PostgreSQL).
→ Containerized with Docker, orchestrated with Kubernetes on *AWS EKS Auto Mode*
→ Infrastructure provisioned with *Terraform* (VPC, EKS, IAM all as code)
→ A full *DevSecOps CI/CD pipeline* with GitHub Actions (10 stages)

 *Security/DevSecOps practices considered:*
✅ ESLint code linting
✅ npm audit (Software Composition Analysis)
✅ Trivy container image vulnerability scanning
✅ Hadolint Dockerfile linting
✅ Checkov IaC security scanning (Terraform + K8s manifests)
✅ Kubernetes NetworkPolicies (zero-trust pod communication)
✅ Non-root containers, read-only filesystems, dropped capabilities
✅ EKS secrets encryption at rest

*CI/CD Pipeline (All Stages):*
1️⃣ Lint Scanning→ 2️⃣ Dependency Audit (SCA) → 3️⃣ Build & Push to GHCR → 4️⃣ Container Image Scan (Trivy) → 5️⃣ IaC Security Scan (Checkov) → 6️⃣ Dockerfile Lint (Hadolint) → 7️⃣ Auto-update K8s manifests (GitOps-style)

*Tech Stack:*
• Frontend: React + Vite + Nginx
• Backend: Node.js + Express
• Database: PostgreSQL
• Containers: Docker + Docker Buildx (multi-stage builds)
• Orchestration: Kubernetes (AWS EKS Auto Mode)
• IaC: Terraform
• CI/CD: GitHub Actions
• Registry: GitHub Container Registry (GHCR)
• Security: Trivy, Checkov, Hadolint, ESLint, npm audit

- https://www.udemy.com/course/ultimate-devops-project-with-resume-preparation/  --> Abhishek --> Tech Stack --> Terraform + EKS ( Used Ingress ) + Prometheus + Grafana

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
