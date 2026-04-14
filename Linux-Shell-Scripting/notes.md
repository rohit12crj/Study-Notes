✅ Videos
- https://www.youtube.com/playlist?list=PLdpzxOOAlwvIZ7u-gtpX_bozrspUbTQ1S --> Shell Scripting Playlist ( Abhishek )
- https://www.youtube.com/watch?v=ygqjCxZTbuk --> 5 Linux Commands that will make you 10X ( Abhishek )
- https://www.youtube.com/watch?v=Mlc0--DO3uE&list=WL&index=1 --> Linux Commands that makes you 10X --> Part 2 ( Abhishek )
- https://github.com/iam-veeramalla/ultimate-linux-guide/  -->  Ultimate Linux Guide Repo ( Abhishek )
- https://github.com/iam-veeramalla/a-to-z-of-networking/  -->  A to Z Networking Repo ( Abhishek )

---
+----------------------------------------------------+
| User Applications (Vim, Docker, Apache, etc.)     |
+----------------------------------------------------+
| Shell (Bash, Zsh, Fish, etc.)                     |  <-- Part of the OS
+----------------------------------------------------+
| System Libraries (glibc, libc, OpenSSL, etc.)     |  <-- Part of the OS
+----------------------------------------------------+
| System Utilities (ls, grep, systemctl, etc.)      |  <-- Part of the OS
+----------------------------------------------------+
| Linux Kernel (Process, Memory, FS, Network)       |  <-- Core of the OS
+----------------------------------------------------+
| Hardware (CPU, RAM, Disk, Network, Peripherals)   |
+----------------------------------------------------+


(a) Hardware Layer

🔹 The physical components of the computer (CPU, RAM, disk, network interfaces, etc.).
🔹 The OS interacts with hardware using device drivers.
(b) Kernel (Core of Linux OS)

🔹 The Linux Kernel is responsible for directly managing system resources, including:

    Process Management – Schedules processes and handles multitasking.

    Memory Management – Allocates and deallocates RAM efficiently.

    Device Drivers – Acts as an interface between software and hardware.

    File System Management – Manages how data is stored and retrieved.

    Network Management – Handles communication between systems.

(c) Shell (Command Line Interface - CLI)

🔹 A command interpreter that allows users to interact with the kernel.
🔹 Examples: Bash, Zsh, Fish, Dash, Ksh.
🔹 Converts user commands into system calls for the kernel.
(d) User Applications

🔹 End-user programs like web browsers, text editors, DevOps tools, etc.
🔹 Applications interact with the OS using system calls via the shell or GUI.
---
✅ Namespaces & Cgroups 
-  Namespaces provide isolation, cgroups provide resource control — together they enable containers
- <img width="239" height="45" alt="image" src="https://github.com/user-attachments/assets/6211dabf-d33e-4d6e-9f85-be040e4b7beb" />
- <img width="367" height="232" alt="image" src="https://github.com/user-attachments/assets/cedb6386-0e21-45e4-adef-e44165c606c8" />
- <img width="422" height="428" alt="image" src="https://github.com/user-attachments/assets/fc59a915-782d-49a6-a0d1-0e48b934c287" />
- <img width="381" height="404" alt="image" src="https://github.com/user-attachments/assets/6fa9b283-c045-4f53-b8d2-3c73dbbf4438" />

---
✅ Explain Linux architecture using diagram

---
✅ CPU Bottlenek vs I/O Bottleneck
- A system is CPU bottlenecked when the processor is fully utilized and cannot handle more work
- <img width="313" height="221" alt="image" src="https://github.com/user-attachments/assets/ae8053b7-779e-4ccf-bc14-983151afb5fe" />
- <img width="297" height="135" alt="image" src="https://github.com/user-attachments/assets/eee1e9f3-de27-4fae-8285-9d52d1a2c2c5" />
- I/O bottleneck occurs when the system spends time waiting on disk or network operations
- <img width="304" height="208" alt="image" src="https://github.com/user-attachments/assets/d944b76e-bbbe-484a-8d8f-92a1f6eb0c3f" />
- <img width="313" height="137" alt="image" src="https://github.com/user-attachments/assets/3c40b6c3-c63c-40b5-b97c-b4d59025ea06" />
- I first check top. If CPU is ~100% and idle is near zero, it's CPU bound. If CPU is idle but wa% is high, it's I/O bound. Then I confirm using iostat for disk utilization and latency

---
✅ lsblk --> to check disk partition

---
✅ iostat command 

---
✅ Difference between TCP & UDP 
- <img width="545" height="304" alt="image" src="https://github.com/user-attachments/assets/ea7ca02b-51cc-4971-84f9-725ffa892b37" />
- <img width="499" height="283" alt="image" src="https://github.com/user-attachments/assets/f1563979-714c-4862-9075-86d06cac0971" />
- <img width="445" height="185" alt="image" src="https://github.com/user-attachments/assets/4e70fcb0-9177-4de8-9a74-848aef9d6e25" />

---
✅ What is the Linux boot process and its stages?
- Linux boot happens in 6 major stages
- <img width="530" height="227" alt="image" src="https://github.com/user-attachments/assets/b1d91701-a69d-4c11-8737-261627b96c95" />

---
✅ What is the difference between BIOS and UEFI?

✅ What is GRUB and how does it work?

✅ What are systemd and init systems in Linux?

---
✅ What is the difference between systemctl and service commands?

---
✅ How does process management work in Linux?

✅ What is the difference between a process and a thread?

✅ How do you check running processes in Linux?

✅ What is nice and renice in Linux?

✅ What is a zombie process?

✅ What is file permission in Linux and how does it work?

✅ What is the difference between hard link and soft link?

✅ What is umask and how is it used?

✅ What are special permissions (SUID, SGID, Sticky Bit)?

✅ What is inode in Linux?

✅ What is LVM and why is it used?

✅ What is the difference between ext4, xfs, and other file systems?

✅ How do you check disk usage and disk performance?

✅ What is fstab and how does it work?

✅ What is swap space and how is it managed?

✅ What is networking in Linux and how do you troubleshoot it?

✅ What is SSH and how does it work?

✅ What is DNS and how is it configured in Linux?

✅ What is a package manager and how does it work?

✅ What is the difference between apt and yum/dnf?

✅ How do you manage services in Linux?

✅ What is log management in Linux and where are logs stored?

---
✅ tar command used for 

---
✅ scp ( Secure Copy Protocol ) used for 

---
✅ which command can i use to check if caddy is running inside ecs task or not ?
- ps aux | grep caddy

---
✅ which command can i use to check on which ports caddy is listening ?
- Ports should be exposed in Dockerfile using expose command
- ports exposed in dockerfile should be the same ports on which ALB listeners should listen
- netstat -tulpn | grep caddy OR ss -tulpn | grep caddy
- if still u r not able to find out on which port caddy is listening . exec into the ECS Task & do curl -v http://private_ip_of_ecs_task:80  --> u shoukd get 200 ok . if not 80 try with other common ports which u think it might be listening on

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
