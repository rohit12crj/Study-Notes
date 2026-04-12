✅ Videos
- https://youtu.be/nHlL_v4wCrM?si=O10oQHPijmDT5qJp  --> SRE Questions
- https://youtu.be/0J9xglafVuI?si=uK4obcxYLTd5Jpfj --> pagerduty --> used for end to end incident management , important for SRE , no need to go through the video ,just for reference

---
✅ Throughput vs latency

---
✅  p50 vs p95 latency

---
✅ what is a runbook 

---
✅ What is the difference between monitoring , observability & tracing

---
✅ What is blameless postmortem

---
✅ Your SLO is consitently violated. What do you do

---
✅ Explain the incident response lifecycle
- Prometheus → Alertmanager → PagerDuty → Auto-create incident

---

✅ What is chaos engineering

---
| Metric   | Full Form                     | Definition                                                           | What it tells                         |
| -------- | ----------------------------- | -------------------------------------------------------------------- | ------------------------------------- |
| **MTTR** | Mean Time To Repair (Recover) | Average time taken to **restore a system after a failure**           | How fast you **recover**              |
| **MTBF** | Mean Time Between Failures    | Average time a system operates **without failing**                   | How **reliable/stable** the system is |
| **MTTA** | Mean Time To Acknowledge      | Average time taken to **acknowledge an alert after it is triggered** | How fast the team **responds**        |
| **MTTD** | Mean Time To Detect           | Average time taken to **detect an issue after it occurs**            | How fast issues are **detected**      |

---
✅ What is an Error Budget, SLA , SLO , SLI
- An Error Budget is the maximum amount of failure or downtime that a system is allowed while still meeting its reliability target (SLO).
- It is mainly used in Site Reliability Engineering (SRE) to balance system reliability vs development speed.
- In our project we had an SLO of 99.9% availability, which gave us an error budget of roughly 43 minutes per month.
- We monitored SLIs such as API success rate and latency using Prometheus and Grafana.
- If the error budget was consumed too quickly, we would freeze new deployments and focus on reliability improvements like fixing bugs, improving monitoring, or scaling infrastructure.

<img width="589" height="341" alt="image" src="https://github.com/user-attachments/assets/04dbf2c5-f080-4a86-80ee-18f712eb45a9" />


| Term                              | Meaning                        |
| --------------------------------- | ------------------------------ |
| **SLA (Service Level Agreement)** | what we promise externally     |
| **SLO (Service Level Objective)** | what do we want to achieve     |
| **SLI (Service Level Indicator)** | what is actually hapenning     |
| **Error Budget**                  | Allowed failure within the SLO |

<img width="469" height="281" alt="image" src="https://github.com/user-attachments/assets/6d3e6c52-682e-470b-9ff2-3d0d20608906" />

---
✅ Can you explain your day-to-day responsibilities in your current DevOps role?

---
✅ What is toil, and how do you reduce it in DevOps/SRE teams?
- Toil is repetitive manual operational work that does not add long-term value and can be automated. Examples include manual deployments, restarting services, or provisioning infrastructure.
- In DevOps/SRE teams we reduce toil by implementing automation, CI/CD pipelines, Infrastructure as Code using Terraform, self-healing systems like Kubernetes, and proper monitoring and alerting.
- This allows engineers to focus more on system reliability and improvements instead of repetitive operational tasks.

---
✅ Why is horizontal scaling generally preferred over vertical scaling?

---
✅ What are the Golden Signals of Monitoring?

<img width="559" height="328" alt="image" src="https://github.com/user-attachments/assets/69cd2ab3-2acb-46fb-b87b-84cb18168939" />

---




