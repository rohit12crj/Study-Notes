✅ rto & rpo & what were these values in your project & how did u achieve this ?
- RTO --> The maximum acceptable time to restore a system after a failure. If RTO = 30 minutes, the system must be restored within 30 minutes after an outage.
- RPO --> The maximum acceptable amount of data loss measured in time. If RPO = 5 minutes, the system can only lose 5 minutes of data.
- In our project the RTO was around 10–15 minutes and RPO was less than 5 minutes.
- We achieved this using Multi-AZ architecture in AWS, where ECS services were deployed across multiple AZs behind an ALB.
- The database was RDS Multi-AZ, which provides synchronous replication and automatic failover within 1–2 minutes.
- We also configured automated backups, snapshots, and S3 cross-region replication for disaster recovery.
- Infrastructure was managed using Terraform, so in case of a disaster we could recreate the infrastructure quickly through our CI/CD pipelines.

---
✅ 

---

✅ explain HA for all aws services in your project ?

Can you explain your day-to-day responsibilities in your current DevOps role?

pagerduty --> used for end to end incident management --> https://youtu.be/0J9xglafVuI?si=uK4obcxYLTd5Jpfj  --> important for SRE --> no need to go through the video --> just for reference

Prometheus → Alertmanager → PagerDuty → Auto-create incident

---

✅ What is an Error Budget, SLA , SLO , SLI
- An Error Budget is the maximum amount of failure or downtime that a system is allowed while still meeting its reliability target (SLO).
- It is mainly used in Site Reliability Engineering (SRE) to balance system reliability vs development speed.
- In our project we had an SLO of 99.9% availability, which gave us an error budget of roughly 43 minutes per month.
- We monitored SLIs such as API success rate and latency using Prometheus and Grafana.
- If the error budget was consumed too quickly, we would freeze new deployments and focus on reliability improvements like fixing bugs, improving monitoring, or scaling infrastructure.

| Term                              | Meaning                        |
| --------------------------------- | ------------------------------ |
| **SLA (Service Level Agreement)** | Contract with customers        |
| **SLO (Service Level Objective)** | Target reliability goal        |
| **SLI (Service Level Indicator)** | Actual measured metric         |
| **Error Budget**                  | Allowed failure within the SLO |

<img width="469" height="281" alt="image" src="https://github.com/user-attachments/assets/6d3e6c52-682e-470b-9ff2-3d0d20608906" />

---

What is the default Prometheus port number?

---

✅ What is toil, and how do you reduce it in DevOps/SRE teams?
- Toil is repetitive manual operational work that does not add long-term value and can be automated. Examples include manual deployments, restarting services, or provisioning infrastructure.
- In DevOps/SRE teams we reduce toil by implementing automation, CI/CD pipelines, Infrastructure as Code using Terraform, self-healing systems like Kubernetes, and proper monitoring and alerting.
- This allows engineers to focus more on system reliability and improvements instead of repetitive operational tasks.

---

Why is horizontal scaling generally preferred over vertical scaling?

---

What are the Golden Signals of Monitoring?

<img width="559" height="328" alt="image" src="https://github.com/user-attachments/assets/69cd2ab3-2acb-46fb-b87b-84cb18168939" />

---

What is the difference between Horizontal Scaling and Vertical Scaling? 

) Explain your roles and responsibilities in your current organization as a DevOps Engineer.

2) A Kubernetes pod is crashing continuously. How would you troubleshoot and resolve the issue?

3)The CI/CD pipeline runs successfully, but application traffic is not reaching the service. What could be the reasons, and how would you debug it?

4) What are Liveness and Readiness probes, and why are they important?

5) What is a memory leak, and how does it affect applications?

6) What is Garbage Collection, and how can it impact application performance or CI/CD pipelines?

7) How would you handle two P1 critical production issues occurring at the same time?

8) How do you create a virtual server in a VMware cloud environment?

9) An application is running in a Kubernetes cluster, but traffic is not reaching it. What could be the reasons, and how would you troubleshoot?

10) How do you design and maintain high availability for an application?


How do you troubleshoot a server that is not working?

What automation have you implemented using Ansible?

How do you list Kubernetes pods based on age?

What is the Terraform workflow?

If a Terraform project has 100 resources, how do you create only one?

Differences between DaemonSet, StatefulSet, and Deployment

What is a headless service and how does it work?

What are Kubernetes probes, and how many types are there?

Can we create a StatefulSet without PV and PVC?

Can we attach PV and PVC to a Deployment?

How do you provide S3 access to an EC2 instance?

What is Backstage and how does it work?

What is APM (Application Performance Monitoring)?

Do you worked on Observability, Monitoring and Tracing?


Level 1

How do you configure Jenkins?

How does Blue/Green deployment work?

Is there any downtime in Blue/Green deployment?

A user is unable to access an S3 bucket - how do you troubleshoot?

How do you design a highly available Kubernetes cluster?

How do you serve 50% traffic to Team A and 50% to Team B in Kubernetes?

What are your roles and responsibilities as a DevOps Engineer?

Where do you deploy applications-on-premises or in the cloud?


What is a recent Kubernetes issue you troubleshot?

Which command is used to check pod logs?



