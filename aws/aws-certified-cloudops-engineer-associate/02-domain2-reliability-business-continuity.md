# Domain 2: Reliability and Business Continuity (22%)

## Task 2.1: Implement Scalability and Elasticity

### Auto Scaling Components

| Component | Description |
|-----------|-------------|
| **Launch Template** | EC2 instance configuration |
| **Auto Scaling Group (ASG)** | Collection of EC2 instances |
| **Scaling Policy** | Rules for scaling actions |
| **Desired Capacity** | Target number of instances |
| **Min/Max Size** | Boundaries for scaling |

---

### Scaling Policy Types

| Policy Type | Description | Use Case |
|-------------|-------------|----------|
| **Target Tracking** | Maintain target metric value | CPU at 50% |
| **Step Scaling** | Scale based on alarm breach severity | Tiered response |
| **Simple Scaling** | Single adjustment per alarm | Basic scaling |
| **Scheduled** | Time-based scaling | Known patterns |
| **Predictive** | ML-based forecasting | Recurring patterns |

#### Target Tracking Metrics
- ASGAverageCPUUtilization
- ASGAverageNetworkIn/Out
- ALBRequestCountPerTarget
- Custom CloudWatch metrics

---

### Cooldown Periods

| Type | Description | Default |
|------|-------------|---------|
| **Default Cooldown** | ASG-level cooldown | 300 seconds |
| **Scaling Cooldown** | Policy-level cooldown | Overrides default |
| **Warm-up Time** | Time for instance to contribute | Target tracking |

> **Exam Tip:** Warm-up period prevents scaling from new instance metrics too early!

---

### Instance Lifecycle

| State | Description |
|-------|-------------|
| **Pending** | Instance launching |
| **Pending:Wait** | Lifecycle hook pause |
| **InService** | Instance healthy and serving |
| **Terminating** | Instance shutting down |
| **Terminating:Wait** | Lifecycle hook pause |
| **Terminated** | Instance terminated |

#### Lifecycle Hooks
- **Launch hooks** - Run actions before instance enters service
- **Terminate hooks** - Run actions before instance terminates
- **Use cases** - Install software, backup data, deregister

---

### Database Scaling

#### RDS Scaling Options
| Type | Method | Description |
|------|--------|-------------|
| **Vertical** | Modify instance class | Larger instance type |
| **Horizontal** | Read replicas | Offload read traffic |
| **Storage** | Storage auto scaling | Automatic disk expansion |

#### DynamoDB Scaling
| Mode | Description | Use Case |
|------|-------------|----------|
| **On-demand** | Automatic scaling | Unpredictable traffic |
| **Provisioned** | Set RCU/WCU | Predictable traffic |
| **Auto Scaling** | Adjust provisioned capacity | Variable traffic |

---

### Caching for Scalability

| Service | Use Case | Protocol |
|---------|----------|----------|
| **CloudFront** | Static/dynamic content | HTTP/HTTPS |
| **ElastiCache Redis** | Session store, real-time analytics | Redis |
| **ElastiCache Memcached** | Simple caching | Memcached |
| **DAX** | DynamoDB acceleration | DynamoDB |

#### CloudFront Cache Behaviors
- Path patterns
- Origin selection
- Cache policies
- TTL settings

---

## Task 2.2: Implement Highly Available Environments

### High Availability Concepts

| Concept | Description |
|---------|-------------|
| **Multi-AZ** | Distribute across Availability Zones |
| **Multi-Region** | Distribute across Regions |
| **Active-Active** | All sites serve traffic |
| **Active-Passive** | Standby site for failover |
| **Fault Tolerance** | Continue operation despite failures |

---

### Elastic Load Balancing (ELB)

| Type | Layer | Use Case |
|------|-------|----------|
| **Application Load Balancer (ALB)** | Layer 7 | HTTP/HTTPS, path-based routing |
| **Network Load Balancer (NLB)** | Layer 4 | TCP/UDP, ultra-low latency |
| **Gateway Load Balancer (GWLB)** | Layer 3 | Third-party virtual appliances |
| **Classic Load Balancer (CLB)** | Layer 4/7 | Legacy (avoid for new) |

#### ALB Features
- Host-based routing
- Path-based routing
- Query string routing
- HTTP header routing
- Source IP routing
- Lambda targets
- Weighted target groups

#### NLB Features
- Static IP per AZ
- Elastic IP support
- Preserve source IP
- Ultra-low latency
- TCP/UDP/TLS protocols

---

### Health Checks

| Type | Description |
|------|-------------|
| **ELB Health Check** | Load balancer checks targets |
| **EC2 Health Check** | Instance status checks |
| **Route 53 Health Check** | Endpoint availability |
| **Auto Scaling Health Check** | EC2 or ELB based |

#### Health Check Configuration
| Parameter | Description |
|-----------|-------------|
| **Protocol** | HTTP, HTTPS, TCP |
| **Path** | Health check URL |
| **Interval** | Time between checks |
| **Timeout** | Wait time for response |
| **Healthy Threshold** | Consecutive successes |
| **Unhealthy Threshold** | Consecutive failures |

---

### Amazon Route 53 Routing Policies

| Policy | Description | Use Case |
|--------|-------------|----------|
| **Simple** | Single resource | Basic routing |
| **Failover** | Active-passive failover | Disaster recovery |
| **Geolocation** | Route by user location | Regional content |
| **Geoproximity** | Route by resource proximity | Traffic shifting |
| **Latency** | Route to lowest latency | Performance |
| **Multivalue** | Multiple healthy resources | Simple load distribution |
| **Weighted** | Distribute by weight | Blue/green deployments |

---

### Multi-AZ Deployments

| Service | Multi-AZ Support |
|---------|------------------|
| **RDS** | Synchronous standby replica |
| **Aurora** | Automatic across 3 AZs |
| **ElastiCache** | Multi-AZ with auto-failover |
| **EFS** | Automatic across multiple AZs |
| **S3** | Automatic across 3+ AZs |
| **DynamoDB** | Automatic across 3 AZs |

---

## Task 2.3: Implement Backup and Restore Strategies

### AWS Backup

| Feature | Description |
|---------|-------------|
| **Backup Plans** | Scheduled backup policies |
| **Backup Vault** | Storage for backup data |
| **Backup Rules** | Schedule, retention, lifecycle |
| **Cross-region** | Copy backups to other regions |
| **Cross-account** | Share backups across accounts |

#### Supported Services
- Amazon EBS
- Amazon EC2
- Amazon RDS
- Amazon Aurora
- Amazon DynamoDB
- Amazon EFS
- Amazon FSx
- Amazon S3
- AWS Storage Gateway

---

### Recovery Objectives

| Objective | Definition | Measurement |
|-----------|------------|-------------|
| **RTO (Recovery Time Objective)** | Maximum acceptable downtime | Time to recover |
| **RPO (Recovery Point Objective)** | Maximum data loss tolerance | Time since last backup |

---

### Disaster Recovery Strategies

| Strategy | RTO/RPO | Cost | Description |
|----------|---------|------|-------------|
| **Backup & Restore** | Hours | Lowest | Restore from backups |
| **Pilot Light** | 10s of minutes | Low | Minimal always-on core |
| **Warm Standby** | Minutes | Medium | Scaled-down environment |
| **Multi-site Active-Active** | Real-time | Highest | Full environment running |

---

### RDS Backup and Restore

| Feature | Description |
|---------|-------------|
| **Automated Backups** | Daily snapshots + transaction logs |
| **Manual Snapshots** | User-initiated, persist after deletion |
| **Point-in-Time Recovery** | Restore to any second in retention |
| **Cross-Region Snapshot Copy** | DR capability |
| **Snapshot Sharing** | Share with other accounts |

#### RDS Restore Options
| Method | Result |
|--------|--------|
| **Restore Snapshot** | New DB instance |
| **Point-in-Time** | New DB instance to specific time |
| **Copy Snapshot** | Copy to another region/account |

> **Exam Tip:** RDS restore creates a NEW instance, not in-place!

---

### S3 Data Protection

| Feature | Description |
|---------|-------------|
| **Versioning** | Keep multiple versions |
| **MFA Delete** | Require MFA for deletion |
| **Object Lock** | WORM (Write Once Read Many) |
| **Replication** | Cross-region or same-region |
| **Lifecycle Rules** | Transition and expiration |

#### S3 Object Lock Modes
| Mode | Description |
|------|-------------|
| **Governance** | Users with permissions can override |
| **Compliance** | Nobody can delete, including root |

---

### EC2 Backup

| Method | Description |
|--------|-------------|
| **AMI** | Machine image (EBS + metadata) |
| **EBS Snapshot** | Point-in-time volume backup |
| **AWS Backup** | Centralized backup service |
| **Data Lifecycle Manager** | Automated snapshot management |

---

### DynamoDB Backup

| Method | Description |
|--------|-------------|
| **On-demand Backup** | Full backup, no performance impact |
| **PITR** | Continuous backup, 35-day window |
| **AWS Backup** | Centralized management |

---

### FSx Backup

| Type | Backup Method |
|------|---------------|
| **FSx for Windows** | Automatic daily backups |
| **FSx for Lustre** | Manual backups to S3 |
| **FSx for ONTAP** | Snapshots and backups |
| **FSx for OpenZFS** | Automatic snapshots |

---

## Exam Tips for Domain 2

1. **Target Tracking** - Preferred scaling policy for most use cases
2. **Warm-up Period** - Critical for preventing premature scaling
3. **ALB vs NLB** - ALB for HTTP, NLB for TCP/UDP and static IP
4. **Route 53 Failover** - Requires health checks
5. **RTO vs RPO** - Know the difference and strategies
6. **Multi-AZ RDS** - Synchronous replication, automatic failover
7. **AWS Backup** - Centralized backup for multiple services
8. **S3 Versioning** - Enable before replication
