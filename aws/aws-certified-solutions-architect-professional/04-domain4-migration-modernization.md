# Domain 4: Accelerate Workload Migration and Modernization (20%)

## Task 4.1: Select Workloads for Migration

### Migration Assessment Tools

| Tool | Purpose |
|------|---------|
| **Migration Hub** | Central migration tracking |
| **Application Discovery Service** | Discover on-premises apps |
| **Migration Evaluator** | Business case analysis |
| **AWS Pricing Calculator** | Cost estimation |

#### Application Discovery Methods
| Method | Description |
|--------|-------------|
| **Agentless** | VMware vCenter integration |
| **Agent-based** | Detailed server data |
| **Import** | Manual data import |

---

### The 7Rs Migration Strategies ⭐

| Strategy | Description | Effort | Use Case |
|----------|-------------|--------|----------|
| **Retire** | Decommission | Low | Unused applications |
| **Retain** | Keep as-is | None | Not ready to migrate |
| **Rehost** | Lift-and-shift | Low | Quick migration |
| **Relocate** | VMware Cloud on AWS | Low | VMware workloads |
| **Repurchase** | Move to SaaS | Medium | Replace with SaaS |
| **Replatform** | Lift-tinker-shift | Medium | Minor optimizations |
| **Refactor** | Re-architect | High | Modernize fully |

> **Exam Tip:** Rehost is fastest; Refactor provides most benefits but highest effort!

---

### Wave Planning

| Phase | Activities |
|-------|------------|
| **Wave 0** | Test migration, validate tools |
| **Wave 1** | Simple, non-critical apps |
| **Wave 2-N** | Progressively complex apps |
| **Final Wave** | Critical, complex apps |

---

### TCO Analysis

| Factor | Consideration |
|--------|---------------|
| **Compute** | EC2 vs on-premises servers |
| **Storage** | S3, EBS vs SAN/NAS |
| **Network** | Data transfer, connectivity |
| **Operations** | Staff, maintenance |
| **Licensing** | BYOL vs included |

---

## Task 4.2: Determine Optimal Migration Approach

### Data Migration Tools ⭐

| Tool | Use Case | Speed |
|------|----------|-------|
| **S3 Transfer Acceleration** | Large files over internet | Fast |
| **DataSync** | NFS, SMB, S3, EFS sync | Fast |
| **Transfer Family** | SFTP, FTPS, FTP to S3 | Medium |
| **Snow Family** | Petabyte-scale offline | Slow-Medium |
| **Storage Gateway** | Hybrid storage | Continuous |

#### Snow Family Comparison
| Device | Capacity | Use Case |
|--------|----------|----------|
| **Snowcone** | 8-14 TB | Edge, small transfers |
| **Snowball Edge** | 80-210 TB | Large migrations, edge compute |
| **Snowmobile** | 100 PB | Massive data center moves |

---

### Application Migration Tools

| Tool | Purpose |
|------|---------|
| **Application Migration Service (MGN)** | Lift-and-shift to EC2 |
| **VM Import/Export** | Import VM images |
| **CloudEndure** | Continuous replication |

#### MGN Features ⭐
| Feature | Description |
|---------|-------------|
| **Continuous Replication** | Block-level sync |
| **Non-disruptive Testing** | Test without impact |
| **Cutover** | Final migration |
| **OS Support** | Windows, Linux |

---

### Database Migration ⭐

| Tool | Use Case |
|------|----------|
| **AWS DMS** | Database migration/replication |
| **AWS SCT** | Schema conversion |
| **RDS Snapshot** | Same-engine migration |
| **Native Tools** | mysqldump, pg_dump |

#### DMS Migration Types
| Type | Description |
|------|-------------|
| **Full Load** | One-time migration |
| **CDC** | Continuous replication |
| **Full Load + CDC** | Initial + ongoing |

#### Schema Conversion Tool (SCT)
| Conversion | Capability |
|------------|------------|
| **Homogeneous** | Same engine (Oracle → Oracle) |
| **Heterogeneous** | Different engine (Oracle → PostgreSQL) |
| **Assessment** | Migration complexity report |

---

### Network Connectivity for Migration

| Option | Use Case |
|--------|----------|
| **VPN** | Quick, encrypted, variable throttle |
| **Direct Connect** | Consistent, large data volumes |
| **Transfer Acceleration** | Internet-based S3 uploads |
| **Snowball** | Offline, massive data |

---

### Identity Migration

| Service | Purpose |
|---------|---------|
| **IAM Identity Center** | SSO, centralized access |
| **AD Connector** | Proxy to on-premises AD |
| **AWS Managed AD** | Managed Active Directory |
| **Simple AD** | Basic directory service |

---

## Task 4.3: Determine New Architecture

### Compute Options ⭐

| Service | Use Case |
|---------|----------|
| **EC2** | Full control, lift-and-shift |
| **ECS** | Docker containers |
| **EKS** | Kubernetes |
| **Fargate** | Serverless containers |
| **Lambda** | Event-driven, serverless |
| **App Runner** | Container web apps |
| **Elastic Beanstalk** | PaaS deployment |

#### Container Decision Tree
```
Need Kubernetes? → EKS
Need Docker only? → ECS
Want serverless? → Fargate
Simple web app? → App Runner
```

---

### Storage Selection

| Requirement | Service |
|-------------|---------|
| **Object Storage** | S3 |
| **Block Storage** | EBS |
| **File Storage (Linux)** | EFS |
| **File Storage (Windows)** | FSx for Windows |
| **High-Performance** | FSx for Lustre |
| **Hybrid** | Storage Gateway |

---

### Database Selection ⭐

| Requirement | Service |
|-------------|---------|
| **Relational, managed** | RDS, Aurora |
| **Relational, high-perf** | Aurora |
| **NoSQL, key-value** | DynamoDB |
| **In-memory cache** | ElastiCache |
| **Document DB** | DocumentDB |
| **Graph DB** | Neptune |
| **Time series** | Timestream |
| **Ledger** | QLDB |
| **Data warehouse** | Redshift |
| **Search** | OpenSearch |

---

## Task 4.4: Determine Modernization Opportunities

### Serverless Modernization ⭐

| Traditional | Serverless |
|-------------|------------|
| **EC2** | Lambda, Fargate |
| **ELB + EC2** | API Gateway + Lambda |
| **RDS** | Aurora Serverless, DynamoDB |
| **Self-managed queue** | SQS |
| **Self-managed pub/sub** | SNS |
| **Cron jobs** | EventBridge Scheduler |

---

### Container Modernization

| From | To |
|------|-----|
| **Monolith on EC2** | Microservices on ECS/EKS |
| **VMs** | Containers on Fargate |
| **Manual deployment** | CI/CD with CodePipeline |
| **Single server** | Auto Scaling, Multi-AZ |

---

### Application Integration ⭐

| Pattern | Service |
|---------|---------|
| **Async messaging** | SQS |
| **Fan-out** | SNS |
| **Event-driven** | EventBridge |
| **Workflows** | Step Functions |
| **API** | API Gateway |
| **GraphQL** | AppSync |

#### Step Functions Use Cases
| Pattern | Description |
|---------|-------------|
| **Orchestration** | Coordinate Lambda functions |
| **Human Approval** | Wait for manual steps |
| **Error Handling** | Retry, catch, fallback |
| **Parallel** | Run tasks concurrently |

---

### Purpose-Built Databases

| Migrate From | Migrate To |
|--------------|------------|
| **Self-managed MySQL** | Aurora MySQL |
| **Self-managed PostgreSQL** | Aurora PostgreSQL |
| **Oracle** | Aurora (with SCT) |
| **SQL Server** | RDS SQL Server, Aurora |
| **MongoDB** | DocumentDB |
| **Cassandra** | Keyspaces |
| **Redis** | ElastiCache Redis |

---

### Decoupling Strategies

| Strategy | Implementation |
|----------|----------------|
| **Message Queue** | SQS between services |
| **Event Bus** | EventBridge for events |
| **API Gateway** | Decouple frontend/backend |
| **Service Mesh** | App Mesh for microservices |

> **Exam Tip:** SQS decouples services; EventBridge enables event-driven architecture!

---

## Exam Tips for Domain 4

1. **7Rs** = know when to use each strategy
2. **MGN (Application Migration Service)** = lift-and-shift tool
3. **DMS + SCT** = database migration, schema conversion
4. **Snow Family** = offline migration for petabytes
5. **DataSync** = online file migration
6. **Aurora Global Database** = cross-region DR for RDS
7. **Fargate** = serverless containers
8. **Step Functions** = orchestrate Lambda workflows
9. **SQS** = decouple services asynchronously
10. **EventBridge** = event-driven architecture
