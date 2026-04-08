✅ Videos
- https://www.youtube.com/playlist?list=PLdpzxOOAlwvI0O4PeKVV1-yJoX2AqIWuf  --> Terraform ( Theory + Project Playlist ) ( Abhishek )
- https://youtu.be/3Ex-HtbgvyE?si=52d-xXioxDR89hCI  -->  Interview Questions ( Abhishek )
- https://youtu.be/JEDp4orr_K8?si=8IeRZlswzb5-ZlL9  --> Most Common Terraform Task Used in Real-Time ( Abhishek )

---
✅ if you are running aws cli on your local machine . never store the AWS keys & secret access key locally using aws-configure . Use tools like pulumi esc or Hashocorp Vault --> https://www.youtube.com/watch?v=z5N3zJuxv3M

---
✅ Difference between mount point & volumes in ecs . Upload pictures pending

---
✅  how will u acheive Cross account Terraform resource creation 

---

✅ Example where terraform says  destroying resources & is stuck in that scenario 

---
✅ In ecs cluster I have 1 service , running tasks with task definition v1 which is front footed by alb . Alb has 1 listener with 2 target group blue & green. Right now only blue has 100% traffic. If I create new task definition of v2 and launch task with it for green environment. How does these tasks get associated with green target group

---
✅ Check if Terraform lifecycle policy ignore is needed or not for ECS blue green deployment ?

---
✅ backend.tf file ?
- it stores location of remote state
- s3 bucket name  --> name of bucket where all state files of dev & prod will be stored 
- s3 key --> actual name of state file which terraform will assign . terraform creates different folder based on dev & prod if using workspaces . if using directory based approach , we can have altogether different backend.tf files which will have different s3 buckets

---
✅ In AWS sg group one rule added manually but trying to apply same rule through Terraform. What would it show

---
✅ Situation where terraform Plan ok but apply gives error

---
✅ Give common terraform workspace commands ?

---
✅ difference between terraform.tfvars & variables.tf 

---
✅ terraform state list command does ?

---
✅ what actually happens during terraform init internally ? 
- after changing any code inside modules we need to do init 

---
✅ what does  .terraform  folder contains ?

---
✅ what does terraform.lock.hcl contains ?

---
✅ why variable type of any is used ?

---
✅ what happens if terraform lock is lost midway ?

---
✅ explain a scenerio where terraform plan shows no changes , but apply still modifies resources 

---
✅ what problems arise when multiple modules reference tge same resource & how do u design around it 

---
✅ why switching between count & for_each can destroy resources ?

---
✅ how does provider version mismatch causes issues & how do u prevent it

---
✅ how do u refactor a terraform codebase without destroying production database 

---
✅ explain depends_on vs implicit dependency & when does terraform gets it wrong

---
✅ what are partial apply & how do u recover safely form a failed apply ?

---

✅ Explain how will you generate multiple security group ingress rules ? Also how will u allow all ports in sg ingress rules . use ( -1 )

---
✅ Explain how will you create an iam policy which must allow resource id of RDS created initially

---
✅ Terraform template files & why will u use them ?

---
✅ write code for referencing variables from local , tfvars & variablles.tf files in main.tf file

---
✅ explain where will use use map data types while creating AWS resource.

---

✅ when will use terraform workspaces vs directories 
- Terraform workspaces allow you to use the same Terraform code but maintain separate state files.
- not recommended for prod
- explain with example how ill u reference the variables when using terraform workspaces for dev & prod

| Feature          | Workspaces                   | Directories                   |
| ---------------- | ---------------------------- | ----------------------------- |
| Code separation  | Same code                    | Different environment configs |
| State            | Separate state per workspace | Separate backend/state        |
| Complexity       | Simple                       | More scalable                 |
| Enterprise usage | Rare                         | Very common                   |
| CI/CD pipelines  | Harder                       | Easier                        |


<img width="536" height="389" alt="image" src="https://github.com/user-attachments/assets/ff95bab3-fbb1-4f20-9de2-e39716f1df0c" />

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
- Check Using_VPC_Module_in_Dev.pdf also
- There are also official terraform modules for aws managed by terraform themselves 

---

✅ Terraform Vault
- https://www.youtube.com/watch?v=rMG7XZqN0PM&list=PLAdTNzDIZj_9c-o43WJ7yYBa5YEAkWiOv&index=5 --> DevOps Shack
- https://www.youtube.com/watch?v=TeD8jDem2gs&t=835s --> DevOps Shack
- https://www.youtube.com/playlist?list=PL7iMyoQPMtAP7XeXabzWnNKGkCex1C_3C --> Rahul ( Playlist )
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

---

✅ Explain Terraform lifecycle commands.
- Terraform lifecycle rules control how resources behave during updates.

Main lifecycle arguments:
- create_before_destroy – Creates new resource before deleting old one to avoid downtime.

<img width="493" height="400" alt="image" src="https://github.com/user-attachments/assets/f57d2ab4-5768-4ac6-84fe-7608a57bb7c1" />

- prevent_destroy – Protects critical resources from accidental deletion.

<img width="455" height="393" alt="image" src="https://github.com/user-attachments/assets/bdca5804-2300-4df3-bcf7-d0690f1204a9" />

- ignore_changes – Ignores external modifications to specific attributes.

<img width="503" height="449" alt="image" src="https://github.com/user-attachments/assets/c9e3043c-56fa-4b80-94c2-1bbf17c9f0f1" />

- replace_triggered_by – Forces resource replacement when dependent resources change.

<img width="511" height="366" alt="image" src="https://github.com/user-attachments/assets/a2c31546-992f-4851-a81b-96096720e0fb" />

---

✅ how will u import already created resources into terraform ?   
- https://www.youtube.com/watch?v=-4IMy5ihiiU&list=PLdpzxOOAlwvI0O4PeKVV1-yJoX2AqIWuf&index=8 ( Abhishek )
- use terraform import block

<img width="725" height="265" alt="image" src="https://github.com/user-attachments/assets/41ef7f1d-228d-4953-8536-5cdedd035463" />

<img width="704" height="313" alt="image" src="https://github.com/user-attachments/assets/50361989-02d4-414b-b450-a3e9ab576d45" />


---

✅  data sources in terraform 
- In Terraform, data sources are used to fetch information about existing infrastructure that is not managed by Terraform. They allow Terraform configurations to reference existing resources like VPCs, AMIs, or subnets without creating them.
---

✅ Difference between count and for_each?
- count creates multiple resources using numeric indexes, while for_each creates resources using keys from a map or set.
- count is suitable for identical resources, whereas for_each is better when resources have unique identifiers and we want stable resource tracking.

| Feature            | `count`             | `for_each`         |
| ------------------ | ------------------- | ------------------ |
| Input type         | Number              | Map / Set          |
| Resource reference | Index `[0]`         | Key `["dev"]`      |
| Stability          | Index can change    | Keys remain stable |
| Use case           | Identical resources | Unique resources   |

<img width="365" height="413" alt="image" src="https://github.com/user-attachments/assets/dde761d5-559e-4b61-966b-c3f41469815f" />

<img width="416" height="426" alt="image" src="https://github.com/user-attachments/assets/d5214192-aa4d-4041-ac86-78aa6b3a758d" />

---

✅ provisioners and when to use them and when not to use them 

✅ How does Terraform manage dependencies?
- Terraform manages dependencies using a dependency graph (DAG). It automatically detects dependencies when resources reference attributes of other resources, known as implicit dependencies. If Terraform cannot detect the dependency automatically, we can define explicit dependencies using the depends_on argument to control the execution order
  
---

✅ What is terraform refresh?
- terraform refresh updates the Terraform state file by querying the actual infrastructure and synchronizing the state with real resources. It does not modify infrastructure; it only updates the state file.

---

✅ What happens if someone manually deletes a resource that Terraform created? 
- again run terraform apply

---

✅ What is lifecycle block?

What is parallelism in Terraform?

What is terraform graph?

How to avoid downtime during updates? create_before_destroy , Blue-Green deployment ,Separate modules

---

✅ What is dynamic block?
- A dynamic block in Terraform is used to dynamically generate repeated nested configuration blocks inside a resource using loops like for_each. It helps reduce repetitive code and makes configurations more flexible, for example when creating multiple security group ingress rules.

<img width="539" height="344" alt="image" src="https://github.com/user-attachments/assets/007b6610-89a0-446f-bffd-eb20580d35a3" />

<img width="301" height="259" alt="image" src="https://github.com/user-attachments/assets/7327a7e4-b83a-4628-b5d6-ee2d34b9309d" />

<img width="487" height="383" alt="image" src="https://github.com/user-attachments/assets/4a5e438f-1d7b-4007-917b-a207795afcd5" />

---

✅  What is null_resource?

<img width="620" height="289" alt="image" src="https://github.com/user-attachments/assets/92c6740f-9ff7-4725-be4b-40b1ed3a57d8" />

<img width="439" height="310" alt="image" src="https://github.com/user-attachments/assets/ecd4db95-abd1-4bbd-af70-3820841cd846" />

<img width="416" height="181" alt="image" src="https://github.com/user-attachments/assets/248c761f-cfb5-416e-a8d7-df2bcbcb6971" />

---

✅  what are terraform triggers ?
- In Terraform, triggers are used inside a null_resource to force the resource to re-run when specific values change.
- Normally Terraform only recreates a resource when its configuration changes.
- But since null_resource doesn’t manage real infrastructure, Terraform needs a way to know when to run it again . That’s where triggers come in.
- Terraform triggers are used inside a null_resource to force Terraform to recreate the resource when specific values change.
- When a trigger value changes, Terraform destroys and recreates the resource, causing provisioners like local-exec to run again. They are commonly used for deployment automation or script execution.

<img width="455" height="265" alt="image" src="https://github.com/user-attachments/assets/83543138-9005-4f7e-8d28-b6345d9759e6" />

---

Explain terraform validate vs fmt.

---

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

terraform local

