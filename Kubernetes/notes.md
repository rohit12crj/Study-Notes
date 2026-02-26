user management in k8s --> Border0

daemonset vs deployments ? when to use which with examples 

There is also a default namespace in k8s

notify me if a pod for payment app crashes more than 2 times in 1hr in payment namespace ?

configure alertmanager to send mail emails when a pod for payment app crashes more than 2 times in 1hr in payment namespace . configure promql to find the info from prometheus server ( need to see the correct metric from online ) --> this will only tell what & when it is happening

for live pods we can exec into it & for pods which already crashed & we can't exec , check logs either from cloudwatch or EFK / Opensearch , this can happen only when streaming is enabled --> this will  tell why it is happening

distributed traceing through jaeger --> --> this will tell how it is happening

