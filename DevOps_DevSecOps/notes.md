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
