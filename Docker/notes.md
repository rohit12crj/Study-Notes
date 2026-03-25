✅ https://www.youtube.com/playlist?list=PLdpzxOOAlwvLjb0vTD9BXLOwwLD_GWCmC  --> Docker (Playlist ) --> Abhishek 

---
✅ Give common docker compose commands ?

---
✅ Real Time Scenario Based Docker Implementations --> DevOps Shack --> https://www.youtube.com/watch?v=9hOSkx9jWPQ

---
✅ Why docker-compose.yml file is used ?
- A docker-compose.yml file is used to define, manage, and run multiple containers together as a single application.
- it can also be used to define networking , volumes
- docker-compose.yml schema = services + networking + storage + configuration in a structured YAML format.

<img width="350" height="160" alt="image" src="https://github.com/user-attachments/assets/7a419138-1354-4951-87ef-60b020e790d2" />

---
✅ docker-compose.yml schema 

<img width="518" height="407" alt="image" src="https://github.com/user-attachments/assets/87123d3b-9881-4a26-b591-7a92635fde09" />
<img width="503" height="394" alt="image" src="https://github.com/user-attachments/assets/e5386246-947f-4130-93d5-128f0656c503" />
<img width="503" height="243" alt="image" src="https://github.com/user-attachments/assets/691fe715-aa2e-4589-990b-8ed58e847021" />
<img width="510" height="391" alt="image" src="https://github.com/user-attachments/assets/0acf490f-d205-49cc-8dd6-23a48d056d9f" />
<img width="310" height="135" alt="image" src="https://github.com/user-attachments/assets/52b60be0-4879-4752-b31c-8d53fb2a60fc" />
<img width="506" height="224" alt="image" src="https://github.com/user-attachments/assets/b6e1140d-83fa-411c-abca-5e9cf0a65011" />





---
✅ what does docker daemon off does in Dockerfile

---
✅ give me an exmaple where u will use both entrypoint & CMD 

---
✅ Difference between package.json & package-lock.json

---
✅ How can u remove Docker Caching if it is causing errors ?

---
✅ Does Dockerfile has any extension ?

---
✅  AS keyword used for in Dockerfile

---
✅  Difference between npm ci , npm install & npm build for nodejs applications

---
✅ Give Dockerfile example 

<img width="445" height="391" alt="image" src="https://github.com/user-attachments/assets/62d51ee2-1c0f-4164-986a-6e6619fa0447" />

<img width="395" height="170" alt="image" src="https://github.com/user-attachments/assets/25bfbc77-05ba-434a-8ca9-2433abbd5c7d" />

---
✅ Give Dockerfile multistage build example  
- Actual Frontend Dockerfile used in DevSecOps pipeline video of Abhishek

<img width="422" height="431" alt="image" src="https://github.com/user-attachments/assets/2788b3fe-66ee-49ef-9cb4-d2a95448c2ff" />

---
✅ Best Practices 
- Multi stage build
- in 2nd stage run as non root user
- use dockerignore file
- use caching effectively
- use distorless images
- always use base images with a particular version to avoid auto upgrade when new images are added to dockerhub
  
---
What is the difference between VM and Docker?

how will u pass env variables when app is building like in a docker 

how will u pass env variables when app is running  like in a ECS Task definition

Difference between CMD and ENTRYPOINT?

copy vs ADD

Difference between bind mount and volume?

What are Docker Volumes?

What is Docker Networking?

distorless images along with advantage and disadvantage

What is multi-stage build? along with advantage and disadvantage

docker compose file 

How do you reduce Docker image size?

Explain Docker Architecture

How do you secure Docker containers?

How do you troubleshoot a container that keeps restarting?

Difference between Docker Compose and Docker Swarm?

What is Docker Layer Caching?

Can two containers communicate?

in dockerfile use non root users 

What is the difference between: docker stop & docker kill

How do you run 3 replicas of a container?

Your Docker image works locally but fails in production. Why?

Possible reasons:

Different environment variables

Port mismatch

Missing dependencies

Architecture mismatch (amd64 vs arm64)

File permission issues

What is Docker Layer Caching?

what is docker daemon ? how will u secure docker daemon

How do you troubleshoot a container that keeps restarting?

Steps:

docker ps -a
docker logs <container_id>
docker inspect <container_id>


Check:

Crash loops

Health checks

Missing environment variables

Port conflicts

How do you secure Docker containers?

Use non-root user

Use minimal base images

Scan images (Trivy)

Use read-only filesystem

Limit CPU & memory

how to pass environment variables in docker file , as these values are required during application building . the env values should be fetched from AWS secret manager

how to pass environment variables in ECS Task definition , as these values are required during application run time  . the env values should be fetched from AWS secret manager & should not be visible when i see the json format of task definition in ECS Console

study 50 FAANG-level Docker Q&A

How would you manage Docker workloads across multiple clouds?

How do you handle image cleanup to prevent disk space issues?

How do you manage multi-container dependencies using Docker Compose?

How do you monitor container performance in production?

Wrote a multi-stage Dockerfile during screen sharing.

How would you manage Docker workloads across multiple clouds?

How do you handle image cleanup to prevent disk space issues?

How do you manage multi-container dependencies using Docker Compose?

How do you monitor container performance in production?

Wrote a multi-stage Dockerfile during screen sharing.

How do you handle multi-cloud Docker deployments with compliance restrictions?

You need live-patching of Docker host kernel without downtime. How do you achieve it?

How do you troubleshoot container DNS resolution failures?

How do you enforce policy-as-code for Docker security?

How would you manage Docker workloads across multiple clouds?

How do you handle image cleanup to prevent disk space issues?

How do you manage multi-container dependencies using Docker Compose?

How do you monitor container performance in production?

Wrote a multi-stage Dockerfile during screen sharing.

How would you manage Docker workloads across multiple clouds?

How do you handle image cleanup to prevent disk space issues?

How do you manage multi-container dependencies using Docker Compose?

How do you monitor container performance in production 

How do you handle multi-cloud Docker deployments with compliance restrictions?

You need live-patching of Docker host kernel without downtime. How do you achieve it? How do you troubleshoot container DNS resolution failures?

How do you enforce policy-as-code for Docker security?

Docker-in-Docker vs Docker socket mount?
DinD: isolated, slower
Socket mount: faster, security risk

How do you build Docker images securely?
Non-root user
Multi-stage builds
Scan images
Minimal base images

How do you clean up Docker resources?
Post-build cleanup
TTL policies
Kubernetes garbage collection



