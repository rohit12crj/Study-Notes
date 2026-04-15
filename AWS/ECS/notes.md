✅ if someone has done wrongful deployment in ECS . how will u rollback it
- seperate procedure for rolling deployment & blue green deploymnet
- for rolling deploymnet , re-deploy the last stable version of your task definition
- for blue green deploymnet , just make traffic weight percent to 100% to blue target group
- for alb , blue green deploymnet , only 1 listener should be present which should redirect traffic to 2 taget groups ( blue & green depending on weight percent ). check image below
- <img width="806" height="335" alt="image" src="https://github.com/user-attachments/assets/cfdb92e4-00b2-41a3-bbb3-73dc282a17df" />


---
✅ Difference betwwen alb health check in target group vs halth check in ecs task definition ? what options presnet in health check

---
✅ Explain dockerfile expose option for ports --> mapped to ecs task definition container to host mapping --> mapped to ALB listener port & tagret grp port . ALB listener & taget group ports can be different . there is a port override option also.

---
✅ task role vs task exection role in ECS
- In ECS, the task execution role is used by the ECS agent to perform actions like pulling container images and sending logs, whereas the task role is assumed by the application inside the container to access AWS services like S3 or DynamoDB
- <img width="383" height="88" alt="image" src="https://github.com/user-attachments/assets/46df7bd1-632b-4352-aeed-35ed2a2aa1a9" />

---
✅ How will u setup monitoring for your ECS Cluster , service & tasks & which metrics will u be tracking . explain with just AWS tools . How will u handle alert grouping in case of multiple alerts of same nature ?

---
✅ how will u make sure your backend tasks are just accessible via frontend tasks & not through ALB 

---
✅ what was the scaling paramter u used ? 
- CPU utilization >= 70 %
