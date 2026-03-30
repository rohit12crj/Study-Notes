✅ Videos
- https://www.youtube.com/playlist?list=PLdpzxOOAlwvLRR_KPxSKCJMf2qDNKrGd-  --> DevSecOps Playlist ( Abhishek )
- https://github.com/iam-veeramalla/DevSecOps-Zero-to-Hero/ --> Github Repo ( Abhishek )
- https://www.youtube.com/watch?v=NnkUGzaqqOc&t=18s --> DevSecOps Pipeline --> DevOps Shack
- https://www.youtube.com/watch?v=fuiTqI3noTo --> --> DevSecOps Pipeline --> Abhishek
  
---
✅ Pipeline Steps ( Java Based Application )
- compile --> to find out syntax based errors
- unit testing --> to check functionality of code --> sonarqube
- code quality check --> sonarqube --> 
- vulnerability Scan --> trivy -->
- Build application --> Generates Artifact --> Maven
- Push artifact to Repository --> Used for Artifact Versioning --> Nexus
- Build & Tag Docker image
- Docker image vulnerability Scan --> Trivy
- Push Docker image to AWS ECR Repository
- Deploy Image to ECS Service
- DAST Scan

<img width="623" height="416" alt="image" src="https://github.com/user-attachments/assets/83917fdd-4199-4c9d-b45d-087fbbbf6757" />

---
✅ why should i use nexus repository along with aws ecr ?

---
✅  Maven build cycle process

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
