user management in k8s --> Border0

csi driver 

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


