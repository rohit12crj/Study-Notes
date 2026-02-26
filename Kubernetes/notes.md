user management in k8s --> Border0

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

