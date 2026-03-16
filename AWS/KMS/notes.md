✅ What is KMS?
- AWS Key Management Service — managed service to create, manage, and control cryptographic keys used to encrypt data across AWS services and applications.
- FIPS 140-2 validated HSM ( Hardware Security Module ) under the hood
- Fully integrated with most AWS services
- All API calls logged in CloudTrail (audit trail)

---

✅ what is key material ?
- Key material = the raw secret bytes that make up an encryption key.

<img width="500" height="258" alt="image" src="https://github.com/user-attachments/assets/e1bc397d-a40e-4bce-aa99-a287e05c03f3" />

---

✅ What is HSM ?
- HSM is a tamper-resistant hardware device used to securely generate, store, and manage cryptographic keys and perform encryption operations.

---

✅ Key Types --> By Origin

<img width="610" height="229" alt="image" src="https://github.com/user-attachments/assets/9bd02e52-2c88-4e80-8b8b-b2f2891bcc3f" />

<img width="372" height="357" alt="image" src="https://github.com/user-attachments/assets/da392dd8-1912-4bf5-b745-0327da4b8ca1" />

---

✅ Key Types --> By Algorithm

<img width="621" height="92" alt="image" src="https://github.com/user-attachments/assets/5753b430-9f2a-42d7-b55b-a9e6159f28b8" />

---

✅ Types of Encryption in KMS

<img width="437" height="203" alt="image" src="https://github.com/user-attachments/assets/4323d43e-85b3-426d-bc46-2ab4b6594697" />

---

✅ Envelope Encryption

<img width="380" height="222" alt="image" src="https://github.com/user-attachments/assets/449fd942-aaa9-48a4-afad-a8fb35b6c204" />

---

✅ KMS Key States

<img width="561" height="248" alt="image" src="https://github.com/user-attachments/assets/17dc25dd-f639-4bfc-8b4d-83624f0335f7" />

---

✅ KMS Key Policies

<img width="453" height="275" alt="image" src="https://github.com/user-attachments/assets/2348f1b0-2fde-46cd-bf2f-396005c2d534" />

---

✅ KMS Grants

<img width="536" height="143" alt="image" src="https://github.com/user-attachments/assets/ec3706da-33f1-482c-b407-9a7cff9717d2" />

- Grants allow temporary access to a KMS key.
- Deleting a key is irreversible — data encrypted with it is permanently unrecoverable. Always disable first.

---

✅ Key rotation

<img width="574" height="275" alt="image" src="https://github.com/user-attachments/assets/7c91367d-523b-4534-a75b-3edb1581c4ea" />

---

<img width="541" height="302" alt="image" src="https://github.com/user-attachments/assets/82e71c69-5130-4b96-9025-0c904fa53547" />

---

✅ Multi region Key

<img width="538" height="173" alt="image" src="https://github.com/user-attachments/assets/d2539bcb-d245-4636-8b75-0975ad4be73b" />

---

You want to move encrypted data from one CMK to another (e.g., old key is being deprecated). How do you do it without exposing the plaintext?

---

✅ Your AWS bill shows very high KMS costs. The application heavily uses S3 with SSE-KMS. How do you reduce costs?

<img width="593" height="296" alt="image" src="https://github.com/user-attachments/assets/30fbcd32-44dd-4ae5-a15d-e987808f9945" />

---

✅ A developer accidentally scheduled a CMK for deletion. Production data is encrypted with it. What do you do?

<img width="590" height="310" alt="image" src="https://github.com/user-attachments/assets/49b856b3-d3d8-4ef3-9dc7-9eeb78d9cf30" />

---

✅ Your financial application has a compliance requirement that cryptographic keys must never leave a dedicated HSM that you control. Can you still use KMS?
- Yes — use KMS Custom Key Store backed by CloudHSM:

---

✅ Team A owns a CMK in Account A. Team B in Account B needs to use that CMK to encrypt/decrypt data. What needs to be configured?

<img width="573" height="390" alt="image" src="https://github.com/user-attachments/assets/0637ab64-9d87-474d-b002-eed99893ffdc" />

---

✅ You're building an active-active application running in us-east-1 and eu-west-1. Data encrypted in us-east-1 needs to be decrypted in eu-west-1. How do you handle this with KMS?

<img width="535" height="281" alt="image" src="https://github.com/user-attachments/assets/d229b36c-5ce9-467e-8796-02e3cf5c0d30" />

---
