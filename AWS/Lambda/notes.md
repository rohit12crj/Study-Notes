✅ Videos 
- https://www.youtube.com/watch?v=XFGSuj83wdc  --> Rahul

---
✅ Lmabda Batch Processing

---
✅ Handler function
- <img width="563" height="217" alt="image" src="https://github.com/user-attachments/assets/52cc35db-b1aa-4a24-94ef-9990b4f13941" />

---
- ✅ Function Lifecycle

<img width="448" height="112" alt="image" src="https://github.com/user-attachments/assets/ae30f250-3704-4437-9a65-daa97b03b254" />

---
- ✅ Cold Start vs Warm Start
- explain in code how will u configure the cold start section ?

<img width="578" height="73" alt="image" src="https://github.com/user-attachments/assets/00b3a7d8-b920-4e9e-ae51-e486414a8251" />

---

✅ Reducing Cold Starts

<img width="463" height="88" alt="image" src="https://github.com/user-attachments/assets/06960560-54c7-45d4-9f10-d84f89ad00f5" />

---

✅ Triggers

<img width="457" height="269" alt="image" src="https://github.com/user-attachments/assets/fc74ec91-66f9-4823-8bd8-e23b088d1f57" />

Lambda retries 2 times automatically if it fails for async

<img width="620" height="151" alt="image" src="https://github.com/user-attachments/assets/0263207f-205f-442f-ac96-85a9f9c4e888" />

---

✅ Lambda Destinations vs DLQ

<img width="557" height="195" alt="image" src="https://github.com/user-attachments/assets/42772a00-e1b0-48e0-a2f7-d34d5174f627" />

---

✅ Lambda Roles

<img width="437" height="187" alt="image" src="https://github.com/user-attachments/assets/6e22601f-3c04-48b4-acd7-f8d9c70c9d1f" />

---

✅ Lambda VPC Access

<img width="537" height="260" alt="image" src="https://github.com/user-attachments/assets/28c96bbe-4824-4b7a-8629-4d2e84a06d03" />

---

✅ Integration with AWS Services

<img width="524" height="341" alt="image" src="https://github.com/user-attachments/assets/f95c4c12-115e-4723-bf97-d1d50769b521" />

---

✅ Lambda@Edge & CloudFront Functions
- explain with example where will u use Lambda@Edge & CloudFront Functions

<img width="521" height="238" alt="image" src="https://github.com/user-attachments/assets/e4ef84b2-0e84-4220-9648-72251608a5a0" />

<img width="626" height="233" alt="image" src="https://github.com/user-attachments/assets/6b4d1c34-7870-4b3f-9c11-2b42372a2963" />

---

✅ Concurrency

reserved concurrency vs provisioned concurrency 

---

✅ Scaling

explain lambda scaling with SQS i.e FAN out pattern . explain with real world scenerio where will u use it ?

---

✅ Lambda Layers 
- Explain how will u reference the same libraries in 2 different function ?
  
<img width="416" height="113" alt="image" src="https://github.com/user-attachments/assets/56b4a88e-74b5-4546-9efd-1a6027595c06" />

---

✅ Lambda Version --> explain with example 

---

✅ Lambda Extensions

---

✅ Key Exam Traps
- By default Lambda needs access to write to cloudwatch logs

<img width="576" height="299" alt="image" src="https://github.com/user-attachments/assets/a29a51f2-8c17-435d-bb77-a9479406a29f" />

---

✅ Lambda Limits

<img width="461" height="347" alt="image" src="https://github.com/user-attachments/assets/832342e7-4cdd-447f-9b73-210ea55620d4" />

---

✅ Lambda Flows

- Sync = API Gateway --> Lmabda --> get results [ no wait , no retries ]
- Async = S3 --> Lambda [ wait , 2 retries , if failed move to DLQ , SQS FIFO ]
- Poll
  
---

write retry logic for failed lambda invocations
