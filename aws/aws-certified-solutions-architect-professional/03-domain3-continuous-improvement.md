# Domain 3: Continuous Improvement for Existing Solutions (25%)

## Task 3.1: Improve Operational Excellence

### Monitoring and Logging ⭐

| Service | Purpose |
|---------|---------|
| **CloudWatch Metrics** | Performance monitoring |
| **CloudWatch Logs** | Centralized logging |
| **CloudWatch Alarms** | Threshold-based alerts |
| **CloudWatch Dashboards** | Custom visualizations |
| **CloudWatch Insights** | Log analytics |
| **CloudWatch Synthetics** | Canary monitoring |

#### CloudWatch Logs Destinations
| Destination | Use Case |
|-------------|----------|
| **S3** | Long-term archive |
| **Kinesis Data Streams** | Real-time processing |
| **Kinesis Firehose** | Load to S3, OpenSearch |
| **Lambda** | Custom processing |
| **OpenSearch** | Analysis and visualization |

---

### Alerting and Remediation

| Pattern | Implementation |
|---------|----------------|
| **Alerting** | CloudWatch Alarms → SNS |
| **Auto-remediation** | EventBridge → Lambda/SSM |
| **Incident Management** | Systems Manager Incident Manager |
| **On-call** | SNS → PagerDuty/OpsGenie |

#### EventBridge for Automation ⭐
```
CloudWatch Alarm → EventBridge Rule → Target (Lambda, SSM, SNS)
Config Rule Violation → EventBridge → SSM Automation
GuardDuty Finding → EventBridge → Lambda Remediation
```

---

### Configuration Management

| Service | Purpose |
|---------|---------|
| **SSM State Manager** | Desired state enforcement |
| **SSM Automation** | Runbooks, workflows |
| **AWS Config** | Configuration tracking |
| **CloudFormation Drift** | Detect manual changes |

---

### CI/CD Improvement

| Improvement | Implementation |
|-------------|----------------|
| **Faster Deployments** | Parallel stages, caching |
| **Safer Deployments** | Canary, Blue/Green |
| **Better Testing** | Automated tests in pipeline |
| **Security Scanning** | CodeGuru, SAST/DAST |

---

### Chaos Engineering

| Tool | Purpose |
|------|---------|
| **AWS FIS** | Fault Injection Simulator |
| **Game Days** | DR testing exercises |
| **Runbooks** | Documented recovery |

---

## Task 3.2: Improve Security

### Secrets Management ⭐

| Service | Use Case |
|---------|----------|
| **Secrets Manager** | Rotate database credentials |
| **Parameter Store** | Configuration, non-rotating secrets |
| **ACM** | SSL/TLS certificates |

#### Secrets Manager vs Parameter Store
| Feature | Secrets Manager | Parameter Store |
|---------|-----------------|-----------------|
| **Rotation** | Automatic | Manual/Lambda |
| **Cost** | Per secret | Free tier |
| **Size** | 64 KB | 8 KB (advanced: 64 KB) |
| **Cross-account** | Yes | Yes (advanced) |

---

### Automated Compliance

| Service | Purpose |
|---------|---------|
| **AWS Config Rules** | Compliance evaluation |
| **Config Remediations** | Auto-fix violations |
| **Security Hub** | Aggregated findings |
| **Conformance Packs** | Compliance frameworks |

#### Config Auto-Remediation
```
Non-compliant Resource → Config Rule → SSM Automation → Remediation
```

---

### Least Privilege Access

| Tool | Purpose |
|------|---------|
| **IAM Access Analyzer** | External access analysis |
| **IAM Policy Simulator** | Test policies |
| **CloudTrail** | API audit |
| **Access Advisor** | Last accessed services |

---

### Vulnerability Management

| Service | Scope |
|---------|-------|
| **Inspector** | EC2, Lambda, ECR |
| **GuardDuty** | Threat detection |
| **Detective** | Security investigation |
| **Macie** | S3 sensitive data |

---

### Patch Management Strategy

| Component | Implementation |
|-----------|----------------|
| **Patch Baselines** | Define approved patches |
| **Maintenance Windows** | Scheduled patching |
| **Patch Groups** | Group by environment |
| **Compliance Reports** | Track patch status |

---

### Backup Strategy

| Service | Backup Method |
|---------|---------------|
| **AWS Backup** | Centralized, cross-service |
| **S3 Versioning** | Object history |
| **EBS Snapshots** | Point-in-time copies |
| **RDS Snapshots** | Database backup |
| **DLM** | EBS lifecycle policies |

---

## Task 3.3: Improve Performance

### Performance Optimization ⭐

| Area | Optimization |
|------|--------------|
| **Compute** | Right-sizing, Graviton |
| **Storage** | Appropriate IOPS, caching |
| **Database** | Read replicas, caching |
| **Network** | CloudFront, Global Accelerator |
| **Application** | Code optimization |

---

### Global Services

| Service | Purpose |
|---------|---------|
| **CloudFront** | CDN, edge caching |
| **Global Accelerator** | Global static anycast IP |
| **Route 53** | DNS routing |
| **Lambda@Edge** | Edge compute |
| **CloudFront Functions** | Lightweight edge compute |

#### CloudFront vs Global Accelerator
| Feature | CloudFront | Global Accelerator |
|---------|------------|-------------------|
| **Content** | Cacheable | Any TCP/UDP |
| **Use Case** | Static/dynamic content | Non-HTTP, gaming |
| **IPs** | Edge IPs vary | Static anycast IPs |
| **Protocol** | HTTP/HTTPS/WebSocket | TCP/UDP |

---

### Right-sizing

| Tool | Recommendation Type |
|------|---------------------|
| **Compute Optimizer** | EC2, Lambda, EBS, ECS |
| **Cost Explorer** | Right-sizing suggestions |
| **S3 Storage Lens** | Storage analytics |
| **Trusted Advisor** | Idle resources |

---

### Bottleneck Identification

| Metric | Tool |
|--------|------|
| **CPU/Memory** | CloudWatch, X-Ray |
| **Disk I/O** | CloudWatch, EBS metrics |
| **Network** | VPC Flow Logs, CloudWatch |
| **Database** | Performance Insights, CloudWatch |
| **Application** | X-Ray, Application Insights |

---

## Task 3.4: Improve Reliability

### Single Points of Failure ⭐

| Component | Remediation |
|-----------|-------------|
| **Single EC2** | Auto Scaling, Multi-AZ |
| **Single AZ** | Multi-AZ deployment |
| **Single Region** | Multi-region architecture |
| **Single NAT** | NAT Gateway per AZ |
| **Single Database** | Multi-AZ, Read Replicas |

---

### Data Replication

| Database | Replication |
|----------|-------------|
| **RDS** | Multi-AZ, Read Replicas, Cross-region |
| **Aurora** | Multi-AZ, Global Database |
| **DynamoDB** | Global Tables |
| **S3** | CRR, SRR |
| **ElastiCache** | Multi-AZ, Global Datastore |

---

### Self-Healing Architecture

| Pattern | Implementation |
|---------|----------------|
| **Auto Recovery** | EC2 Auto Recovery |
| **Auto Scaling** | Replace unhealthy instances |
| **Health Checks** | ELB, Route 53 |
| **Auto-remediation** | Lambda triggered by events |

---

### Service Quotas

| Management | Tool |
|------------|------|
| **View Quotas** | Service Quotas console |
| **Request Increase** | Service Quotas, Support |
| **Monitor Usage** | CloudWatch metrics |
| **Alerts** | Trusted Advisor |

---

## Task 3.5: Identify Cost Optimization Opportunities

### Identifying Waste ⭐

| Resource | Detection Tool |
|----------|----------------|
| **Idle EC2** | Trusted Advisor, Cost Explorer |
| **Unattached EBS** | Trusted Advisor |
| **Old Snapshots** | Cost Explorer, DLM |
| **Unused EIPs** | Trusted Advisor |
| **Over-provisioned** | Compute Optimizer |

---

### Cost Allocation

| Method | Implementation |
|--------|----------------|
| **Tags** | Cost allocation tags |
| **Accounts** | Separate by business unit |
| **Budgets** | AWS Budgets with alerts |
| **Reports** | Cost & Usage Reports |

---

### Cost Anomaly Detection

| Feature | Description |
|---------|-------------|
| **AWS Cost Anomaly Detection** | ML-based anomaly alerts |
| **Budget Alerts** | Threshold notifications |
| **Cost Explorer** | Trend analysis |

---

### Reserved Capacity Analysis

| Tool | Recommendation |
|------|----------------|
| **Cost Explorer** | RI/Savings Plans recommendations |
| **Trusted Advisor** | RI optimization |
| **AWS Budgets** | RI utilization tracking |

---

## Exam Tips for Domain 3

1. **CloudWatch Logs Insights** = query logs with SQL-like syntax
2. **EventBridge + Lambda** = auto-remediation pattern
3. **Secrets Manager** = automatic rotation
4. **Config + SSM Automation** = auto-fix non-compliant resources
5. **AWS Backup** = centralized backup management
6. **Compute Optimizer** = right-sizing recommendations
7. **Global Accelerator** = static IPs, TCP/UDP acceleration
8. **Multi-AZ everything** = eliminate single points of failure
9. **Cost Anomaly Detection** = ML-based spending alerts
10. **Tag strategy** = mandatory for cost allocation
