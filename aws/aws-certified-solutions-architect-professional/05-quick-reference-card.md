# AWS Solutions Architect Professional - Quick Reference Card

## Domain Weights
| Domain | Weight |
|--------|--------|
| D1 - Organizational Complexity | 26% |
| D2 - New Solutions | 29% |
| D3 - Continuous Improvement | 25% |
| D4 - Migration & Modernization | 20% |

---

## Networking Quick Reference

| Need | Solution |
|------|----------|
| Multi-VPC hub-spoke | **Transit Gateway** |
| 1:1 VPC connection | VPC Peering |
| Private AWS service access | VPC Endpoints, PrivateLink |
| Dedicated on-prem connection | **Direct Connect** |
| Quick encrypted connection | Site-to-Site VPN |
| Hybrid DNS | Route 53 Resolver |
| Global static IPs | Global Accelerator |
| CDN | CloudFront |

---

## DR Strategies Quick Reference

| Strategy | RTO | RPO | Cost |
|----------|-----|-----|------|
| Backup & Restore | Hours | Hours | $ |
| Pilot Light | 10-30 min | Minutes | $$ |
| Warm Standby | Minutes | Seconds | $$$ |
| Multi-site Active | Near-zero | Near-zero | $$$$ |

---

## Multi-Account Quick Reference

| Need | Solution |
|------|----------|
| Account governance | **AWS Organizations** |
| Guardrails (SCPs) | Organizations |
| Automated landing zone | **Control Tower** |
| Cross-account sharing | AWS RAM |
| Centralized logging | CloudTrail Organization Trail |
| Centralized identity | IAM Identity Center |

---

## Deployment Strategies

| Strategy | Risk | Rollback |
|----------|------|----------|
| Blue/Green | Low | Instant |
| Canary | Low | Quick |
| Rolling | Medium | Manual |
| All-at-once | High | Manual |

---

## The 7Rs Migration

| R | Strategy | Effort |
|---|----------|--------|
| 1 | **Retire** - Decommission | Low |
| 2 | **Retain** - Keep on-prem | None |
| 3 | **Rehost** - Lift-and-shift | Low |
| 4 | **Relocate** - VMware to AWS | Low |
| 5 | **Repurchase** - Move to SaaS | Medium |
| 6 | **Replatform** - Minor changes | Medium |
| 7 | **Refactor** - Re-architect | High |

---

## Migration Tools

| Data Type | Tool |
|-----------|------|
| Files (NFS, SMB) | **DataSync** |
| Petabyte offline | **Snow Family** |
| Database | **DMS + SCT** |
| VMs to EC2 | **MGN** |
| SFTP/FTP to S3 | Transfer Family |

---

## Database Selection

| Need | Service |
|------|---------|
| High-perf relational | **Aurora** |
| Key-value, low latency | **DynamoDB** |
| In-memory cache | **ElastiCache** |
| MongoDB compatible | DocumentDB |
| Graph database | Neptune |
| Time series | Timestream |
| Data warehouse | Redshift |

---

## Serverless Decision

| Traditional | Serverless |
|-------------|------------|
| EC2 | Lambda, Fargate |
| ELB + EC2 | API Gateway + Lambda |
| RDS | Aurora Serverless, DynamoDB |
| Cron | EventBridge Scheduler |
| VMs | Fargate containers |

---

## Cost Optimization

| Need | Solution |
|------|----------|
| Steady workload savings | **Savings Plans**, Reserved Instances |
| Fault-tolerant workload | **Spot Instances** |
| Auto storage tiering | **S3 Intelligent-Tiering** |
| Right-sizing | **Compute Optimizer** |
| Reduce data transfer | VPC Endpoints, PrivateLink |
| Cost visibility | Tags, Cost Explorer |

---

## Security Quick Reference

| Need | Solution |
|------|----------|
| DDoS protection | **Shield + WAF** |
| Key management | **KMS**, CloudHSM |
| Secret rotation | **Secrets Manager** |
| API audit | **CloudTrail** |
| Config compliance | **AWS Config** |
| Threat detection | **GuardDuty** |
| Vulnerability scan | **Inspector** |
| S3 data discovery | **Macie** |

---

## High Availability Patterns

| Component | HA Solution |
|-----------|-------------|
| EC2 | Auto Scaling across AZs |
| RDS | Multi-AZ deployment |
| S3 | Cross-region replication |
| DynamoDB | Global Tables |
| Aurora | Global Database |
| DNS | Route 53 health checks |

---

## Integration Services

| Pattern | Service |
|---------|---------|
| Message queue | **SQS** |
| Pub/sub | **SNS** |
| Event bus | **EventBridge** |
| Workflows | **Step Functions** |
| API management | **API Gateway** |

---

## Exam Day Reminders

1. **Transit Gateway** = multi-VPC, multi-account hub
2. **Direct Connect** = consistent latency, dedicated
3. **Organizations + SCPs** = governance guardrails
4. **Blue/Green** = instant rollback
5. **DMS + SCT** = database migration
6. **MGN** = lift-and-shift VMs
7. **Aurora Global** = cross-region DR < 1 second RPO
8. **SQS** = decouple services
9. **EventBridge** = event-driven architecture
10. **Savings Plans** = more flexible than RIs
