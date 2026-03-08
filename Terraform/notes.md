✅ https://www.youtube.com/playlist?list=PLdpzxOOAlwvI0O4PeKVV1-yJoX2AqIWuf  --> Terraform ( Theory + Project Playlist ) ( Abhishek )

✅ https://youtu.be/3Ex-HtbgvyE?si=52d-xXioxDR89hCI  -->  Interview Questions ( Abhishek )

✅ https://www.youtube.com/watch?v=-4IMy5ihiiU&list=PLdpzxOOAlwvI0O4PeKVV1-yJoX2AqIWuf&index=8 --> Interview Questions --> Part 2 ( Abhishek )

✅ https://youtu.be/JEDp4orr_K8?si=8IeRZlswzb5-ZlL9  --> Most Common Terraform Task Used in Real-Time ( Abhishek )

✅ when will use terraform workspaces vs directories 

---

✅ Terraform directory structure

<img width="380" height="284" alt="image" src="https://github.com/user-attachments/assets/423d9dc0-bb31-4c81-b4f3-fb65c7f5d307" />

---

✅ Variables precedence order 

<img width="203" height="175" alt="image" src="https://github.com/user-attachments/assets/d867b9c1-f32b-444f-986d-1020bf026e58" />

---

✅ Is DynamoDB still required for Terraform state locking?
- Earlier Terraform required a DynamoDB table for state locking when using an S3 backend. However, newer Terraform versions (1.10+) support native S3 state locking using a lock file (use_lockfile = true). Because of this, DynamoDB-based locking is now deprecated and will eventually be removed. Many legacy infrastructures still use DynamoDB, but new setups typically rely on S3 native locking

<img width="555" height="335" alt="image" src="https://github.com/user-attachments/assets/7cbedf82-70a7-471d-8f70-beab8ba5040f" />

---

✅ how will u do terraform version upgrade withput downtime

---

✅ What is IaC ? Why Terraform ?

---

✅ What are modules in Terraform?
- We create reusable infrastructure components like VPC, ECS, and RDS inside a modules directory. Each environment such as dev or stage contains a main.tf that calls these modules using a relative path like ../../modules/vpc. Environment-specific values are passed through terraform.tfvars, while the module itself contains the actual resource definitions.
- Check Terraform_Module_Flow.pdf also

---

✅ Terraform Vault
- https://youtu.be/7xl4hY-W6Ts?si=lFPcBacoJWgNcr6G --> Abhishek
- integrate Terraform vault with terraform tf files & access secret stored in vault in pass it to ec2 as tag -->  https://youtu.be/sIkRK33AY50?si=jgoAaf3ZRPvrQAxV --> Abhishek  --> needs vault provider & approle / azure ad authentication type
- integrate Terraform vault with jenkins
- integrate Terraform vault with github actions
- steps on how to setup vault --> https://github.com/iam-veeramalla/terraform-zero-to-hero/blob/main/Day-7/02-vault-integration.md
- vault comes in dev & prod mode
- vault port no --> 8200 TCP

---

✅ What is a state file in Terraform ?

✅ What are some of the most common Terraform CLI commands that you use every day ?

✅ What is Terraform backend ?

✅ What is Terraform Remote banckend ?

✅ What is Resource Graph in Terraform ?

---

✅ What is Terraform State Locking ?

<img width="620" height="379" alt="image" src="https://github.com/user-attachments/assets/0383b732-59a2-47e8-b14e-481fcdefc29d" />

---

✅ What is a Tainted Terraform resource ?
- A tainted resource in Terraform is a resource marked for recreation. Terraform will destroy and recreate the resource during the next terraform apply. This usually happens when a resource creation fails or when the user manually taints it.
- terraform apply -replace --> forces Terraform to destroy and recreate a specific resource in a single step
- terraform destroy deletes all infrastructure managed by the Terraform configuration.

---

✅ What is Terraform State Rollback ?
- Terraform State Rollback is the process of restoring a previous version of the Terraform state file when the current state becomes inconsistent or corrupted. It is commonly done using state file backups or versioning in remote backends like S3.

---

✅ terraform plan shows correctly what resources will be created , however when terraform apply is used only half of the reources got created . what will u do now ? from next time how will u setup terraform rollback if even 1 resource fail to get created ?
- If terraform apply partially creates resources, I would first check the Terraform state using **terraform state list** and verify the actual infrastructure. Then I would run terraform plan again to see what resources are missing and run terraform apply to complete the deployment since Terraform is idempotent.
- For future prevention, I would use remote state with S3 versioning, DynamoDB state locking, CI/CD approval stages, and infrastructure modularization to ensure safe deployments and allow state rollback if needed.

---

✅ drift detection ?
- using pipeline --> In CI/CD pipelines, it is usually implemented by running terraform plan -detailed-exitcode on a schedule and triggering alerts when changes are detected

<img width="662" height="431" alt="image" src="https://github.com/user-attachments/assets/ec33cc91-3453-4615-8342-7065ead3447a" />

<img width="660" height="163" alt="image" src="https://github.com/user-attachments/assets/ba3e6d16-2d9c-4a5a-9e80-f53e38b263ec" />

- using terraform cloud --> Terraform Cloud provides built-in drift detection through health checks that periodically compare the Terraform state with actual infrastructure.

---

✅ which terraform version were u using ?
- v1.14
  
---

✅ if someone deletes state file what will happen ? check for s3 versioning 

---

✅ How to handle multi-account AWS setup? use alias

---

✅ What makes Terraform declarative?

✅ Explain Terraform lifecycle commands.

---

✅ how will u import already created resources into terraform ?   
- terraform import

---

✅  data sources in terraform 

---

✅ Difference between count and for_each?

✅ provisioners and when to use them and when not to use them 

✅ How does Terraform manage dependencies?

✅ What is terraform refresh?

✅ What is lifecycle block?

What is parallelism in Terraform?

What is terraform graph?

How to avoid downtime during updates? create_before_destroy , Blue-Green deployment ,Separate modules

What is dynamic block?

What is null_resource?

Explain terraform validate vs fmt.

What is terraform console?

What is sensitive variable?

How to detect cyclic dependency?

What is partial configuration?

What is terraform destroy target?

How to prevent accidental production deletion?

How to migrate local state to remote?

---

What is state file corruption recovery?

---

✅ Explain blue-green deployment in Terraform.

---

✅ What is terraform plan exit code? --> check drift section

---

What are Terraform limitations?  --> not good with provisioners 

---

✅ What are Terraform Workspaces and when should you use them?  --> use them for local but not good for prod

---

How does Terraform state management work internally?

What is terraform import and in which real-world scenario is it used?

version pinning

---

✅ Your Terraform deployment takes 40 minutes and often fails. How do you redesign it? 
- Split state files  , Modularize ,Introduce CI gating ,Increase parallelism carefully ,Use multi-account isolation ,Reduce monolithic design

---

why terraform should not be used to manage resources configuration unlike ansible ?

What to do when your Terraform state file becomes too large?

Terraform plan shows destroy + recreate for a critical DB- how to prevent downtime?

How do you manage Terraform provider versioning?

How would you provision infra across 10 AWS regions

Terraform state got corrupted - what will you do?

Your Terraform apply succeeded, but some resources are not behaving as expected. How do you debug? Your Terraform state file is getting too large. How do you manage it?

How do you run Terraform safely in CI/CD pipelines?

You want faster Terraform runs in large projects. What optimizations would you apply?

A production deployment failed halfway, leaving some resources created and others not. How do you recover?

