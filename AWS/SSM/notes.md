✅ What is AWS SSM (Systems Manager) ?
- Centralized management service for EC2, on-prem servers, and VMs
- Helps with patching, automation, configuration, and remote command execution
- Works via SSM Agent

---

✅ SSM Agent
- Installed on instances

<img width="621" height="154" alt="image" src="https://github.com/user-attachments/assets/7c48cd7a-139d-4fdd-a0a2-a232bdc656d1" />

<img width="530" height="248" alt="image" src="https://github.com/user-attachments/assets/5a54a55a-de1e-4c7b-9b52-f324f4765300" />

<img width="646" height="263" alt="image" src="https://github.com/user-attachments/assets/31fd923d-f5c8-468d-8341-bad5fb2eaeb4" />

<img width="622" height="178" alt="image" src="https://github.com/user-attachments/assets/8af402cf-a321-4136-8499-feafc59cb352" />

<img width="596" height="284" alt="image" src="https://github.com/user-attachments/assets/12487f2b-11e0-4406-a4d0-e2b2097fe960" />

<img width="590" height="113" alt="image" src="https://github.com/user-attachments/assets/8e6e7a0c-ee48-4fae-80c3-e08da662f3c7" />

---

✅ SSM Run Command
- Execute commands on instances without SSH.

Example use cases:
- restart services
- install packages
- run scripts

✅ SSM Session Manager
- Secure shell access without SSH or bastion host.

Benefits:
- No port 22 required
- No SSH keys
- All sessions logged

---

✅ SSM Patch Manager
- Automates OS patching.
- Supports --> Linux , Windows

Capabilities:
- scan patches
- install patches
- schedule patching

---

✅ SSM Automation
- Run automated workflows.

Example tasks:
- create AMI
- restart instance
- attach EBS volume
- security remediation
- Automation uses SSM documents.

✅ SSM Parameter Store
- Secure storage for configuration values 

✅ State Manager
- Maintains desired configuration state.

<img width="304" height="119" alt="image" src="https://github.com/user-attachments/assets/38218144-99f4-41c2-bc6b-d767924e297b" />

✅ Inventory
- Collect metadata from instances.

Example data:
- OS version
- installed packages
- applications
- network configuration

✅ Distributor
- Install software packages across instances.

Example:
- CloudWatch agent
- security tools
- custom applications

<img width="409" height="445" alt="image" src="https://github.com/user-attachments/assets/26c3a33f-f134-445c-9e53-3824799d1f4f" />

<img width="262" height="312" alt="image" src="https://github.com/user-attachments/assets/508c2f0e-d5f9-46b9-9bf2-05e82d0fda3d" />

<img width="578" height="200" alt="image" src="https://github.com/user-attachments/assets/3dfe577f-d3bb-4455-99d7-77335ac4c679" />

---

- You want to access EC2 in private subnet without SSH --> Use Session Manager.
- Need to run commands on 500 servers. --> Use Run Command.
- Ensure servers are patched monthly --> Use Patch Manager + maintenance window.
- Store database passwords securely --> Use AWS Secrets manager instead of SSM Paramter Store

---

<img width="478" height="295" alt="image" src="https://github.com/user-attachments/assets/6ab62fce-2380-4278-a103-28d5b0f30f01" />

---

<img width="563" height="167" alt="image" src="https://github.com/user-attachments/assets/76f432bc-d323-4e41-b88f-cc57342c2080" />

<img width="515" height="418" alt="image" src="https://github.com/user-attachments/assets/30f36028-bb1d-49ac-b65a-247d72e998c2" />
