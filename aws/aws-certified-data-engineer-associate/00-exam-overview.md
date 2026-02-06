# AWS Certified Data Engineer - Associate (DEA-C01) - Exam Overview

## Exam Information

| Attribute | Details |
|-----------|---------|
| **Exam Code** | DEA-C01 |
| **Duration** | 130 minutes |
| **Questions** | 50 scored + 15 unscored (65 total) |
| **Passing Score** | 720 out of 1000 |
| **Format** | Multiple choice, Multiple response |
| **Experience** | 2-3 years data engineering, 1-2 years AWS |

---

## Domain Weightings

| Domain | Content | Weight |
|--------|---------|--------|
| **Domain 1** | Data Ingestion and Transformation | 34% |
| **Domain 2** | Data Store Management | 26% |
| **Domain 3** | Data Operations and Support | 22% |
| **Domain 4** | Data Security and Governance | 18% |

> **Exam Tip:** Domain 1 (Data Ingestion) has the highest weighting at 34% - master ETL concepts!

---

## Target Candidate Profile

### Expected Experience
- 2-3 years in data engineering roles
- 1-2 years hands-on experience with AWS services
- Understanding of volume, variety, and velocity (3 Vs of Big Data)

### Recommended IT Knowledge
- Setup and maintenance of ETL pipelines
- High-level programming concepts (language-agnostic)
- Git commands for source control
- Data lakes and data warehouse concepts
- Networking, storage, and compute fundamentals
- Vector concepts for modern data systems

### Recommended AWS Knowledge
- AWS data ingestion and transformation services
- Encryption, governance, and data protection
- Cost, performance, and functional service comparison
- SQL queries on AWS services
- Data quality and consistency verification

### What is NOT Expected (Out of Scope)
- Perform ML training and inferences
- Programming language-specific syntax
- Draw business conclusions based on data
- Complex algorithm implementation

---

## Core Exam Tasks

1. **Ingest and transform data**, orchestrate data pipelines with programming concepts
2. **Choose optimal data stores**, design data models, catalog schemas
3. **Operationalize and monitor** data pipelines, ensure data quality
4. **Implement security** - authentication, authorization, encryption, governance

---

## In-Scope AWS Services (Quick Reference)

### Analytics (Core) ⭐
| Service | Primary Use |
|---------|-------------|
| **Amazon Athena** ⭐ | Serverless SQL queries on S3 |
| **Amazon EMR** ⭐ | Big data processing (Spark, Hadoop) |
| **AWS Glue** ⭐ | ETL, Data Catalog, Crawlers |
| **AWS Glue DataBrew** | Visual data preparation |
| **AWS Lake Formation** ⭐ | Data lake management and security |
| **Amazon Kinesis Data Streams** ⭐ | Real-time streaming |
| **Amazon Kinesis Data Firehose** ⭐ | Streaming ETL to destinations |
| **Amazon Managed Flink** | Stream processing |
| **Amazon MSK** | Managed Kafka |
| **Amazon OpenSearch** | Search and analytics |
| **Amazon QuickSight** | BI and visualization |

### Database ⭐
| Service | Primary Use |
|---------|-------------|
| **Amazon Redshift** ⭐ | Data warehouse |
| **Amazon Aurora** | High-performance relational |
| **Amazon RDS** | Managed relational databases |
| **Amazon DynamoDB** ⭐ | NoSQL database |
| **Amazon DocumentDB** | MongoDB-compatible |
| **Amazon Neptune** | Graph database |
| **Amazon Keyspaces** | Cassandra-compatible |
| **Amazon MemoryDB** | Redis-compatible |

### Application Integration
| Service | Primary Use |
|---------|-------------|
| **Amazon AppFlow** | SaaS data integration |
| **Amazon EventBridge** | Event routing |
| **Amazon MWAA** | Managed Apache Airflow |
| **AWS Step Functions** | Workflow orchestration |
| **Amazon SNS/SQS** | Messaging services |

### Compute
| Service | Primary Use |
|---------|-------------|
| **AWS Lambda** | Serverless compute |
| **AWS Batch** | Batch processing |
| **Amazon EC2** | Virtual servers |
| **AWS SAM** | Serverless framework |

### Storage
| Service | Primary Use |
|---------|-------------|
| **Amazon S3** ⭐ | Object storage, data lake |
| **Amazon S3 Glacier** | Archive storage |
| **Amazon EBS** | Block storage |
| **Amazon EFS** | File storage |
| **AWS Backup** | Centralized backup |

### Migration & Transfer
| Service | Primary Use |
|---------|-------------|
| **AWS DMS** ⭐ | Database migration |
| **AWS DataSync** | Data transfer |
| **AWS Snow Family** | Offline data transfer |
| **AWS Transfer Family** | SFTP/FTPS to S3 |

### Developer Tools
| Service | Primary Use |
|---------|-------------|
| **AWS CloudFormation** | Infrastructure as Code |
| **AWS CDK** | Programmatic IaC |
| **AWS CodePipeline** | CI/CD pipelines |
| **AWS CodeBuild** | Build service |

### Security
| Service | Primary Use |
|---------|-------------|
| **AWS IAM** | Access management |
| **AWS KMS** | Key management |
| **AWS Secrets Manager** | Secrets storage |
| **Amazon Macie** | S3 data discovery |
| **AWS WAF** | Web application firewall |

### Monitoring
| Service | Primary Use |
|---------|-------------|
| **Amazon CloudWatch** | Metrics and logs |
| **AWS CloudTrail** | API audit logging |
| **AWS Config** | Configuration compliance |
| **Amazon Managed Grafana** | Visualization |

---

## Study Tips

1. **AWS Glue is Central** - Master ETL, Crawlers, Data Catalog, Job Bookmarks
2. **Data Ingestion (34%)** - Focus on Kinesis, Glue, EMR, Firehose
3. **Data Stores (26%)** - Redshift vs Athena vs EMR, when to use each
4. **Lake Formation** - Data lake security and governance
5. **Streaming** - Kinesis Data Streams vs Firehose vs MSK
6. **Security** - Encryption, IAM, Lake Formation permissions

---

## Cheatsheet Files

- [Domain 1: Data Ingestion and Transformation](./01-domain1-data-ingestion-transformation.md)
- [Domain 2: Data Store Management](./02-domain2-data-store-management.md)
- [Domain 3: Data Operations and Support](./03-domain3-data-operations-support.md)
- [Domain 4: Data Security and Governance](./04-domain4-security-governance.md)
- [**Quick Reference Card (1-page summary)**](./05-quick-reference-card.md)
