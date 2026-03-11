✅ What is RDS?
- Amazon Relational Database Service — managed relational DB service 
- Supported Engine --> MySQL, PostgreSQL, MariaDB, Oracle, Microsoft SQL Server
- Amazon Aurora (AWS-native, MySQL/PostgreSQL compatible)

---

✅ Instances & Storage
- Instance types: db.t3, db.m5, db.r5 (memory-optimized for heavy workloads)
- Storage types: gp2/gp3 (general purpose SSD), io1/io2 (provisioned IOPS), magnetic (legacy)
- Storage autoscaling — can auto-expand when threshold is breached

✅ Multi-AZ
- Synchronous replication to standby in another AZ
- Standby is not readable — purely for failover
- Failover is automatic (~60–120s), DNS flips to standby
- Protects against AZ failure, instance failure, storage failure
- Not a scaling solution — only HA

✅ Read Replicas
- Asynchronous replication
- Up to 5 read replicas per DB instance (15 for Aurora)
- Can be in same AZ, different AZ, or different region (cross-region RR)
- Replicas can be promoted to standalone DB (breaks replication)
- Can have Multi-AZ enabled on a replica

<img width="572" height="194" alt="image" src="https://github.com/user-attachments/assets/9b4c97fd-98b7-4abc-90ba-2572775c720c" />

✅ Aurora
- 5x faster than MySQL, 3x faster than PostgreSQL (AWS claim)
- Storage auto-scales in 10GB increments up to 128TB
- 6 copies of data across 3 AZs (4/6 writes, 3/6 reads quorum)
- One primary + up to 15 Aurora Replicas (low replica lag ~10ms)
- Aurora Global Database — spans multiple regions, < 1s replication lag
- Aurora Serverless — auto-scales compute capacity; good for intermittent/unpredictable workloads
- Aurora Multi-Master — multiple write nodes (MySQL-compatible only)
- Backtrack — rewind DB to a point in time without restore (Aurora MySQL only)


✅ Automated Backups
- Enabled by default, retained 1–35 days (default 7)
- Daily full backup during backup window + transaction logs every 5 min
- Allows point-in-time recovery (PITR) to any second within retention period
- Stored in S3 (free storage up to DB size)
- Deleted when DB is deleted (unless you take a final snapshot)

✅ Manual Snapshots
- User-initiated, retained indefinitely until deleted
- Can be shared with other AWS accounts or copied across regions
- Restoring creates a new DB instance

✅ Aurora Backups
- Continuous backups to S3 (cannot be disabled)
- No performance impact during backup

✅ Encryption
- At rest: AES-256 via KMS; must be enabled at creation
- Encrypted instance → all snapshots, replicas, logs also encrypted
- To encrypt an unencrypted DB: snapshot → copy snapshot with encryption → restore
- In transit: SSL/TLS enforced via parameter groups (rds.force_ssl)

✅ Network Security
- Deploy in VPC — use private subnets
- Security Groups control inbound/outbound traffic
- DB Subnet Group — must span at least 2 AZs
- No public access by default (PubliclyAccessible = false)

✅ IAM Authentication
- Use IAM roles/users to authenticate instead of DB passwords
- Works with MySQL and PostgreSQL
- Token-based, valid for 15 minutes
- Enabled via --enable-iam-database-authentication

✅ Secrets Manager
- Rotate DB credentials automatically
- Integrates natively with RDS

✅ DB Parameter Groups 
- DB engine configuration (e.g., max_connections, innodb_buffer_pool_size)

2 Types
- Static params: require reboot
- Dynamic params: apply immediately

---

CloudWatch Metrics

CPUUtilization, DatabaseConnections, FreeStorageSpace, ReadIOPS, WriteIOPS, FreeableMemory

Enhanced Monitoring

OS-level metrics (processes, threads) at up to 1-second granularity
Sent to CloudWatch Logs

Performance Insights

Visualize DB load, identify top SQL queries/waits
Retention: 7 days free, up to 2 years (paid)

RDS Logs

Error log, slow query log, general log, audit log
Publishable to CloudWatch Logs

Events & Event Subscriptions

SNS notifications for DB events (failover, backup, parameter change)



✅ Vertical Scaling
- Change instance type — causes brief downtime (unless Multi-AZ, then minimal)
- Can schedule during maintenance window

✅ Horizontal Scaling
- Add read replicas for read-heavy workloads

✅ Storage Scaling
- Storage autoscaling — grows automatically, never shrinks
- Can't decrease allocated storage

✅ RDS Proxy
- Fully managed DB proxy sitting between app and RDS
- Pools and shares DB connections — reduces connection overhead
- Improves failover time by up to 66%
- Enforces IAM auth, integrates with Secrets Manager
- Supports MySQL, PostgreSQL, MariaDB, SQL Server, Aurora

<img width="362" height="213" alt="image" src="https://github.com/user-attachments/assets/93a0f83a-d141-473b-a352-5e23557e3a2f" />

<img width="289" height="227" alt="image" src="https://github.com/user-attachments/assets/d9882e3d-2472-4826-bb0e-3befd25a3e25" />

✅ Maintenance
- Maintenance window — weekly window for OS/DB patching
- Multi-AZ: standby patched first, then failover, then primary patched
- Auto minor version upgrades can be enabled


✅ Key Exam Traps

- Multi-AZ standby cannot serve reads — use Read Replicas for that
- Snapshots restore to a new DB instance, not in-place
- Encrypting existing DB requires snapshot → copy → restore workflow
- RDS Proxy is key for Lambda + RDS (Lambda creates many short-lived connections)
- Aurora replicas also serve as failover targets (unlike RDS read replicas which need promotion)
- Cross-region read replicas help with DR, not just read scaling
- DeleteAutomatedBackups=true by default when deleting — set to false or take final snapshot

