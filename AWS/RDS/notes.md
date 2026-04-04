✅ why aurora over normal RDS 

---
✅ How will u setup connetion to allow someone form on prem to login to aws rds & run querry running in private subnet ?

---
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

| Feature        | RDS Proxy                  | Read Replica             |
| -------------- | -------------------------- | ------------------------ |
| Purpose        | Connection management      | Read scaling             |
| Handles        | Database connections       | Database queries         |
| Replication    | No replication             | Asynchronous replication |
| Improves       | Connection efficiency      | Read performance         |
| Used by        | Applications/Lambda        | Read-heavy workloads     |
| Writes allowed | Yes (to primary DB)        | No (read-only)           |
| Failover       | Faster connection failover | Manual promotion         |


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

---

✅ Database is slow during peak hours. What will you check?
- Check CloudWatch metrics (CPUUtilization, ReadIOPS, WriteIOPS, DBConnections).
- Enable Performance Insights to identify slow queries.
- Check slow query logs.
- Increase instance class or storage IOPS if needed.
- Add read replicas to offload read traffic.

✅ Application requires high availability for database. What architecture will you use?
- Use RDS Multi-AZ deployment.

How it works:
- Primary DB in AZ-1
- Standby DB in AZ-2
- Synchronous replication
- Automatic failover
- If primary fails → standby becomes primary automatically.
- Good for HA but not read scaling.

✅ Your application has heavy read traffic. How will you scale the database?
- Use Read Replicas.
- Writes still go to primary DB.
- asynchronous replication


✅ Database storage is almost full in production. What will you do?
- Modify RDS instance. --> Increase allocated storage.
- Enable Storage Autoscaling.
- RDS supports online storage scaling, so downtime is minimal.

✅ Your RDS database is accidentally deleted. How will you recover it?
- Restore from:
- Automated backups
- Manual snapshots
- Point-in-time recovery (PITR)

✅ Developer accidentally ran DELETE query without WHERE clause. How will you recover data?
- Use Point-in-Time Recovery.
- Restore DB to timestamp before query execution
- Extract required data
- Insert back into production DB.

✅ Your RDS instance suddenly shows high CPU utilization. What will you investigate?
- Check Performance Insights
- Identify long-running queries
- Check DB connections spike
- Review recent deployments
- Tune queries or add indexes.

✅ Application servers cannot connect to RDS. What will you check?

Answer:

Check:

Security Group

Port 3306/5432 open?

VPC configuration

Same VPC or peering?

Subnet group

Public/Private access

Network ACL

✅ How will you migrate an on-premise database to AWS RDS with minimal downtime?
- Use AWS Database Migration Service (DMS).

✅ Your RDS instance is running out of connections. What will you do?
- Enable RDS Proxy
- Increase max_connections

✅ Developers want a copy of production DB for testing. What will you do?
- Create a DB Snapshot and restore it to a new RDS instance.

Benefits:
- Safe copy of production
- No impact to production DB.

<img width="612" height="267" alt="image" src="https://github.com/user-attachments/assets/cd2271ac-1eab-4529-b013-34a95ac6944e" />

✅ Production DB should not be publicly accessible. How will you secure it?
- Deploy RDS in private subnet
- Disable Public Access
- Use Security Groups
- Connect via application servers / bastion host


✅ What is the difference between Multi-AZ and Read Replica?
| Feature      | Multi-AZ          | Read Replica |
| ------------ | ----------------- | ------------ |
| Purpose      | High Availability | Read Scaling |
| Replication  | Synchronous       | Asynchronous |
| Failover     | Automatic         | Manual       |
| Read traffic | No                | Yes          |


✅ Your RDS instance suddenly fails. How does failover work in Multi-AZ?
- RDS promotes the standby instance automatically.
- DNS endpoint is updated to point to standby.
- Failover usually takes 60–120 seconds.

✅ How does RDS storage autoscaling work?
- RDS automatically increases storage when:
- Free storage < 10%
- Storage increase threshold reached
- Within maximum storage limit
- Prevents database outages due to disk full.

✅ What is RDS Proxy and when would you use it?
- RDS Proxy manages database connections.

Benefits:
- Connection pooling
- Improves scalability
- Handles connection storms
- Faster failover
- Used in serverless architectures like Lambda.

✅ What is the difference between automated backups and snapshots?
| Feature                | Automated Backup | Snapshot  |
| ---------------------- | ---------------- | --------- |
| Created automatically  | Yes              | No        |
| Retention              | 1-35 days        | Unlimited |
| Point-in-time recovery | Yes              | No        |
| Storage cost           | Included         | Charged   |


✅ How does RDS encryption work?
- Encryption uses AWS KMS.

Two types:
- Encryption at rest
- Encryption in transit (SSL/TLS)
- Once enabled → cannot disable encryption.
- Encryption must be enabled during DB creation.

✅ Can you enable encryption on an existing RDS instance?
- No directly.

Steps:
- Take snapshot
- Copy snapshot with encryption
- Restore encrypted DB.

✅ How does RDS handle patching and maintenance?
- RDS performs maintenance during maintenance window:

✅ What is Performance Insights?
- A monitoring tool that helps:
- Identify slow queries
- Detect database bottlenecks
- View top SQL queries
- Analyze CPU usage by query.

✅ How can you reduce failover time in RDS?
- Use Aurora instead of standard RDS
- Reduce long-running transactions
- Use RDS Proxy
- Optimize DNS caching

✅ What is the maximum number of read replicas supported?
- Depends on engine:
- MySQL → 5 replicas
- PostgreSQL → 5 replicas
- Aurora → 15 replicas

✅ What happens if the primary RDS instance fails in a Read Replica setup?
- Read replicas do not failover automatically.
- You must promote a replica manually.

✅ How can you monitor RDS performance?
- CloudWatch Metrics
- Enhanced Monitoring
- Performance Insights
- Database logs.

✅ What are the different types of RDS storage?
| Storage Type                  | Use Case         |
| ----------------------------- | ---------------- |
| General Purpose SSD (gp2/gp3) | Most workloads   |
| Provisioned IOPS (io1/io2)    | High performance |
| Magnetic                      | Legacy workloads |

---

✅ What is Enhanced Monitoring?
- Provides OS-level metrics like --> CPU --> Memory --> Disk usage --> Processes
- Granularity up to 1 second.

✅ How do you achieve cross-region disaster recovery for RDS?
- Cross-region read replicas
- Snapshot copy to another region
- Use Aurora Global Database
- For dynamo Db use Global Tables

✅ What is Aurora Global Database?
- global database architecture that allows:
- Replication across multiple regions
- Failover within seconds
- Low latency global reads.

✅ Your application needs database scaling without downtime. What will you do?
- Use Aurora auto scaling
- Add read replicas
- Upgrade instance class.

✅ What are RDS parameter groups?
- Parameter groups allow you to configure database engine settings.

Examples:
- max_connections
- query_cache_size
- log settings

✅ What are option groups in RDS?
- Option groups enable additional database features.

Example:
- Oracle TDE
- SQL Server replication
- Native backup/restore.

✅ How can you migrate a large database to AWS?
- Use AWS Database Migration Service (DMS).

---

| Feature         | RDS        | Aurora          |
| --------------- | ---------- | --------------- |
| Performance     | Standard   | Up to 5x faster |
| Replicas        | 5          | 15              |
| Storage scaling | Manual     | Auto            |
| Failover        | 60-120 sec | <30 sec         |

---

✅ Production database crashed because disk became full.
- Enable RDS Storage Autoscaling.
- Prevents storage exhaustion
- No downtime
- AWS RDS supports Storage Autoscaling, which automatically increases storage when free space drops below 10%.
- AWS RDS can automatically scale storage, but only if Storage Autoscaling is enabled
- Requires setting maximum storage threshold
---

✅ Does RDS automatically shrink storage when usage drops?
- No. RDS only scales up, not down.

<img width="577" height="302" alt="image" src="https://github.com/user-attachments/assets/5e0976d5-c529-4e9d-839d-1b91e6f0086b" />

<img width="665" height="196" alt="image" src="https://github.com/user-attachments/assets/a99868f6-1c53-4f83-bff3-d5b7b3ba57b1" />


<img width="674" height="373" alt="image" src="https://github.com/user-attachments/assets/a1da0cbe-be9b-4a58-b46f-21ed8899729f" />


<img width="631" height="167" alt="image" src="https://github.com/user-attachments/assets/bb858566-b59a-434d-886d-e811f1ac5fc1" />


<img width="581" height="57" alt="image" src="https://github.com/user-attachments/assets/19f2d2d2-0d87-450d-82cc-b464d227d2c0" />

What is the difference between Snapshot Restore and Point-in-Time Recovery in RDS
- Snapshot Restore restores a database from a specific snapshot backup, while Point-in-Time Recovery restores the database to any exact timestamp using automated backups and transaction logs.
- For PITR to work automated snapshots are mandatory
- PITR never overwrites the existing DB. It creates a new database instance.

---

Explain RDS Event streaming & how will u achieve it & use cases where will u use it 
