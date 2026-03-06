https://www.youtube.com/playlist?list=PLdpzxOOAlwvIZ7u-gtpX_bozrspUbTQ1S --> Shell Scripting Playlist ( Abhishek )

---

https://github.com/iam-veeramalla/ultimate-linux-guide/  -->  Ultimate Linux Guide ( Abhishek )

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

https://github.com/iam-veeramalla/a-to-z-of-networking/  -->  A to Z Networking ( Abhishek )

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

https://www.youtube.com/watch?v=ygqjCxZTbuk --> 5 Linux Commands that will make you 10X ( Abhishek )

https://www.youtube.com/watch?v=Mlc0--DO3uE&list=WL&index=1 --> Linux Commands that makes you 10X --> Part 2 ( Abhishek )


1. what shell scripting u did in your project ?

we had several ssl certs imported into AWS ACM . these certs were imported from Cloufare registrar . however these certs had a 365 days expiration policy . hecne we need to renew these certs periodically . we had schell scripts created for this which would generate the csr for the domain and send it to cloudfare registrar through api post . then cloufare would sigh the csr and give us the website chain & body file . the script would then upload these to aws acm to renew the certificates . if the scripts would fail for any reason we would get emails for it 

What is the purpose of #!/bin/bash or #!/bin/sh?

What is the difference between ksh, bash and dash?

How to execute a Shell Script?

How to grant permissions in Linux?

chmod command and how to use?

How to check CPU (nproc) , RAM  / memory ( free ) and disk space ( df -h ) of a Linux Machine?

trapping signals in linux

What is "Top" command and why is it used?

memory management with respect to swap memeory 

soft links vs hard links

ps -ef | grep "amazon" | awk -F" " '{print $2}' --> What are processes, how to list them and find process ID ?

grep command can also be used with files like below
nano test
grep name test

set -x --> debug shell script 
set -e --> exists scripts when there is error
set -o

curl vs wget

find command

why pipe command wont work with date comamnd like below 
date | echo "today is "

cron syntax

How do u manage logs of a system which generates 1000s of logs files --> logrotate

how will u sort names in a file --> for particular shell scripting questions answer them using python

network troubleshooting commands 

is bash dynamic or statically typed language ? --> dynamic

disadvantages of shell scripting 

open a  file in read only mode 

write a script to print only errors from a remote log file --> curl pipe & grep

when should u use python scripting vs linux scripting ? ---> Shell scripting is best for quick OS automation, while Python is preferred for complex, scalable, and maintainable automation involving APIs, data, or cloud services

---

iptables vs nftables ?

| Feature         | iptables                   | nftables            |
| --------------- | -------------------------- | ------------------- |
| Year Introduced | 1998 ( Legacy )            | 2014 ( Modern       |
| IPv4 / IPv6     | Separate tools             | Unified             |
| Performance     | Slower for large rule sets | Faster              |
| Syntax          | Complex                    | Simpler             |
| Data structures | Basic lists                | Efficient sets/maps |

---

What is the difference between netstat and ss?
- ss is faster and modern
- netstat is older and deprecated

---
