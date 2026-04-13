✅ Videos
- https://www.youtube.com/playlist?list=PLdpzxOOAlwvLRR_KPxSKCJMf2qDNKrGd-  --> DevSecOps Playlist ( Abhishek )
- https://github.com/iam-veeramalla/DevSecOps-Zero-to-Hero/ --> Github Repo ( Abhishek )
- https://www.youtube.com/watch?v=NnkUGzaqqOc&t=18s --> DevSecOps Pipeline with Jenkins --> DevOps Shack
- https://www.youtube.com/watch?v=FTrTFOLbdm4&list=PLAdTNzDIZj_9c-o43WJ7yYBa5YEAkWiOv&index=24 --> DevSecOps Pipeline with Github Actions --> DevOps Shack
- https://www.youtube.com/watch?v=fuiTqI3noTo --> --> DevSecOps Pipeline --> Abhishek
  
---
✅ Pipeline Steps ( Java OR Node Based Application  )
- compile --> to find out syntax based errors --> Maven --> mvm compile / npm install / npm ci ( u need install maven in github action runner first)
- unit testing --> to check functionality of code --> Maven --> mvm test / npm test
- <img width="384" height="317" alt="image" src="https://github.com/user-attachments/assets/ac426d83-335c-450e-8059-9b0a209fd353" />
- SAST --> code quality check -->  --> use quality gates . fail pipeline if quality gates requirement not satisfied --> sonarqube
- SCA --> package vulnerability Scan ( used for scanning package.json in node & pom.xml file in java ) --> trivy --> Generated report & saves it as an artifact
- Build application --> Generates Artifact ( jar / war )--> Maven  --> mvm package / npm run build 
- Push artifact to Repository --> Used for Artifact Versioning --> Nexus --> mvm deploy ( but deploy to nexus registry , check ai for commands ) / npm deploy
- Build & Tag Docker image through Dockerfile using the artifact generated in the build step
- Docker image vulnerability Scan --> Trivy
- Push Docker image to AWS ECR Repository
- Deploy Image to ECS Service
- DAST --> OWASP ZAP
- Checkov --> (Terraform + K8s manifests) scanning
- <img width="623" height="416" alt="image" src="https://github.com/user-attachments/assets/83917fdd-4199-4c9d-b45d-087fbbbf6757" />

---
✅ explain ci & cd with different pipelines & why to follow this approach 
- ci pipeline worflow  final step --> Push Docker Image to ECR Repo
- cd pipeline workflow --> download ECR Images --> updates Task definition ( if any ) --> deploy Image to ECS Services ( Update Task definition image section if necessary ) --> DAST Scan
- cd is seperate because if in ECS 2 task conatiners are required instead of 1 , then we should just modify the cd part ( Update task definition ) , no need to build the images .
- ci pipeline should not run when task definition file is modified in the cd folder as it would create loop ( updating cd folder triggers ci pipleine which again triggers cd pipeline ) . use path ignore in ci pipeline to fix this 
- cd pipleine should only run when ci is completed successfully without errors , check below images how it can be done in github actions
- <img width="471" height="311" alt="image" src="https://github.com/user-attachments/assets/c86dc2e1-83fb-4050-974e-54d29e8a4723" />
- <img width="513" height="335" alt="image" src="https://github.com/user-attachments/assets/5efe9241-2b18-43e1-81d4-a97bf221bb2e" />
- <img width="368" height="163" alt="image" src="https://github.com/user-attachments/assets/c87c2f23-bebe-4f65-839e-bc7c4ebcc61b" />

---
✅  Among the above stages which all stages can go in parallel 

---
✅ what are artifacts ? what is it necessary ?  what are the differnet types of artifacts generated ?
- In a DevOps pipeline, artifacts like JAR and WAR files are packaged, deployable outputs of your build stage. Think of them as the final product that moves across pipeline stages (build → test → deploy)
- <img width="368" height="281" alt="image" src="https://github.com/user-attachments/assets/3acee9d0-bfce-48eb-bec5-f3ac49001e53" />
- <img width="377" height="132" alt="image" src="https://github.com/user-attachments/assets/40fab27d-3a74-467f-90a4-ebbb51417699" />
- java --> jar/war
- node --> need to check

---
✅ explain how jar artifact is used in dockerfile
- <img width="376" height="264" alt="image" src="https://github.com/user-attachments/assets/f5ad38d1-25e0-445e-83da-78449a13762c" />
- <img width="319" height="239" alt="image" src="https://github.com/user-attachments/assets/c596cdc9-fcd3-4776-af99-0048f376eb46" />

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
- https://www.youtube.com/watch?v=-KqyFvEJYz0&t=202s  --> Devops Shack
- Maven has three lifecycles --> Clean (for removing old builds) --> Default (for building and deploying the application) -->  Site (for generating documentation).
- The Default lifecycle is the most important as it handles the complete build process.
- <img width="301" height="263" alt="image" src="https://github.com/user-attachments/assets/49ab700f-2a52-4050-a389-7cc1953d5d28" />
- <img width="564" height="470" alt="image" src="https://github.com/user-attachments/assets/4eaddac2-1193-421e-b8fb-c7d0ab019ea9" />


---
✅ Maven Vs Gradle

---
✅ Nexus Artifactory / Registry Concepts
- Different Repo Types ( Hosted , Proxy , Group )
- <img width="581" height="201" alt="image" src="https://github.com/user-attachments/assets/1d12a10d-8089-4fb1-bb64-93daa48e2a2a" />
- Snapshots are mutable development versions updated frequently, while releases are immutable, stable artifacts meant for production use
- <img width="529" height="236" alt="image" src="https://github.com/user-attachments/assets/f90e1950-72bd-476e-ab0b-507f9bdbed6f" />
- repositories ( maven/npm/pypi/nuget/docker) , permissions , retention , replication , build info .
- https://www.youtube.com/watch?v=zH3cjXQmqJo&t=1147s  --> Devops Shack

---
✅ What is “Shift Left Security”?
- Shift Left Security means integrating security earlier in the SDLC instead of testing security only at the final stage.
- In our CI/CD pipeline, we implemented shift-left by integrating SonarQube for SAST, TruffleHog for secret detection, Trivy for container scanning, and Checkov for Terraform IaC scanning.
- This allowed us to catch vulnerabilities during the pull request stage itself, preventing insecure code or misconfigured infrastructure from reaching production.

---
✅ Sonarqube
- Tool used in SAST
- https://youtu.be/r2UVTDpIUj8?si=-_itvd9yclLW1s4j --> DevOps Shack
- explain quality gates , quality profiles , code coverage , code smells , project onboarding , branch/PR analysis
- https://www.youtube.com/watch?v=qyYsLVZDieU --> Good Video
- <img width="887" height="377" alt="image" src="https://github.com/user-attachments/assets/886d552e-ed0b-4b6f-ab3c-7bfc6cfbe971" />


---
✅ Trivy
- Tool used in SCA
- Trivy command used --> trivy fs --format table -o trivy-fs-report.html
- https://youtu.be/dwce6Yl9N9Q?si=aiYFls_loIIN7PRu --> DevOps Shack
- SBOM with Trivy Explained (CycloneDX vs SPDX) --> https://www.youtube.com/watch?v=AsPEbZ21PdA
- Generate SBOM with Trivy --> https://www.youtube.com/watch?v=QOVXr8Zmm_Q
- Centralized Trivy server for all pipleines to call --> https://www.youtube.com/watch?v=z65g9gFIYEA&list=PLBr8obKbpkYsGrdKwFXqRTYcvv1KELCLn&index=16
- integrating Trivy with Jenkins & generating SBOM --> https://www.youtube.com/watch?v=FdbWJzvymkg&list=PLBr8obKbpkYsGrdKwFXqRTYcvv1KELCLn&index=17
- <img width="465" height="231" alt="image" src="https://github.com/user-attachments/assets/28ecd50f-c56a-4b52-b549-624e33837ef6" />
- Github repo for Jenkins File integrating with Trivy --> https://github.com/theshubhamgour/trivy-tutorial/blob/main/9-trivy-artifact-project/Jenkinsfile

---
✅ OWASP ZAP
- Tool used in DAST
- https://youtu.be/DjUPVrFHd1I?si=Tz2FfKUvfM-1LfIm --> DevOps Shack --> video made private
- https://www.youtube.com/watch?v=wLfRz7rRsH4

---

✅ OWASP Top 10 security issues
- https://youtu.be/cLOXIodlw74?si=7dWVwbcxbw-0dYmM --> DevOps Shack --> video made private
- https://www.youtube.com/watch?v=HE244moNuXE --> Theory
- https://www.youtube.com/playlist?list=PLa2xctTiNSCiUcETvRT7lbYuKTpgKDchD  --> Playlist --> Practical

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
                                                      #### Github Actions 
---
---
---
✅ Reusable workflow vs Composite Action
- Reusable workflows are used to share entire CI/CD pipelines across repositories, while composite actions are used to reuse a set of steps within a job
- <img width="481" height="432" alt="image" src="https://github.com/user-attachments/assets/86ffe1b8-79a5-4ee6-970d-9a50f79f9b95" />
| Feature          | Reusable Workflow       | Composite Action      |
| ---------------- | ----------------------- | --------------------- |
| Level            | Job / Workflow          | Step                  |
| Defined in       | `.github/workflows`     | `action.yml`          |
| Supports jobs    | ✅ Yes                   | ❌ No                  |
| Multiple runners | ✅ Yes                   | ❌ No                  |
| Secrets          | ✅ Yes                   | ⚠️ Limited            |
| Use case         | CI/CD pipelines         | Step reuse            |
| Called via       | `uses:` (workflow path) | `uses:` (action path) |


---
✅ How to login to github hosted runners for troubleshooting
- use tmate
- https://www.youtube.com/watch?v=HWGot0-P1Ps

---
✅ how to write pipeline file for monorepos
- <img width="590" height="329" alt="image" src="https://github.com/user-attachments/assets/0f0549f0-de11-4635-be39-34b7c085b34d" />
- <img width="545" height="371" alt="image" src="https://github.com/user-attachments/assets/30a37e86-0b15-47a4-9396-78ca71502646" />
- <img width="517" height="365" alt="image" src="https://github.com/user-attachments/assets/2b7b5f0b-b643-43aa-8cab-7186ebcc799b" />
- <img width="570" height="268" alt="image" src="https://github.com/user-attachments/assets/a9f0af84-38b6-464d-9a45-8cd4e2dc830d" />
- <img width="569" height="208" alt="image" src="https://github.com/user-attachments/assets/52bc8535-f169-4e81-927b-986404a9a594" />

---
✅ Github SBOM ( Software Bill of Materials )
- <img width="815" height="437" alt="image" src="https://github.com/user-attachments/assets/291ac125-7302-414f-91e4-5fb42762771d" />

---
✅ Branch Protection rules
- <img width="687" height="254" alt="image" src="https://github.com/user-attachments/assets/eb474c99-3a82-41b6-b6e4-3226157a5e44" />

---
✅ what is github issue
- A GitHub Issue is a built-in way in GitHub to track work, bugs, ideas, and discussions related to a repository.

---
✅ my terraform deletion taking too much time . how many minutes will be github pipleine wait ?
- GitHub Actions will wait up to the job timeout limit — by default 360 minutes (6 hours).
- <img width="445" height="412" alt="image" src="https://github.com/user-attachments/assets/e7e18562-487d-40fc-adb3-91802b56917b" />
- <img width="574" height="334" alt="image" src="https://github.com/user-attachments/assets/50340f76-0cae-4b4c-a720-cd58d78c8e08" />

---
✅ Github Actions specific Devsecops tools
- https://www.youtube.com/watch?v=BNCv2O6vQC8
- https://www.youtube.com/watch?v=PHmnkhLZWj0&t=240s
- SAST --> code scanning --> By adding a workflow (codeql.yml) inside .github/workflows/
- SCA --> dependency scanning  --> Create dependabot.yml in .github/
- secret scanning  --> no yaml file presnet . But can configure push
- no DAST tool from github
- How do you control updates in dependabot ?
- <img width="212" height="88" alt="image" src="https://github.com/user-attachments/assets/efdacda1-60da-4aaf-ae69-b40d8b5c374e" />
- <img width="836" height="480" alt="image" src="https://github.com/user-attachments/assets/d4a6ecdf-b264-48b2-b004-59132311a81f" />
- <img width="565" height="308" alt="image" src="https://github.com/user-attachments/assets/5972522e-c7d8-472e-9e56-d835a7bda2ae" />
- <img width="560" height="127" alt="image" src="https://github.com/user-attachments/assets/cd898793-3183-43a9-87ed-27d9f7a65081" />
- <img width="409" height="298" alt="image" src="https://github.com/user-attachments/assets/f39f93d8-9173-4576-a701-6f64d25c95f9" />
- <img width="386" height="140" alt="image" src="https://github.com/user-attachments/assets/7a1cebd6-7a59-45f8-95b9-bf344aa756ca" />

---
✅ Different types of workflow 
- <img width="522" height="257" alt="image" src="https://github.com/user-attachments/assets/9f9fa9e7-412d-430e-b1aa-3d1c45610ea4" />
- <img width="539" height="252" alt="image" src="https://github.com/user-attachments/assets/e96fda19-e7cb-42a9-b9c8-6dd8f670ffbe" />
- <img width="535" height="266" alt="image" src="https://github.com/user-attachments/assets/0eb875c3-a37d-4aca-8891-5a6dc182b3c0" />
- <img width="511" height="254" alt="image" src="https://github.com/user-attachments/assets/a5c5cbcc-845a-44a9-a732-85e8cba0236b" />
- <img width="472" height="229" alt="image" src="https://github.com/user-attachments/assets/f6c59f95-1944-4d96-ad57-4fe1cfe29492" />
- <img width="476" height="117" alt="image" src="https://github.com/user-attachments/assets/1c0f4ad8-0b12-4be0-9b7e-ff1cbbbbaf0d" />

---
✅ Github Cache --> https://www.youtube.com/watch?v=7PVUjRXUY0o

---
✅ GitHub Actions Artifacts vs Github Packages vs GitHub Container Registry (GHCR)
- Only Docker images should be stored in GitHub Container Registry (GHCR)
- npm packages & other similar packages should be stored in Github Packages . persistent storage
- GitHub Actions artifacts are scoped to a single workflow run and cannot be directly shared across pipelines . To share artifacts across workflows, we typically use GitHub Packages
- https://www.youtube.com/watch?v=p9ismkYDxWQ

| Feature    | GitHub Actions Artifacts    | GitHub Packages            |
| ---------- | --------------------------- | -------------------------- |
| Purpose    | Temporary build artifacts   | Long-term artifact storage |
| Retention  | Short-term                  | Persistent                 |
| Versioning | ❌ No                        | ✅ Yes                      |
| Use case   | Pipeline intermediate files | Production-ready packages  |
| Example    | `.zip`, logs                | npm package  |

---
✅ I want to use azure AD users to have access to github repos . how can i set it up ?

---
✅ How will u secure Github Runners ?

---
✅ How will u configure EKS Github runners ?

---
✅ Github Action Template ?

---
✅ Dynamic Github Action Pipeline ? Master child Pipeline

---
✅ Pipeline Tag strategy , artifacts caching strategy , Cache invalidation strategy , testing strategy

---
✅ Dependabot vs CodeQL . Use of Dependabot.yml file 
- <img width="216" height="274" alt="image" src="https://github.com/user-attachments/assets/9a9b7099-af37-4022-9a77-88da086a3deb" />
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
- In the pipeline, I build and push the Docker image to ECR using the commit SHA as the tag. Then I pass this tag as a variable to Terraform through env variables in github action pipeline file , which updates the ECS task definition with the new image version, triggering a deployment

---
✅ what is image digest ?
- Image digest is a SHA256 hash that uniquely and immutably identifies a Docker image

---
✅ Dependency graph in github ? DAG ?

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
