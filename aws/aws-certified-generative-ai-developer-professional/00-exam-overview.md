# AWS Certified Generative AI Developer - Professional (AIP-C01) - Exam Overview

## Exam Information

| Attribute | Details |
|-----------|---------|
| **Exam Code** | AIP-C01 |
| **Level** | Professional |
| **Duration** | 180 minutes |
| **Questions** | 65 scored + 10 unscored (75 total) |
| **Passing Score** | 750 out of 1000 |
| **Format** | MC, MR, Ordering, Matching |
| **Experience** | 2+ years app dev, 1+ year GenAI |

> ⚠️ **Professional Level Exam** - Deep GenAI expertise required!

---

## Domain Weightings

| Domain | Content | Weight |
|--------|---------|--------|
| **Domain 1** | FM Integration, Data Management, Compliance | 31% |
| **Domain 2** | Implementation and Integration | 26% |
| **Domain 3** | AI Safety, Security, and Governance | 20% |
| **Domain 4** | Operational Efficiency and Optimization | 12% |
| **Domain 5** | Testing, Validation, and Troubleshooting | 11% |

---

## Target Candidate Profile

### Expected Experience
- 2+ years building production-grade applications on AWS
- General AI/ML or data engineering experience
- 1+ year hands-on experience implementing GenAI solutions

### Recommended AWS Knowledge
- AWS compute, storage, and networking services
- AWS security best practices and identity management
- AWS deployment and Infrastructure as Code (IaC) tools
- AWS monitoring and observability services
- AWS cost optimization principles

### Core Exam Tasks
1. Design solutions using **vector stores, RAG, knowledge bases**
2. **Integrate FMs** into applications and business workflows
3. Apply **prompt engineering** and management techniques
4. Implement **agentic AI** solutions
5. Optimize for **cost, performance, and business value**
6. Implement **security, governance, and Responsible AI**
7. **Troubleshoot, monitor, and optimize** GenAI applications
8. **Evaluate FMs** for quality and responsibility

### What is NOT Expected (Out of Scope)
- Model development and training
- Advanced ML techniques
- Data engineering and feature engineering

---

## In-Scope AWS Services (Quick Reference)

### Amazon Bedrock (Core) ⭐
| Feature | Primary Use |
|---------|-------------|
| **Foundation Models** ⭐ | Access to Claude, Titan, Llama, etc. |
| **Knowledge Bases** ⭐ | Managed RAG with vector stores |
| **Agents** ⭐ | Autonomous AI workflows |
| **Guardrails** ⭐ | Content filtering and safety |
| **Model Evaluation** | Compare FM performance |
| **Fine-tuning** | Customize FMs |
| **Prompt Management** | Version and manage prompts |
| **Batch Inference** | Cost-effective batch processing |

### Vector Stores
| Service | Primary Use |
|---------|-------------|
| **Amazon OpenSearch Service** ⭐ | Vector search, k-NN |
| **Amazon Aurora (pgvector)** | PostgreSQL vector extension |
| **Amazon Neptune** | Graph + vector |
| **Pinecone (via Bedrock)** | Managed vector database |
| **Redis (MemoryDB)** | In-memory vector search |

### AI/ML Services
| Service | Primary Use |
|---------|-------------|
| **Amazon SageMaker** | Model hosting, inference |
| **AWS Lambda** | Serverless inference |
| **Amazon Kendra** | Enterprise search |
| **Amazon Q** | Business AI assistant |
| **Amazon Lex** | Conversational AI |

### Developer Tools
| Service | Primary Use |
|---------|-------------|
| **AWS CDK** | Infrastructure as Code |
| **AWS SAM** | Serverless framework |
| **CodePipeline** | CI/CD for AI apps |
| **CloudFormation** | IaC templates |

### Monitoring & Observability
| Service | Primary Use |
|---------|-------------|
| **CloudWatch** | Metrics, logs, alarms |
| **X-Ray** | Distributed tracing |
| **CloudTrail** | API audit logging |

### Security
| Service | Primary Use |
|---------|-------------|
| **IAM** | Access management |
| **KMS** | Encryption keys |
| **Secrets Manager** | API keys, credentials |
| **VPC** | Network isolation |

---

## Study Tips

1. **Bedrock is Central (57%)** - Knowledge Bases, Agents, Guardrails, Prompt Flows
2. **RAG Architecture** - Vector stores, embeddings, hybrid search, reranker models
3. **Prompt Engineering** - Techniques, management, Prompt Flows, chaining
4. **Agents & Multi-Agent** - Strands Agents, AWS Agent Squad, MCP Protocol
5. **Responsible AI** - Guardrails, content filtering, contextual grounding
6. **Cost Optimization** - Token usage, caching, batch inference, model selection
7. **Security** - IAM, model access policies, VPC endpoints, data protection
8. **Model Customization** - LoRA, adapters, fine-tuning lifecycle
9. **CI/CD for GenAI** - Prompt testing, model evaluation, deployment pipelines
10. **Amazon Q** - Q Developer, Q Business for enterprise integration

---

## Cheatsheet Files

- [Domain 1: FM Integration, Data Management, Compliance](./01-domain1-fm-integration-data.md)
- [Domain 2: Implementation and Integration](./02-domain2-implementation.md)
- [Domain 3: AI Safety, Security, and Governance](./03-domain3-ai-safety-security.md)
- [Domain 4: Operational Efficiency and Optimization](./04-domain4-optimization.md)
- [Domain 5: Testing, Validation, and Troubleshooting](./05-domain5-testing-validation.md)
- [**Quick Reference Card (1-page summary)**](./06-quick-reference-card.md)
