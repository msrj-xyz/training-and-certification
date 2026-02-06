# AWS Certified DevOps Engineer - Professional (DOP-C02) - Exam Overview

## Exam Information

| Attribute | Details |
|-----------|---------|
| **Exam Code** | DOP-C02 |
| **Level** | Professional |
| **Duration** | 180 minutes |
| **Questions** | 65 scored + 10 unscored (75 total) |
| **Passing Score** | 750 out of 1000 |
| **Format** | Multiple choice, Multiple response |
| **Experience** | 2+ years AWS DevOps experience |

> ⚠️ **Professional Level Exam** - More complex scenarios, deeper technical knowledge required!

---

## Domain Weightings

| Domain | Content | Weight |
|--------|---------|--------|
| **Domain 1** | SDLC Automation | 22% |
| **Domain 2** | Configuration Management and IaC | 17% |
| **Domain 3** | Resilient Cloud Solutions | 15% |
| **Domain 4** | Monitoring and Logging | 15% |
| **Domain 5** | Incident and Event Response | 14% |
| **Domain 6** | Security and Compliance | 17% |

---

## Target Candidate Profile

### Expected Experience
- 2+ years provisioning, operating, managing AWS environments
- Software development lifecycle knowledge
- Programming and/or scripting experience
- Building highly automated infrastructure
- Operating systems administration
- Modern DevOps processes and methodologies

### Core Exam Tasks
1. Implement and manage **continuous delivery systems** on AWS
2. Implement and automate **security controls** and compliance validation
3. Define and deploy **monitoring, metrics, and logging** systems
4. Implement **highly available, scalable, self-healing** systems
5. Design, manage, maintain tools to **automate operational processes**

### What is NOT Expected (Out of Scope)
- Advanced networking (routing algorithms, failover techniques)
- Deep-level security recommendations to developers
- Database design, query optimization
- Full-stack application code development

---

## In-Scope AWS Services (Quick Reference)

### Developer Tools ⭐ (Core)
| Service | Primary Use |
|---------|-------------|
| **CodePipeline** ⭐ | CI/CD orchestration |
| **CodeBuild** ⭐ | Build and test |
| **CodeDeploy** ⭐ | Deployment automation |
| **CodeArtifact** | Package management |
| **CodeGuru** | Code review and profiling |
| **AWS CDK** ⭐ | Programmatic IaC |
| **X-Ray** | Distributed tracing |
| **Fault Injection Simulator** | Chaos engineering |

### Containers ⭐
| Service | Primary Use |
|---------|-------------|
| **ECS** ⭐ | Container orchestration |
| **EKS** ⭐ | Managed Kubernetes |
| **Fargate** ⭐ | Serverless containers |
| **ECR** | Container registry |
| **App Runner** | Simplified container deployment |
| **Copilot** | ECS/Fargate CLI |

### Management and Governance ⭐
| Service | Primary Use |
|---------|-------------|
| **CloudFormation** ⭐ | Infrastructure as Code |
| **Systems Manager** ⭐ | Operations automation |
| **CloudWatch** ⭐ | Monitoring and logs |
| **Config** ⭐ | Configuration compliance |
| **Organizations** | Multi-account management |
| **Control Tower** | Landing zone setup |
| **Service Catalog** | Product provisioning |
| **Resilience Hub** | Resilience assessment |

### Compute
| Service | Primary Use |
|---------|-------------|
| **EC2** | Virtual servers |
| **EC2 Auto Scaling** ⭐ | Dynamic scaling |
| **Elastic Beanstalk** | PaaS deployment |
| **EC2 Image Builder** | AMI automation |

### Serverless
| Service | Primary Use |
|---------|-------------|
| **Lambda** ⭐ | Event-driven compute |
| **Step Functions** ⭐ | Workflow orchestration |
| **API Gateway** | API management |
| **SAM** | Serverless framework |
| **SNS/SQS** | Messaging |
| **EventBridge** ⭐ | Event routing |

### Security ⭐
| Service | Primary Use |
|---------|-------------|
| **IAM** ⭐ | Access management |
| **KMS** ⭐ | Key management |
| **Secrets Manager** ⭐ | Secrets storage |
| **Security Hub** | Security posture |
| **GuardDuty** | Threat detection |
| **Inspector** | Vulnerability scanning |
| **Macie** | Data discovery |
| **WAF/Shield** | Web protection |

### Storage
| Service | Primary Use |
|---------|-------------|
| **S3** ⭐ | Object storage |
| **EBS** | Block storage |
| **EFS** | File storage |
| **Elastic Disaster Recovery** | DR solution |
| **AWS Backup** | Centralized backup |

### Networking
| Service | Primary Use |
|---------|-------------|
| **ELB** (ALB, NLB) | Load balancing |
| **CloudFront** | CDN |
| **Route 53** | DNS and routing |
| **VPC** | Network isolation |
| **Transit Gateway** | Network hub |

---

## Study Tips

1. **CI/CD is Central (22%)** - Deep dive into CodePipeline, CodeBuild, CodeDeploy
2. **IaC Mastery** - CloudFormation advanced features, StackSets, nested stacks
3. **Container Expertise** - ECS, EKS, Fargate deployment strategies
4. **Systems Manager** - Know all capabilities (Run Command, Patch Manager, SSM Agent)
5. **Monitoring** - CloudWatch Logs Insights, alarms, dashboards, X-Ray
6. **Security** - IAM policies, SCPs, Secrets Manager, KMS
7. **Multi-account** - Organizations, Control Tower, cross-account access

---

## Cheatsheet Files

- [Domain 1: SDLC Automation](./01-domain1-sdlc-automation.md)
- [Domain 2: Configuration Management and IaC](./02-domain2-config-iac.md)
- [Domain 3: Resilient Cloud Solutions](./03-domain3-resilient-solutions.md)
- [Domain 4: Monitoring and Logging](./04-domain4-monitoring-logging.md)
- [Domain 5: Incident and Event Response](./05-domain5-incident-response.md)
- [Domain 6: Security and Compliance](./06-domain6-security-compliance.md)
- [**Quick Reference Card (1-page summary)**](./07-quick-reference-card.md)
