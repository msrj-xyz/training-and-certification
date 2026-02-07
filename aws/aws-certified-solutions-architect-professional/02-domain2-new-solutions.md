# Domain 2: Design for New Solutions (29%)

## Task 2.1: Design Deployment Strategy

### Infrastructure as Code (IaC)

| Tool | Description | Use Case |
|------|-------------|----------|
| **CloudFormation** | AWS native IaC | Standard deployments |
| **AWS CDK** | Programmatic IaC | Python, TypeScript, Java |
| **Terraform** | Multi-cloud IaC | Hybrid environments |
| **AWS SAM** | Serverless IaC | Lambda applications |

#### CloudFormation Features ⭐
| Feature | Description |
|---------|-------------|
| **StackSets** | Deploy across accounts/regions |
| **Nested Stacks** | Modular templates |
| **Drift Detection** | Configuration changes |
| **Change Sets** | Preview changes |
| **Rollback Triggers** | Auto-rollback on failure |

---

### CI/CD Services

| Service | Role |
|---------|------|
| **CodeCommit** | Git repository |
| **CodeBuild** | Build and test |
| **CodeDeploy** | Deployment automation |
| **CodePipeline** | Pipeline orchestration |
| **CodeArtifact** | Package management |

---

### Deployment Strategies ⭐

| Strategy | Description | Risk | Rollback |
|----------|-------------|------|----------|
| **All-at-once** | Deploy to all at once | High | Manual |
| **Rolling** | Incremental batches | Medium | Manual |
| **Rolling with batch** | Keep capacity during roll | Medium | Manual |
| **Blue/Green** | Parallel environments | Low | Instant |
| **Canary** | Small % first | Low | Quick |
| **Linear** | Gradual % increase | Low | Quick |

> **Exam Tip:** Blue/Green provides instant rollback; Canary catches issues early!

---

### Systems Manager

| Feature | Purpose |
|---------|---------|
| **Run Command** | Remote command execution |
| **Patch Manager** | Automated patching |
| **State Manager** | Configuration enforcement |
| **Session Manager** | Secure shell access |
| **Parameter Store** | Configuration storage |
| **Automation** | Runbooks for workflows |

---

## Task 2.2: Design Solutions for Business Continuity

### Business Continuity Components

| Component | Purpose |
|-----------|---------|
| **Multi-AZ** | High availability within region |
| **Multi-Region** | Disaster recovery |
| **Auto Scaling** | Automatic capacity |
| **Load Balancing** | Traffic distribution |
| **Health Checks** | Failure detection |

---

### Database High Availability ⭐

| Database | HA Strategy | DR Strategy |
|----------|-------------|-------------|
| **RDS Multi-AZ** | Synchronous standby | Cross-region read replicas |
| **Aurora** | Multi-AZ, 6-way replication | Global Database |
| **DynamoDB** | Multi-AZ by default | Global Tables |
| **ElastiCache** | Multi-AZ, replicas | Cross-region replication |

#### Aurora Global Database
| Feature | Description |
|---------|-------------|
| **RPO** | < 1 second |
| **RTO** | < 1 minute |
| **Read Replicas** | Up to 16 per region |
| **Write Forwarding** | Write from secondary |
| **Regions** | Up to 5 secondary regions |

---

### S3 Replication

| Type | Use Case |
|------|----------|
| **Same-Region Replication (SRR)** | Compliance, log aggregation |
| **Cross-Region Replication (CRR)** | DR, latency reduction |
| **S3 Replication Time Control (RTC)** | 15-minute SLA |

---

### Data Replication Patterns

| Pattern | Description | RPO |
|---------|-------------|-----|
| **Synchronous** | Real-time, consistent | Zero |
| **Asynchronous** | Near real-time | Seconds-minutes |
| **Backup/Restore** | Periodic snapshots | Hours |

---

## Task 2.3: Determine Security Controls

### Defense in Depth ⭐

| Layer | Controls |
|-------|----------|
| **Edge** | CloudFront, Shield, WAF |
| **VPC** | Security Groups, NACLs, VPC Flow Logs |
| **Subnet** | Public/Private separation |
| **Instance** | Security Groups, host firewall |
| **Application** | Authentication, authorization |
| **Data** | Encryption at rest and in transit |

---

### Web Application Protection

| Service | Purpose |
|---------|---------|
| **AWS WAF** | Web application firewall |
| **AWS Shield** | DDoS protection |
| **CloudFront** | Edge caching, SSL termination |
| **API Gateway** | API management |

#### WAF Rules ⭐
| Rule Type | Protection |
|-----------|------------|
| **Rate-based** | DDoS, brute force |
| **Managed Rules** | OWASP Top 10, bots |
| **IP Set** | Block/allow IPs |
| **Regex** | Custom patterns |
| **Geo Match** | Country-based |

---

### Encryption Strategies

| Data State | Solution |
|------------|----------|
| **At Rest** | KMS, CloudHSM, S3 SSE |
| **In Transit** | TLS/SSL, ACM certificates |
| **In Use** | Nitro Enclaves |

---

### Patch Management

| Tool | Capability |
|------|------------|
| **SSM Patch Manager** | Automated patching |
| **Maintenance Windows** | Scheduled operations |
| **Patch Baselines** | Patch rules |
| **Compliance Reports** | Patch status |

---

## Task 2.4: Design Strategy for Reliability

### High Availability Patterns ⭐

| Pattern | Description |
|---------|-------------|
| **Active-Active** | All instances serve traffic |
| **Active-Passive** | Standby on failure |
| **N+1** | Extra capacity for failures |
| **Pilot Light** | Minimal standby |

---

### Load Balancing

| Type | Layer | Use Case |
|------|-------|----------|
| **ALB** | Layer 7 | HTTP/HTTPS, path routing |
| **NLB** | Layer 4 | TCP/UDP, ultra-low latency |
| **GWLB** | Layer 3 | Network appliances |
| **CLB** | Layer 4/7 | Legacy (deprecated) |

#### ALB Features
- Path-based routing
- Host-based routing
- Weighted target groups
- Lambda targets
- OIDC authentication

---

### Auto Scaling ⭐

| Policy Type | Trigger |
|-------------|---------|
| **Target Tracking** | Maintain metric target |
| **Step Scaling** | CloudWatch alarm thresholds |
| **Scheduled** | Time-based |
| **Predictive** | ML-based forecasting |

---

### Loose Coupling

| Pattern | Service |
|---------|---------|
| **Message Queue** | SQS |
| **Pub/Sub** | SNS |
| **Event Bus** | EventBridge |
| **Workflows** | Step Functions |

> **Exam Tip:** Loose coupling improves reliability and scalability!

---

### Route 53 Health Checks

| Type | Monitors |
|------|----------|
| **Endpoint** | HTTP, HTTPS, TCP |
| **Calculated** | Multiple health checks |
| **CloudWatch Alarm** | CloudWatch metric |

---

## Task 2.5: Design Solutions for Performance

### Caching Strategies ⭐

| Layer | Solution | Use Case |
|-------|----------|----------|
| **Edge** | CloudFront | Static content, global |
| **Application** | ElastiCache | Session, database cache |
| **Database** | DAX | DynamoDB acceleration |
| **API** | API Gateway | Response caching |

---

### Storage Performance

| Storage | Performance Characteristics |
|---------|----------------------------|
| **S3** | High throughput, unlimited scale |
| **EBS gp3** | Baseline 3000 IOPS, configurable |
| **EBS io2** | Up to 64,000 IOPS |
| **EFS** | Elastic throughput |
| **FSx Lustre** | High-performance HPC |
| **Instance Store** | Lowest latency, ephemeral |

---

### Compute Optimization

| Strategy | Description |
|----------|-------------|
| **Instance Types** | Match to workload |
| **Placement Groups** | Cluster, Spread, Partition |
| **Spot Instances** | Cost-effective, interruptible |
| **Graviton** | ARM-based, better price/performance |

#### Placement Groups
| Type | Use Case |
|------|----------|
| **Cluster** | Low latency, HPC |
| **Spread** | Critical instances, HA |
| **Partition** | Large distributed systems |

---

### Purpose-Built Databases

| Database | Use Case |
|----------|----------|
| **Aurora** | High-performance relational |
| **DynamoDB** | Key-value, low latency |
| **ElastiCache** | In-memory, microseconds |
| **Neptune** | Graph relationships |
| **Timestream** | Time series |
| **QLDB** | Immutable ledger |
| **DocumentDB** | MongoDB compatible |
| **Keyspaces** | Cassandra compatible |

---

## Task 2.6: Determine Cost Optimization Strategy

### Cost Optimization Principles

| Principle | Implementation |
|-----------|----------------|
| **Right-sizing** | Match instance to workload |
| **Reserved Capacity** | Commit for discounts |
| **Spot Instances** | Fault-tolerant workloads |
| **Storage Tiering** | Lifecycle policies |
| **Data Transfer** | Minimize cross-region |

---

### Storage Tiering ⭐

| S3 Class | Access Pattern | Cost |
|----------|---------------|------|
| **Standard** | Frequent | $$$ |
| **Intelligent-Tiering** | Unknown | $$ |
| **Standard-IA** | Infrequent | $$ |
| **One Zone-IA** | Infrequent, single AZ | $ |
| **Glacier Instant** | Rare, instant access | $ |
| **Glacier Flexible** | Rare, hours retrieval | ¢ |
| **Glacier Deep Archive** | Archive, 12h retrieval | ¢ |

---

### Data Transfer Costs ⭐

| Transfer Type | Cost |
|---------------|------|
| **Inbound** | Free |
| **Same AZ** | Free |
| **Cross-AZ** | $ |
| **Same Region (public IP)** | $ |
| **Cross-Region** | $$ |
| **Internet Outbound** | $$$ |

> **Exam Tip:** Use VPC Endpoints and PrivateLink to reduce data transfer costs!

---

## Exam Tips for Domain 2

1. **CloudFormation StackSets** = multi-account, multi-region deployments
2. **Blue/Green** = instant rollback, zero downtime
3. **Aurora Global Database** = sub-second RPO, cross-region DR
4. **DynamoDB Global Tables** = active-active multi-region
5. **WAF + Shield** = DDoS and application protection
6. **ALB** = Layer 7, path/host routing
7. **NLB** = Layer 4, ultra-low latency
8. **Auto Scaling Predictive** = ML-based, proactive scaling
9. **ElastiCache** = reduce database load
10. **S3 Intelligent-Tiering** = automatic cost optimization
