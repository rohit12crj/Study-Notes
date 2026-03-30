✅ Videos
- https://www.youtube.com/playlist?list=PLdpzxOOAlwvLRR_KPxSKCJMf2qDNKrGd-  --> DevSecOps Playlist ( Abhishek )
- https://github.com/iam-veeramalla/DevSecOps-Zero-to-Hero/ --> Github Repo ( Abhishek )
- https://www.youtube.com/watch?v=NnkUGzaqqOc&t=18s --> DevSecOps Pipeline --> DevOps Shack
- https://www.youtube.com/watch?v=fuiTqI3noTo --> --> DevSecOps Pipeline --> Abhishek
  
---
✅ Pipeline Steps ( Java OR Node Based Application  )
- compile --> to find out syntax based errors --> Maven --> mvm compile ( u need install maven in github action runner first)
- unit testing --> to check functionality of code --> Maven --> mvm test
- SAST --> code quality check --> sonarqube --> use quality gates . fail pipeline if quality gates requirement not satisfied 
- SCA --> package vulnerability Scan ( used for scanning package.json in node & pom.xml file in java ) --> trivy --> Generated report & saves it as an artifact
- Build application --> Generates Artifact ( jar / war )--> Maven  --> mvm package 
- Push artifact to Repository --> Used for Artifact Versioning --> Nexus --> mvm deploy ( but deploy to nexus registry , check ai for commands )
- Build & Tag Docker image
- Docker image vulnerability Scan --> Trivy
- Push Docker image to AWS ECR Repository
- Deploy Image to ECS Service
- DAST --> OWASP ZAP
- Checkov IaC security scanning (Terraform + K8s manifests)

<img width="623" height="416" alt="image" src="https://github.com/user-attachments/assets/83917fdd-4199-4c9d-b45d-087fbbbf6757" />

---
✅  Among the above stages which all stages can go in parallel 

---
✅ How are code dependencies ensured?
- java --> pom.xml
- node --> package-lock.json

---
✅  Difference between npm install vs npm ci
- <img width="520" height="434" alt="image" src="https://github.com/user-attachments/assets/ced0414a-8cd6-453d-9642-7e96b1d5c739" />
- <img width="485" height="417" alt="image" src="https://github.com/user-attachments/assets/f44f5272-ab42-429b-99f3-0b920a3545a0" />
- <img width="421" height="155" alt="image" src="https://github.com/user-attachments/assets/4210ef7e-50d8-4459-92de-c4727683547f" />


| Feature                            | `npm install`                   | `npm ci`                    |
| ---------------------------------- | ------------------------------- | --------------------------- |
| Purpose                            | General install (dev + updates) | Clean, reproducible install |
| Uses `package-lock.json` strictly? | ❌ No (can update it)            | ✅ Yes (must match exactly)  |
| Deletes `node_modules` first?      | ❌ No                            | ✅ Yes                       |
| Speed                              | Slower                          | Faster                      |
| Deterministic builds               | ❌ Not guaranteed                | ✅ Guaranteed                |
| Use case                           | Local development               | CI/CD pipelines             |

---
✅ why should i use nexus repository along with aws ecr ?
- For artifacts versioning & Lifecycle

---
✅ Difference between package.json & package.lock.json
- <img width="473" height="303" alt="image" src="https://github.com/user-attachments/assets/0236a96b-b829-4efb-ab01-708319e38efb" />
- <img width="534" height="340" alt="image" src="https://github.com/user-attachments/assets/49976270-f45d-4b5c-81df-2bdcab1ed9c3" />
- <img width="442" height="445" alt="image" src="https://github.com/user-attachments/assets/d8c8df8f-f158-4b16-be79-9ff243c71e25" />
- <img width="448" height="398" alt="image" src="https://github.com/user-attachments/assets/178d6425-7703-49e1-b488-ef9cdcc96a14" />



| Feature                   | `package.json`               | `package-lock.json`             |
| ------------------------- | ---------------------------- | ------------------------------- |
| Purpose                   | Defines project dependencies | Locks exact dependency versions |
| Created by                | Developer (manual/edit)      | Automatically by npm            |
| Version flexibility       | Uses ranges (`^`, `~`)       | Exact versions (no ranges)      |
| Includes sub-dependencies | ❌ No                         | ✅ Yes (full dependency tree)    |
| Used for install          | ✅ Yes                        | ✅ Yes (priority in npm)         |
| Editable                  | ✅ Yes                        | ❌ No (should not edit manually) |
| Deterministic builds      | ❌ No                         | ✅ Yes                           |


---
✅  Maven build cycle process
- Maven has three lifecycles --> Clean (for removing old builds) --> Default (for building and deploying the application) -->  Site (for generating documentation).
- The Default lifecycle is the most important as it handles the complete build process.
- <img width="301" height="263" alt="image" src="https://github.com/user-attachments/assets/49ab700f-2a52-4050-a389-7cc1953d5d28" />

- <img width="564" height="470" alt="image" src="https://github.com/user-attachments/assets/4eaddac2-1193-421e-b8fb-c7d0ab019ea9" />


---
✅ Maven Vs Gradle

---

✅ Artifactory Concepts --> repositories ( maven/npm/pypi/nuget/docker) , permissions , retention , replication , build info .

---

✅ EXplain nexus artifactory & jFrog artifactory

---

✅ Sonarqube concepts --> quality gates , quality profiles , project onboarding , branch/PR analysis 

---
✅ What is “Shift Left Security”?
- Shift Left Security means integrating security earlier in the SDLC instead of testing security only at the final stage.
- In our CI/CD pipeline, we implemented shift-left by integrating SonarQube for SAST, TruffleHog for secret detection, Trivy for container scanning, and Checkov for Terraform IaC scanning.
- This allowed us to catch vulnerabilities during the pull request stage itself, preventing insecure code or misconfigured infrastructure from reaching production.

---
✅ Sonarqube
- Tool used in SAST
- https://youtu.be/r2UVTDpIUj8?si=-_itvd9yclLW1s4j --> DevOps Shack

---
✅ Trivy
- Tool used in SCA
- Trivy command used --> trivy fs --format table -o trivy-fs-report.html
- https://youtu.be/dwce6Yl9N9Q?si=aiYFls_loIIN7PRu --> DevOps Shack

---
✅ OWASP ZAP
- Tool used in DAST
- https://youtu.be/DjUPVrFHd1I?si=Tz2FfKUvfM-1LfIm --> DevOps Shack

---

✅ OWASP Top 10 security issues
- https://youtu.be/cLOXIodlw74?si=7dWVwbcxbw-0dYmM --> DevOps Shack

OWASP Top 10 security issues
- Broken Access Control
- Cryptographic Failure
- Injection
- Insecure Design
- Security Misconfiguration
- Vulnerable and Outdated Components
- Identification & Authentication Failures
- Security Logging and monitoring failures
- Server Side Request Forgery (SSRF)

---

✅ What is SAST ( Static Application Security Testing )
- Scans source code
- Does not run the application
- Finds insecure coding patterns
- Best used early (IDE / Pull Requests)

Examples:
- Hardcoded secrets --> Storing sensitive data like passwords or API keys directly in your code where anyone who sees the file can steal them.
- SQL injection patterns --> Building database queries by gluing strings together, which lets hackers "trick" your database into deleting data or leaking secrets.
- Command execution risks --> Passing user input directly to your operating system, allowing a hacker to run any command (like format C:) on your server.
- Insecure Deserialization (pickle) --> Using the pickle tool on data from the internet, which can automatically run hidden malicious code the moment the file is opened.
- Arbitrary Code Execution (eval/exec) --> Using functions that turn text into live code, effectively giving a stranger the keyboard to your application.
- Unsafe YAML Loading (yaml.load) --> Opening configuration files in a way that allows the file itself to trigger Python commands during the reading process.
- Path Traversal (tarfile.extractall) --> Unzipping files without checking their names, which can let a malicious file overwrite important system files outside your project folder.
- Insecure SSL/TLS (verify=False) --> Turning off "security checks" for internet connections, making it easy for hackers to spy on your encrypted data.
- Weak Cryptography (MD5/SHA1 usage) --> Using "broken" mathematical formulas to hide data that modern computers can crack in seconds.
- Insecure Temp Files (tempfile.mktemp) --> Creating a temporary file name without instantly "locking" it, creating a tiny window of time for a hacker to swap it with a malicious file.

---

✅ What is SCA ( Software Composition Analysis )
- Scans dependencies
- Matches versions against known CVEs
- Answers: “Are we using vulnerable packages?”
- Even perfect code can be insecure because of vulnerable libraries.

---

✅ What is DAST ( Dynamic Application Security Testing _
- Attacks a running application
- No access to source code
- Simulates real attackers
- Finds exploitable vulnerabilities

---

<img width="365" height="191" alt="image" src="https://github.com/user-attachments/assets/a495c126-adcb-4c0f-b472-b173e4ab2a4d" />

---
---
---
                            #### Github Actions ####
---
---
---

✅ Dependabot vs CodeQL
- <img width="935" height="278" alt="image" src="https://github.com/user-attachments/assets/158bb5f0-b316-45ef-91a2-14bb6326f95f" />
- <img width="559" height="433" alt="image" src="https://github.com/user-attachments/assets/9ba58705-33f9-414e-8cd8-781b54d9fef6" />


| Feature       | **Dependabot**                      | **CodeQL**                                 |
| ------------- | ----------------------------------- | ------------------------------------------ |
| Type          | SCA (Software Composition Analysis) | SAST (Static Application Security Testing) |
| Focus         | Vulnerable dependencies             | Vulnerabilities in your code               |
| What it scans | `package.json`, `pom.xml`, etc.     | Source code (Java, JS, Python, etc.)       |
| Output        | PRs to update dependencies          | Security alerts & reports                  |
| Fix method    | Auto PRs                            | Manual code fix                            |
| Trigger       | Scheduled / on push                 | On push / PR / scheduled                   |
| Example issue | Log4j vulnerability                 | SQL Injection                              |

---
✅ suppose i have deployed ecs task definition of frontend service through terraform & in it the container image should reference the image in ecr frontend repo with specific hash of lastest docker image build commit . how can i do it through pipeline
- In the pipeline, I build and push the Docker image to ECR using the commit SHA as the tag. Then I pass this tag as a variable to Terraform, which updates the ECS task definition with the new image version, triggering a deployment

---
✅ what is image digest ?
- Image digest is a SHA256 hash that uniquely and immutably identifies a Docker image

---
✅ Dependency graph in github

---
✅ connect sonarqube with github action 
- use sonaeqube URL , generate token from sonarqube dashboard & pass it through github action pipeline
- For quality gates , you need to generate webhook URL from sonarqube dashboard . it is used in github actions if we need to fail piplines if gates requirements not satisfied

---
✅ connect Trivy with github action 
- use sonaeqube URL , generate token from sonarqube dashboard & pass it through github action pipeline

---
✅ Connect nexus registry with github action 
- <img width="670" height="398" alt="image" src="https://github.com/user-attachments/assets/439344a2-5cc5-4a13-8bc5-d914ce5b52fb" />


---
✅ explain where  u will use matrix strategies  , composite actions , action versioning & publishing , signed actions / pinning , github packages 

---

✅ Dependabot config across ecosystems , scheduling , grouping , ignore policies , security update handling

✅ how will u pass artifacts between different jobs and different pipelines

✅  how will u make pipeline faster ? explain with respect to github cache 

✅ how will u invoke manual , push , pull , reusable workflow , cron workflow

✅ explain ci & cd with different pipelines

✅ explain different plugins which u were using and explain how was it connected with github actions

✅ explain sast , dca , dast , unit testing and image vulnerabilty scans with sonarqube

✅ explain dependabots , branch protection rules , codeowner file 

✅ Workflows --> Events --> Jobs --> Steps --> Runners

✅ if one job depends on another how will u do it 

✅ when will use github hosted runner vs self hosted runner

✅ using roles to connect to aws

✅ How do you implement approval before production deployment? explain with respect to environments , required approvers and wait timers 

how will u do blue green deployment using terraform in github action pipeline ? --> use terraform + codedeploy
Secrets leaked in logs. What will you do?

How do you pass data between jobs? explain artifacts and outputs

Do jobs share filesystem?

How do you rollback a failed deployment?

What happens if a step fails? --> By default → job fails --> Can override with continue-on-error: true

Concurrency control

Artifact vs Cache

Workflow optimization

when will u use matrix 

codeql scanning

in security tab , there are 3 things ( dependabots alerts --> get notified when your dependincies has a vulneribility , code scanning alerts --> automatically detect common vulnerabilities and coding errors , secret scanning alerts --> get notified when a secret is pushed to the repository )

there are 2 developers . both are working on 2 different files so there is no conlict . developer 1 pushes code at 5pm & developer 2 pushes code ar 5:01 pm . there is no sense in running the pipeline 2 times as when developer 2 will push the code he needs to do a git pull first . how will u make sure only the pipeline runs for teh 2nd developer ?  --> I would enable branch-level concurrency control in CI/CD so that when a new commit is pushed, any currently running pipeline for that branch is automatically cancelled. This ensures only the latest commit triggers the pipeline, avoiding redundant executions and saving compute cost.


How would you design a secure CI/CD pipeline for production?

How do you handle secret management in pipelines?

How do you implement least privilege access across environments?


How do you reuse workflows across repositories?

How to manage large workflow files efficiently?

What's the difference between public and private workflow repositories?

How to implement workflow concurrency?

How do you handle failed workflows?

How do you reuse workflows across repositories?

How to manage large workflow files efficiently?

What's the difference between public and private workflow repositories?

How to implement workflow concurrency?

How do you handle failed workflows?
