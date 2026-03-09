
https://www.youtube.com/playlist?list=PLdpzxOOAlwvLRR_KPxSKCJMf2qDNKrGd-  --> DevSecOps Playlist ( Abhishek )

https://github.com/iam-veeramalla/DevSecOps-Zero-to-Hero/ --> Github Repo ( Abhishek )

---

✅ What is “Shift Left Security”?
- Shift Left Security means integrating security earlier in the SDLC instead of testing security only at the final stage.
- In our CI/CD pipeline, we implemented shift-left by integrating SonarQube for SAST, TruffleHog for secret detection, Trivy for container scanning, and Checkov for Terraform IaC scanning.
- This allowed us to catch vulnerabilities during the pull request stage itself, preventing insecure code or misconfigured infrastructure from reaching production.

---

✅ Trivy --> https://youtu.be/dwce6Yl9N9Q?si=aiYFls_loIIN7PRu  ( DevOps Shack )

---

✅ OWASP ZAP --> https://youtu.be/DjUPVrFHd1I?si=Tz2FfKUvfM-1LfIm  ( DevOps Shack )

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

Scans third-party libraries

Finds vulnerable dependencies

What is Infrastructure as Code (IaC) Security?  Checkov

What is Policy as Code?

Difference between DevOps and DevSecOps?

Developer pushes vulnerable library. What do you do?

container security

k8 security

Example: Snyk, Dependabot

