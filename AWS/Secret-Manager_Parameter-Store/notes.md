✅ Secret manager is a global service . hence ecs tasks which are private needs to be associated with Secret manager VPC endpoint to access the stored secrets 

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

- If rotation fails before finishSecret, but DB password is already changed → App still reads AWSCURRENT (old password) → connection failure .
- to prevent this use 2 admin user rotation strategy

#### Step 4: finishSecret

<img width="221" height="115" alt="image" src="https://github.com/user-attachments/assets/3899113a-9959-466d-8e70-ed3354e89d7c" />

<img width="206" height="72" alt="image" src="https://github.com/user-attachments/assets/5b422640-f91c-49aa-9a97-88201395201d" />

#### What Happens to Your Application?

<img width="402" height="192" alt="image" src="https://github.com/user-attachments/assets/69838d46-ecfa-4596-9b69-822d6f015e21" />

<img width="416" height="40" alt="image" src="https://github.com/user-attachments/assets/8b264bb2-4386-4ba9-9741-b3ba80a3ef1a" />

### Demo using Key Versions

<img width="418" height="295" alt="image" src="https://github.com/user-attachments/assets/c2910c84-99e5-4b07-aca6-d125fb6e1a66" />

### Secret Replication
- Yes, AWS Secrets Manager secrets are region-specific by default, but they can be replicated to other regions using the replication feature.
- “AWS Secrets Manager replication is used in RDS multi-region architectures to keep database credentials consistent across regions. For example, if an RDS database is running in a primary region like ap-south-1 and has a cross-region read replica or DR setup in us-east-1, the database credentials stored in Secrets Manager can be replicated to the secondary region.
- During password rotation, the secret is updated in the primary region and automatically synchronized to the replica region. This ensures that applications in both regions can securely access the database using the same credentials without manual updates, enabling low-latency access and seamless disaster recovery.”
- “Each replicated secret is encrypted with a region-specific KMS key, while the secret value remains consistent across regions.”

### Rotation Strategy 

<img width="386" height="351" alt="image" src="https://github.com/user-attachments/assets/f80f776c-fbe7-4595-910d-eac9461681c1" />

#### secret manager vs parameter store 

| Feature                 | Secrets Manager               | Parameter Store           |
| ----------------------- | ----------------------------- | ------------------------- |
| Purpose                 | Secrets (passwords, API keys) | Config + secrets          |
| Auto Rotation           | ✅ Yes                         | ❌ No                      |
| Built-in DB integration | ✅ Yes (RDS, etc.)             | ❌ No                      |
| Encryption              | ✅ Always (KMS)                | ✅ Optional (SecureString) |
| Versioning              | ✅ Yes                         | ✅ Yes                     |
| Cost                    | 💲 Paid                       | 🆓 Free (standard tier)   |
| Lambda integration      | ✅ Native rotation             | ❌ Manual                  |
| Cross-account access    | ✅ Yes                         | ✅ Yes                     |

---
