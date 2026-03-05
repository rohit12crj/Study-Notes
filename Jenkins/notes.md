https://www.youtube.com/playlist?list=PLdpzxOOAlwvJDIAQZtMjUUbiVUDfGaCIX  --> Abhishek Playlist

https://youtu.be/SsN0ILXq5Lw?si=5VwjcdU0NiYQRpS8  --> interview questions

https://youtu.be/LAYV7x_aIC0?si=ieOYjnGrUKqT4aeY  --> interview questions

✅ explain ci & cd with different pipelines & why to follow this approach ( wait for abhishek pipeline video )

- ci end goal is docker image creation , cd end goal is to create containers 
- cd is seperate because if in ECS 2 task conatiners are required instead of 1 , then we should just modify the cd part , no need to build the images .
- ci workflow ( git push --> git repo checkout --> install dependencies --> dockerfile build --> )
- cd worflow ( will run only when ci completes without fail & deploys to eks )

how will do unit testing , sast, dca , dast , image vulnerability scans ? explain with respect to sonarqube also ?

share artifacts between different jobs and different stages in same job ?

What are Jenkins jobs?

What is Jenkins Pipeline?

What is a Jenkinsfile?

Declarative vs Scripted Pipeline

What is an Agent in Jenkins?

What is a Node?

What are Jenkins plugins? along with which plugins were u using in your project ? & how did u setup those plugins ?

How does Jenkins store credentials? 

How do you secure Jenkins?

What is Multibranch Pipeline?

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

How do you upgrade Jenkins safely?

Jenkins push or pull? → Pull
Jenkins config file? → config.xml
Language used? → Java
Jenkins pipeline language? → Groovy
Default port? → 8080

Explain Jenkins architecture in production
Controller–Agent architecture


Controller: scheduling, UI, pipeline orchestration


Agents: execute jobs


Communication via JNLP / SSH


Controller should be stateless as much as possible



2. Why should Jenkins controller not run builds?


Single point of failure


Resource contention


Security risk
👉 Controller should only orchestrate



3. How does Jenkins scale horizontally?


Multiple agents


Dynamic agents (Docker / Kubernetes)


Parallel stages


Multibranch pipelines



4. Difference between Node and Executor?


Node: machine running Jenkins agent


Executor: parallel slot on a node



5. What happens if an agent goes down mid-build?


Build fails


Workspace lost (unless externalized)


Jenkins retries only if pipeline supports it



🔹 Pipelines & Groovy
6. Declarative vs Scripted pipeline—when to use scripted?
Use Scripted when:


Complex logic


Dynamic stages


Advanced Groovy control



7. How do you pass data between stages?


Environment variables


Files in workspace


stash / unstash



8. How does stash/unstash work internally?


Compresses files


Stores on controller


Restores on agent
⚠ Not for large artifacts



9. How do you make pipelines reusable?


Shared Libraries


Parameterized pipelines


Template Jenkinsfiles



10. How do you version Jenkins pipelines?


Jenkinsfile in Git


Tagged releases


Shared library versioning



🔹 Security (VERY IMPORTANT)
11. How does Jenkins store secrets?


Encrypted on disk (credentials.xml)


Master key + secret key


Can integrate with Vault



12. Why is Jenkins Script Console dangerous?


Executes arbitrary Groovy on controller


Full system access
👉 Restrict to admins only



13. How do you prevent secret leakage in logs?


Mask passwords


Use credentials binding


Disable set -x


Rotate leaked secrets immediately



14. How do you integrate Jenkins with Vault?


Vault plugin


Dynamic secret injection


No secrets stored in Jenkins



15. How do you implement RBAC in Jenkins?


Role Strategy Plugin


Folder-based permissions


LDAP / SSO integration



🔹 Jenkins + Git
16. How does Multibranch Pipeline detect changes?


Periodic indexing


Webhooks


SCM polling (not recommended)



17. Jenkins pull or push model?
Pull-based


Jenkins pulls code


Triggered by webhook events



18. How do you secure Jenkins webhooks?


GitHub secret token


IP whitelisting


HTTPS only



19. How do you handle mono-repo builds?


Path-based conditions


Selective pipelines


Parallel builds per service



20. How do you avoid rebuilding unchanged code?


ChangeSet detection


Conditional stages


Cache dependencies



🔹 Jenkins + Docker
21. Jenkins inside Docker—good or bad?
✅ Good for:


Easy setup


Reproducibility


❌ Bad for:


Persistent storage


Docker-in-Docker complexity



22. Docker-in-Docker vs Docker socket mount?


DinD: isolated, slower


Socket mount: faster, security risk



23. How do you build Docker images securely?


Non-root user


Multi-stage builds


Scan images


Minimal base images



24. How do you clean up Docker resources?


Post-build cleanup


TTL policies


Kubernetes garbage collection



🔹 Jenkins + Kubernetes (FAANG Favorite)
25. How does Jenkins Kubernetes plugin work?


Controller requests pod


Pod spins up as agent


Executes job


Pod destroyed



26. Benefits of Jenkins on Kubernetes?


Auto-scaling


Ephemeral agents


Cost efficiency


Isolation



27. How do you persist workspace in Kubernetes?


PVC


External artifact storage (S3)


Avoid local storage



28. What happens if Kubernetes pod dies?


Build fails


Restart requires rerun


Use checkpoints carefully



29. How do you limit resource usage per build?


Pod resource limits


Executor limits


Namespace quotas



🔹 Performance & Reliability
30. Jenkins is slow—how do you debug?


Thread dumps


JVM heap analysis


Plugin audit


Disk I/O check



31. How do you tune Jenkins JVM?


Increase heap


GC tuning


Disable unnecessary plugins



32. Why are too many plugins dangerous?


Memory leaks


Compatibility issues


Security vulnerabilities



33. How do you handle flaky tests?


Retry logic


Quarantine tests


Test categorization



34. How do you implement build timeouts?
options { timeout(time: 30, unit: 'MINUTES') }


🔹 Disaster Recovery & Maintenance
35. What should be backed up in Jenkins?


$JENKINS_HOME


Job configs


Credentials


Plugins list



36. Jenkins HA—how do you design it?


Stateless controller


External DB/storage


Backup + fast restore
❗ Jenkins is not truly HA



37. Safe Jenkins upgrade strategy?


Backup first


Upgrade plugins gradually


Test in staging


Use LTS



38. Jenkins data corruption—what now?


Restore backup


Verify filesystem


Audit plugins



🔹 Advanced Pipelines & Deployment
39. How do you implement Blue-Green deployment?


Two environments


Switch traffic via LB


Rollback by traffic flip



40. Canary deployment using Jenkins?


Gradual rollout


Metrics monitoring


Auto rollback



41. How do you handle approvals in Jenkins?


input step


RBAC


Timeout on approvals



42. How do you trigger downstream pipelines?


build step


Events


Artifacts as input



43. How do you share artifacts across pipelines?


Artifact repository (Nexus, Artifactory, S3)


Versioned artifacts



🔹 Observability & Monitoring
44. How do you monitor Jenkins?


Prometheus metrics


JVM metrics


Disk usage


Build queue length



45. Key Jenkins metrics to watch?


Queue size


Build duration


Agent utilization


Failure rate



46. Jenkins logging best practices?


Centralized logs


Log rotation


Avoid sensitive data



🔹 Real FAANG Scenarios
47. Pipeline stuck in queue—why?


No matching agents


Label mismatch


Resource exhaustion



48. Production secret leaked via Jenkins—actions?


Rotate secret immediately


Audit logs


Restrict permissions


Post-mortem



49. Jenkins vs GitHub Actions—when Jenkins?


Complex workflows


Self-hosted infra


Custom agents


Enterprise control



50. If you had to redesign Jenkins today?


Stateless controller


Event-driven pipelines


Native secrets


Immutable infrastructure


Cloud-native by default


If the Cl stage succeeds but the CD stage fails in Jenkins, how would you debug and resolve it?

Which tools have you used to build a complete CI/CD pipeline?

If you encounter a secret-related error in your application or pipeline, how would you troubleshoot it?

How would you design a CI/CD pipeline for 50+ microservices with zero downtime deployment? 

