✅ Videos
- https://youtu.be/QLbfY_Uh63c?si=QPOYoZkWQmIT1GpS --> Real Time Scenario Based Kubernetes Implementations --> DevOps Shack
- https://www.youtube.com/watch?v=Axplgxm4K3U  --> Kubernetes Toughest Interview Scenarios & Questions  --> Abhishek
- https://www.youtube.com/playlist?list=PLdpzxOOAlwvJdsW6A0jCz_3VaANuFMLpc  --> k8s Playlist --> Abhishek
- https://www.youtube.com/playlist?list=PLdpzxOOAlwvIrFBI1farpLS_OSUBXJMLX --> Troubleshooting k8s --> Abhishek
- https://www.youtube.com/watch?v=viMWeRQnZOE --> Kubernetes Troubleshooting MasterClass ¦ 13 Real-World Scenarios Explained by ADAM

---
✅ encoding vs encrypting ?

---
✅ Pod Status ?
- Iidentify the Pod's status:
- Pending: Pod is not assigned to a node. Check for insufficient resources --> missing node affinity labels --> unbound persistent volume claims -->  or taints/tolerations 
- Container Creation Issues: Pod is assigned but failing to start. Investigate missing ConfigMaps (35:10), missing Secrets (38:36), image pull failures (47:46), or resource quota restrictions (41:21).
- Runtime Errors: Pod is running but crashing or unreachable. Check for Out of Memory (OOM) errors (52:28), liveness/readiness probe failures (58:06), init container failures (103:22), application log errors (106:08), or service endpoint connectivity issues (110:35).

---
✅ Advantage of KEDA over HPA ?
- https://www.youtube.com/watch?v=LwC71u2DyD0&list=PLdpzxOOAlwvJdsW6A0jCz_3VaANuFMLpc&index=38  --> Abhishek
- https://github.com/iam-veeramalla/KEDA-Zero-to-Hero

---
✅ How to secure your EKS Cluster
- <img width="776" height="258" alt="image" src="https://github.com/user-attachments/assets/311a61ad-1249-4577-a6bb-8c96e413c9d2" />
- <img width="761" height="215" alt="image" src="https://github.com/user-attachments/assets/6c56905b-1ca4-49c0-a08f-dcc9303c6cdd" />
- Rest of the Doc --> https://github.com/iam-veeramalla/Kubernetes-Zero-to-Hero/blob/main/Security/Manage_Security_Like_Pro.md#enable-the-encryption-provider-feature-by-configuring-the-kubernetes-api-server

---
✅ Kyverno
- https://www.youtube.com/watch?v=o7-H8QWUPqI&t=2105s

---
✅ Node Cordone
- <img width="480" height="155" alt="image" src="https://github.com/user-attachments/assets/91cdd169-ecf5-4dd3-a1fe-2b845ad518b1" />

---
✅ Resource Sharing and Blast Radius Control
- When multiple teams share a single cluster, one service leaking memory can crash others. To mitigate this use
- Namespaces: Use them to isolate projects or teams
- Resource Quotas: Set these at the Namespace level to prevent one team from consuming cluster-wide resources
- Resource Limits: Define these at the Pod level to ensure a single microservice failure doesn't impact other pods within the same namespace

---
✅ Handling OOM (Out of Memory) Killed Errors 
- If a pod is in a CrashLoopBackOff state due to an OOMKilled error, simply adding more memory isn't always the solution.
- Analysis: Log into the pod to capture Thread Dumps and Heap Dumps (e.g., using kill -3 or jstack for Java apps) to identify memory-leaking threads.
- Collaboration: Share these dumps with the development team so they can perform a root cause analysis and deploy a fixed version of the application (22:40).

---
✅ Config map vs secrets
- <img width="368" height="117" alt="image" src="https://github.com/user-attachments/assets/b88da5a9-d0bb-4b0c-950a-538f6115b737" />

---
✅ Explain RBAC with respect to users , Service Account , Role / Cluster role , Role Binding / Cluster Role Binding
- <img width="531" height="195" alt="image" src="https://github.com/user-attachments/assets/74f074f7-7bae-4fc8-a743-bc67bf661a04" />
- <img width="332" height="148" alt="image" src="https://github.com/user-attachments/assets/b9cf9d11-f51d-4757-a3cc-baab7834b201" />
- Users --> External identities (not managed by Kubernetes directly)
- Service Account --> Kubernetes-managed identities for pods
- Role / Cluster role --> Defines Permission
- Role Binding / Cluster Role Binding --> Binds Role / Cluster role to Users / Service Account
- Differnce between Role vs Cluster Role --> Cluster Role have Cluster Level permission whereas normal roles dont have them

---
✅ ssl termination vs ssl passthrough vs ssl bridging
- <img width="431" height="130" alt="image" src="https://github.com/user-attachments/assets/707c315f-7b95-4293-a545-b2ffd0e1cbf1" />

---
✅ ingress controller used for
- <img width="547" height="167" alt="image" src="https://github.com/user-attachments/assets/db692341-b68e-4a32-88a6-c2d23a47120e" />

---
✅ what is namespace in k8s and can 2 pods in 2 different namespace talk to each other & how to restrict talking ?
- <img width="527" height="116" alt="image" src="https://github.com/user-attachments/assets/40818d0c-ea07-45ac-87db-b88aa11e9b97" />
- <img width="404" height="104" alt="image" src="https://github.com/user-attachments/assets/7618d5c4-df65-4d67-8b93-ad4148d994ab" />
- <img width="398" height="174" alt="image" src="https://github.com/user-attachments/assets/1aab8f0b-7c96-459c-acb1-363f06a08dc8" />

---
✅ Pods vs container
- <img width="448" height="127" alt="image" src="https://github.com/user-attachments/assets/fe2e1f19-0f62-4962-bb92-847503b25260" />
- <img width="320" height="143" alt="image" src="https://github.com/user-attachments/assets/63ce8ebc-a475-41be-befe-cf10d2d0fa12" />
- <img width="385" height="149" alt="image" src="https://github.com/user-attachments/assets/27f4b1ae-72a9-4ac9-b3f2-d59cdc526ad5" />

---
✅ Init Container vs Side car Container
- Init containers run before the main application container to perform setup tasks and must complete successfully, whereas sidecar containers run alongside the main container to provide supporting functionality like logging, proxying, or monitoring throughout the pod’s lifecycle
- <img width="367" height="128" alt="image" src="https://github.com/user-attachments/assets/11b395a8-cdb1-4663-a7c5-351e1e98b2a8" />
- <img width="348" height="277" alt="image" src="https://github.com/user-attachments/assets/abeb21d0-716a-4c1c-8451-c5ae3cc6e6b7" />
- <img width="423" height="250" alt="image" src="https://github.com/user-attachments/assets/247b701a-8c1d-4915-a1fc-8d0788bf4684" />

---
✅ Why use Ingress over Service Load Balancers?
- Ingress is preferred over LoadBalancer services because it allows exposing multiple services through a single external IP with advanced routing features like host and path-based routing, SSL termination, and lower cost, whereas LoadBalancer creates a separate external load balancer per service with limited routing capabilities
- <img width="422" height="119" alt="image" src="https://github.com/user-attachments/assets/6dbb6dee-eb25-4940-85ca-2b40c35ce38a" />
- <img width="308" height="143" alt="image" src="https://github.com/user-attachments/assets/62aea5c4-5cf7-4e7e-b76c-c75d1229edaa" />
- <img width="328" height="99" alt="image" src="https://github.com/user-attachments/assets/734cf245-d1bb-4958-8f7c-d5ee3fd892ff" />

---
✅ K8s Architecture

#### Control Plane / Master
- API server --> The API Server is the entry point to the Kubernetes cluster and exposes REST APIs. All components (kubectl, controllers, scheduler) communicate through it. It validates and processes requests, then updates the cluster state in etcd.
- etcd --> etcd is a distributed key-value store that holds the entire cluster state (config, secrets, metadata). It is the single source of truth in Kubernetes. If etcd is lost, the cluster state is lost.
- Scheduler --> The Scheduler assigns Pods to nodes based on resource availability and constraints. It considers factors like CPU/memory, affinity rules, taints/tolerations. It only decides placement—not execution
- Controller Manager --> Controller Manager runs controllers that maintain the desired state (e.g., ReplicaSet, Node controller). It continuously compares desired vs actual state and takes corrective actions. Example: creates new pods if replicas are missing.
- CCM ( Not presnet in local installations ) = Cloud Controller Manager --> CCM integrates Kubernetes with cloud providers (AWS, Azure, GCP) . It manages cloud resources like load balancers, volumes, and node lifecycle. Not present in local clusters like Minikube.

#### Data Plane / Worker
- Container Runtime --> The container runtime (e.g., containerd, CRI-O) is responsible for running containers. It pulls images, starts/stops containers, and manages container lifecycle. It works via the CRI (Container Runtime Interface).
- Kubelet --> Kubelet is an agent running on each node that communicates with the API Server. It ensures containers described in Pod specs are running. It also performs health checks and reports node status.
- Kube Proxy --> Kube Proxy manages networking rules for Services

#### Other Notable Parts
- kubectl --> CLI
- Pod --> 
- Deployment --> Flow = Deployment ( Yaml ) --> Replica Set ( Controller ) --> Pod ( More detailed flow is Deployment --> API Server --> Scheduler --> Kubelet --> Pods )
- Service  --> Service ( also called as svc ) internally uses Kube Proxy . Can be of Nodeport , Cluster IP ( Default , Access to Pods avilable only within Cluster ) , Load Balancer ( Access to Pods avilable from internet ) . Service Discovery uses Labels & Selectors
- Service Mesh ---> A Service Mesh manages service-to-service communication using sidecars. It provides traffic control, observability, and security. Example: Istio.
- Headless Service --> A Headless Service does not assign a cluster IP. It directly exposes Pod IPs via DNS. Useful for StatefulSets and direct pod communication.
- Istio --> Istio is a popular service mesh that provides traffic management, security, and observability. It uses Envoy sidecars to intercept traffic. Helps with retries, circuit breaking, and mTLS.
- Ingress
- Ingress Controller --> Ingress Controller implements Ingress rules (e.g., NGINX, Traefik). It watches Ingress resources and configures routing. Without it, Ingress does nothing.
- Gateway API --> Gateway API is the next-gen replacement for Ingress.
- Replica Set --> ReplicaSet ensures a specified number of pod replicas are always running. It replaces failed pods automatically. Usually managed indirectly via Deployment.
- Config Map --> ConfigMap stores non-sensitive configuration data (like env variables). It decouples config from application code. Can be mounted as files or env variables.
- Secrets -->  Secrets store sensitive data like passwords, tokens, and keys. They are base64-encoded and can be mounted into Pods. More secure than ConfigMaps.
- Daemon Set --> DaemonSet ensures a pod runs on every node (or selected nodes). Used for logging, monitoring, and networking agents. Example: Fluentd, Node Exporter.
- RBAC --> Flow ( Pod --> Service Account --> Role --> Role Binding )
- Service Account --> Service Account provides identity to Pods. It is used for authentication with the API Server. Each Pod can be assigned a ServiceAccount.
- Role --> used to give permission to a pod within a single namespace
- Users --> Users represent human or external identities. Kubernetes does not manage users directly—it relies on external auth providers. RBAC rules apply to them.
- Cluster role --> used to give permission to a pod across all namepaces in a cluster
- cluster role binding --> Binds a ClusterRole to users or service accounts
- Custom Resource --> CRDs allow you to extend Kubernetes with custom objects. Example: defining your own resource like “Database”. It makes Kubernetes extensible.
- Custom Resource Controller --> This controller watches custom resources and ensures desired state. It works like built-in controllers but for CRDs
- Admission Controller --> Admission Controllers intercept API requests before persistence. They validate or mutate requests. Example: enforcing security policies.
- HPA --> Horizontal Pod Autoscaler --> HPA automatically scales pods based on metrics like CPU/memory. It adjusts replicas dynamically. Helps handle variable workloads.
- KEDA --> KEDA enables event-driven autoscaling. It scales pods based on external metrics like queues (Kafka, RabbitMQ). Works alongside HPA.
- Kyverno --> Kyverno is a policy engine for Kubernetes. It validates, mutates, and generates resources. Used for security and compliance enforcement.
- Kustomize
- Persistent Volume ( pv ) --> PV is a cluster-wide storage resource. It represents actual storage like EBS, NFS
- Persistent Volume Claim ( pvc ) --> PVC is a request for storage by a Pod
- Statefulset --> Flow ( Statefulset --> pvc --> sc ( storage class eg EBS  ) --> provisioner --> pv . Used for DBs.
- CSI Driver --> CSI (Container Storage Interface) allows Kubernetes to interact with storage systems. It enables dynamic provisioning. Example: AWS EBS CSI. 
- Network Policy --> control traffic between Pods. block frontend pods from reaching db pods directly . enforced using calico or kyverno
- Calico --> Networking and security solution for Kubernetes. It enforces network policies. It uses BGP and iptables.
- Border0 --> 3rd party tool user for RBAC
- Namespace --> logically isolates resources in a cluster
- Resource Quota --> limits total resource usage in a namespace.
- Resource Limit --> limits total resource usage in a pod
- Liveness Probe --> Health Check
- Node Selector --> Node Selector is a simple way to schedule pods on specific nodes using labels. It matches exact key-value pairs. Basic scheduling constraint.
- Node Affinity --> Node Affinity is an advanced version of nodeSelector. It supports required and preferred rules. Provides more flexibility in scheduling.
- Taints --> Taints are applied to nodes to repel pods. Only pods with matching tolerations can be scheduled. Used to control placement
- Tolerations --> Tolerations allow pods to be scheduled on tainted nodes.
- Node Cordone --> Node cordon marks a node as unschedulable. No new pods are placed on it. Used during maintenance. Implemented using Taint
- Node Drain --> Node drain safely evicts pods from a node. It prepares the node for maintenance. Respects Pod Disruption Budgets.
- Pod Eviction --> Pod eviction is the process of removing pods gracefully. It ensures proper shutdown. Triggered during drain or resource pressure.
- Pod Distruption Budget ( PDB ) --> PDB ensures minimum availability during voluntary disruptions. It limits how many pods can be down. Important for high availability.
- Voluntry & Involuntry Disruption --> Voluntary disruptions are planned (e.g., node drain). Involuntary are unexpected (node crash). PDB mainly protects against voluntary ones.
- External secret operator --> It syncs secrets from external systems (AWS Secrets Manager, Vault) into Kubernetes. Avoids storing secrets directly in cluster. Improves security.

---
✅ K8s Common Errors
- Image Pull Back Off
- <img width="816" height="197" alt="image" src="https://github.com/user-attachments/assets/fddf346c-5983-4004-bcf4-11c313bcacf1" />
- Crash Loop Back Off
- <img width="808" height="369" alt="image" src="https://github.com/user-attachments/assets/6098c3ef-0ba7-42a4-99c3-24e392124c8a" />
- <img width="809" height="451" alt="image" src="https://github.com/user-attachments/assets/05de4405-c6f9-4e69-a747-54a3b191b437" />
- OOM Killed
- 

---
✅ Kops
- <img width="516" height="395" alt="image" src="https://github.com/user-attachments/assets/ad7bad6f-2537-4bc4-9637-62affb7bae02" />

---
✅ why k8s is used over docker ?
- Auto scaling  --> using replica sets
- Auto healing  --> using health check

---
✅ what is conatiner runtime & what container runtime does k8s & docker supports ?
- A container runtime is the software responsible for running containers.
- Docker uses containerd
- <img width="446" height="99" alt="image" src="https://github.com/user-attachments/assets/2eba02ae-8058-4a7c-9a42-5218ba00d2f0" />

  
---
✅ what all resources in k8s appear as pods ?

---
✅ if someone has done wrongful deploymnet in eks . how will u rollback it
- seperate procedure for rolling deployment & blue green deploymnet

---
✅ Gateway API 
- https://www.youtube.com/watch?v=h-AabKgZw1s  --> Devops Shack  --> k8s Componenet not present in k8s_components.html
- limitation of service & ingress 

---
✅ k8s Cluster Upgrade
- https://www.youtube.com/watch?v=1l2agEQ1Ngo --> DevOps Shack
- https://www.youtube.com/watch?v=Cfznp8jRh7I&list=PLdpzxOOAlwvJdsW6A0jCz_3VaANuFMLpc&index=17  --> Abhishek
- version upgrade is not reversible
- version 1.33 needs to be upgraded to 1.34 first & not to 1.35 directly
  
#### Core Prerequisites for Upgrades 
- Node Cordone
- Compatibility: Ensure the Control Plane, Nodes, Kubelet, and Cluster Autoscaler are all compatible with the target version 
- Release Notes: Always review the changelog for API deprecations or major changes that could break existing resources (e.g., Ingress API shifts)
- Environment Testing: Never upgrade production first. Always validate the upgrade in lower environments (dev/staging/pre-prod) first 
- Networking: Ensure there are at least 5 available IP addresses in your subnet 

#### The Three-Phase Upgrade Process:
- Control Plane Upgrade: Use eksctl, the AWS CLI, or the AWS UI to trigger the version update for the management layer
- Data Plane (Node) Upgrade: Upgrade the nodes. For managed node groups, the presenter recommends a rolling update strategy to achieve zero downtime
- Add-ons Upgrade: Finally, update cluster add-ons like VPC CNI and CoreDNS to match the new version
- Testing --> After the upgrade, the presenter advises running functional or regression tests to ensure that all services, controllers, and applications (such as Argo CD or Prometheus) are operating as expected 

---
✅ eks nodegroup 
- <img width="740" height="65" alt="image" src="https://github.com/user-attachments/assets/30e3e30f-9d73-413e-8782-6a74128dc35c" />


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






























