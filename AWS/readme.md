Other Notable AWS Services

- Opensearch , Opensearch Pipelines , Index Lifecycle Policy
- Dynamo Db , DynamoDb Streams , DAX 
- Athena
- Quicksight
- Redshift
- Glue , crawler
- Codepipeline
- Codedeploy
- Cognito ( User Pool , Identity Pool )
- SES ( Simple Email Service )
- AWS Control Tower --> automates  setup and governance of a secure multi-account AWS environment using a landing zone, guardrails, and automated account provisioning integrated with AWS Organizations.
- ECR - images Lifecycle Policy --> image versioning --> Image vulnerability Scanning --> need to generate ECR login password before pushing docker images in pipeline --> password can be genertaed while running the command specified in " view & push images " from ECR Console

---

✅ Secret Manager --> Automatic Secret Rotation Process

<img width="353" height="90" alt="image" src="https://github.com/user-attachments/assets/0a16d6e0-f6ed-4e01-b3f0-068d7140cd92" />

#### Initial State (Before Rotation)

<img width="259" height="91" alt="image" src="https://github.com/user-attachments/assets/f9491ecb-e399-424e-8e20-3a0f4a8d3de6" />

#### Step 1: createSecret

<img width="269" height="136" alt="image" src="https://github.com/user-attachments/assets/6f528fad-b79d-4918-bb36-10419e2d5c09" />

<img width="491" height="149" alt="image" src="https://github.com/user-attachments/assets/0667119e-e23d-4e28-9e63-63f00a4f8bfc" />


#### Step 2: setSecret

<img width="338" height="170" alt="image" src="https://github.com/user-attachments/assets/01c46724-8701-4e86-b5c3-1949c0f51d07" />

#### Step 3: testSecret

<img width="263" height="85" alt="image" src="https://github.com/user-attachments/assets/ba16c105-d1d3-4b74-bd6d-d83a7a86202c" />

<img width="286" height="189" alt="image" src="https://github.com/user-attachments/assets/450d295b-e169-46e4-adca-fcd10ab05250" />


#### Step 4: finishSecret

<img width="221" height="115" alt="image" src="https://github.com/user-attachments/assets/3899113a-9959-466d-8e70-ed3354e89d7c" />

<img width="206" height="72" alt="image" src="https://github.com/user-attachments/assets/5b422640-f91c-49aa-9a97-88201395201d" />


---
