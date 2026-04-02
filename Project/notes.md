✅ https://www.youtube.com/playlist?list=PLAdTNzDIZj_9c-o43WJ7yYBa5YEAkWiOv --> DevOps Shack Project Playlist

---
✅ Terraform Vault Setup in EKS with HA architecture
- https://www.youtube.com/watch?v=rMG7XZqN0PM&list=PLAdTNzDIZj_9c-o43WJ7yYBa5YEAkWiOv&index=5  --> DevOps Shack
- https://www.youtube.com/watch?v=TeD8jDem2gs&t=835s  --> DevOps Shack

---
✅ Observability Project
- https://www.youtube.com/watch?v=n7WWme--U2I&t=204s --> Devops Shack

---
✅ 3 Tier Web App 

#### Tech Stack ####
- Iaac = Terraform
- Containerization Framework = ECS
- Devsecops Pipeline
- ⭐ Pipeline --> Github Actions
- ⭐ SCA --> Github Dependabot
- ⭐ SAST --> Github QL ( Code Quality ) + Secret Scanning
- ⭐ DAST --> not supported natively by Github . Used External tools like OWASP ZAP
- ⭐ Docker Image Storage & Scanning --> AWS ECR Advanced Scanning
- ⭐ Artifact Management --> Nexus Artifactory
- Observability Tools --> AWS Native

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

---
✅ On prem VM to AWS Cloud Migration
- Check migration folder inside AWS

---
✅ Platform Migration ❌ not decided to be included in CV or not
- Acquia Cloud to AWS Cloud Migration 

---
✅ AiOps Project ❌ not decided to be included in CV or not
- https://www.youtube.com/watch?v=D36ZkJ-sKyQ  --> Run AI based DevSecOps on every Pull request | No coding 
