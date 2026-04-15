✅ https://www.youtube.com/playlist?list=PLdpzxOOAlwvLjb0vTD9BXLOwwLD_GWCmC  --> Docker (Playlist ) --> Abhishek 

---
✅ Give common docker compose commands ?

---
✅ How will u secure docker daemon ?

---
✅ Real Time Scenario Based Docker Implementations --> DevOps Shack --> https://www.youtube.com/watch?v=9hOSkx9jWPQ

---
✅ Why docker-compose.yml file is used ?
- A docker-compose.yml file is used to define, manage, and run multiple containers together as a single application.
- it can also be used to define networking , volumes
- docker-compose.yml schema = services + networking + storage + configuration in a structured YAML format.
- <img width="350" height="160" alt="image" src="https://github.com/user-attachments/assets/7a419138-1354-4951-87ef-60b020e790d2" />

---
✅ docker-compose.yml schema 
- <img width="518" height="407" alt="image" src="https://github.com/user-attachments/assets/87123d3b-9881-4a26-b591-7a92635fde09" />
- <img width="503" height="394" alt="image" src="https://github.com/user-attachments/assets/e5386246-947f-4130-93d5-128f0656c503" />
- <img width="503" height="243" alt="image" src="https://github.com/user-attachments/assets/691fe715-aa2e-4589-990b-8ed58e847021" />
- <img width="510" height="391" alt="image" src="https://github.com/user-attachments/assets/0acf490f-d205-49cc-8dd6-23a48d056d9f" />
- <img width="310" height="135" alt="image" src="https://github.com/user-attachments/assets/52b60be0-4879-4752-b31c-8d53fb2a60fc" />
- <img width="506" height="224" alt="image" src="https://github.com/user-attachments/assets/b6e1140d-83fa-411c-abca-5e9cf0a65011" />

---
✅ Best Pracrtices for writing docker-compose.yml file 
<img width="527" height="347" alt="image" src="https://github.com/user-attachments/assets/555a9eca-a5ab-4ede-afd7-947968eee440" />

---
✅ In the docker compose file how can i make sure the backend container should only be accessible from frontend , & db container from backend . let's assume frontend is running on port 8080 , backend on port 3000 , & db on port 5432
- You enforce this using network isolation + no port exposure
- Frontend and backend share one network, backend and DB share another, and since frontend is not part of the DB network, it cannot access the database directly
- <img width="480" height="398" alt="image" src="https://github.com/user-attachments/assets/c1661f74-01de-48cd-a97f-10291741f10b" />
- <img width="378" height="364" alt="image" src="https://github.com/user-attachments/assets/6ed5f4da-e322-4e2c-b3b9-6cd73122e4f9" />


----
✅ Docker Compose with Nginx Reverse Proxy
- <img width="519" height="105" alt="image" src="https://github.com/user-attachments/assets/b7bc5ae3-e5a7-4e9e-8c1d-1d35aa8cf0bd" />
- <img width="523" height="414" alt="image" src="https://github.com/user-attachments/assets/61372dce-b788-4bc0-ac41-24af37200acf" />
- <img width="535" height="170" alt="image" src="https://github.com/user-attachments/assets/4c94cd75-63e0-44b4-bba0-2767b68b111d" />
- <img width="520" height="320" alt="image" src="https://github.com/user-attachments/assets/e5f69c76-6cb8-4653-a602-e4cee30a6e90" />
- <img width="483" height="397" alt="image" src="https://github.com/user-attachments/assets/5f04120b-921f-411a-975e-cd35f0a2d16e" />
- <img width="510" height="375" alt="image" src="https://github.com/user-attachments/assets/9e8b2f07-5d8a-42e4-96e9-39eb6a405887" />
- <img width="418" height="434" alt="image" src="https://github.com/user-attachments/assets/7c187d94-99d7-43a6-ab73-8b3624aa51b2" />
- <img width="569" height="344" alt="image" src="https://github.com/user-attachments/assets/ed8d3291-e70a-495b-a375-8400b33a90c9" />
- <img width="589" height="383" alt="image" src="https://github.com/user-attachments/assets/ae522009-8550-4f9f-9294-904549a2a0d6" />
- <img width="330" height="83" alt="image" src="https://github.com/user-attachments/assets/6789b46c-45bf-490d-bee5-152d519dae00" />

---
✅ what advantage do i get of using nginx between frontend & backend ?
- Hides backend (improves security, no direct access)
- Acts as a single entry point for all traffic
- Enables load balancing across backend instances
- Supports path-based routing (e.g., /api → backend)
- Handles SSL termination (HTTPS offloading)
- Provides rate limiting and basic DDoS protection
- Enables response caching for better performance
- Allows header manipulation (auth, client IP, etc.)
- Decouples frontend from backend (loose coupling)
- Supports zero-downtime deployments (blue-green / rolling)

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
✅ What is the difference between VM and Docker?
- Docker containers share the host OS kernel and provide lightweight process-level isolation, whereas virtual machines run a full guest OS on a hypervisor, making them heavier but more isolated.
- <img width="289" height="92" alt="image" src="https://github.com/user-attachments/assets/90d31cad-301f-4209-b3ca-77497f35c101" />
- <img width="403" height="449" alt="image" src="https://github.com/user-attachments/assets/9b5fbd2d-8ee3-4a44-b147-38b46721384e" />

---
✅ type 1 hypervisor vs type 2 hypervisor
- <img width="386" height="137" alt="image" src="https://github.com/user-attachments/assets/c2cc66d2-07cf-4420-9a3c-84a8c40ae976" />
- <img width="496" height="329" alt="image" src="https://github.com/user-attachments/assets/7733c876-4109-4a9d-a953-470b1e72e896" />

---
✅ how will u pass env variables when app is building like in a docker without storing teh secret in docker layer
- Secrets should never be passed using ENV or ARG in Dockerfile because they get stored in image layers. Instead, pass them at runtime using environment variables, Docker/Kubernetes secrets, or external secret managers. For build-time needs, use Docker BuildKit secrets which are not persisted in layers

---
how will u pass env variables when app is running  like in a ECS Task definition

---
✅ CMD and ENTRYPOINT?
- <img width="515" height="128" alt="image" src="https://github.com/user-attachments/assets/a5b58096-6c50-4c58-8037-ae39606597b8" />
- <img width="357" height="198" alt="image" src="https://github.com/user-attachments/assets/07a6ef26-c395-4d70-8097-10b439f6d09e" />
- <img width="367" height="86" alt="image" src="https://github.com/user-attachments/assets/481e28b6-93ce-4e09-b7e5-7ecd9145544d" />

---
✅ copy vs ADD
- <img width="477" height="90" alt="image" src="https://github.com/user-attachments/assets/3b58156c-69bd-4880-abd3-60c5b548664c" />

---
✅ Difference between bind mount and volume?
- Bind mounts map a specific host directory into a container and are tightly coupled with the host, whereas volumes are managed by Docker, making them more portable, secure, and suitable for production use
- <img width="423" height="350" alt="image" src="https://github.com/user-attachments/assets/98f8b4cb-44c3-466f-b922-4001f2d0d66b" />

---
✅ Named vs anonymous volumes
- Named volumes are user-defined and reusable, making them ideal for persistent data, while anonymous volumes are auto-created by Docker with random names and are harder to manage, usually used for temporary data
- <img width="443" height="83" alt="image" src="https://github.com/user-attachments/assets/c80805f3-5418-4538-be7c-cb91e82263dc" />

---
✅ Docker host vs docker bridge netorking ?
- docker bridge is also called docker 0 . default network
- <img width="401" height="127" alt="image" src="https://github.com/user-attachments/assets/e375f8f2-29b8-47ad-82f3-f583326fb949" />
- <img width="469" height="388" alt="image" src="https://github.com/user-attachments/assets/830363fa-c1cd-46d5-b578-e8b692c10b73" />
- <img width="404" height="366" alt="image" src="https://github.com/user-attachments/assets/7b48ece5-aaef-4b73-9c5b-ca0f04f431b9" />
- <img width="399" height="349" alt="image" src="https://github.com/user-attachments/assets/16bc570e-9209-471a-afb7-f07dfb43c327" />
- <img width="554" height="305" alt="image" src="https://github.com/user-attachments/assets/1fef7b9f-42cd-430d-b7ab-20aa75f55c2a" />

---
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



