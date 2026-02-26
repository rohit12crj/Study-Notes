Monitoring + Observability ( Only Prometheus , Grafana ) ( Theory + Project Playlist ) ( Study Only Day 1 to 4 ) ( Abhishek ) --> https://www.youtube.com/playlist?list=PLdpzxOOAlwvJUIfwmmVDoPYqXXUNbdBmb

Github Repo ( Abhishek )  --> https://github.com/iam-veeramalla/observability-zero-to-hero/

Difference between Observability and monitoring 

monitoring ( Prometheus , alert manager , grafana ) vs logging ( ELK ) vs traces ( Jaeger )

How will u find out disk utilization of a node in a eks cluster 

Distributed tracing through jaeger

open telemetry concepts

pagerduty

ebpf

prometheus architecture ( https://github.com/iam-veeramalla/observability-zero-to-hero/blob/main/day-2/readme.md )

what metrics & custom metrics were u monitoring in your eks cluster

installation of prometheus , grafana ( in the helm chart which abhishek uses both prometheus & grafana are installed ) , alert manager ( since alert manager is not a default component of prometheus , how will u install it ) on eks cluster ( https://github.com/iam-veeramalla/observability-zero-to-hero/blob/main/day-2/readme.md )

difference between node exporter pod (for hardware metrics) , kube state metrics pods , appliation metrics (/metrics endpoint ) in prometheus ?

suppose u have 5 eks custer . how will u setup 1 single setup prometheus & connect with all 5 cluster ?

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

How do you scale Prometheus?

What is Federation?

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

What are the four metric types in Prometheus?

Counter – monotonically increasing value (e.g., total requests)
Gauge – value that can go up or down (e.g., memory usage)
Histogram – samples observations into configurable buckets (e.g., request latency)
Summary – similar to histogram but calculates quantiles on the client side

What is the difference between rate() and irate()?
rate() calculates the per-second average rate over the entire range window — it's smoother and better for alerting. irate() uses only the last two data points — it's more responsive to spikes, better for graphing.

since the tsdb stores data for only 15 days .if i want to querry data from 30 days back how can i do it

i want to get alerted if cpu utilization of a pod exceeds 80 % in eks cluster using prometheus . how can i do it
