✅ What is IAM?
- AWS Identity and Access Management 
- Authentication — who are you ?
- Authorization — what are you allowed to do ?
- Free service

---

✅ Core Components

Users
- A user can belong to multiple groups

Groups
- Collection of IAM users
- Policies attached to group apply to all members
- Groups cannot contain other groups

Roles
- Identity with permissions, assumed temporarily by trusted entities
- No long-term credentials — generates temporary security credentials (STS)

Policies
- JSON document defining permissions
- Attached to users, groups, or roles

<img width="569" height="215" alt="image" src="https://github.com/user-attachments/assets/d2193e0e-ef30-406f-9d10-b396b9053838" />

---

✅ Policy Structure

<img width="428" height="307" alt="image" src="https://github.com/user-attachments/assets/41c57c63-9174-420b-a6e3-67e7a68656fd" />

<img width="464" height="191" alt="image" src="https://github.com/user-attachments/assets/edefdcdc-97cd-4915-a18d-bca559d202d2" />

---

✅ Managed vs Inline Policies

<img width="554" height="264" alt="image" src="https://github.com/user-attachments/assets/83c013b2-631d-48b4-9fa1-ccd89161a7f8" />

---

✅ Policy Evaluation Logic

<img width="542" height="396" alt="image" src="https://github.com/user-attachments/assets/0a1c647f-3df6-4b7f-9f97-805af17a7ab5" />

---

✅ Resource-Based Policies vs Roles

<img width="582" height="240" alt="image" src="https://github.com/user-attachments/assets/1aa4f121-6edf-4e73-85d9-71e1d3fdbe87" />

---

✅ Cross-Account S3 Access — Detailed Explanation

<img width="548" height="105" alt="image" src="https://github.com/user-attachments/assets/0c1dc59e-1bf6-4ce8-aa03-62db9075f3de" />

#### Approach 1 — Role Assumption

<img width="532" height="202" alt="image" src="https://github.com/user-attachments/assets/79133799-1e1e-46f3-98e2-5fa73aa5e8f6" />

#### Approach 2 — Bucket Policy

<img width="561" height="128" alt="image" src="https://github.com/user-attachments/assets/16d84330-faec-49bc-8a34-c91717754a48" />

---

<img width="633" height="377" alt="image" src="https://github.com/user-attachments/assets/aaa288cb-e34b-402f-9f52-e8ee5ce8d51f" />

#### Which Approach to Use When?

Use Role Assumption when --> 
- EC2 needs access to multiple resources across Account B (S3 + DynamoDB + SQS)
- You want a clean identity separation — Account B sees its own role, not Account A's identity
- The service doesn't support resource-based policies

Use Bucket Policy when --> 
- Access is limited to one specific S3 bucket
- You want a simpler setup with fewer steps
- You want Account B's CloudTrail to show exactly which principal from Account A accessed the bucket
- You don't want the application code to handle role assumption

<img width="556" height="169" alt="image" src="https://github.com/user-attachments/assets/1ab99fc6-abda-4323-8760-5b34e91af3ab" />
