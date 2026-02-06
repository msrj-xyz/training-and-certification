# AWS Certified AI Practitioner (AIF-C01) - Exam Overview

## Exam Information

| Attribute | Details |
|-----------|---------|
| **Exam Code** | AIF-C01 |
| **Duration** | 90 minutes |
| **Questions** | 50 scored + 15 unscored (65 total) |
| **Passing Score** | 700 out of 1000 |
| **Format** | Multiple choice, Multiple response, Ordering, Matching |
| **Experience** | ~6 months exposure to AI/ML technologies on AWS |

---

## Domain Weightings

| Domain | Content | Weight |
|--------|---------|--------|
| **Domain 1** | Fundamentals of AI and ML | 20% |
| **Domain 2** | Fundamentals of GenAI | 24% |
| **Domain 3** | Applications of Foundation Models | 28% |
| **Domain 4** | Guidelines for Responsible AI | 14% |
| **Domain 5** | Security, Compliance, and Governance for AI Solutions | 14% |

> **Exam Tip:** Domain 3 (28%) has the highest weightage. Focus heavily on Foundation Models, RAG, and Prompt Engineering.

---

## Question Types Explained

1. **Multiple Choice** - One correct answer, three distractors
2. **Multiple Response** - Two or more correct answers from 5+ options; must select ALL correct
3. **Ordering** - Arrange 3-5 responses in correct sequence
4. **Matching** - Match responses to 3-7 prompts; all pairs must be correct

> **Important:** There is NO penalty for guessing. Always answer every question.

---

## Target Candidate Profile

### Expected Knowledge
- Familiarity with core AWS services (EC2, S3, Lambda, Bedrock, SageMaker AI)
- Understanding of AWS shared responsibility model
- Basic knowledge of IAM for access control
- Awareness of AWS service pricing models

### What is NOT Expected (Out of Scope)
- Developing/coding AI/ML models or algorithms
- Feature engineering or data engineering
- Hyperparameter tuning or model optimization
- Building and deploying ML pipelines
- Mathematical/statistical analysis of models
- Implementing security protocols for AI/ML systems
- Developing governance frameworks

---

## In-Scope AWS Services (Complete List)

> ⭐ = High-priority service (frequently tested)

### Machine Learning Services
| Service | Primary Use Case | Priority |
|---------|------------------|----------|
| **Amazon Bedrock** | Foundation Models, GenAI, Knowledge Bases, Agents, Guardrails | ⭐⭐⭐ |
| **Amazon SageMaker AI** | End-to-end ML platform, Clarify, Model Monitor | ⭐⭐⭐ |
| **Amazon Q Developer** | AI-powered coding assistance | ⭐⭐ |
| **Amazon Q Business** | AI assistant for enterprise knowledge | ⭐⭐ |
| **Amazon Comprehend** | NLP text analysis, sentiment, entities | ⭐⭐ |
| **Amazon Transcribe** | Speech to text | ⭐ |
| **Amazon Translate** | Language translation | ⭐ |
| **Amazon Polly** | Text to speech | ⭐ |
| **Amazon Lex** | Conversational AI (chatbots) | ⭐⭐ |
| **Amazon Rekognition** | Image and video analysis | ⭐⭐ |
| **Amazon Textract** | Document text extraction (OCR) | ⭐ |
| **Amazon Kendra** | Intelligent enterprise search | ⭐ |
| **Amazon Personalize** | Recommendation engine | ⭐ |
| **Amazon Fraud Detector** | Fraud detection | ⭐ |
| **Amazon Augmented AI (A2I)** | Human review workflows | ⭐⭐ |
| **Amazon Nova** | AWS foundation models in Bedrock | ⭐⭐ |

### Analytics Services
| Service | Primary Use Case | Priority |
|---------|------------------|----------|
| **AWS Glue** | ETL and data cataloging | ⭐⭐ |
| **AWS Glue DataBrew** | Visual data preparation | ⭐ |
| **Amazon OpenSearch Service** | Vector database, semantic search | ⭐⭐ |
| **AWS Lake Formation** | Data lake management | ⭐ |
| **Amazon Redshift** | Data warehousing | ⭐ |
| **Amazon QuickSight** | Business intelligence, visualization | ⭐ |
| **Amazon EMR** | Big data processing (Spark, Hadoop) | ⭐ |
| **AWS Data Exchange** | Data marketplace | - |

### Database Services (Vector Storage)
| Service | Primary Use Case | Priority |
|---------|------------------|----------|
| **Amazon RDS (PostgreSQL)** | Relational DB with pgvector | ⭐ |
| **Amazon Aurora** | Managed PostgreSQL with vector support | ⭐⭐ |
| **Amazon Neptune** | Graph database with vectors | ⭐ |
| **Amazon MemoryDB** | In-memory vector search | ⭐ |
| **Amazon DynamoDB** | NoSQL database | ⭐ |
| **Amazon ElastiCache** | Caching layer | - |
| **Amazon DocumentDB** | MongoDB-compatible | - |

### Security & Governance Services
| Service | Primary Use Case | Priority |
|---------|------------------|----------|
| **AWS IAM** | Access management (roles, policies) | ⭐⭐⭐ |
| **AWS KMS** | Key management, encryption | ⭐⭐ |
| **Amazon Macie** | Data security, PII detection | ⭐⭐ |
| **AWS CloudTrail** | API activity logging, auditing | ⭐⭐ |
| **Amazon CloudWatch** | Monitoring, logs, metrics | ⭐⭐ |
| **AWS Config** | Configuration compliance | ⭐ |
| **AWS Audit Manager** | Audit automation | ⭐ |
| **AWS Artifact** | Compliance documentation | ⭐ |
| **Amazon Inspector** | Vulnerability assessment | ⭐ |
| **AWS Secrets Manager** | Store API keys, secrets | ⭐ |
| **AWS Trusted Advisor** | Best practice recommendations | ⭐ |
| **AWS Well-Architected Tool** | Architecture review | - |

### Compute & Storage
| Service | Primary Use Case | Priority |
|---------|------------------|----------|
| **Amazon S3** | Object storage, training data | ⭐⭐ |
| **Amazon S3 Glacier** | Archive storage | - |
| **Amazon EC2** | Compute instances | ⭐ |
| **Amazon ECS/EKS** | Container orchestration | - |
| **Amazon VPC** | Network isolation | ⭐ |
| **Amazon CloudFront** | Content delivery | - |

---

## Study Tips

1. **Focus on Business Applications** - The exam tests practical business use, not deep technical implementations
2. **Know AWS Services** - Understand what each ML service does and when to use it
3. **Understand Trade-offs** - Cost vs. performance, accuracy vs. speed, customization vs. out-of-box
4. **Master GenAI Concepts** - Domains 2 & 3 together account for 52% of the exam
5. **Don't Skip Responsible AI** - Domain 4 & 5 together are 28% - significant!

---

## Cheatsheet Files

- [Domain 1: Fundamentals of AI and ML](./01-domain1-ai-ml-fundamentals.md)
- [Domain 2: Fundamentals of GenAI](./02-domain2-genai-fundamentals.md)
- [Domain 3: Applications of Foundation Models](./03-domain3-foundation-models.md)
- [Domain 4: Guidelines for Responsible AI](./04-domain4-responsible-ai.md)
- [Domain 5: Security, Compliance, and Governance](./05-domain5-security-compliance-governance.md)
- [**Quick Reference Card (1-page summary)**](./06-quick-reference-card.md)
