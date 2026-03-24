https://www.youtube.com/playlist?list=PLdpzxOOAlwvJDIAQZtMjUUbiVUDfGaCIX  --> Abhishek Playlist

https://youtu.be/SsN0ILXq5Lw?si=5VwjcdU0NiYQRpS8  --> interview questions

https://github.com/iam-veeramalla/Jenkins-Zero-To-Hero/blob/main/Interview_Questions.md  -> interview question Repo ( Abhishek )

https://youtu.be/LAYV7x_aIC0?si=ieOYjnGrUKqT4aeY  --> interview questions

✅ explain ci & cd with different pipelines & why to follow this approach ( wait for abhishek pipeline video )
- ci pipeline worflow --> git repo checkout --> install dependencies ( packagelock.json file ) --> build the application --> SAST Scan Unit --> Docker Image Creation --> SCA Scan --> Push Docker Image to ECR Repo
- cd pipeline workflow --> download ECR Images --> updates Task definition ( if any ) --> deploy Image to ECS Services ( Update Task definition if necessary ) --> DAST Scan
- cd is seperate because if in ECS 2 task conatiners are required instead of 1 , then we should just modify the cd part ( Update task definition ) , no need to build the images .
- ci pipeline should not run when task definition file is modified in the cd folder as it would create loop ( updating cd folder triggers ci pipleine which again triggers cd pipeline ) . use path ignore in ci pipeline to fix this 
- cd pipleine should only run when ci is completed successfully without errors

---

✅ Why build application before creating Docker image?
- The application is built first to produce the final runnable artifact (such as a JAR, binary, or compiled bundle). The Docker image then packages this artifact along with the runtime environment. This keeps Docker images smaller, speeds up builds, enables security scans earlier, and separates application build from container packaging. Docker images should package artifacts, not compile source code

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
- SAST --> Static Application Security Testing --> Unit Testing ( Unit Testing means testing individual units of code like functions, methods, classes ) also done in this --> Scans Source Code ( SQL Injection , Hardcoded secrets , Code Smells , Code Coverage ) --> Sonarqube
- SCA --> Software Composition Analysis --> Scans Dependencies / libraries ( Docker Images , OS packages , Node modules , Python libraries ) --> Trivy , Github Dependabot 
- DAST - Dynamic Application Security Testing --> Scans Running Applications -->  OWASP ZAP , invicti

- Someone pushed secrets to gihub code --> uses tools like trufflehog , Gitleaks or Github Secret Scanning Alerts in pipeline
- need to scan docker image before pushing to AWS ECR --> Use Trivy
- scan image already pushed to AWS ECR --> Use AWS ECR advanced image scanner
- policy as a code in pipeline --> To stop pipleine if public s3 bucket is being cretaed using terraform --> Checkov
  
---

✅ What are SonarQube Quality Gates?

Quality Gates are a set of conditions defined in SonarQube that determine whether the code passes or fails quality checks based on metrics like bugs, vulnerabilities, code coverage, and duplication.

---

✅ how will u use secrets from terraform vault inside jenkinsfile
- In our pipelines we store sensitive credentials in HashiCorp Vault and integrate Jenkins with Vault using the HashiCorp Vault plugin. Jenkins authenticates to Vault using AppRole or a token and retrieves secrets during pipeline execution. Using the withVault block, the secrets are injected as environment variables and used securely in the Jenkinsfile without exposing them in the code

<img width="539" height="185" alt="image" src="https://github.com/user-attachments/assets/26a4ee28-7e11-4dae-82be-eb60c60e0e61" />

---

✅ What are Code Smells?
- Code Smells are indicators of poor coding practices such as duplicated code, long methods, or complex logic that make the code difficult to maintain.

---

✅ Explain GitHub webhooks & jenkins integration ?
- it helps for automated pipeline runs when someone commits a code in github repo
- webhooks needs to be created in github & in pipeline also u need to do configuration
- https://www.youtube.com/watch?v=Uu8_cb0WRAw

---

✅ What is jenkins shared library ?
- https://youtu.be/BLQ0PDjgN8w?si=acykbfsz8xnSRmIB --> Abhishek

---

✅ How will u take jenkins backup & how will u restore a job which was deleted ?
- In Jenkins the most important directory to backup is JENKINS_HOME, which by default is located at /var/lib/jenkins in Linux systems. This directory contains all job configurations, plugins, credentials, build history, and system settings. By backing up this folder we can fully restore the Jenkins server if needed
- https://youtu.be/5Tb-AOUFuKQ?si=cyR2-GLsYSS30g-J

---

✅ how to add manual approval stage in Jenkins for production
- https://www.youtube.com/watch?v=yMK-l1t4KqQ
- In Jenkins we add a manual approval stage using the input step in the pipeline. The pipeline pauses and waits for a user to approve before proceeding to production deployment. We usually restrict approval to specific users like DevOps_leads ( user group should be created with this name ) using the submitter option and sometimes add a timeout to avoid pipelines waiting indefinitely.

<img width="500" height="107" alt="image" src="https://github.com/user-attachments/assets/7b02af78-c190-4dd3-8319-93b1e6e265ea" />

---

✅ What is Code Coverage?
- Code Coverage measures the percentage of application code executed by automated tests. in our project we used min 80% needs to pass

---

✅ How do you integrate SonarQube with Jenkins?
- Install the SonarQube plugin in Jenkins, configure the SonarQube server URL and authentication token .Use Terraform Vault to store the secrets .
- In Jenkins pipelines we enforce SonarQube quality gates using the waitForQualityGate step provided by the SonarQube Jenkins plugin. After running the SonarQube analysis, Jenkins waits for the quality gate result from SonarQube, and if the gate fails the pipeline is automatically aborted using abortPipeline: true. This ensures only code that meets quality standards can proceed to deployment.
- If SonarQube is slow or the webhook fails, Jenkins could keep waiting for the quality gate result indefinitely. To prevent this, we wrap the waitForQualityGate step inside a timeout block so that if the result is not received within a defined time, the pipeline fails automatically. In production setups we also configure a SonarQube webhook so Jenkins receives the quality gate status immediately

<img width="474" height="235" alt="image" src="https://github.com/user-attachments/assets/c7c714f3-b2d9-4161-97d4-fea04cb21012" />

<img width="371" height="230" alt="image" src="https://github.com/user-attachments/assets/f250b14e-2ecf-48f6-bc9d-d031d725a2ea" />

<img width="563" height="226" alt="image" src="https://github.com/user-attachments/assets/b8f7d046-15e9-43b3-b276-49ce39d49d94" />

---

✅ How do you integrate Terraform Vault with Jenkins?
- Jenkins integrates with HashiCorp Vault using the Vault plugin. Jenkins authenticates to Vault using AppRole
- https://youtu.be/sIkRK33AY50?si=E_Q0ql3KqvUXIp2y --> Abhishek Video
- i want to do checkout from git repo inside jenkinsfile , i dont want to use credentials plugin . i want to use the github PAT stored in terraform vault . same techniquie can be used here also 
---

✅ Your Jenkins Pipeline is slower . How will u make it faster ?
- Use parallel stages to execute independent tasks simultaneously.
- Example: Parallelize static code analysis, unit tests, and integration tests.
- Use build caching mechanisms like Docker layer caching or dependency caching.
- 
---

✅ Jenkins Version ? 
- LTS (Long-Term Support)	2.541.1	Stable version recommended for production

---

✅ Nre docker & k8s plugin necessary with jenkins ? 
- no , u can just install docker inside jenkins agent and run the docker build commands . for eks , u can ekscli to connect with k8s cluster

---

✅ were u using iam keys or roles to connect with aws  ? 
- IAM roles are preferred because they provide temporary, automatically rotated credentials without hardcoding secrets, improving security and manageability. but first we need to configure oidc provider in iam , & add the github repo url in trust relationships of the iam role , so that only the github repo urls can invoke the pipeline

---

✅ How will u upgrade Jenkins Controller to new version ? 

---

✅ what different Jenkins Plugin were u using ? 
- Sonarqube , Vault ,

---

✅ How will u do RBAC in Jenkins . Which plugin will u be using ? 
- Use the Role-Based Authorization Strategy plugin.
- Define roles like Admin, Developer, and Viewer, and assign permissions for jobs, folders, and builds accordingly.
- https://www.youtube.com/watch?v=dWIfBoHOCDI

---

✅ share artifacts between different stages in same job in jenkins ?
- n Jenkins pipelines, artifacts between stages in the same job can be shared because stages usually use the same workspace. However, for more reliable handling, especially when different agents are involved, we use stash and unstash to temporarily store and retrieve artifacts between stages. If we want to persist artifacts after the build, we use archiveArtifacts
  
---

✅ How do you share artifacts across pipelines/jobs?
- Artifact repository (Nexus, Artifactory, S3)
- Versioned artifacts

---

✅ dev 1 --> repo 1 , dev 2 --> repo 2 , how will u give separate access to Jenkins job ?
- To give separate access to Jenkins jobs, we implement role-based access control using the Role-Based Authorization Strategy plugin. We create item roles mapped to specific job patterns and assign users or groups to those roles. This ensures that each developer can only view or trigger the Jenkins jobs related to their repository

<img width="392" height="242" alt="image" src="https://github.com/user-attachments/assets/44d72336-feb7-464c-9e89-5c0ff2752e94" />

---

✅ How will u rollback a deployment ?
- If your GitHub Actions pipeline deploys the service, rollback usually means redeploying a previous commit or image tag.
<img width="278" height="122" alt="image" src="https://github.com/user-attachments/assets/0b1924c1-1bb5-4c47-b332-1057147e31a4" />

---

✅ Jenkins blue green deployment ? --> 
- https://www.youtube.com/watch?v=tstBG7RC9as
- in terrafom initailly both blue & green services should be present 
- For Blue-Green deployments in ECS without CodeDeploy, we maintain two ECS services — blue and green — behind an ALB. Jenkins builds the Docker image, pushes it to ECR, and deploys it to the inactive service (green). After validation, Jenkins can switch the ALB listener to route traffic to the green target group. Infrastructure such as the green ECS service and target groups are typically provisioned using Terraform
- in jenkins build with paramter option is used
- If the Jenkins pipeline modifies the ALB listener to switch traffic between blue and green target groups, Terraform will detect drift during the next plan because the listener configuration differs from the Terraform state. To avoid this, we typically use a lifecycle rule like ignore_changes for the listener default action so Terraform does not attempt to revert the traffic switch

<img width="415" height="263" alt="image" src="https://github.com/user-attachments/assets/691c2759-8971-4431-919e-b2c5683a3d87" />

<img width="437" height="325" alt="image" src="https://github.com/user-attachments/assets/086a4b65-0d37-42cb-a12c-8c64b34327e4" />

<img width="416" height="176" alt="image" src="https://github.com/user-attachments/assets/b5ca1adb-1cd0-4d35-8f24-b841c03e67ed" />

---

✅ build with parameters in jenkins ?
- used in blue green depoyment & for switching traffic from blue to green

---

✅ What are Jenkins jobs , Jenkinsfile , Jenkins Pipeline ( Groovy Language )

---

✅ Different types of pipelines in jenkins ?
- Declarative Pipeline 
- Scripted Pipeline
- multibranch pipeline
- multi stage pipelines

---

What is an Agent in Jenkins?

What is a Node?

How does Jenkins store credentials? 

How do you secure Jenkins? --> RBAC

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

---

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

