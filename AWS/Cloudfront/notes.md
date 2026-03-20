✅ Cloudfront
- Content Delivery Network (CDN)
- Delivers Content from edge locations

---

✅ Distribution 
- The main CloudFront resource you create. Has a domain like d1234abcd.cloudfront.net. You can attach a custom domain (CNAME).

---

✅ Origin
- Where CloudFront fetches content when it's not cached. Can be S3, ALB, EC2, API Gateway, or any custom HTTP server.

<img width="527" height="331" alt="image" src="https://github.com/user-attachments/assets/cc43cc54-de64-4f0d-9286-6af37f5c7576" />

---

✅ Edge Location
- AWS data center where content is cached. 400+ globally.

---

✅ Regional Edge Cache
- sits between edge locations and origins. Larger cache, fewer cache misses reaching origin.

---

✅ Behavior
- rules that define how CloudFront handles requests for specific URL path patterns. Each distribution has one default behavior and can have many additional ones.

<img width="506" height="296" alt="image" src="https://github.com/user-attachments/assets/8fa525ec-e6f6-4674-989c-4123744e49a8" />

---

✅ Caching

<img width="575" height="325" alt="image" src="https://github.com/user-attachments/assets/f06aede8-b493-4f7b-b9a5-c1cb5e41c432" />

<img width="603" height="127" alt="image" src="https://github.com/user-attachments/assets/6da6385a-9f61-45e8-9d62-4f77b0cbdd2b" />

---

✅ HTTPS & SSL/TLS

<img width="578" height="341" alt="image" src="https://github.com/user-attachments/assets/04ff66c2-5ad8-4806-b687-bdb1ffa98a12" />

---

✅ Custom Domains

<img width="554" height="103" alt="image" src="https://github.com/user-attachments/assets/f296cb3d-5448-4349-9f46-ae7ac576fa97" />

---

✅ Geo Restriction

<img width="558" height="157" alt="image" src="https://github.com/user-attachments/assets/3b7d4766-ee38-432b-946a-95ac28ff56b9" />

---

✅ Signed URLs & Signed Cookies

<img width="610" height="288" alt="image" src="https://github.com/user-attachments/assets/192c2427-10fe-4753-8511-701c5a233662" />

---

✅ Compression

<img width="577" height="151" alt="image" src="https://github.com/user-attachments/assets/85e495b6-9b79-41ae-b71a-af691c709b6b" />

---

✅ CloudFront & S3 — Common Patterns

<img width="523" height="214" alt="image" src="https://github.com/user-attachments/assets/0a176c4d-ece1-4b27-beac-2696aef44b54" />

---

✅ Routing & Failover Patterns

<img width="536" height="173" alt="image" src="https://github.com/user-attachments/assets/c281cfbf-a3fb-42e3-8fc6-2c857a1c1857" />

---

✅ Key Exam / Interview Points

<img width="570" height="367" alt="image" src="https://github.com/user-attachments/assets/87e585c0-6a1d-4be9-bfd4-d46a48ffe479" />

<img width="557" height="180" alt="image" src="https://github.com/user-attachments/assets/ac4fdaa0-0e4a-4ebc-b62b-2f8842ccfd32" />

---

✅ Give me real world expample when you will use signed cookies vs signed urls 

https://youtu.be/JIW_pV3zau8?si=QTc8DOxg7VgIE2w-

| Feature      | Signed URL        | Signed Cookies   |
| ------------ | ----------------- | ---------------- |
| Access scope | Single file       | Multiple files   |
| Use case     | One-time download | Streaming / apps |
| Complexity   | Simple            | Slightly complex |
| Example      | Paid PDF          | OTT platform     |

---

✅ difference between cloudfront signed url vs s3 signed url

<img width="395" height="127" alt="image" src="https://github.com/user-attachments/assets/2558a3a0-4877-4b93-83a8-61a7d76f9fc9" />

---

✅ Give me real world expample when u will use compression

<img width="427" height="455" alt="image" src="https://github.com/user-attachments/assets/56304b87-87d4-421f-82aa-971bade47570" />

---

✅ Give me real world expample where u will use cloudfornt Function vs Lambda@edge functions


---

✅ Can cloudfront be used for dynamic content


---

✅ Security
- Integrate with WAF

