✅ Other Notable AWS Services

- Opensearch , Opensearch Pipelines ( With Grok Parameter ) , Index Lifecycle Policy
- Dynamo Db , DynamoDb Streams , DAX 
- Athena
- Quicksight
- Redshift
- ACM --> https://youtu.be/_bEPuvrjB5Y?si=bQN1HauiAWRx1Z8b --> Rahul
- Route53 --> https://youtu.be/tXgOSt80Mtg?si=OcMZWZCJPRd_YiVH --> Rahul
- X-Ray --> Tracing tool from AWS . Alternative to opensource Jaeger

---
- ✅ RAM ( Resource Access Manager )
- https://youtu.be/I9aKCdLokOs?si=00I4LUEePWpihwvu
- advantages of RAM

---

- Glue --> check Data Engineering Folder 
- MSK ( Managed Streaming for Kafka ) --> check Data Engineering Folder
- EKS --> Check Kubernetes Folder
- Codepipeline
- Codedeploy
- Cognito ( User Pool , Identity Pool )
- SES ( Simple Email Service )
---
- ✅ Global Accelerator
- https://youtu.be/CUYvzTd9vKE?si=njv9D_89fG0YuovZ
- when you should Accelerator vs Cloudfront
---
- Budget & Cost Explorer
- Backup --> centrally manage & automate backup across various AWS services --> https://www.youtube.com/watch?v=6fvGbywTjsU&t=671s
- Organization --> https://youtu.be/bQ2EtLnN6KQ?si=ndPd9V0fPFHl2EUu --> Rahul
- ECR - images Lifecycle Policy --> image versioning --> Image vulnerability Scanning --> need to generate ECR login password before pushing docker images in pipeline --> password can be genertaed while running the command specified in " view & push images " from ECR Console

---
✅ AWS Secure Landing Zone with Control Tower & Account Factory 
- AWS Control Tower --> automates  setup and governance of a secure multi-account AWS environment using a landing zone, guardrails, and automated account provisioning integrated with AWS Organizations.
- https://tcsglobal.udemy.com/course/aws-secure-landing-zone-with-control-tower-and-aft/  --> not required but check for the terraform part if it is needed or not
- https://www.youtube.com/watch?v=ew16ZDoAals
- difference between OU ( organization unit ) & actual AWS account
- account factory

---

#### Common Architecture Questions

---
✅ Monolithic Vs Microservice architecture --> https://www.youtube.com/watch?v=7nz_VKeuFFc&t=1418s

---
✅ Explain Network Layers with diagram ( Top to Bottom )
- <img width="281" height="202" alt="image" src="https://github.com/user-attachments/assets/59c4053a-5ddf-43b9-896e-a2ba06683850" />

| Layer | Name             | What it does           | Example Tech  | Gmail Login Example                 |
| ----- | ---------------- | ---------------------- | ------------- | ----------------------------------- |
| 7     | **Application**  | User interaction       | HTTP, DNS     | You type `www.gmail.com` in browser |
| 6     | **Presentation** | Encryption, formatting | SSL/TLS       | Data is encrypted using HTTPS       |
| 5     | **Session**      | Session management     | Session IDs   | Maintains login session with Gmail  |
| 4     | **Transport**    | End-to-end delivery    | TCP/UDP       | TCP handshake (SYN, SYN-ACK, ACK)   |
| 3     | **Network**      | Routing via IP         | IP protocol   | Request routed to Google server IP  |
| 2     | **Data Link**    | Local delivery         | MAC, Ethernet | Frame sent via router using MAC     |
| 1     | **Physical**     | Transmission medium    | WiFi, Fiber   | Bits travel over WiFi/fiber         |


---
✅ Explain security hub & security lake with respect to AWS 

---
✅ Difference between headers , querry string , cookies

---
✅ Difference between Forward Proxy & reverse proxy with examples 
- https://youtu.be/CmYI2R2D2M0?si=Wg0v_FmG1_agkMIn --> Abhishek

---

✅ What are the common Reverse proxy used ? 
- Nginx
- Apache
- Caddy

---

✅ Explain Architecture flow
- Internet -> ALB -> Reverse Proxy -> ECS Tasks

<img width="912" height="336" alt="image" src="https://github.com/user-attachments/assets/daeea042-3628-42d1-9dfa-d623c6b1c2ad" />

---
| Feature           | Nginx                         | API Gateway                   | Load Balancer                   |
| ----------------- | ----------------------------- | ----------------------------- | ------------------------------- |
| Primary Role      | Reverse proxy + web server    | API management layer          | Traffic distribution            |
| Layer             | L7 (Application)              | L7 (Application)              | L4 (TCP) / L7                   |
| Routing           | Path-based, host-based        | Advanced API routing          | Basic (IP/port or simple rules) |
| Load Balancing    | ✅ Yes                         | ✅ Yes                         | ✅ Core function                 |
| Authentication    | ❌ Basic (manual config)       | ✅ Built-in (JWT, OAuth, etc.) | ❌ No                            |
| Rate Limiting     | ⚠️ Basic                      | ✅ Advanced                    | ❌ No                            |
| Caching           | ✅ Yes                         | ✅ Yes                         | ❌ No                            |
| SSL Termination   | ✅ Yes                         | ✅ Yes                         | ✅ Yes                           |
| Service Discovery | ❌ Limited                     | ✅ Yes                         | ❌ No                            |
| Observability     | ⚠️ Limited                    | ✅ Rich (metrics, tracing)     | ⚠️ Basic                        |
| Use Case          | Reverse proxy, static serving | Microservices API control     | Distribute traffic              |

---
✅ Why ALB & reverse Proxy are both used 
- ALB can handle basic application-level routing like path and host-based rules, but it lacks advanced capabilities such as URL rewriting, caching, custom authentication logic, and internal container routing.
- That’s why a reverse proxy like Nginx is often used alongside ALB to handle fine-grained application-level control.
- <img width="247" height="215" alt="image" src="https://github.com/user-attachments/assets/8d2fb7d8-0058-478b-ada2-2d0822b2a593" />
- <img width="290" height="113" alt="image" src="https://github.com/user-attachments/assets/76a8e2b4-0394-46e0-946d-d64f7346c506" />

---
✅ SAML vs OIDC ?
- SAML is an XML-based, enterprise-focused SSO protocol, while OIDC is a modern, lightweight authentication layer using JWT built on OAuth 2.0, better suited for web and mobile applications
- https://www.youtube.com/watch?v=iX8g4LqF8p8&t=97s
- https://www.youtube.com/watch?v=_lTECv25N2U
- <img width="348" height="209" alt="image" src="https://github.com/user-attachments/assets/6dc0901e-2528-4061-8a30-854fee112901" />

---
✅ what are jump servers / bastion servers ? why do u need them ? How do u secure them ? Can we eliminate them using AWS Native services 
- https://youtu.be/pI6glWVEkcY?si=Wc_B8NiUvt7_n0YS

---
✅ How does webhooks works actually & why should we use them ?

---
✅ Difference between SSL , TLS & mTLS
- SSL is an outdated protocol, TLS is its secure successor used for encrypted communication, and mTLS extends TLS by enabling mutual authentication where both client and server verify each other
- <img width="533" height="380" alt="image" src="https://github.com/user-attachments/assets/68677afe-1ad5-43b4-a68e-462fb90baa68" />

---
✅ encoding vs encrption vs hashing
- <img width="460" height="220" alt="image" src="https://github.com/user-attachments/assets/381a8e89-d257-4a38-8c63-c049b123b6d5" />
- <img width="428" height="253" alt="image" src="https://github.com/user-attachments/assets/f50a5a8b-a0d2-4c76-914e-65cfd718cfd1" />
- <img width="378" height="215" alt="image" src="https://github.com/user-attachments/assets/910fd02f-4e0e-43ae-b61a-29bf5b7a0a87" />
- <img width="518" height="271" alt="image" src="https://github.com/user-attachments/assets/5bdbc904-23e0-4765-828f-a1b1c3ab457f" />
- Remember passwords are always hashed (not encrypted)

---
✅ SSL termination vs passthrough vs bridging
- SSL termination decrypts traffic at the load balancer, passthrough keeps it encrypted end-to-end, and bridging decrypts at the load balancer but re-encrypts before sending to the backend

---
✅ GraphQL API ?
- REST requires multiple endpoints and calls for related data, GraphQL consolidates everything into a single flexible query, and HTTP API is a lightweight implementation (like AWS API Gateway HTTP API) used to expose APIs efficiently.

---
✅ Dual Connectivity from on prem to AWS to Azure / GCP
- https://youtu.be/doAVHFjWTqo?si=Zn1UOhFbslwsvGSZ
- <img width="491" height="106" alt="image" src="https://github.com/user-attachments/assets/8099cae6-ba08-4cca-bf53-24388b37f41a" />
- "In a hybrid multi-cloud setup, OSPF is used for internal routing within on-prem, while BGP is used for external connectivity with AWS Direct Connect and Azure ExpressRoute. Routes are redistributed between OSPF and BGP at the edge router. Primary and secondary paths are controlled using BGP attributes like Local Preference and AS Path Prepending. In case of failure of the primary path (AWS), BGP automatically fails over to the secondary path (Azure)."
- <img width="410" height="265" alt="image" src="https://github.com/user-attachments/assets/9ce85682-e0f6-4861-b7b3-cfcb8a65d4d4" />

---
✅ Nginx Full Course of 1 hr 
- https://youtu.be/9jZEfW8h5fQ?si=yt6wvKd30Btxn44d --> Abhishek

---
✅ explain HA for all aws services in your project ?  --> check DR Project

