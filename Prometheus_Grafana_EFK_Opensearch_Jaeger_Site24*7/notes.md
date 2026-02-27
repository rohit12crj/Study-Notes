Monitoring + Observability ( Only Prometheus , Grafana ) ( Theory + Project Playlist ) ( Study Only Day 1 to 4 ) ( Abhishek ) --> https://www.youtube.com/playlist?list=PLdpzxOOAlwvJUIfwmmVDoPYqXXUNbdBmb

Github Repo ( Abhishek )  --> https://github.com/iam-veeramalla/observability-zero-to-hero/

Difference between Observability and monitoring 

monitoring ( Prometheus , alert manager , grafana ) vs logging ( ELK ) vs traces ( Jaeger )

Distributed tracing through jaeger --> https://youtu.be/U9qInvWTe9w?si=nfmy-tpkuX2pszIk

open telemetry concepts

in prometheus ( specify the manifest file type of prometheus server , alertmanager , node exporter , kube state metrics )

hybrid cloud monitoring using site 24*7 --> https://youtu.be/jRc2vFmkILs?si=RAzcWPt7OJzxEJeC  --> u will use helm charts to install site 24*7 agent in k8s cluster 

opensearch --> https://www.youtube.com/watch?v=OSShB_cXisE&list=PLdpzxOOAlwvJUIfwmmVDoPYqXXUNbdBmb&index=9  -> fetches the logs from k8s cluster using fluentbit into opensearch & visualizes using opensearch dashboards  . fluentbit needs to be deployed as daemon set with configuration of how to connect to aws managed opensearch in config maps or secrets

ebpf

what is push gateway ? give an example where u would use pushgateway ?

what is prom-client ? similar like boto3 for aws SDK

prometheus architecture ( https://github.com/iam-veeramalla/observability-zero-to-hero/blob/main/day-2/readme.md )

what metrics & custom metrics were u monitoring in your eks cluster

installation of prometheus , grafana ( in the helm chart which abhishek uses both prometheus & grafana are installed ) , alert manager ( since alert manager is not a default component of prometheus , how will u install it ) on eks cluster ( https://github.com/iam-veeramalla/observability-zero-to-hero/blob/main/day-2/readme.md )

difference between node exporter pod ( runs as daemon set , used for hardware level metrics like CPU , memory , node exporter pod pulls info from host on which nodes are running & keeps the info at node_ip/metrics endpoint , prometheus server scraps this endpoint & keeps in TSDB ) , kube state metrics pods (  kube state metrics pulls the info from Kube API Server & keeps info at pod_ip/metrics endpoint , prometheus server scraps this endpoint & keeps in TSDB from Kube API server ) , custom metrics (from your_application/metrics endpoint --> application level metrics , u need to write service discovery manifest yaml file , else prometheus will not know from where to go to find the endpoint ) in prometheus ? There are also db exporters which are not installed by default ?

suppose u have 5 eks custer . how will u setup 1 single setup prometheus & connect with all 5 cluster ?

should prometheus be setup using terraform ?

how will u install promethus on ecs cluster 

how will u install promethus on ec2 

how will u install promethus on on-prem device

how will u scrap custom metrics

What is Prometheus?

when to use pull based or push based

what db is prometheus using

What are the main components of Prometheus?
Prometheus Server

Exporters

Alertmanager

Pushgateway

PromQL

Time-Series Database (TSDB)

What is Service Discovery in Prometheus?

How does Prometheus store data internally?

How do you scale Prometheus if u are not using AMP ( Amazon Managed Prometheus ) & using helm charts ?

What is Federation? is it an older model ?

What is the data model in Prometheus?

How does alerting work in Prometheus?
Prometheus evaluates alerting rules at regular intervals. When a condition is met for a duration (for clause), it fires an alert to the Alertmanager, which handles deduplication, grouping, silencing, and routing to receivers (PagerDuty, Slack, email, etc.).
What is the for clause in an alerting rule?
It specifies how long the condition must be true before the alert fires. It prevents flapping alerts from noisy transient spikes.
What is inhibition in Alertmanager?
Inhibition suppresses certain alerts when other specific alerts are already firing. For example, suppress instance-level alerts when a whole cluster-down alert is active.

When would you use the Pushgateway?
For short-lived jobs (like batch jobs or cron jobs) that may finish before Prometheus scrapes them. The job pushes its metrics to the Pushgateway, which Prometheus then scrapes.
What is the Prometheus TSDB retention period by default?
15 days. It can be configured with --storage.tsdb.retention.time.

since the tsdb stores data for only 15 days .if i want to querry data from 30 days back how can i do it

What are the four metric types in Prometheus? https://github.com/iam-veeramalla/observability-zero-to-hero/blob/main/day-4/readme.md

Counter – monotonically increasing value (e.g., total requests)
Gauge – value that can go up or down (e.g., memory usage)
Histogram – samples observations into configurable buckets (e.g., request latency)
Summary – similar to histogram but calculates quantiles on the client side

What is the difference between rate() and irate()?
rate() calculates the per-second average rate over the entire range window — it's smoother and better for alerting. irate() uses only the last two data points — it's more responsive to spikes, better for graphing.

Where is tsdb in prometheus stored actually. My Prometheus is running in eks cluster using helm charts

since the tsdb stores data for only 15 days .if i want to querry data from 30 days back how can i do it . thanos needs to run as a sidecar container along with the main application

i want to get alerted if cpu utilization of a pod exceeds 80 % in eks cluster using prometheus . write the promql querry . how can i do it

Give example of metrics in prometheus ? ( https://github.com/iam-veeramalla/observability-zero-to-hero/blob/main/day-3/readme.md ) tell me how can u find out how many configmaps are created in a k8s cluster in last 1 hr or how many times a particular pod crashed in 1 hr ? answer - use promql & select the appropriate metric

How will u setup authentication & authorization for prometheus UI ?

Grafana
==============

which built in / default dashboards were u using ?

how will u connect grafana to prometheus ? --> setup up a new data connection . all can be done through UI 

how will u setup custom dashboard & share ot with end users ? same process like quicksight -->  just need to find out the correct Promql querry 

How will u setup authentication & authorization for grafana UI & give different users specific access to different dashboards ?

which db is grfana using since  it needs to store user RBAC ?

EFK/Opensearch for Logging & Jaeger for Traceing
===================================================

https://github.com/iam-veeramalla/observability-zero-to-hero/blob/main/day-4/readme.md

what is instrumentation ? it is necessary if u wnat to find out how many users logged in to your app within last 1 hr . this can't be obtained using node exporters & kube state metrics . u need to implement custom metrics for your app

🛠️ Implement Custom Metrics in Node.js Application: Use the prom-client library to write and expose custom metrics in the Node.js application.

🚨 Set Up Alerts in Alertmanager: Configure Alertmanager to send email notifications if a container crashes more than two times. Basically u need to setup alertmanger manifest files with all rules ( what & when to fire them ) . to send emails , u need to connect to smtp servers. credentials needs to be stored in aws secret manager & accessed inside k8s . 

📝 Set Up Logging: Implement logging on both application and cluster (node) logs for better observability using EFK stack(Elasticsearch, FluentBit, Kibana).

📸 Implement Distributed Tracing for Node.js Application: Enhance observability by instrumenting the Node.js application for distributed tracing using Jaeger. enabling better performance monitoring and troubleshooting of complex, multi-service architectures.

what exact custom metrics were u using for your app . explain in counter , guage & histogram what exactly were u using ? for ex- in histogram u can mention like duration of http requests in milliseconds ( 5 , 10 , 15 )

How do you stop Alertmanager from sending too many alerts?
