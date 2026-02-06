# Domain 3: Resilient Cloud Solutions (15%)

## Task 3.1: High Availability and Fault Tolerance

### Multi-AZ Architecture Patterns

| Pattern | Description | Use Case |
|---------|-------------|----------|
| **Active-Active** | All AZs serve traffic | Max availability |
| **Active-Passive** | Standby in second AZ | Failover |
| **Multi-Region** | Distribute globally | DR, performance |

---

### AWS High Availability Services

| Service | HA Feature |
|---------|------------|
| **RDS** | Multi-AZ (synchronous standby) |
| **Aurora** | Multi-AZ by design (6 copies) |
| **ElastiCache** | Multi-AZ with replication |
| **DynamoDB** | Multi-AZ, global tables |
| **EFS** | Multi-AZ automatically |
| **S3** | 11 9s durability, cross-region |
| **ELB** | Multi-AZ automatically |

---

### Auto Scaling ⭐

| Type | Description |
|------|-------------|
| **Target Tracking** | Maintain target metric |
| **Step Scaling** | CloudWatch alarm-based |
| **Simple Scaling** | Basic alarm-based |
| **Scheduled Scaling** | Time-based |
| **Predictive Scaling** | ML-based forecasting |

#### Auto Scaling Key Concepts
| Concept | Description |
|---------|-------------|
| **Launch Template** | Instance configuration |
| **Desired Capacity** | Target instance count |
| **Min/Max Size** | Scaling boundaries |
| **Cooldown** | Wait between scaling |
| **Warm-Up** | Time to metrics accuracy |
| **Health Checks** | EC2 or ELB health |
| **Lifecycle Hooks** | Custom actions |

> **Exam Tip:** Lifecycle hooks = run scripts before instance launch/terminate!

---

### Elastic Load Balancing ⭐

| Type | Layer | Use Case |
|------|-------|----------|
| **ALB** | Layer 7 | HTTP/HTTPS, path routing |
| **NLB** | Layer 4 | TCP/UDP, high performance |
| **GWLB** | Layer 3 | Security appliances |
| **CLB** | Layer 4/7 | Legacy (deprecated) |

#### Health Checks
| Check Type | Description |
|------------|-------------|
| **HTTP/HTTPS** | Path-based health |
| **TCP** | Port connectivity |
| **Custom** | Application-specific |

---

## Task 3.2: Disaster Recovery

### DR Strategies

| Strategy | RTO | RPO | Cost |
|----------|-----|-----|------|
| **Backup & Restore** | Hours | Hours | $ |
| **Pilot Light** | Minutes-Hours | Minutes | $$ |
| **Warm Standby** | Minutes | Seconds-Minutes | $$$ |
| **Multi-Site Active** | Near-zero | Near-zero | $$$$ |

> **Exam Tip:** Know RTO/RPO trade-offs for each DR strategy!

---

### AWS Elastic Disaster Recovery

| Feature | Description |
|---------|-------------|
| **Continuous Replication** | Block-level replication |
| **Recovery Instances** | On-demand recovery |
| **Drill Testing** | Non-disruptive testing |
| **Automated Failback** | Return to primary |

---

### Backup Strategies

| Service | Backup Method |
|---------|---------------|
| **RDS** | Automated backups, snapshots |
| **Aurora** | Continuous backup to S3 |
| **DynamoDB** | Point-in-time recovery |
| **EBS** | Snapshots, DLM policies |
| **EFS** | AWS Backup |
| **S3** | Versioning, replication |

---

### AWS Backup

| Feature | Description |
|---------|-------------|
| **Backup Plans** | Centralized policies |
| **Backup Vaults** | Secure storage |
| **Cross-account** | Organization backups |
| **Cross-region** | DR replication |
| **Vault Lock** | WORM compliance |

---

## Task 3.3: Container Resilience

### ECS High Availability ⭐

| Feature | Description |
|---------|-------------|
| **Multi-AZ** | Tasks across AZs |
| **Service Scheduler** | Maintain desired count |
| **Task Placement** | Spread, binpack, random |
| **Circuit Breaker** | Deployment rollback |
| **Service Auto Scaling** | Target tracking |

---

### EKS High Availability ⭐

| Feature | Description |
|---------|-------------|
| **Multi-AZ Control Plane** | AWS managed |
| **Node Groups** | Multi-AZ worker nodes |
| **Pod Disruption Budgets** | Update control |
| **Horizontal Pod Autoscaler** | Pod scaling |
| **Cluster Autoscaler** | Node scaling |
| **Karpenter** | Efficient node provisioning |

---

## Task 3.4: Serverless Resilience

### Lambda Resilience

| Feature | Description |
|---------|-------------|
| **Multi-AZ** | Automatic |
| **Provisioned Concurrency** | Cold start elimination |
| **Reserved Concurrency** | Limit function usage |
| **Dead Letter Queues** | Failed invocations |
| **Retry Behavior** | Async retries |

---

### Step Functions Patterns

| Pattern | Description |
|---------|-------------|
| **Retry** | Automatic retries with backoff |
| **Catch** | Error handling |
| **Fallback** | Alternative path on failure |
| **Parallel** | Concurrent execution |
| **Map** | Dynamic parallelism |

---

## Task 3.5: Chaos Engineering

### AWS Fault Injection Simulator (FIS)

| Feature | Description |
|---------|-------------|
| **Experiments** | Controlled failure injection |
| **Actions** | EC2, ECS, EKS, RDS failures |
| **Targets** | Resource selection |
| **Stop Conditions** | Safety guardrails |
| **Integrations** | CloudWatch, EventBridge |

#### FIS Experiment Types
- CPU stress
- Memory stress
- Network latency/packet loss
- Instance termination
- API throttling
- AZ failure simulation

---

## Exam Tips for Domain 3

1. **DR strategy selection:**
   - Backup & Restore = lowest cost, highest RTO
   - Pilot Light = core systems running
   - Warm Standby = scaled-down production
   - Multi-Site = highest cost, lowest RTO
2. **Auto Scaling essentials:**
   - Target Tracking = most common
   - Lifecycle Hooks = custom actions
   - Cooldown = prevent thrashing
3. **Multi-AZ patterns:**
   - RDS Multi-AZ = synchronous standby
   - Aurora = 6 copies across 3 AZs
   - DynamoDB = automatic
4. **ELB selection:**
   - ALB = HTTP routing, containers
   - NLB = TCP, high performance
   - GWLB = security appliances
5. **Container resilience:**
   - ECS Circuit Breaker = auto-rollback
   - EKS Pod Disruption Budgets
   - Multi-AZ node placement
6. **AWS Backup:**
   - Central backup management
   - Cross-account, cross-region
   - Vault Lock = WORM
7. **Lambda resilience:**
   - Provisioned Concurrency = cold start
   - Dead Letter Queues = failed events
   - Retry with exponential backoff
8. **FIS usage:**
   - Validate recovery procedures
   - Stop conditions = safety
   - Test in non-production first
9. **RTO vs RPO:**
   - RTO = recovery time objective
   - RPO = recovery point objective
   - Lower values = higher cost
10. **Elastic Disaster Recovery:**
    - Continuous replication
    - On-demand recovery instances
    - Non-disruptive drills
