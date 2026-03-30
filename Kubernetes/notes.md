✅ Videos
- https://youtu.be/QLbfY_Uh63c?si=QPOYoZkWQmIT1GpS --> Real Time Scenario Based Kubernetes Implementations --> DevOps Shack
- https://www.youtube.com/playlist?list=PLdpzxOOAlwvJdsW6A0jCz_3VaANuFMLpc  --> k8s Playlist --> Abhishek
- https://www.youtube.com/playlist?list=PLdpzxOOAlwvIrFBI1farpLS_OSUBXJMLX --> Troubleshooting k8s --> Abhishek 

---
✅ what are the different components of k8s?
-  check k8s_components.html file
-  download the html file & then view

---
✅ explain network flow of all different componenst of k8s?
-  check k8s_network_flow.html file
-  download the html file & then view
  
---
✅ difference between container-d & docker 

---
✅ explain service & it's types ? explain labels & selectors ?

---
✅ For your EKS project explain what all manifest u created 
- namespace --> where u  using single namespace or 3 different namespace ?
- Db --> statefulset + SVC + PVC ( persistent volume claim --> defines size of storage ) + SC ( storage class --> defines whetehr u want to use ebs , efs or fsx ) + NP
- Backend --> Deployment + SVC ( Service --> clusterip ) + NP ( Network Policy --> used for restricting backend pods only to be accessible from Frontend pods ) , Secrets ( For storing Db username & password )
- Frontend --> Deployment + SVC

Note -->  Ingress should ideally be used in place  of SVC ( service ) . if service is used , u need to do port forward also 

Flow = Frontend --> Backend --> DB

---
✅ Through terraform i created an eks cluster . however while writing k8s manifest file i created pvc & sc of ebs type for db stateful set . shouldn't this ebs also be done through terraform ?

<img width="560" height="277" alt="image" src="https://github.com/user-attachments/assets/592b5492-564d-4d73-a700-3ec7fa5cc12b" />

---
✅ to create rds within eks what all u need to create /
- statefulset
- pvc ( persistent volume claim ) --> defines size of storage
- sc ( storage class )  --> defines whetehr u want to use ebs , efs or fsx

---
✅ what is EKS auto mode ?
- Karpenter is used internally by AWS
- Both Control plane & Data plane managed by AWS
- no need to create nodegroup , as soon as any deployment happens , karpenter creates a node & runs the pod on it 

---
✅ how will u connect ur local machine aws cli to use a specific EKS cluster 
<img width="450" height="40" alt="image" src="https://github.com/user-attachments/assets/d940ee09-65b3-408f-b472-c11aa79d4c73" />

---
✅ local k8s setup --> vind ( has gui )

---
✅ user management in k8s --> Border0

---
✅ csi driver & EBS CSI Driver

---
✅ explain network policies & admission controllers & how will u secure backend pods are accessible only from frontend pods ?

---
service account

In my eks cluster my payment pod needs to store data to AWS ebs . How can it do that explain with reference to service account, iam roles , csi driver . also if my pod az changes & my ebs volume is in different az how will it work then ?

daemonset vs deployments ? when to use which with examples 

There is also a default namespace in k8s

notify me if a pod for payment app crashes more than 2 times in 1hr in payment namespace ?

configure alertmanager to send mail emails when a pod for payment app crashes more than 2 times in 1hr in payment namespace . configure promql to find the info from prometheus server ( need to see the correct metric from online ) --> this will only tell what & when it is happening

for live pods we can exec into it & for pods which already crashed & we can't exec , check logs either from cloudwatch or EFK / Opensearch , this can happen only when streaming is enabled --> this will  tell why it is happening

distributed traceing through jaeger --> --> this will tell how it is happening

suppose u need to run a sidecar container in each pod along with the main app . is there a better strategy to implement this ?

If I need a sidecar in every pod, I would not manually modify deployments.
I would implement a Mutating Admission Webhook to auto-inject the sidecar based on namespace or label.
If the use case is networking, I would prefer a service mesh.
If it’s node-level functionality, I would use a DaemonSet instead.

do all manifest files in k8s create pods ? give examples of which manifests create pods & which dont

in my eks email pods within email namespace , it send out email , so it needs to connect to AWS SES . the credentials of SMTP server are stored in AWS Secret manager . how will u connect to secret manager , basically which role u would be attaching the permission & how to rotate the secrets

when u should use daemon set vs sidecar container

pod intent ?

how will u do canary deployment in k8s ?

how does rollback deploymnet work internally ?

what is hpa & how does it scales pods ?

diff between hpa , keda & carpenter ?

why do we still need hpa if karpenter is used ?

How does Service A communicate with Service B inside Kubernetes?

How would you restrict pod-to-pod communication using Calico Network Policies?

How does a pod securely connect to S3 without hardcoding credentials?

How do you implement RBAC policies in Kubernetes?


A production pod is in CrashLoopBackOff. How would you troubleshoot and identify the root cause?

A node shows NotReady status during peak traffic. What steps would you take to diagnose and recover it?

After a new deployment, users report errors. How do you perform a safe rollback with zero downtime?

Application is not accessible via Service (LoadBalancer/NodePort). How do you debug networking issues?

Pods are stuck in ImagePullBackOff from a private registry. What checks will you perform?

ConfigMap is updated but changes are not reflected in running pods. Why and how to fix it?

HPA is configured but pods are not scaling under load. What could be the issue?

Persistent Volume is not mounting in a stateful application. How do you troubleshoot PVC/PV?

What is the current version of K8s you are using in your project?

What was the last production issue you faced and how did you resolve it?

A pod is stuck in CrashLoopBackOff. Logs show failure during initialization - how do you troubleshoot?

How do you enforce tenant isolation in a multi-tenant Kubernetes setup?

During high traffic, your app shows intermittent 502 errors through Ingress how do you debug?

How do you prevent bad configs from reaching production in a CI/CD pipeline?

How would you ensure zero-downtime deployment during a critical update?

Helm deployment fails due to insufficient cluster resources - what's your approach?

How do you share Helm charts internally?

What is Helm chart testing and how is it done?

If a node has 10 pods running, where 7 are healthy and 3 are failing, how would you troubleshoot and fix the issue?

What is the difference between Kubernetes Ingress and Istio?

What is the difference between Horizontal Scaling and Vertical Scaling?

How does kube-proxy work?

How does Kubernetes implement service load balancing?

What is Cluster Autoscaler and how does it work?

How do you secure Kubernetes clusters in production?

How do RBAC and service accounts work?

How do you manage secrets securely at scale?

What is network policy and why is it important?

How do you design Kubernetes for high availability?

How do you troubleshoot performance issues in Kubernetes?

Multi-cluster Kubernetes when and why?

When should Kubernetes NOT be used?

Explain Kubernetes control plane internals in detail

How does etcd maintain consistency and high availability?

What happens when the API server goes down?

How does Kubernetes handle node failure?

What is Pod Disruption Budget (PDB)?

How does Kubernetes networking work internally (CNI)?

Difference between Calico, Flannel, and Weave

How does kube-proxy work?

How does Kubernetes implement service load balancing?

What is Cluster Autoscaler and how does it work?

How do you secure Kubernetes clusters in production?

How do RBAC and service accounts work?

How do you manage secrets securely at scale?

What is network policy and why is it important?

How do you upgrade a Kubernetes cluster safely?

How do you design Kubernetes for high availability?

Pod in CrashLoopBackOff - what's your debugging approach?

Deployment failed after merge - how do you roll back?

What is Helm chart testing and how is it done?

What is the current version of K8s you are using in your project?

What was the last production issue you faced and how did you resolve it?

A pod is stuck in CrashLoopBackOff. Logs show failure during initialization - how do you troubleshoot?

How do you enforce tenant isolation in a multi-tenant Kubernetes setup?

During high traffic, your app shows intermittent 502 errors through Ingress- how do you debug?

How do you prevent bad configs from reaching production in a CI/CD pipeline?

How would you ensure zero-downtime deployment during a critical update?

Helm deployment fails due to insufficient cluster resources - what's your approach?

How do you share Helm charts internally?

What is Helm chart testing and how is it done?

Your Ingress controller crashes repeatedly under heavy load. How do you stabilize it?

An entire Kubernetes region goes down. How do you failover workloads? All pods in one namespace suddenly fail readiness checks. What's your step to troubleshoot? Your cluster is hit by a massive traffic surge, and HPA cannot scale pods fast enough. How do you handle it? A node hosting critical workloads crashes permanently. How do you ensure workloads recover automatically?

A Pod is stuck in ImagePullBackOff. How do you troubleshoot?

Here are 5 real-world scenarios every on-call engineer should master:

1 When kubectl freezes it's not just annoying, it could signal a failing control plane.

2 A rollout gets stuck - can you safely undo and debug under pressure?

3 Works in staging, fails in prod - how fast can you spot subtle config mismatches?

4 Pod-to-pod latency - do you know the right tools to trace and fix it?

5 Node turns NotReady with no clear kubelet logs can you think beyond the obvious?

2 In Kubernetes, how do you debug a pod stuck in CrashLoopBackOff?

3 Explain what happens internally when you run: docker run nginx

4 How do you manage Terraform state across multiple environments safely?

5 Design a highly available Kubernetes cluster across multiple AZs.

6 Blue-Green vs Canary vs Rolling deployment when do you use each?

7 How would you secure secrets in a CI/CD pipeline?

8 What strategy would you use to reduce alert fatigue in monitoring systems?

9 How do you implement GitOps in production?

10 What happens when etcd fails in Kubernetes?

11 How do you handle database schema changes in CI/CD?

12 How would you migrate a monolithic app to microservices?

13 How do namespaces and cgroups work inside Docker?

14 How do you design disaster recovery with defined RTO & RPO?

15 Production server CPU is 100% at 2 AM - what is your first action?


1 


Kubernetes (Production Troubleshooting)

Pods are not starting how do you troubleshoot? Scenario: Deployment created, but pods stuck Pending or CrashLoopBackOff.

Step-by-step approach:

1. Check pod status

kubectl get pods -n prod

2. Describe pod (MOST IMPORTANT)

kubectl describe pod pod-name-n prod

Look for

ImagePullBackOff

Insufficient CPU/Memory

PVC not bound

Failed scheduling

3. Check logs

kubeett ings -pod-namen prod

Real Production Issues I've Seen

Wrong image tag

Secret missing

Liveness probe falling

Node out of memory

What are common real-time issues faced in Kubemeties?

In production, most common issues

COMKilled

DNS resclution failure

Pod-to-pod networking issue

Node NotReady

Storage mount faйилн

Example Fod OGMKIBed

Check

kubectl describe pod pod name

If you see Reason: OGMKilled

Fix by increasing limits

resources requests

memory: "256Mi"

memory: 512M

Lesson: Always define resource requests & limits

How do you achieve node-level and pod-level

autoscaling?

Two different scaling types

Pod-Level Autoscaling (HPA)

Scales pods based on CPU/memory

kubectl autoscale deployment my-app apu-percent 70%

min-21

-max 10

Check

kubectl get hpa

Node-Level Autoscaling (Cluster Autoscaler)

When pods can't schedule due to lack of nodes.

Cluster Autoscaler adds new nodes automatically.

Used in managed services like

Amazon EKS

GKE

AK

Production insight.

HPA scales pods

Cluster Autoscaler scales nodes.


1. Pod in Crash LoopBackOff

What to check

Problem: Pod keeps restarting.

kubectl logs

kubectl describe pod

Check COMKilled

Wrong em/config

Image issues

Correct config Redeploy Rollout restart

2. Application Not Accessible via Service

Probles Problem: Pods running but service not working.

Check:

Service type (ClusterP/NodePort/B)

Endpoints

Label selector mimatch

TargetPort mismatch

Most common issue: Label mismatch

NetworkPolicy

3. Node in NotReady State

Check

kubectl describe node

Disk pressure

Memory pressure

Kubelet status Container runtime

Often caused by full or kubelet crash

4. Pods Getting DOMKRed

Check

Resource requests & limits

kubectl top pods

Fix

Adjust memory limits

Configure HPA

Optimize application

5. Falled Deployment-Need Rollback

Solution: Check rollout history

Rollback to previous version

Best Practice

Versioned image tags

Rolling updates

Bue/Green deployment

6. Pod Stuck in Pending

Reasons No resources

Taints & tolerations

PVC not bound

Node selector mismatch

7. ConfigMap Updated but App Not Heflecting

Changes

ConfigMaps don't auto-reload

Restart pod

Use checksum annotation

Use reloader controller

8 Cluster Performance issu

Check

Node CPU/Memory

Podrestarts

etcd health

Network latency

Storage performance

9. Zero Downtime Deployment

Use Roting update strategy

Readiness probes

Multiple replicas

PodDisruptionlludget

Database Pod Deleted

If using

StatefulSet PVC Data safe

If using empty Dir Data lost



CI/CD


How do you design a production-grade pipeline? What happens internally when a pipeline fails?

How do you implement rollback strategies?

How do you standardize pipelines across multiple

How do you manage artifact versioning?

teams?

Git & Branching Strategy

Which branching strategy do you use and why?

How do you prevent direct commits to the main

How do you handle hotfix releases?

branch?

How do you resolve merge conflicts in critical deployments?

How do you manage release tagging?

Kubernetes (Very Important)

What happens when a Pod goes into

CrashLoopBackOff?

Difference between Deployment and StatefulSet?

How does HPA work internally?

How do you debug networking issues inside a cluster?

How do Services and Ingress differ?

How do you manage Secrets securely? Cloud (Azure/AWS/GCP)

How would you design a secure VPC/VNet architecture?

How do you implement least-privilege IAM?

How do you reduce cloud costs?

How do you monitor infrastructure health?

How do you design a highly available architecture?

Terraform/Infrastructure as Code

How does Terraform state work?

What if the state file gets corrupted?

How do you manage remote state securely?

How do you structure Terraform for multiple

How do you prevent configuration drift?

environments?

Where do you integrate security scanning in CI/CD?

DevSecOps

How do you manage secrets (Vault/Key Vault)?

How do you scan container images?

How do you ensure compliance in pipelines?

What is SAST vs DAST?

Monitoring & Reliability

What is the difference between monitoring and

observability? How do you define SLis, SLOs, and SLAs?

How do you reduce MTTR?

How do you handle alert fatigue?

Production deployment failed what are your first 5

Real-World Scenarios

steps?

How would you scale CI/CD from 5 to 50

microservices?

How would you migrate on-prem infrastructure to

cloud?

How do you design a disaster recovery strategy?

For 3-5 years DevOps roles, companies are not hiring:

Reality Check

X Tool operators

They are hiring:

System thinkers

Automation designers

Reliability-focused engineers

Cost-aware architects

If you're preparing for DevOps interviews, focus on

architecture thinking not just commands.

What's the toughest DevOps interview question you've

faced recently?


AWS, Jenkins CICD, Docker & Kubernetes, Terraform

Here are some questions they asked

AWS

What is Auto Scaling and how do you configure scaling policies?

How to optimize cost for EC2, RDS and S3?

What is VPC and difference between Public/Private subnet?

How do you secure S3 buckets?

How does Load Balancer work in multi AZ setup?

Jenkins

Explain pipeline as code?

What stages you use in CI/CD?

How do you trigger pipeline automatically?

How do you store and use credentials in Jenkins?

What is Blue-Green deployment?

Docker & Kubernetes

Difference between Docker image and container?

What is multi-stage build in Docker?

Explain pod, deployment, service?

How to troubleshoot pod CrashLoopBackOff?

What is Ingress and how it works?

Terraform

What is laC and why Terraform?

Explain Terraform state file?

How do you manage multiple environments (dev/qa/prod)?

What is the use of variables and outputs?

How to handle Terraform remote backend?


container efficiency.

2. What is Immutable Infrastructure?

Why rebuilding infrastructure instead of modifying it improves consistency and reduces configuration drift.

3. Explain CI vs CD clearly.

Continuous Integration ensures code quality.

Continuous Delivery/Deployment ensures faster, automated releases.

4. What happens when a deployment fails? How do you handle it?

Rollback strategy, logs analysis, root cause identification, pipeline validation.

5. Git Merge vs Git Rebase - When do you use each?

Maintaining clean commit history vs preserving context.

6. What is Idempotency in Terraform?

Running the same configuration multiple times should produce the same infrastructure state.

7. How does Auto Scaling work in AWS?

Metrics-based scaling policies, health checks, and high availability design.

8. How do you troubleshoot a production issue?

monitor metrics validate recent

Check logs deployments RCA. isolate root cause fix document


How do you persist workspace in Kubernetes?
PVC
External artifact storage (S3)
Avoid local storage

What happens if Kubernetes pod dies?
Build fails
Restart requires rerun
Use checkpoints carefully

How do you limit resource usage per build?
Pod resource limits
Executor limits
Namespace quotas

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

Differences between DaemonSet, StatefulSet, and Deployment

What is a headless service and how does it work?

What are Kubernetes probes, and how many types are there?

Can we create a StatefulSet without PV and PVC?

Can we attach PV and PVC to a Deployment?

How do you provide S3 access to an EC2 instance?

What is Backstage and how does it work?

What is APM (Application Performance Monitoring)?

A user is unable to access an S3 bucket - how do you troubleshoot?

How do you design a highly available Kubernetes cluster?

How do you serve 50% traffic to Team A and 50% to Team B in Kubernetes?

What are your roles and responsibilities as a DevOps Engineer?

Where do you deploy applications-on-premises or in the cloud?


What is a recent Kubernetes issue you troubleshot?

Which command is used to check pod logs?






























