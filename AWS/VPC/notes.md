✅ Your EC2 instance is running in a private subnet, and it cannot access the internet to install packages
- EC2 → NAT Gateway → Internet Gateway → Internet
  
---

<img width="503" height="283" alt="image" src="https://github.com/user-attachments/assets/b769e9fa-11b5-4bf7-af58-ab35a3ecfbc7" />

---

<img width="527" height="389" alt="image" src="https://github.com/user-attachments/assets/6504eecb-2354-4bd2-89ac-44f7a3db9803" />

---

✅ Connect on-premise data center to AWS VPC
--> Site-to-Site VPN --> Customer Gateway ( CGW ) --> Virtual Private Gateway ( VGW )
--> Direct Connection 

---

✅ VPCs in two regions need to communicate
--> VPC peering --> Transitive peering doesn't works i.e if a --> B & b --> c doesn't means a --> c
--> Transit Gateway --> Hub & Spoke model

---

<img width="549" height="278" alt="image" src="https://github.com/user-attachments/assets/a900eaca-6326-4f68-b31b-c2d4335cc6cf" />

---

<img width="508" height="187" alt="image" src="https://github.com/user-attachments/assets/d060151f-1fb3-4f8d-8048-78bd08ea7faf" />

---

<img width="479" height="353" alt="image" src="https://github.com/user-attachments/assets/2f0ac6f9-5551-417a-8b08-940b0d608595" />

---

<img width="501" height="397" alt="image" src="https://github.com/user-attachments/assets/5d98f02b-43f0-4b8a-ab6f-4e3334f82753" />

---
