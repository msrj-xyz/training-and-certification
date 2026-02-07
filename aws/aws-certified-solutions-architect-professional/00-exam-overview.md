# AWS Certified Solutions Architect - Professional (SAP-C02)

## Exam Information

| Aspect | Details |
|--------|---------|
| **Exam Code** | SAP-C02 |
| **Level** | Professional |
| **Duration** | 180 minutes |
| **Questions** | 75 questions |
| **Passing Score** | 750/1000 |
| **Format** | Multiple choice, multiple response |
| **Cost** | $300 USD |
| **Languages** | English, Japanese, Korean, Simplified Chinese |

---

## Domain Weightings

| Domain | Weight | Focus Areas |
|--------|--------|-------------|
| **Domain 1** | 26% | Design Solutions for Organizational Complexity |
| **Domain 2** | 29% | Design for New Solutions |
| **Domain 3** | 25% | Continuous Improvement for Existing Solutions |
| **Domain 4** | 20% | Accelerate Workload Migration and Modernization |

---

## Target Candidate Profile

- 2+ years hands-on experience designing AWS solutions
- Ability to evaluate cloud application requirements
- Make architectural recommendations for implementation
- Provide implementation guidance based on best practices

---

## Core Exam Responsibilities

1. **Design and deploy** dynamically scalable, highly available, fault-tolerant systems
2. **Select appropriate AWS services** based on data, compute, database, or security requirements
3. **Migrate complex, multi-tier applications** to AWS
4. **Design and deploy** enterprise-wide scalable operations
5. **Implement cost-control strategies**
6. **Design solutions** that meet organizational complexity requirements

---

## Key Technologies and Concepts

### Networking
- VPC, Subnets, Route Tables, NAT Gateway
- Direct Connect, Site-to-Site VPN, Transit Gateway
- Route 53 (DNS routing policies)
- VPC Peering, PrivateLink, VPC Endpoints
- AWS Global Accelerator, CloudFront

### Compute
- EC2 (instance types, placement groups, Spot, Reserved)
- Lambda, Fargate, ECS, EKS
- Elastic Beanstalk, App Runner

### Storage
- S3 (storage classes, lifecycle, replication)
- EBS, EFS, FSx
- Storage Gateway, DataSync, Snow Family

### Database
- RDS (Multi-AZ, Read Replicas, Aurora)
- DynamoDB (Global Tables, DAX)
- ElastiCache, Neptune, Redshift

### Security
- IAM, Organizations, Control Tower, IAM Identity Center
- KMS, Secrets Manager, Certificate Manager
- WAF, Shield, GuardDuty, Security Hub, Inspector
- CloudTrail, Config, Macie

### Migration
- Migration Hub, Application Migration Service
- Database Migration Service (DMS), Schema Conversion Tool
- DataSync, Transfer Family, Snow Family

---

## In-Scope AWS Services

### Compute
| Service | Key Use Case |
|---------|--------------|
| EC2 | Virtual servers |
| Lambda | Serverless functions |
| ECS/EKS | Container orchestration |
| Fargate | Serverless containers |
| Elastic Beanstalk | PaaS deployment |
| Outposts | On-premises AWS |

### Networking
| Service | Key Use Case |
|---------|--------------|
| VPC | Virtual network |
| Direct Connect | Dedicated connection |
| Transit Gateway | Multi-VPC hub |
| Route 53 | DNS and routing |
| CloudFront | CDN |
| Global Accelerator | Global traffic |
| PrivateLink | Private connectivity |

### Storage
| Service | Key Use Case |
|---------|--------------|
| S3 | Object storage |
| EBS | Block storage |
| EFS | File storage |
| FSx | Windows/Lustre FS |
| Storage Gateway | Hybrid storage |

### Database
| Service | Key Use Case |
|---------|--------------|
| RDS | Managed relational |
| Aurora | High-performance relational |
| DynamoDB | NoSQL |
| ElastiCache | In-memory cache |
| Redshift | Data warehouse |

### Security & Identity
| Service | Key Use Case |
|---------|--------------|
| IAM | Access management |
| Organizations | Multi-account |
| Control Tower | Landing zone |
| KMS | Encryption keys |
| Secrets Manager | Secrets storage |
| WAF/Shield | Web protection |

### Management
| Service | Key Use Case |
|---------|--------------|
| CloudFormation | IaC |
| Systems Manager | Operations |
| CloudWatch | Monitoring |
| CloudTrail | API auditing |
| Config | Compliance |
| Trusted Advisor | Best practices |

---

## Exam Tips Summary

1. **Focus on Well-Architected Framework** - All 6 pillars
2. **Understand DR strategies** - RTO/RPO trade-offs
3. **Know networking deeply** - Transit Gateway, Direct Connect, VPN
4. **Multi-account strategies** - Organizations, Control Tower, SCPs
5. **Cost optimization** - RI, Savings Plans, Spot, right-sizing
6. **Migration strategies** - 7Rs, migration tools
7. **Security at every layer** - Defense in depth
8. **High availability patterns** - Multi-AZ, multi-Region
9. **Hybrid architectures** - On-premises integration
10. **Serverless and containers** - When to use each
