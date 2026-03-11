What is AWS SSM (Systems Manager) ?
- Centralized management service for EC2, on-prem servers, and VMs
- Helps with patching, automation, configuration, and remote command execution
- Works via SSM Agent

---

Core Components
1. SSM Agent

Installed on instances

Communicates with SSM service

Runs commands, automation, patching

2. Managed Instance

Servers managed by SSM:

EC2 instances

On-prem servers

Other cloud VMs

Requirements:

SSM agent

IAM role

Network access to SSM endpoints

Important SSM Features
3. Run Command

Execute commands on instances without SSH.

Example use cases:

restart services

install packages

run scripts

Example command:

sudo yum update
4. Session Manager

Secure shell access without SSH or bastion host.

Benefits:

No port 22 required

No SSH keys

All sessions logged

Architecture

User → AWS Console → SSM → EC2 instance
5. Patch Manager

Automates OS patching.

Supports:

Linux

Windows

Capabilities:

scan patches

install patches

schedule patching

6. Automation

Run automated workflows.

Example tasks:

create AMI

restart instance

attach EBS volume

security remediation

Automation uses SSM documents.

7. Parameter Store

Secure storage for:

configuration values

secrets

API keys

Example:

/prod/db/password

Types:

Standard

Advanced

SecureString (encrypted)

Uses AWS Key Management Service

8. State Manager

Maintains desired configuration state.

Example:

Ensure nginx always installed.

Install nginx
Keep service running
9. Inventory

Collect metadata from instances.

Example data:

OS version

installed packages

applications

network configuration

10. Distributor

Install software packages across instances.

Example:

CloudWatch agent

security tools

custom applications

Networking Requirements

Instances must reach:

ssm.amazonaws.com
ec2messages.amazonaws.com
ssmmessages.amazonaws.com

If private subnet:

Use SSM VPC endpoints.

Required IAM Role

Typical role:

AmazonSSMManagedInstanceCore

This allows:

Run commands

Session manager

patching

automation

SSM vs SSH
Feature	SSH	SSM
Port needed	22	none
Key management	required	not required
Audit logging	limited	CloudTrail logs
Bastion host	required	not required
Common Interview Scenarios
Scenario 1

You want to access EC2 in private subnet without SSH.

Solution:

Use Session Manager.

Scenario 2

Need to run commands on 500 servers.

Solution:

Use Run Command.

Scenario 3

Ensure servers are patched monthly.

Solution:

Use Patch Manager + maintenance window.

Scenario 4

Store database passwords securely.

Solution:

Use Parameter Store SecureString.

Important Interview Tip

SSM helps achieve zero SSH architecture.

Architecture:

User
  ↓
SSM Session Manager
  ↓
EC2 private subnet

No bastion host required.

One-Line Memory Trick
Feature	Purpose
Run Command	execute commands remotely
Session Manager	SSH without SSH
Patch Manager	automate patching
Parameter Store	store configs & secrets
Automation	run workflows
State Manager	enforce configuration
Inventory	collect instance data

If you want, I can also give 15 advanced SSM interview questions (very commonly asked in DevOps interviews) like:

SSM vs Secrets Manager

SSM vs SSH bastion

How Session Manager works internally

How SSM works in private VPC without internet.
