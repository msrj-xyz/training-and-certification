# AWS SysOps Administrator (SOA-C03) - Quick Reference Card

## Exam at a Glance
| | |
|---|---|
| **Duration** | 130 min | **Questions** | 65 (50 scored) |
| **Passing** | 720/1000 | **Format** | Multiple choice/response |

## Domain Weights
```
Domain 1: Monitoring & Performance ████████████████     22%
Domain 2: Reliability & BC         ████████████████     22%
Domain 3: Deployment & Automation  ████████████████     22%
Domain 4: Security & Compliance    ████████████         16%
Domain 5: Networking & CDN         █████████████        18%
```

---

## 🔥 Top Services to Know

### Operations Core
| Service | Purpose |
|---------|---------|
| **CloudWatch** | Metrics, logs, alarms, dashboards |
| **Systems Manager** | Session, Run Command, Patch Manager |
| **EventBridge** | Event routing, automation |
| **CloudTrail** | API audit logging |
| **Config** | Configuration compliance |

### Compute & Storage
| Service | Key Feature |
|---------|-------------|
| **Auto Scaling** | Target tracking, predictive |
| **ELB** | ALB (L7), NLB (L4), GWLB |
| **S3** | Versioning, lifecycle, replication |
| **EBS** | gp3 (general), io2 (IOPS) |

---

## 📊 CloudWatch Quick Reference

### Metrics Requiring Agent
| Requires Agent | Default Metrics |
|----------------|-----------------|
| Memory | CPU |
| Disk space | Network I/O |
| Processes | Status checks |

### Alarm States
`OK` → `ALARM` → `INSUFFICIENT_DATA`

### Resolution
- Standard: 5 min (free)
- Detailed: 1 min (cost)
- High-res: 1 sec (custom only)

---

## 🔄 High Availability Essentials

### DR Strategies (Cost Order)
| Strategy | RTO | Cost |
|----------|-----|------|
| Backup & Restore | Hours | $ |
| Pilot Light | 10s min | $$ |
| Warm Standby | Minutes | $$$ |
| Active-Active | Real-time | $$$$ |

### Load Balancer Selection
| ALB | NLB |
|-----|-----|
| Layer 7 (HTTP) | Layer 4 (TCP/UDP) |
| Path routing | Static IP |
| Lambda targets | Ultra-low latency |

---

## 🛠️ Systems Manager Components

| Feature | Use Case |
|---------|----------|
| **Session Manager** | SSH without ports |
| **Run Command** | Remote execution |
| **Patch Manager** | Automated patching |
| **Parameter Store** | Configuration |
| **Automation** | Runbooks |

---

## 🔐 Security Quick Reference

### IAM Policy Evaluation
`Explicit Deny → Explicit Allow → Implicit Deny`

### Key Management
| Type | Control |
|------|---------|
| AWS Managed | AWS controls |
| Customer Managed | You control |
| AWS Owned | Shared |

### Security Services
| Service | Function |
|---------|----------|
| **GuardDuty** | Threat detection |
| **Security Hub** | Aggregation |
| **Inspector** | Vulnerability scan |
| **Config** | Compliance |

---

## 🌐 Networking Quick Reference

### SG vs NACL
| Security Group | NACL |
|----------------|------|
| Stateful | Stateless |
| Allow only | Allow + Deny |
| Instance level | Subnet level |

### VPC Endpoints
| Gateway (FREE) | Interface ($$) |
|----------------|----------------|
| S3, DynamoDB | All other services |

### Subnet Reserved IPs
AWS reserves 5 IPs per subnet: `+0, +1, +2, +3, last`

---

## ✅ Exam Day Reminders

1. **CloudWatch Agent** = memory + disk metrics
2. **EventBridge** > CloudWatch Events
3. **Target Tracking** = preferred scaling
4. **Multi-AZ RDS** = sync replication, auto-failover
5. **VPC Flow Logs REJECT** = SG or NACL blocking
6. **Gateway endpoints** = FREE for S3/DynamoDB
7. **Alias** = zone apex, **CNAME** = cannot be apex
8. **SCPs** = don't apply to management account
