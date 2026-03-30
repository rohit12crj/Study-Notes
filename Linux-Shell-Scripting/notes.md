✅ Videos
- https://www.youtube.com/playlist?list=PLdpzxOOAlwvIZ7u-gtpX_bozrspUbTQ1S --> Shell Scripting Playlist ( Abhishek )
- https://www.youtube.com/watch?v=ygqjCxZTbuk --> 5 Linux Commands that will make you 10X ( Abhishek )
- https://www.youtube.com/watch?v=Mlc0--DO3uE&list=WL&index=1 --> Linux Commands that makes you 10X --> Part 2 ( Abhishek )
- https://github.com/iam-veeramalla/ultimate-linux-guide/  -->  Ultimate Linux Guide Repo ( Abhishek )
- https://github.com/iam-veeramalla/a-to-z-of-networking/  -->  A to Z Networking ( Abhishek )

---
✅  scp ( Secure Copy Protocol ) used for 

---
✅ which command can i use to check if caddy is running inside ecs task or not ?
- ps aux | grep caddy

---
✅ which command can i use to check on which ports caddy is listening ?
- Ports should be exposed in Dockerfile using expose command
- ports exposed in dockerfile should be the same ports on which ALB listeners should listen
- netstat -tulpn | grep caddy OR ss -tulpn | grep caddy

---
✅ https://github.com/iam-veeramalla/ultimate-linux-guide/  -->  Ultimate Linux Guide ( Abhishek )
- getting-started
- folder-structure
- user-management
- file-management
- vi-shortcuts
- file-permissions
- process-management
- monitoring
- networking
- disk-management

---
✅ https://github.com/iam-veeramalla/a-to-z-of-networking/  -->  A to Z Networking ( Abhishek )
- IP Addressing & Subnetting
- TCP/IP & OSI Model
- DNS (Domain Name System)
- HTTP/HTTPS & Web Protocols
- Load Balancing
- Firewalls & Security Groups
- VPN & Tunneling
- Proxies & Reverse Proxies
- CDN (Content Delivery Network)
- Docker Networking
- Kubernetes Networking

---
✅ what shell scripting u did in your project ?
- we had several ssl certs imported into AWS ACM . these certs were imported from Cloufare registrar . however these certs had a 365 days expiration policy . hecne we need to renew these certs periodically . we had schell scripts created for this which would generate the csr for the domain and send it to cloudfare registrar through api post . then cloufare would sigh the csr and give us the website chain & body file . the script would then upload these to aws acm to renew the certificates . if the scripts would fail for any reason we would get emails for it
- passing of secrets manger values while building a dockerfile inside github action through a .env file 

---
✅ What is the purpose of #!/bin/bash or #!/bin/sh?

---
✅ What is the difference between ksh, bash and dash?

---
✅ How to execute a Shell Script?

---
✅ How to grant permissions in Linux?

---
✅ chmod command and how to use?

---
✅ How to check CPU (nproc) , RAM  / memory ( free ) and disk space ( df -h ) of a Linux Machine?

---
✅ trapping signals in linux

---
✅ What is "Top" command and why is it used?

---
✅ memory management with respect to swap memeory 

---
✅ soft links vs hard links

---
✅ ps -ef | grep "amazon" | awk -F" " '{print $2}' --> What are processes, how to list them and find process ID ?

---
✅ grep command can also be used with files like below
nano test
grep name test

set -x --> debug shell script 
set -e --> exists scripts when there is error
set -o

---
✅ curl vs wget

---
✅ find command

---
✅ why pipe command wont work with date comamnd like below 
date | echo "today is "

---
✅ cron syntax

---
✅ How do u manage logs of a system which generates 1000s of logs files --> logrotate

---
✅ how will u sort names in a file --> for particular shell scripting questions answer them using python

---
✅ network troubleshooting commands 

---
✅ is bash dynamic or statically typed language ? --> dynamic

---
✅ disadvantages of shell scripting 

---
✅ open a  file in read only mode 

---
✅ write a script to print only errors from a remote log file --> curl pipe & grep

---
✅ when should u use python scripting vs linux scripting ? ---> Shell scripting is best for quick OS automation, while Python is preferred for complex, scalable, and maintainable automation involving APIs, data, or cloud services

---
✅ iptables vs nftables ?

| Feature         | iptables                   | nftables            |
| --------------- | -------------------------- | ------------------- |
| Year Introduced | 1998 ( Legacy )            | 2014 ( Modern       |
| IPv4 / IPv6     | Separate tools             | Unified             |
| Performance     | Slower for large rule sets | Faster              |
| Syntax          | Complex                    | Simpler             |
| Data structures | Basic lists                | Efficient sets/maps |

---
✅ What is the difference between netstat and ss?
- ss is faster and modern
- netstat is older and deprecated

---
✅ explain the full Linux packet flow (PREROUTING → INPUT → FORWARD → OUTPUT → POSTROUTING) in Netfilter framework ?

---
✅ when does NAT happens in Linux
- <img width="491" height="107" alt="image" src="https://github.com/user-attachments/assets/b1665bdc-1f98-4b47-9c6c-101b4ef4d4e5" />

---
✅ Why does Docker need MASQUERADE?
- Containers have private IP addresses, so MASQUERADE performs SNAT to replace container IP with host IP for outgoing traffic.

---
✅ Explain Docker networking packet flow using iptables
- Docker networking uses the Linux bridge (docker0) and iptables NAT rules to route traffic between the host and containers. When a port is mapped (e.g., -p 8080:80), Docker creates a DNAT rule in the PREROUTING chain to redirect traffic to the container IP, and a MASQUERADE rule in POSTROUTING to allow container responses to reach external clients.

---
✅ What is MASQUERADE?
- MASQUERADE is a special type of SNAT used when the public IP is dynamic, commonly used in Docker and home routers.

---
✅ SNAT VS DNAT
- SNAT modifies the source IP address of outgoing packets, typically used when private systems access external networks. DNAT modifies the destination IP address of incoming packets, typically used for port forwarding or directing external traffic to internal servers.
- <img width="463" height="400" alt="image" src="https://github.com/user-attachments/assets/fcb55407-1d32-42d5-aaac-973c3fe89067" />

---
✅ How Kubernetes Services Use DNAT (kube-proxy + iptables)
- In Kubernetes, a Service provides a stable IP and DNS name to access a group of Pods.
- This works using kube-proxy which configures iptables rules that perform DNAT (Destination NAT).
- DNAT allows Kubernetes to redirect traffic from the Service IP to one of the backend Pods.
- Kubeproxy runs as a daemon set

---
✅ What are the three kube-proxy modes?

| Mode      | Description                     |
| --------- | ------------------------------- |
| userspace | Old method using proxy process  |
| iptables  | Most common                     |
| IPVS      | High-performance load balancing |

---
✅ explain Kubernetes networking from Pod → Service → kube-proxy → CNI → Node → Internet
- In Kubernetes networking, a Pod sends traffic to a Service which provides a stable virtual IP. kube-proxy creates iptables rules that perform DNAT to redirect traffic to backend Pods. The CNI plugin manages Pod networking and routing across nodes. When traffic leaves the cluster, the node performs SNAT so external systems can respond.
- <img width="398" height="211" alt="image" src="https://github.com/user-attachments/assets/cafa0316-19a4-4a04-b695-51c18b8faac5" />

---
✅ 4 types of Kubernetes Services (ClusterIP, NodePort, LoadBalancer, Headless)

---
✅ Difference between export & set ?
- The export command in Linux is used to set environment variables and make them available to child processes started from the current shell.
- <img width="630" height="431" alt="image" src="https://github.com/user-attachments/assets/19e8ee3c-bdec-4582-948c-d979d7c12350" />
- <img width="620" height="242" alt="image" src="https://github.com/user-attachments/assets/9961aeca-1a65-4d45-9ce9-3106557559c6" />

---
