# AWS Certified Machine Learning Engineer - Associate (MLA-C01) - Exam Overview

## Exam Information

| Attribute | Details |
|-----------|---------|
| **Exam Code** | MLA-C01 |
| **Duration** | 130 minutes |
| **Questions** | 50 scored + 15 unscored (65 total) |
| **Passing Score** | 720 out of 1000 |
| **Format** | Multiple choice, Multiple response, Ordering, Matching |
| **Experience** | 1+ year with SageMaker and AWS ML services |

---

## Domain Weightings

| Domain | Content | Weight |
|--------|---------|--------|
| **Domain 1** | Data Preparation for Machine Learning | 28% |
| **Domain 2** | ML Model Development | 26% |
| **Domain 3** | Deployment and Orchestration of ML Workflows | 22% |
| **Domain 4** | ML Solution Monitoring, Maintenance, and Security | 24% |

> **Exam Tip:** Domain 1 (Data Preparation) has the highest weighting at 28%!

---

## Target Candidate Profile

### Expected Experience
- 1+ year using Amazon SageMaker and AWS ML services
- 1+ year in related role (backend developer, DevOps, data engineer, data scientist)

### Recommended IT Knowledge
- Basic understanding of ML algorithms and use cases
- Data engineering fundamentals (formats, ingestion, transformation)
- Software engineering best practices (modular code, debugging)
- CI/CD pipelines and Infrastructure as Code (IaC)
- Version control systems (Git)

### AWS Knowledge Expected
- SageMaker capabilities and algorithms
- AWS data storage and processing services
- Monitoring tools for logging and troubleshooting
- CI/CD automation and orchestration services
- AWS security best practices (IAM, encryption)

### What is NOT Expected (Out of Scope)
- Designing full end-to-end ML solutions
- Setting up ML strategies and best practices
- Deep expertise in multiple ML domains (NLP, computer vision)
- Quantizing models and analyzing accuracy impact
- Integration with wide array of new tools

---

## Core Exam Tasks

1. **Ingest, transform, validate, and prepare data** for ML modeling
2. **Select modeling approaches**, train models, tune hyperparameters
3. **Choose deployment infrastructure** and configure auto scaling
4. **Set up CI/CD pipelines** to automate ML workflows
5. **Monitor models**, data, and infrastructure
6. **Secure ML systems** with access controls and compliance

---

## In-Scope AWS Services (Quick Reference)

### Analytics
| Service | Primary Use |
|---------|-------------|
| Amazon Athena | SQL queries on S3 |
| Amazon EMR | Big data processing (Spark, Hadoop) |
| AWS Glue | ETL and data catalog |
| AWS Glue DataBrew | Visual data preparation |
| AWS Glue Data Quality | Data quality validation |
| Amazon Kinesis | Real-time streaming |
| AWS Lake Formation | Data lake management |
| Amazon Redshift | Data warehouse |
| Amazon QuickSight | BI and visualization |

### Machine Learning (Core)
| Service | Primary Use |
|---------|-------------|
| **Amazon SageMaker** | Full ML platform |
| Amazon Bedrock | Foundation models |
| SageMaker JumpStart | Pre-built solutions |
| SageMaker Feature Store | Feature management |
| SageMaker Data Wrangler | Visual data prep |
| SageMaker Ground Truth | Data labeling |
| SageMaker Clarify | Bias detection and explainability |
| SageMaker Model Monitor | Production monitoring |
| SageMaker Pipelines | ML workflow orchestration |
| SageMaker Model Registry | Model versioning |

### ML AI Services
| Service | Primary Use |
|---------|-------------|
| Amazon Comprehend | NLP/text analysis |
| Amazon Rekognition | Image/video analysis |
| Amazon Transcribe | Speech-to-text |
| Amazon Translate | Language translation |
| Amazon Textract | Document extraction |
| Amazon Polly | Text-to-speech |
| Amazon Personalize | Recommendation systems |
| Amazon Lex | Conversational AI |
| Amazon Kendra | Enterprise search |
| Amazon Fraud Detector | Fraud detection |

### Compute & Containers
| Service | Primary Use |
|---------|-------------|
| Amazon EC2 | Virtual servers (GPU/CPU) |
| AWS Lambda | Serverless compute |
| AWS Batch | Batch processing jobs |
| Amazon ECS | Container orchestration |
| Amazon EKS | Kubernetes |
| Amazon ECR | Container registry |

### Storage
| Service | Primary Use |
|---------|-------------|
| Amazon S3 | Object storage (ML data) |
| Amazon EBS | Block storage |
| Amazon EFS | Shared file storage |
| Amazon FSx | High-performance file systems |

### Developer Tools
| Service | Primary Use |
|---------|-------------|
| AWS CodePipeline | CI/CD pipelines |
| AWS CodeBuild | Build service |
| AWS CodeDeploy | Deployment automation |
| AWS CDK | IaC (programmatic) |
| AWS X-Ray | Distributed tracing |

### Management & Monitoring
| Service | Primary Use |
|---------|-------------|
| Amazon CloudWatch | Metrics and logs |
| AWS CloudTrail | API audit logging |
| AWS Compute Optimizer | Right-sizing |
| AWS Cost Explorer | Cost analysis |
| AWS Trusted Advisor | Best practices |

### Application Integration
| Service | Primary Use |
|---------|-------------|
| Amazon EventBridge | Event routing |
| AWS Step Functions | Workflow orchestration |
| Amazon MWAA | Managed Airflow |
| Amazon SNS/SQS | Messaging |

### Security
| Service | Primary Use |
|---------|-------------|
| AWS IAM | Access management |
| AWS KMS | Key management |
| AWS Secrets Manager | Secrets storage |
| Amazon Macie | Data security (S3) |

---

## Study Tips

1. **SageMaker is Central** - Master all SageMaker components
2. **Data Prep (28%)** - Focus on Glue, Data Wrangler, Feature Store
3. **Model Development (26%)** - Built-in algorithms, hyperparameter tuning
4. **Deployment (22%)** - Endpoint types, CI/CD with CodePipeline
5. **Monitoring (24%)** - Model Monitor, Clarify, CloudWatch
6. **Security** - IAM roles, VPC configs, encryption

---

## Cheatsheet Files

- [Domain 1: Data Preparation for ML](./01-domain1-data-preparation.md)
- [Domain 2: ML Model Development](./02-domain2-model-development.md)
- [Domain 3: Deployment and Orchestration](./03-domain3-deployment-orchestration.md)
- [Domain 4: Monitoring, Maintenance, and Security](./04-domain4-monitoring-security.md)
