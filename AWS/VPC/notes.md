✅ Videos
- Transit Gateway --> https://www.youtube.com/watch?v=GV4KreiF_D4
- VPC Peering --> https://youtu.be/36qsohuPzMQ?si=R3EQ1wELh7ubU-b3
- WAN  --> https://www.youtube.com/watch?v=fcHIRvZTods
- VPC Flow Logs --> https://youtu.be/2PQIDssp9ts?si=974gn-WI20hk_2jT
- on prem to AWS VPN i.e site to site VPN --> https://youtu.be/h6YuZ9k9nzo?si=_Se7cHiFMzsngF7_
- NAT Gateway --> https://youtu.be/ydxEeVAqVdo?si=E4l8_m1ERj0rj_vd
- VPC Endpoint --> https://youtu.be/vzTKr035ORQ?si=EMcFcrbJ0ah8HtGB
- VPC Endpoint Service ( PrivateLink ) --> https://youtu.be/o0mzIlKHZdw?si=ocwE32uAtzUJYte9
- Full networking course --> 

---
- use VPC peering to connect 1:1 VPC .
- If multiple VPC , use 1 Transit Gateway to connect to multiple VPC .
- Transit geteway are generally 1 per region . u can create multiple transit getway per region , but that is a bad practice , but u can do so for compliance purpose like dev transit gateway , prod transit gateway.
- to connect to vpc of another region use transit gateway peering.
- if multiple region are there u need to create multiple transit gateway peering  . then u should use Cloud WAN



---
✅ after creating internet gateway & nat gateway , you need to attache it to VPC & also add the default routes for public route table & private route table accordingly 

---
✅ Your EC2 instance is running in a private subnet, and it cannot access the internet to install packages
- EC2 → NAT Gateway → Internet Gateway → Internet
  
---

<img width="503" height="283" alt="image" src="https://github.com/user-attachments/assets/b769e9fa-11b5-4bf7-af58-ab35a3ecfbc7" />

---

<img width="527" height="389" alt="image" src="https://github.com/user-attachments/assets/6504eecb-2354-4bd2-89ac-44f7a3db9803" />

---

✅ Connect on-premise data center to AWS VPC
- Site-to-Site VPN --> Customer Gateway ( CGW ) --> Virtual Private Gateway ( VGW )
- Direct Connection 

---

✅ VPCs in two regions need to communicate
- VPC peering --> Transitive peering doesn't works i.e if a --> B & b --> c doesn't means a --> c
- Transit Gateway --> Hub & Spoke model

---

<img width="549" height="278" alt="image" src="https://github.com/user-attachments/assets/a900eaca-6326-4f68-b31b-c2d4335cc6cf" />

---

<img width="508" height="187" alt="image" src="https://github.com/user-attachments/assets/d060151f-1fb3-4f8d-8048-78bd08ea7faf" />

---

<img width="479" height="353" alt="image" src="https://github.com/user-attachments/assets/2f0ac6f9-5551-417a-8b08-940b0d608595" />

---

<img width="501" height="397" alt="image" src="https://github.com/user-attachments/assets/5d98f02b-43f0-4b8a-ab6f-4e3334f82753" />

---

- Default VPC: Created automatically in each region. Has a default subnet in each AZ, internet gateway attached, and default route table.
- Custom VPC: You define everything from scratch.
- AWS reserves 5 IPs per subnet: network address, VPC router, DNS, future use, broadcast.
- Public Subnet	Has route to Internet Gateway; instances can get public IPs
- Private Subnet	No direct internet route; uses NAT for outbound
- Isolated Subnet	No internet access at all
- Subnets are AZ-specific (one AZ per subnet, multiple subnets per AZ).
- Enable auto-assign public IPv4 at subnet level for public subnets.
- Subnet CIDR must be a subset of the VPC CIDR and cannot overlap with other subnets.

---

✅ Internet Gateway (IGW)
- Allows communication between VPC and the internet.
- Horizontally scaled, redundant, highly available — no bandwidth bottleneck.
- Only one IGW per VPC.
- Must add a route in the route table: 0.0.0.0/0 → IGW.
- Performs NAT for instances with public IPs.

---

✅ Route Tables
- Every subnet must be associated with a route table.
- Main route table: Default for all subnets not explicitly associated.
- Custom route tables: For more granular routing.
- Local route (10.0.0.0/16 → local) is always present and cannot be deleted.
- Longest prefix match determines routing priority.

---

✅ Security Groups (SGs)
- Stateful firewall — return traffic is automatically allowed.
- Operates at the instance/ENI level.
- Only allow rules (no explicit deny).
- Default SG: allows all outbound, blocks all inbound (except from same SG).
- SGs can reference other SGs (e.g., allow traffic from another SG).
- Up to 5 SGs per ENI.

---

✅ Network ACLs (NACLs)
- Stateless firewall — return traffic must be explicitly allowed.
- Operates at the subnet level.
- Supports both allow and deny rules.
- Rules evaluated in ascending order (lowest number first); first match wins.
- Default NACL: allows all inbound and outbound traffic.
- Custom NACL: denies all by default.
- Ephemeral ports (1024–65535) must be allowed for return traffic.

<img width="540" height="167" alt="image" src="https://github.com/user-attachments/assets/89e0100a-49d7-4a35-a4e3-e4e58bfc8b25" />

---

✅ VPC Peering
- Connect two VPCs privately (same or different accounts/regions).
- Traffic uses AWS backbone, not internet.
- No transitive peering — A↔B, B↔C does not mean A↔C.
- CIDR blocks must not overlap.
- Route tables must be updated in both VPCs.
- Supports inter-region VPC peering.

✅ VPC Endpoints
- Allow private connectivity to AWS services without internet, NAT, or VPN.

---

✅ Interface Endpoint (AWS PrivateLink)
- Creates an ENI with a private IP in your subnet.
- Supports most AWS services (S3, DynamoDB, etc.) and third-party services.
- DNS resolution directs traffic to the ENI.
- Costs money (per hour + per GB).
- Secured with Security Groups.
  
---

✅ Gateway Endpoint
- Only for S3 and DynamoDB.
- Adds a route to the route table.
- Free.
- Does not use DNS — uses route table entries.

---

✅ AWS Transit Gateway (TGW)
- Central hub to connect multiple VPCs, VPNs, and Direct Connect.
- Solves the n² peering problem — no need for full mesh.
- Supports transitive routing.
- Route tables per TGW control which attachments can communicate.
- Supports multicast.
- Cross-region peering available.
- Charged per attachment and per GB.

---

✅ VPN Connections

Site-to-Site VPN
- Connects on-premises network to VPC over the internet (encrypted with IPsec).
- Requires a Virtual Private Gateway (VGW) on AWS side and a Customer Gateway (CGW) on-prem.
- Two tunnels per VPN for redundancy.
- Bandwidth: up to 1.25 Gbps per tunnel.
- Supports static or BGP dynamic routing.
  

✅ AWS Direct Connect (DX)
- Dedicated private network connection from on-premises to AWS.
- Not encrypted by default — use IPsec over DX for encryption.
- Speeds: 1 Gbps, 10 Gbps, 100 Gbps (hosted: 50 Mbps–10 Gbps).
- Lower latency, consistent throughput vs VPN.
- Failover: combine DX (primary) + Site-to-Site VPN (backup).
  
Direct Connect Gateway: connect one DX to multiple VPCs in different regions.

---

✅ Elastic Network Interface (ENI)
- Virtual network card attached to EC2 instances.
- Has primary private IP, one or more secondary private IPs, public IP, EIP, MAC address, and SGs.
- ENIs can be moved between instances in the same AZ (useful for failover).
- eth0 = primary ENI; additional ENIs = eth1, eth2, etc.

---

✅ Elastic IP (EIP)
- Static public IPv4 address allocated to your account.
- Can be associated with an instance or ENI.
- Charged when not associated with a running instance.
- Used for predictable IPs (e.g., whitelisting).

---

✅ VPC Flow Logs
- Capture IP traffic information for VPCs, subnets, or ENIs.
- Stored in CloudWatch Logs or S3.
- Does not capture: DNS traffic, instance metadata, DHCP, Windows license activation.
- Log format includes: version, account-id, interface-id, src/dst IP, src/dst port, protocol, packets, bytes, action (ACCEPT/REJECT).
- Used for security analysis, troubleshooting, and compliance.

---

✅ VPC Sharing (RAM)
- Share subnets with other AWS accounts within the same AWS Organization using AWS Resource Access Manager (RAM).
- Owner VPC account manages the VPC; participant accounts deploy resources into shared subnets.
- Reduces number of VPCs needed; centralized networking.
- Transit Gtaway can also be shared
  
---

✅ DHCP Option Set
- used to give out IPs from ip pool

---

✅ PrivateLink
- can be used when consumer VPC wants to just access another serv
- Expose your service privately to other VPCs without VPC peering.
- Uses Interface Endpoints on consumer side and NLB on provider side.
- Traffic never leaves AWS network , so no traversing internet
- Supports across accounts and regions.

---

- NACLs are stateless — always open ephemeral ports (1024–65535).
- No transitive peering — use TGW for hub-and-spoke.
- Gateway Endpoint is free; Interface Endpoint costs money.
- NAT Gateway is AZ-specific — deploy one per AZ for HA.
- Default VPC has IGW; custom VPC does not.
- Direct Connect is not encrypted by default.
- EIP is charged when unattached to a running instance.
- SGs are stateful — no need to open return traffic.
- VPC Peering requires non-overlapping CIDRs.
- Flow Logs don't capture DNS queries within the VPC.

