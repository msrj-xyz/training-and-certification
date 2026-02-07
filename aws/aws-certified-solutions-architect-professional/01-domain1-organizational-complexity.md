# Domain 1: Design Solutions for Organizational Complexity (26%)

## Task 1.1: Architect Network Connectivity Strategies

### AWS Global Infrastructure

| Component | Description |
|-----------|-------------|
| **Regions** | Geographic locations with multiple AZs |
| **Availability Zones** | Isolated data centers within a region |
| **Edge Locations** | CDN and DNS endpoints |
| **Local Zones** | Low-latency compute in metro areas |
| **Wavelength Zones** | Ultra-low latency for 5G |
| **Outposts** | AWS infrastructure on-premises |

---

### VPC Connectivity Options

| Option | Use Case | Bandwidth |
|--------|----------|-----------|
| **VPC Peering** | 1:1 VPC connection | No limit |
| **Transit Gateway** | Hub-spoke multi-VPC | 50 Gbps |
| **PrivateLink** | Private service access | No limit |
| **VPC Endpoints** | AWS service access | No limit |

#### Transit Gateway ⭐
| Feature | Description |
|---------|-------------|
| **Hub-spoke** | Connect multiple VPCs centrally |
| **Cross-region** | Peering between TGWs |
| **Route Tables** | Segment traffic |
| **Multicast** | Supported |
| **RAM Sharing** | Share across accounts |

> **Exam Tip:** Transit Gateway is the go-to for multi-VPC, multi-account connectivity!

---

### Hybrid Connectivity

| Option | Use Case | Latency | Bandwidth |
|--------|----------|---------|-----------|
| **Site-to-Site VPN** | Quick, encrypted | Variable | 1.25 Gbps/tunnel |
| **Direct Connect** | Consistent, dedicated | Low | 1-100 Gbps |
| **DX + VPN** | Encrypted DX | Low | 1.25 Gbps |
| **Cloud WAN** | Global network | Variable | Variable |

#### Direct Connect ⭐
| Component | Description |
|-----------|-------------|
| **Dedicated Connection** | 1, 10, 100 Gbps |
| **Hosted Connection** | 50 Mbps - 10 Gbps |
| **Virtual Interface (VIF)** | Private, Public, Transit |
| **LAG** | Link Aggregation Group |
| **Direct Connect Gateway** | Multi-region access |

```
Private VIF → VPC (via VGW or TGW)
Public VIF → AWS public services
Transit VIF → Transit Gateway
```

> **Exam Tip:** DX provides consistent latency; VPN is faster to deploy but variable latency!

---

### DNS and Route 53

| Routing Policy | Use Case |
|----------------|----------|
| **Simple** | Single resource |
| **Weighted** | A/B testing, gradual migration |
| **Latency** | Lowest latency routing |
| **Failover** | Active-passive DR |
| **Geolocation** | Location-based content |
| **Geoproximity** | Shift traffic with bias |
| **Multivalue** | Multiple healthy endpoints |

#### Route 53 Resolver ⭐
| Feature | Description |
|---------|-------------|
| **Inbound Endpoint** | On-prem → AWS DNS |
| **Outbound Endpoint** | AWS → On-prem DNS |
| **Resolver Rules** | Forward queries |
| **RAM Sharing** | Share rules across accounts |

---

### Network Monitoring

| Tool | Purpose |
|------|---------|
| **VPC Flow Logs** | IP traffic logging |
| **Traffic Mirroring** | Packet-level capture |
| **Network Access Analyzer** | Verify connectivity |
| **Reachability Analyzer** | Path analysis |
| **CloudWatch Metrics** | Network performance |

---

## Task 1.2: Prescribe Security Controls

### IAM and Identity

| Service | Purpose |
|---------|---------|
| **IAM** | Users, roles, policies |
| **IAM Identity Center** | SSO, centralized access |
| **AWS Directory Service** | AD integration |
| **Cognito** | App user authentication |

#### Cross-Account Access ⭐
| Method | Use Case |
|--------|----------|
| **IAM Roles** | Assume role across accounts |
| **Resource-based Policies** | Direct resource sharing |
| **AWS RAM** | Share resources (TGW, subnets) |
| **Organizations** | Centralized management |

---

### Encryption and Key Management

| Service | Purpose |
|---------|---------|
| **AWS KMS** | Key management |
| **CloudHSM** | Hardware security modules |
| **ACM** | SSL/TLS certificates |
| **Secrets Manager** | Rotate secrets |
| **Parameter Store** | Config and secrets |

#### KMS Key Types ⭐
| Type | Management | Use Case |
|------|------------|----------|
| **AWS Managed** | AWS | Default encryption |
| **Customer Managed** | Customer | Custom policies |
| **Custom Key Store** | CloudHSM-backed | Regulatory compliance |

---

### Security Tools

| Tool | Purpose |
|------|---------|
| **CloudTrail** | API audit logging |
| **Config** | Configuration compliance |
| **Security Hub** | Aggregated security view |
| **GuardDuty** | Threat detection |
| **Inspector** | Vulnerability scanning |
| **Macie** | S3 data discovery |
| **IAM Access Analyzer** | External access analysis |

---

## Task 1.3: Design Reliable and Resilient Architectures

### RTO and RPO

| Metric | Definition |
|--------|------------|
| **RTO** | Maximum acceptable downtime |
| **RPO** | Maximum acceptable data loss |

---

### Disaster Recovery Strategies ⭐

| Strategy | RTO | RPO | Cost |
|----------|-----|-----|------|
| **Backup & Restore** | Hours | Hours | $ |
| **Pilot Light** | Minutes-Hours | Minutes | $$ |
| **Warm Standby** | Minutes | Seconds | $$$ |
| **Multi-site Active/Active** | Near-zero | Near-zero | $$$$ |

#### DR Strategy Details
| Strategy | Description |
|----------|-------------|
| **Backup & Restore** | Data backup only, rebuild on failure |
| **Pilot Light** | Core components running, scale on failure |
| **Warm Standby** | Scaled-down full environment |
| **Multi-site** | Full active-active across regions |

> **Exam Tip:** Match DR strategy to RTO/RPO requirements and budget!

---

### AWS Elastic Disaster Recovery (DRS)

| Feature | Description |
|---------|-------------|
| **Continuous Replication** | Block-level replication |
| **Low RPO** | Seconds of data loss |
| **Low RTO** | Minutes to recover |
| **Cross-region** | Replicate to DR region |
| **Non-disruptive Testing** | Test without impact |

---

### Backup Solutions

| Service | Use Case |
|---------|----------|
| **AWS Backup** | Centralized backup management |
| **S3 Replication** | Cross-region data copy |
| **RDS Snapshots** | Database point-in-time |
| **EBS Snapshots** | Volume backup |
| **DynamoDB PITR** | Table recovery |

---

## Task 1.4: Design Multi-Account AWS Environment

### AWS Organizations ⭐

| Component | Description |
|-----------|-------------|
| **Root** | Top-level container |
| **OUs** | Organizational Units for grouping |
| **Accounts** | Individual AWS accounts |
| **SCPs** | Service Control Policies |
| **Consolidated Billing** | Single payer account |

#### SCP Use Cases
| SCP Type | Purpose |
|----------|---------|
| **Deny List** | Block specific actions |
| **Allow List** | Permit only specific actions |
| **Region Restriction** | Limit to specific regions |
| **Service Restriction** | Block certain services |

---

### AWS Control Tower ⭐

| Feature | Description |
|---------|-------------|
| **Landing Zone** | Multi-account setup |
| **Guardrails** | Preventive and detective rules |
| **Account Factory** | Automated account provisioning |
| **Dashboard** | Compliance visibility |

#### Guardrail Types
| Type | Behavior |
|------|----------|
| **Preventive** | Block non-compliant actions (SCPs) |
| **Detective** | Alert on non-compliance (Config) |
| **Proactive** | Check before provisioning |

---

### Resource Sharing

| Service | Shareable Resources |
|---------|---------------------|
| **AWS RAM** | Subnets, TGW, Route 53 Resolver, License Manager |
| **Service Catalog** | Portfolios |
| **Systems Manager** | Documents, patches |

---

### Centralized Logging ⭐

| Pattern | Description |
|---------|-------------|
| **Log Archive Account** | Dedicated account for all logs |
| **S3 Bucket** | Central log storage |
| **CloudTrail Organization Trail** | All accounts logging |
| **CloudWatch Cross-Account** | Centralized dashboards |

```
All Accounts → Organization Trail → Log Archive S3 → Analysis
```

---

## Task 1.5: Determine Cost Optimization and Visibility

### Cost Management Tools

| Tool | Purpose |
|------|---------|
| **Cost Explorer** | Visualize spending |
| **AWS Budgets** | Set spending alerts |
| **Cost & Usage Report** | Detailed billing data |
| **Pricing Calculator** | Estimate costs |
| **Trusted Advisor** | Cost optimization checks |

---

### Purchasing Options ⭐

| Option | Discount | Commitment | Use Case |
|--------|----------|------------|----------|
| **On-Demand** | 0% | None | Variable workloads |
| **Reserved Instances** | Up to 72% | 1-3 years | Steady-state |
| **Savings Plans** | Up to 72% | 1-3 years | Flexible compute |
| **Spot Instances** | Up to 90% | None | Fault-tolerant |
| **Dedicated Hosts** | Varies | Hourly/Reserved | Licensing |

---

### Right-sizing Tools

| Tool | Purpose |
|------|---------|
| **Compute Optimizer** | EC2, Lambda, EBS recommendations |
| **S3 Storage Lens** | Storage analytics |
| **Cost Explorer** | Right-sizing recommendations |
| **Trusted Advisor** | Idle resource detection |

---

### Tagging Strategy ⭐

| Tag Category | Examples |
|--------------|----------|
| **Cost Allocation** | CostCenter, Project, Team |
| **Environment** | Environment: prod/dev/test |
| **Owner** | Owner, Contact |
| **Automation** | AutoStart, AutoStop |

> **Exam Tip:** Implement mandatory tagging via SCPs for cost allocation!

---

## Exam Tips for Domain 1

1. **Transit Gateway** = hub-spoke for multi-VPC, multi-account
2. **Direct Connect** = consistent latency, dedicated bandwidth
3. **VPN** = quick deployment, encrypted, variable latency
4. **Route 53 Resolver** = hybrid DNS integration
5. **Organizations + SCPs** = guardrails across accounts
6. **Control Tower** = automated landing zone setup
7. **DR strategies** = match RTO/RPO to cost
8. **AWS Backup** = centralized backup management
9. **Cost allocation tags** = mandatory via SCPs
10. **Savings Plans** = more flexible than Reserved Instances
