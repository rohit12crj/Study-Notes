https://www.youtube.com/playlist?list=PLdpzxOOAlwvJDIAQZtMjUUbiVUDfGaCIX  --> Abhishek Playlist

https://youtu.be/SsN0ILXq5Lw?si=5VwjcdU0NiYQRpS8  --> interview questions

https://youtu.be/LAYV7x_aIC0?si=ieOYjnGrUKqT4aeY  --> interview questions

✅ explain ci & cd with different pipelines & why to follow this approach ( wait for abhishek pipeline video )

- ci pipeline worflow --> git repo checkout --> install dependencies ( packagelock.json file ) --> build the application --> Unit Testing ( Unit Testing means testing individual units of code like functions, methods, classes ) --> SAST Scan --> Docker Image Creation --> SCA Scan --> Push Docker Image to ECR Repo
- cd pipeline workflow --> download ECR Images --> deploy Image to ECS Services ( Update Task definition if necessary ) --> DAST Scan
- cd is seperate because if in ECS 2 task conatiners are required instead of 1 , then we should just modify the cd part , no need to build the images .

---

✅ Why build application before creating Docker image?

The application is built first to produce the final runnable artifact (such as a JAR, binary, or compiled bundle). The Docker image then packages this artifact along with the runtime environment. This keeps Docker images smaller, speeds up builds, enables security scans earlier, and separates application build from container packaging. Docker images should package artifacts, not compile source code

---

✅ Tool used for building applications

- Java - Maven
- Python - pip
- Node - npm

---

✅ Tool used for Unit Testing

- Java - Junit
- Python - PyTest
- Node - Jest / Mocha

---

✅ Tool used for security testing

- SAST --> Static Application Security Testing --> Scans Source Code ( SQL Injection , Hardcoded secrets , Code Smells , Code Coverage ) --> Sonarqube
- SCA --> Software Composition Analysis --> Scans Dependencies / libraries ( Docker Images , OS packages , Node modules , Python libraries ) --> Trivy , Github Dependabot 
- DAST - Dynamic Application Security Testing --> Scans Running Applications -->  OWASP ZAP , invicti

- Someone pushed secrets to gihub code --> uses tools like trufflehog & Github Secret Scanning Alerts
- need to scan docker image before pushing to AWS ECR --> Use Trivy
- scan image already pushed to AWS ECR --> Use AWS ECR advanced image scanner
- policy as a code in pipeline --> To stop pipleine if public s3 bucket is being cretaed using terraform --> Checkov
  
---

✅ What are SonarQube Quality Gates?

Quality Gates are a set of conditions defined in SonarQube that determine whether the code passes or fails quality checks based on metrics like bugs, vulnerabilities, code coverage, and duplication.

---

✅ What are Code Smells?

Code Smells are indicators of poor coding practices such as duplicated code, long methods, or complex logic that make the code difficult to maintain.

---

✅ What is jenkins shared library ?

https://youtu.be/BLQ0PDjgN8w?si=acykbfsz8xnSRmIB --> Abhishek

---

✅ How will u take jenkins backup ?

https://youtu.be/5Tb-AOUFuKQ?si=cyR2-GLsYSS30g-J

---

✅ What is Code Coverage?

Code Coverage measures the percentage of application code executed by automated tests. in our project we used min 80% needs to pass

---

✅ How do you integrate SonarQube with Jenkins?

Install the SonarQube plugin in Jenkins, configure the SonarQube server URL and authentication token .Use Terraform Vault to store the secrets .

---

✅ How do you integrate Terraform Vault with Jenkins?

- Jenkins integrates with HashiCorp Vault using the Vault plugin. Jenkins authenticates to Vault using AppRole
- https://youtu.be/sIkRK33AY50?si=E_Q0ql3KqvUXIp2y --> Abhishek Video

---

✅ How do you integrate Git with Jenkins?

---

✅ Your Jenkins Pipeline is slower . How will u make it faster ?
- Use parallel stages to execute independent tasks simultaneously.
- Example: Parallelize static code analysis, unit tests, and integration tests.
- Use build caching mechanisms like Docker layer caching or dependency caching.
- 
---

✅ Jenkins Version ? 

LTS (Long-Term Support)	2.541.1	Stable version recommended for production

---

✅ are docker & k8s plugin necessary with jenkins ? 

no , u can just install docker inside jenkins agent and run the docker build commands . for eks , u can ekscli to connect with k8s cluster

---

✅ were u using iam keys or roles to connect with aws  ? 

IAM roles are preferred because they provide temporary, automatically rotated credentials without hardcoding secrets, improving security and manageability. but first we need to configure oidc provider in iam , & add the github repo url in trust relationships of the iam role , so that only the github repo urls can invoke the pipeline

---

✅ How will u upgrade Jenkins Controller to new version ? 

---

✅ what different Jenkins Plugin were u using ? 

- Sonarqube , Vault ,

---

✅ How will u tag your production releases & make sure manual approvals are required before production pipeline runs ? 

---

✅ How will u do RBAC in Jenkins . Which plugin will u be using ? 

- Use the Role-Based Authorization Strategy plugin.
- Define roles like Admin, Developer, and Viewer, and assign permissions for jobs, folders, and builds accordingly.

---

share artifacts between different jobs and different stages in same job in jenkins ?

How do you share artifacts across pipelines?
Artifact repository (Nexus, Artifactory, S3)
Versioned artifacts

How will u rollback a deployment ?

What are Jenkins jobs , Jenkinsfile , Jenkins Pipeline ( Groovy Language )

Declarative vs Scripted Pipeline , multibranch pipeline , multi stage pipelines

What is an Agent in Jenkins?

What is a Node?

How does Jenkins store credentials? 

How do you secure Jenkins?

What is Jenkins Shared Library?

What is Blue Ocean?

What is Build Trigger? & different ways to trigger build ?

How does Jenkins handle parallel execution?

Build is failing intermittently. How will you debug?

How do you rollback a failed deployment?

How do you handle secrets in Jenkinsfile?

Jenkins is slow. How will you optimize?

Jenkins crashes frequently. What do you check?

Where does Jenkins store data?

Jenkins Backup & Restore

Job stuck in queue – reasons?

Jenkins push or pull? → Pull
Jenkins config file? → config.xml
Language used? → Java
Jenkins pipeline language? → Groovy
Default port? → 8080

Explain Jenkins architecture in production
Controller: scheduling, UI, pipeline orchestration
Agents: execute jobs
Communication via JNLP / SSH
Controller should be stateless as much as possible

Why should Jenkins controller not run builds?
Single point of failure
Resource contention
Security risk --> Controller should only orchestrate

How does Jenkins scale horizontally?
Multiple agents
Dynamic agents (Docker / Kubernetes)
Parallel stages
Multibranch pipelines

Difference between Node and Executor?
Node: machine running Jenkins agent
Executor: parallel slot on a node

What happens if an agent goes down mid-build?
Build fails
Workspace lost (unless externalized)
Jenkins retries only if pipeline supports it

Declarative vs Scripted pipeline—when to use scripted?
Use Scripted when:
Complex logic
Dynamic stages
Advanced Groovy control

How do you pass data between stages?
Environment variables
Files in workspace
stash / unstash

How does stash/unstash work internally?
Compresses files
Stores on controller
Restores on agent --> Not for large artifacts

How do you make pipelines reusable?
Shared Libraries
Parameterized pipelines
Template Jenkinsfiles

How do you version Jenkins pipelines?
Jenkinsfile in Git
Tagged releases
Shared library versioning

Why is Jenkins Script Console dangerous?
Executes arbitrary Groovy on controller

How do you prevent secret leakage in logs?
Mask passwords
Use credentials binding
Disable set -x
Rotate leaked secrets immediately

How do you implement RBAC in Jenkins?
Role Strategy Plugin
Folder-based permissions
LDAP / SSO integration

How does Multibranch Pipeline detect changes?
Periodic indexing
Webhooks
SCM polling (not recommended)


Jenkins pull or push model?
Pull-based
Jenkins pulls code
Triggered by webhook events

How do you secure Jenkins webhooks?
GitHub secret token
IP whitelisting
HTTPS only

How do you handle mono-repo builds?
Path-based conditions
Selective pipelines
Parallel builds per service

How do you avoid rebuilding unchanged code?
ChangeSet detection
Conditional stages
Cache dependencies

Jenkins inside Docker --> good or bad?
✅ Good for:
Easy setup
Reproducibility
❌ Bad for:
Persistent storage
Docker-in-Docker complexity

Benefits of Jenkins on Kubernetes?
Auto-scaling
Ephemeral agents
Cost efficiency
Isolation

Jenkins is slow—how do you debug?
Thread dumps
JVM heap analysis
Plugin audit
Disk I/O check

How do you tune Jenkins JVM?
Increase heap
GC tuning
Disable unnecessary plugins

Why are too many plugins dangerous?
Memory leaks
Compatibility issues
Security vulnerabilities

How do you handle flaky tests?
Retry logic
Quarantine tests
Test categorization

How do you implement build timeouts?
options { timeout(time: 30, unit: 'MINUTES') }

What should be backed up in Jenkins?
$JENKINS_HOME
Job configs
Credentials
Plugins list

Jenkins HA—how do you design it?
Stateless controller
External DB/storage
Backup + fast restore

Safe Jenkins upgrade strategy?
Backup first
Upgrade plugins gradually
Test in staging
Use LTS

Jenkins data corruption—what now?
Restore backup
Verify filesystem
Audit plugins

How do you implement Blue-Green deployment?
Two environments
Switch traffic via LB
Rollback by traffic flip

Canary deployment using Jenkins?
Gradual rollout
Metrics monitoring
Auto rollback

How do you handle approvals in Jenkins?
input step
RBAC
Timeout on approvals

How do you trigger downstream pipelines?
build step
Events
Artifacts as input

How do you monitor Jenkins?
Prometheus metrics
JVM metrics
Disk usage
Build queue length

Key Jenkins metrics to watch?
Queue size
Build duration
Agent utilization
Failure rate

Jenkins logging best practices?
Centralized logs
Log rotation
Avoid sensitive data

Pipeline stuck in queue—why?

No matching agents
Label mismatch
Resource exhaustion

Production secret leaked via Jenkins—actions?
Rotate secret immediately
Audit logs
Restrict permissions

✅ when jenkins & when github ?


✅ If you had to redesign Jenkins today?

Stateless controller --> HA Architectecture --> Configure Jenkins Backup --> RBAC Jenkins Controller Security --> agents running in k8s or in ECS --> Terraform Vault Integration 

---

✅ If the Cl stage succeeds but the CD stage fails in Jenkins, how would you debug and resolve it? --> check pipeline logs 

---

✅ Which tools have you used to build a complete CI/CD pipeline? already answeres in ci & cd workflow

---

✅ If you encounter a secret-related error in your application or pipeline, how would you troubleshoot it? --> check pipeline logs

---

✅ How would you design a CI/CD pipeline for 50+ microservices with zero downtime deployment? 

