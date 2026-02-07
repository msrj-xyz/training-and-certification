# Domain 1: Detection (16%)

## Task 1.1: Design and Implement Monitoring and Alerting

### AWS Security Monitoring Services ⭐

| Service | Purpose | Key Features |
|---------|---------|--------------|
| **GuardDuty** | Threat detection | ML-based, findings by severity |
| **Security Hub** | Centralized security | Aggregates findings, standards |
| **Macie** | S3 data discovery | PII, sensitive data detection |
| **Inspector** | Vulnerability scanning | EC2, Lambda, ECR scanning |
| **Detective** | Security investigation | Graph analysis, root cause |
| **Config** | Configuration compliance | Rules, conformance packs |

---

### Amazon GuardDuty ⭐

| Finding Type | Description |
|--------------|-------------|
| **Reconnaissance** | Port scanning, API probing |
| **Instance Compromise** | Malware, C&C communication |
| **Account Compromise** | Anomalous API calls, credential compromise |
| **Bucket Compromise** | S3 anomalous access |
| **Kubernetes** | EKS threat detection |
| **Runtime Monitoring** | EC2, ECS, EKS runtime threats |

#### GuardDuty Data Sources
- CloudTrail management events
- CloudTrail S3 data events
- VPC Flow Logs
- DNS logs
- EKS audit logs
- Runtime monitoring

> **Exam Tip:** GuardDuty doesn't prevent attacks, it detects them!

---

### AWS Security Hub ⭐

| Feature | Description |
|---------|-------------|
| **Standards** | CIS, AWS Foundational, PCI DSS |
| **Findings** | Aggregated from 50+ services |
| **Insights** | Grouped findings analysis |
| **Automations** | EventBridge integration |
| **Cross-Region** | Aggregate across regions |

#### Security Hub Integrations
- GuardDuty findings
- Inspector findings
- Macie findings
- Config rules
- Firewall Manager
- IAM Access Analyzer
- Third-party tools

---

### Amazon Security Lake

| Feature | Description |
|---------|-------------|
| **OCSF Format** | Open Cybersecurity Schema Framework |
| **Data Sources** | CloudTrail, VPC Flow Logs, Route 53, S3 |
| **Storage** | S3-based data lake |
| **Subscribers** | Athena, OpenSearch, SIEM tools |
| **Retention** | Configurable lifecycle |

---

### CloudWatch for Security

| Component | Security Use Case |
|-----------|-------------------|
| **Metrics** | Track security metrics |
| **Alarms** | Alert on thresholds |
| **Logs** | Centralized log storage |
| **Logs Insights** | Query and analyze logs |
| **Contributor Insights** | Top N analysis |
| **Synthetics** | Canary monitoring |

---

### Alerting Patterns

| Pattern | Implementation |
|---------|----------------|
| **Real-time** | EventBridge → Lambda/SNS |
| **Threshold-based** | CloudWatch Alarms |
| **Anomaly-based** | GuardDuty findings |
| **Compliance** | Config Rules → SNS |
| **Aggregated** | Security Hub → EventBridge |

---

## Task 1.2: Design and Implement Logging Solutions

### AWS CloudTrail ⭐

| Feature | Description |
|---------|-------------|
| **Management Events** | Control plane operations |
| **Data Events** | S3, Lambda, DynamoDB operations |
| **Insights Events** | Unusual activity detection |
| **Organization Trail** | All accounts logging |
| **Log File Integrity** | Validate log integrity |

#### CloudTrail Best Practices
- Enable in all regions
- Create organization trail
- Send to centralized S3 bucket
- Enable log file validation
- Encrypt with KMS
- Enable CloudTrail Insights

---

### Logging Sources

| Source | Log Type |
|--------|----------|
| **CloudTrail** | API calls |
| **VPC Flow Logs** | Network traffic |
| **ALB/NLB Access Logs** | Load balancer access |
| **CloudFront Logs** | CDN access |
| **S3 Access Logs** | Bucket access |
| **Route 53 Resolver Logs** | DNS queries |
| **WAF Logs** | Web traffic |
| **Network Firewall Logs** | Firewall traffic |

---

### VPC Flow Logs ⭐

| Field | Description |
|-------|-------------|
| **version** | Flow log version |
| **account-id** | AWS account |
| **interface-id** | ENI ID |
| **srcaddr/dstaddr** | Source/destination IP |
| **srcport/dstport** | Source/destination port |
| **protocol** | IP protocol number |
| **packets/bytes** | Traffic volume |
| **action** | ACCEPT or REJECT |

#### Flow Log Destinations
- CloudWatch Logs
- S3
- Kinesis Data Firehose

---

### Log Analysis Services

| Service | Use Case |
|---------|----------|
| **CloudWatch Logs Insights** | Quick queries |
| **Amazon Athena** | SQL queries on S3 logs |
| **OpenSearch Service** | Full-text search, dashboards |
| **Security Lake** | Standardized security data |
| **QuickSight** | Visualization |

---

### Centralized Logging Architecture

```
All Accounts → Organization Trail → Log Archive Account → S3 Bucket
                                                        ↓
                                        Athena / OpenSearch / Security Lake
```

---

## Task 1.3: Troubleshoot Monitoring and Logging

### Common Issues

| Issue | Resolution |
|-------|------------|
| **Missing CloudTrail logs** | Check S3 bucket policy, KMS permissions |
| **Flow Logs not appearing** | Check IAM role, CloudWatch Logs permissions |
| **CloudWatch Agent** | Verify agent config, instance role |
| **GuardDuty not detecting** | Enable required data sources |
| **Security Hub no findings** | Enable integrations, check regions |

---

### Troubleshooting Tools

| Tool | Purpose |
|------|---------|
| **CloudTrail Event History** | Quick API lookup |
| **CloudWatch Logs Insights** | Query log patterns |
| **IAM Policy Simulator** | Test IAM permissions |
| **VPC Reachability Analyzer** | Network path analysis |

---

## Exam Tips for Domain 1

1. **GuardDuty** = threat detection (ML-based), doesn't block
2. **Security Hub** = aggregated findings, compliance standards
3. **Security Lake** = OCSF format, security data lake
4. **CloudTrail Organization Trail** = centralized audit logging
5. **VPC Flow Logs** = network traffic analysis
6. **Config** = configuration compliance rules
7. **Detective** = root cause investigation
8. **Macie** = S3 sensitive data (PII) discovery
9. **Inspector** = vulnerability scanning (EC2, Lambda, ECR)
10. **EventBridge** = real-time security automation
