# AWS Certified CloudOps Engineer - Associate (SOA-C03) - Exam Overview

## Exam Information

| Attribute | Details |
|-----------|---------|
| **Exam Code** | SOA-C03 |
| **Duration** | 130 minutes |
| **Questions** | 50 scored + 15 unscored (65 total) |
| **Passing Score** | 720 out of 1000 |
| **Format** | Multiple choice, Multiple response |
| **Experience** | 1 year deployment, management, troubleshooting on AWS |

---

## Domain Weightings

| Domain | Content | Weight |
|--------|---------|--------|
| **Domain 1** | Monitoring, Logging, Analysis, Remediation, and Performance Optimization | 22% |
| **Domain 2** | Reliability and Business Continuity | 22% |
| **Domain 3** | Deployment, Provisioning, and Automation | 22% |
| **Domain 4** | Security and Compliance | 16% |
| **Domain 5** | Networking and Content Delivery | 18% |

> **Exam Tip:** Domains 1, 2, and 3 each have 22% weightage. Equal focus on these three is crucial!

---

## Target Candidate Profile

### Expected Knowledge
- Techniques for monitoring, logging, and troubleshooting
- Networking concepts (DNS, TCP/IP, firewalls)
- High availability, performance, capacity implementation
- Scripting language familiarity
- Operating system familiarity
- CI/CD and Git understanding
- Containerization and orchestration basics

### AWS Knowledge Expected
- AWS Well-Architected Framework
- Storage and container solutions
- AWS monitoring tools (CloudWatch, CloudTrail)
- AWS Console, CLI, and CloudFormation
- AWS networking and security services
- Cloud financial management
- Hybrid and multi-VPC environments

### What is NOT Expected (Out of Scope)
- Design distributed architectures
- Design CI/CD pipelines
- Design hybrid and multi-VPC networking
- Develop software
- Define security, compliance, and governance requirements
- Develop ransomware defense strategies
- Assess and plan resource capacity
- Manage billing and invoicing

---

## Core Exam Responsibilities

1. Support and maintain workloads according to AWS Well-Architected Framework
2. Perform operations using AWS Console and CLI
3. Implement security controls for compliance
4. Monitor, log, and troubleshoot systems
5. Apply networking concepts (DNS, TCP/IP, firewalls)
6. Implement HA, performance, and capacity requirements
7. Perform business continuity and disaster recovery
8. Identify, classify, and remediate incidents

---

## In-Scope AWS Services (Quick Reference)

### Compute & Containers
| Service | Primary Use |
|---------|-------------|
| Amazon EC2 | Virtual servers |
| EC2 Image Builder | AMI creation |
| AWS Lambda | Serverless compute |
| Amazon ECS | Container orchestration |
| Amazon EKS | Kubernetes |
| Amazon ECR | Container registry |

### Storage
| Service | Primary Use |
|---------|-------------|
| Amazon S3 | Object storage |
| Amazon EBS | Block storage |
| Amazon EFS | File storage (NFS) |
| Amazon FSx | Managed file systems |
| AWS Backup | Centralized backup |
| AWS Storage Gateway | Hybrid storage |

### Database
| Service | Primary Use |
|---------|-------------|
| Amazon RDS | Relational databases |
| Amazon Aurora | High-performance relational |
| Amazon DynamoDB | NoSQL database |
| Amazon ElastiCache | In-memory caching |
| RDS Proxy | Connection pooling |
| DAX | DynamoDB acceleration |

### Monitoring & Management
| Service | Primary Use |
|---------|-------------|
| Amazon CloudWatch | Metrics, logs, alarms |
| AWS CloudTrail | API audit logging |
| AWS Config | Configuration compliance |
| AWS Systems Manager | Operations management |
| AWS Trusted Advisor | Best practices |
| AWS Compute Optimizer | Right-sizing |
| AWS Organizations | Multi-account management |
| AWS Control Tower | Landing zone |

### Networking
| Service | Primary Use |
|---------|-------------|
| Amazon VPC | Virtual networking |
| Amazon Route 53 | DNS management |
| Amazon CloudFront | CDN |
| Elastic Load Balancing | Traffic distribution |
| AWS Transit Gateway | VPC connectivity |
| AWS PrivateLink | Private connectivity |
| VPC Flow Logs | Network traffic logging |
| AWS Global Accelerator | Global traffic optimization |

### Security
| Service | Primary Use |
|---------|-------------|
| AWS IAM | Access management |
| AWS KMS | Key management |
| AWS Certificate Manager | SSL/TLS certificates |
| AWS Secrets Manager | Secrets storage |
| Amazon GuardDuty | Threat detection |
| AWS Security Hub | Security posture |
| Amazon Inspector | Vulnerability scanning |
| AWS WAF | Web application firewall |
| AWS Shield | DDoS protection |

### Application Integration
| Service | Primary Use |
|---------|-------------|
| Amazon EventBridge | Event routing |
| Amazon SNS | Notifications |
| Amazon SQS | Message queuing |
| AWS Step Functions | Workflow orchestration |

### Infrastructure as Code
| Service | Primary Use |
|---------|-------------|
| AWS CloudFormation | IaC templates |
| AWS CDK | Programmatic IaC |

---

## Study Tips

1. **CloudWatch is King** - Master metrics, alarms, dashboards, and logs
2. **Systems Manager** - Know Session Manager, Run Command, Automation, Patch Manager
3. **High Availability** - Multi-AZ, Auto Scaling, Load Balancing
4. **Backup and Recovery** - AWS Backup, RTO/RPO, point-in-time recovery
5. **Security** - IAM policies, encryption, security services integration
6. **Networking** - VPC components, Route 53, troubleshooting connectivity

---

## Cheatsheet Files

- [Domain 1: Monitoring, Logging, Analysis, Remediation, and Performance Optimization](./01-domain1-monitoring-logging-performance.md)
- [Domain 2: Reliability and Business Continuity](./02-domain2-reliability-business-continuity.md)
- [Domain 3: Deployment, Provisioning, and Automation](./03-domain3-deployment-automation.md)
- [Domain 4: Security and Compliance](./04-domain4-security-compliance.md)
- [Domain 5: Networking and Content Delivery](./05-domain5-networking-content-delivery.md)
