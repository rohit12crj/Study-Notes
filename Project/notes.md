✅ https://www.youtube.com/playlist?list=PLAdTNzDIZj_9c-o43WJ7yYBa5YEAkWiOv --> DevOps Shack Project Playlist

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
#### Project Overview 
Led an end-to-end migration of the organization's entire source control ecosystem ( around 500+ Repos ) from GitLab to GitHub Enterprise, encompassing repository migration, CI/CD pipeline conversion, secret management, commit history preservation, Git LFS asset migration, and user identity federation through Azure Active Directory and GitHub Teams. The migration was executed with zero data loss and minimal disruption to active development workflows.

#### Process Flow
⭐ Git Repository Migration
- Audited all GitLab groups and projects to inventory repositories, including size, branch count, open MRs, and activity recency.
- Used git clone --mirror to create bare clones of all GitLab repos, preserving all branches, tags, and full commit history.
- Pushed mirrored repos to GitHub using git push --mirror to the new GitHub remote, ensuring 1:1 parity of all refs.
- Validated post-migration integrity by comparing branch counts, latest commit SHAs, and tag lists between source and destination.
- Configured GitLab repositories as read-only post-cutover with redirect notices pointing to new GitHub URLs.

⭐ CI/CD Pipeline Conversion (GitLab CI → GitHub Actions)
- Mapped GitLab CI/CD constructs (.gitlab-ci.yml) to GitHub Actions equivalents: stages → jobs, before_script → steps with run, artifacts → actions/upload-artifact, cache → actions/cache.
- Rewrote pipeline YAML for all services, converting GitLab's DAG-based pipeline dependencies (needs:) to GitHub Actions' job-level needs: dependencies with matrix strategies where applicable.
- Converted GitLab environment-specific deployments (deploy_staging, deploy_prod) to GitHub Actions environments with branch protection rules and required reviewers.
- Replaced GitLab-specific Docker-in-Docker (dind) builds with GitHub Actions' native docker/build-push-action for container image builds.
- Tested all converted pipelines in dry-run mode before decommissioning GitLab runners.

⭐ Repository Secrets & Variables Migration
- Inventoried all GitLab CI/CD variables (project-level, group-level, protected, masked) using the GitLab API.
- Categorized secrets by scope (environment-specific vs. global) and sensitivity level to map them to GitHub's equivalent scopes: repository secrets, environment secrets, and organization secrets.
- Used GitHub CLI (gh secret set) and GitHub API to bulk-import secrets programmatically, avoiding manual entry and ensuring consistency.
- Validated secret availability within GitHub Actions workflows by running test pipeline runs post-migration.
- Implemented secret rotation post-migration to ensure no credentials were retained in the old GitLab environment.

⭐ Git Commit History Migration
- Preserved complete commit history including author names, emails, commit timestamps, commit messages, and parent relationships using git clone --mirror.
- Where author email mismatches existed between GitLab users and GitHub/Azure AD identities, used git-filter-repo to rewrite author metadata, ensuring commits in GitHub attributed correctly to active user accounts.
- Verified history integrity post-migration by comparing git log --oneline output and commit counts between GitLab and GitHub for all critical repositories.

⭐ Git LFS Migration
- Identified all repositories using Git LFS by scanning .gitattributes files and running git lfs ls-files across all repos.
- Used git lfs fetch --all and git lfs push --all to migrate LFS objects from GitLab's LFS storage to GitHub LFS, preserving all pointer files and actual binary assets (design files, media, large datasets).
- Validated LFS integrity by checking out LFS-tracked files on developer machines post-migration and confirming file sizes and checksums matched pre-migration values.
- Updated internal documentation to reflect GitHub LFS billing implications and storage quotas.

⭐ GitLab Users to Azure AD — GitHub Teams Federation
- Mapped all active GitLab users to their corresponding Azure AD identities, resolving discrepancies in usernames and email addresses with HR records
- Configured GitHub Enterprise with Azure AD SAML SSO, enabling employees to authenticate to GitHub using their existing corporate credentials.
- Enabled SCIM provisioning between Azure AD and GitHub Enterprise to automate user lifecycle management: new hires auto-provisioned, leavers auto-deprovisioned.
- Recreated GitLab group structures as GitHub Teams, mirroring permission hierarchies (Owner → Admin, Maintainer → Maintain, Developer → Write, Reporter → Read).
- Assigned GitHub Teams to repositories with equivalent access levels, validated by audit of repository permission reports post-migration.
- Deactivated GitLab accounts post-migration to eliminate dual identity sprawl and reduce license costs.

#### Outcomes Achieved
- Migrated 100+ repositories with zero data loss and full commit history preservation
•	All CI/CD pipelines converted and operational on GitHub Actions within the migration window
•	Eliminated GitLab licensing costs post-migration
•	Azure AD SSO + SCIM eliminated manual user provisioning, reducing onboarding time for new engineers
•	Unified identity model via Azure AD improved security posture and simplified access reviews
•	Git LFS assets fully intact with no broken large file references

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
✅ Log Migration
- Splunk Logs to Opensearch Logs migration with opensearch pipelines with grok parameter & index alias with index lifecycle policy.  initially logs are coming to splunk via acquia server
- Flow = Logs from acquia --> syslog ( from acquia side ) --> fluentbit ( EC2 ) --> SQS --> opensearch ingestion index 1 --> opensearch pipeline ( grok parameter ) --> opensearch ingestion index 2
- ⭐ difference between syslog & syslog-ng
- ⭐ Can syslog-ng be used in place of fluentbit ?
- ⭐ what type of SQS queue will u use ? FIFO or Standard ?
- ⭐ how does the fan out pattern works & on what triggers ?
- ⭐ how does messages from dlq goes for retyring 
- ⭐ can fluenbit run on lambda or ECS ?
- ⭐ if my ec2 breaks down . what will happen ? how will u modify the architecture 
- ⭐ give 2 scenerios where grok will help me in this case 
- ⭐ in which case will u use sns before sqs step
- ⭐ what monitoring will u use to monitor sqs queue ?

---
✅ On prem VM to AWS Cloud Migration
- Check migration folder inside AWS

---
✅ Platform Migration ❌ not decided to be included in CV or not
- Acquia Cloud to AWS Cloud Migration 

---
✅ AiOps Project ❌ not decided to be included in CV or not
- https://www.youtube.com/watch?v=D36ZkJ-sKyQ  --> Run AI based DevSecOps on every Pull request | No coding 
