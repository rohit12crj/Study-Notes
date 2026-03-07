✅ https://www.youtube.com/playlist?list=PLdpzxOOAlwvI0O4PeKVV1-yJoX2AqIWuf  --> Terraform ( Theory + Project Playlist ) ( Abhishek )

✅ https://youtu.be/3Ex-HtbgvyE?si=52d-xXioxDR89hCI  -->  Interview Questions ( Abhishek )

✅ https://youtu.be/JEDp4orr_K8?si=8IeRZlswzb5-ZlL9  --> Interview Questions --> Part 2 ( Abhishek )

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

✅ What is a state file in Terraform ?

✅ What are some of the most common Terraform CLI commands that you use every day ?

✅ What is Terraform backend ?

✅ What is Terraform Remote banckend ?

✅ How do you handle secrets in terraform ?

✅ What is Resource Graph in Terraform ?

✅ What is Terraform State Locking ?

✅ What is a Tainted Terraform resource ?

✅ What is Terraform State Rollback ?

✅ terraform plan shows correctly what resources will be created , however when terraform apply is used only half of the reources got created . what will u do now ? from next time how will u setup terraform rollback if even 1 resource fail to get created ?

✅ drift detection using pipeline and terraform cloud

✅ how terraform vault works and how will u integrate with pipelines

---

✅ which terraform version were u using ?

---

✅ terraform state file lock in s3 ? if 2 developers are trying to modify same resource what will happen 

---

✅ if someone deletes state file what will happen

✅ terraform multiple providers ? use alias

✅ What makes Terraform declarative?

✅ Explain Terraform lifecycle commands.

✅ how will u import already created resources into terraform 

✅ What are data sources?

✅ Difference between count and for_each?

✅ provisioners and when to use them and when not to use them 

✅ How does Terraform manage dependencies?

✅ What is terraform import?

✅ What is terraform refresh?

✅ What is lifecycle block?

What is parallelism in Terraform?

What is terraform graph?

How to avoid downtime during updates? create_before_destroy , Blue-Green deployment ,Separate modules

What is dynamic block?

What is null_resource?

Explain terraform validate vs fmt.

What is terraform console?

Explain variable precedence.

What is sensitive variable?

How to detect cyclic dependency?

What is partial configuration?

What is terraform destroy target?

How to prevent accidental production deletion?

How to migrate local state to remote?

How to handle multi-account AWS setup? use alias

What is state file corruption recovery?

Explain blue-green deployment in Terraform.

What is terraform plan exit code?

How to manage multiple regions?

What are Terraform limitations?

What are Terraform Workspaces and when should you use them?

How does Terraform state management work internally?

What is terraform import and in which real-world scenario is it used?

version pinning

Your Terraform deployment takes 40 minutes and often fails. How do you redesign it? --> Expected Answer: Split state files  , Modularize ,Introduce CI gating ,Increase parallelism carefully ,Use multi-account isolation ,Reduce monolithic design

why terraform should not be used to manage resources configuration unlike ansible ?

What to do when your Terraform state file becomes too large?

Terraform plan shows destroy + recreate for a critical DB- how to prevent downtime?

How do you manage Terraform provider versioning?

How would you provision infra across 10 AWS regions

Have you configured secrets or handled Terraform state files? Explain your experience.

Terraform state got corrupted - what will you do?

Terraform plan shows destroy + recreate for a critical DB- how to prevent downtime?

Your Terraform apply succeeded, but some resources are not behaving as expected. How do you debug? Your Terraform state file is getting too large. How do you manage it?

How do you run Terraform safely in CI/CD pipelines?

You want faster Terraform runs in large projects. What optimizations would you apply?

A production deployment failed halfway, leaving some resources created and others not. How do you recover?

