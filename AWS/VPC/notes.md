
### ✅ Scenario 1: Basic VPC Design
You need to design a 3-tier architecture (web, app, DB) for a production workload. How would you structure your VPC?


- Create a VPC with CIDR e.g. 10.0.0.0/16
- Public Subnets (e.g. 10.0.1.0/24, 10.0.2.0/24) — for ALB, NAT Gateway, Bastion Host across 2 AZs
- Private App Subnets (e.g. 10.0.3.0/24, 10.0.4.0/24) — for EC2/ECS app tier, no direct internet
- Private DB Subnets (e.g. 10.0.5.0/24, 10.0.6.0/24) — for RDS, ElastiCache, strictly isolated
- Attach an Internet Gateway to the VPC for public subnet access
- Deploy NAT Gateway in each public subnet (one per AZ for HA) for private subnet outbound traffic
- Use Security Groups as stateful firewalls per tier + NACLs for subnet-level stateless control


Scenario 2: VPC Peering
Q: Two teams have separate VPCs (VPC-A: 10.0.0.0/16, VPC-B: 10.1.0.0/16) and need to communicate privately. How do you set this up?
A:

Create a VPC Peering Connection between VPC-A and VPC-B
Accept the peering request from the other side
Update Route Tables in both VPCs:

VPC-A route table: destination 10.1.0.0/16 → target: peering connection ID
VPC-B route table: destination 10.0.0.0/16 → target: peering connection ID


Update Security Groups to allow traffic from the peer VPC CIDR
Key limitations to mention:

No transitive peering — if VPC-A peers B and B peers C, A cannot reach C via B
No overlapping CIDRs allowed
For many VPCs, recommend Transit Gateway instead




Scenario 3: Overlapping CIDRs
Q: You need to connect two VPCs but both use 10.0.0.0/16. VPC peering won't work. What are your options?
A:

Option 1: AWS PrivateLink — expose a specific service from one VPC as an endpoint service, consumer VPC connects via Interface VPC Endpoint. No CIDR dependency.
Option 2: NAT + re-addressing — use NAT to translate overlapping IPs, though this is complex
Option 3: Re-CIDR one VPC — if early in lifecycle, change one VPC's CIDR (requires planning, downtime)
Option 4: Transit Gateway with NAT appliance — use a third-party NAT between TGW attachments
Best answer in interviews: PrivateLink is the AWS-recommended solution for overlapping CIDRs


Scenario 4: On-Premises Connectivity
Q: Your company wants to connect its on-premises data center to AWS VPC securely. What options do you have?
A:

Option 1: AWS Site-to-Site VPN

IPsec VPN tunnel over public internet
Create a Virtual Private Gateway (attach to VPC) and Customer Gateway (your on-prem router)
Fast to set up, encrypted, but limited bandwidth (~1.25 Gbps) and latency variability


Option 2: AWS Direct Connect

Dedicated private network connection (1G, 10G, 100G)
Lower latency, consistent bandwidth, no internet traversal
Use Direct Connect Gateway to connect to multiple VPCs across regions


Option 3: VPN over Direct Connect — combine both for encrypted + consistent connectivity
For HA: use two VPN tunnels (AWS provides two by default) or redundant DX connections


Scenario 5: Transit Gateway
Q: You have 10 VPCs across multiple accounts and all need to communicate with each other and on-premises. How do you architect this?
A:

Use AWS Transit Gateway (TGW) as a central hub
Attach all 10 VPCs + VPN/Direct Connect to the TGW
TGW acts as a regional router — eliminates the need for N×(N-1)/2 peering connections
Use TGW Route Tables to control which VPCs can talk to each other (segmentation)

e.g., Prod VPCs in one route table, Dev VPCs in another, no cross-table routing


Share TGW across accounts using AWS Resource Access Manager (RAM)
For multi-region: use TGW Peering between regional TGWs
Cost consideration: TGW charges per attachment + per GB data processed


Scenario 6: Security Group vs NACL
Q: You blocked port 443 in a NACL but the Security Group allows it. Users still can't access the app. Why, and when would you use each?
A:

NACLs are stateless and operate at the subnet level — they evaluate both inbound AND outbound rules independently
Security Groups are stateful — if inbound is allowed, the return traffic is automatically allowed
Since NACL blocked 443 inbound, traffic never reaches the instance regardless of SG rules — NACLs are evaluated first at subnet boundary
Fix: Allow port 443 inbound in NACL, and also allow ephemeral ports (1024–65535) outbound in NACL for return traffic
When to use each:

SG: instance-level, stateful, allow-only rules, primary defense layer
NACL: subnet-level, stateless, explicit allow+deny, secondary layer, good for blocking specific IPs quickly




Scenario 7: Private EC2 Internet Access
Q: EC2 instances in a private subnet need to download patches from the internet but must not be directly reachable from the internet. How?
A:

Deploy a NAT Gateway in the public subnet
Public subnet must have a route to Internet Gateway
In the private subnet route table, add: destination 0.0.0.0/0 → target: NAT Gateway ID
EC2 in private subnet can now initiate outbound connections; inbound connections from internet are blocked
For cost optimization: use a NAT Instance (EC2-based) if traffic is low, but NAT Gateway is managed and scales automatically
For S3/DynamoDB access: use VPC Gateway Endpoints to avoid NAT costs entirely


Scenario 8: VPC Endpoints
Q: Your Lambda in a private VPC needs to call S3 and SSM Parameter Store. Traffic should not leave the AWS network. How?
A:

For S3 and DynamoDB → use Gateway VPC Endpoint (free, route table-based)

Add endpoint to VPC, update route table: destination pl-xxxxxxxx (S3 prefix list) → endpoint


For SSM, SSM Messages, EC2 Messages → use Interface VPC Endpoint (PrivateLink-based, charges apply)

Creates an ENI in your subnet with a private IP
Lambda resolves ssm.us-east-1.amazonaws.com to the private ENI IP


Enable Private DNS on interface endpoints so existing SDK calls work without code changes
Ensure Security Group on the endpoint ENI allows port 443 from Lambda's SG


Scenario 9: VPC Flow Logs Troubleshooting
Q: An app can't connect to an RDS instance. Both are in the same VPC. How do you troubleshoot using VPC Flow Logs?
A:

Enable VPC Flow Logs on the VPC or specific subnet/ENI → send to CloudWatch Logs or S3
Flow log fields to check: srcaddr, dstaddr, dstport, action (ACCEPT/REJECT), protocol
Look for traffic from app's IP to RDS IP on port 3306 (MySQL) or 5432 (Postgres)
If you see REJECT → Security Group or NACL is blocking — check:

RDS Security Group: does it allow inbound 3306 from app's Security Group?
NACL: are inbound 3306 and outbound ephemeral ports open?


If you see no flow log entry → traffic never left the source — check app-level config (wrong endpoint, DNS resolution failure)
Use CloudWatch Logs Insights query to filter: filter action="REJECT" and dstPort=3306


Scenario 10: Cross-Region Connectivity
Q: An app in us-east-1 VPC needs low-latency, private access to a database in eu-west-1 VPC. How do you achieve this?
A:

Use VPC Peering across regions (Inter-Region VPC Peering)

Traffic travels over AWS backbone network, not public internet
Encrypted in transit
Update route tables in both regions to route to peer VPC CIDR


Alternative: Transit Gateway Inter-Region Peering

Better when multiple VPCs are involved in each region
Peer the two regional TGWs, propagate routes


No support for: IPv6 inter-region peering, referencing SG from peer region (use CIDR instead)
For read-heavy DB workloads: consider RDS Cross-Region Read Replica instead of routing writes cross-region


Scenario 11: Bastion Host / Session Manager
Q: Developers need SSH access to private EC2 instances. You don't want to open port 22 to the internet. What do you recommend?
A:

Option 1: Bastion Host (Jump Box)

EC2 in public subnet with port 22 open only to known office IPs (SG: source = corporate IP/32)
Developers SSH to bastion first, then SSH to private instances
Limitation: key management overhead, single point of failure


Option 2: AWS Systems Manager Session Manager (recommended)

No port 22 needed, no public IP, no bastion
EC2 needs SSM Agent + IAM role with AmazonSSMManagedInstanceCore policy
Private instance needs Interface VPC Endpoint for SSM (if no internet access)
Full audit trail in CloudTrail + S3, session logging to CloudWatch
Access controlled via IAM policies, integrates with MFA




Scenario 12: IP Address Planning
Q: You're planning a VPC for a large enterprise that will eventually have 50+ subnets and connect to on-premises. How do you plan CIDRs?
A:

Choose a large, non-overlapping CIDR from RFC1918 that doesn't conflict with on-premises (e.g., 10.10.0.0/16 gives 65,536 IPs)
Plan subnet sizing by tier and AZ:

Public subnets: /24 (251 usable — AWS reserves 5 IPs per subnet)
App subnets: /22 for room to grow
DB subnets: /24


Use secondary CIDR blocks if primary runs out (up to 5 CIDRs per VPC)
Reserve CIDR ranges for future VPCs to avoid peering/TGW overlap issues
Coordinate with network team to ensure on-prem ranges don't overlap
Document in an IPAM tool — or use AWS VPC IP Address Manager (IPAM) to centrally track and allocate CIDRs across accounts/regions


Key concepts to always mention: Stateful vs Stateless, Transitive routing limitations, PrivateLink for overlapping CIDRs, TGW for hub-and-spoke, NAT Gateway for outbound, VPC Endpoints to keep traffic private, and Flow Logs for troubleshooting.
