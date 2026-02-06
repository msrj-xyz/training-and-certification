# AWS AI Practitioner (AIF-C01) - Quick Reference Card

## Exam at a Glance
| | |
|---|---|
| **Duration** | 90 min | **Questions** | 65 (50 scored) |
| **Passing** | 700/1000 | **Format** | Multiple choice/response |

## Domain Weights
```
Domain 3: Foundation Models   ████████████████████ 28%  ← Highest!
Domain 2: GenAI Fundamentals  ██████████████████   24%
Domain 1: AI/ML Basics        ███████████████      20%
Domain 4: Responsible AI      ██████████           14%
Domain 5: Security/Compliance ██████████           14%
```

---

## 🔥 Most Tested Concepts

### AI/ML Hierarchy
```
AI → ML → Deep Learning → GenAI (Foundation Models → LLMs)
```

### Learning Types
| Type | Data | Use Case |
|------|------|----------|
| **Supervised** | Labeled | Classification, Regression |
| **Unsupervised** | Unlabeled | Clustering, Anomaly |
| **Reinforcement** | Rewards | Robotics, Games |

### Inference Types
| Real-time | Batch |
|-----------|-------|
| Milliseconds | Hours |
| Higher cost | Lower cost |
| Fraud detection | Reports |

---

## 🧠 GenAI Essentials

### Tokens
- 1 token ≈ 4 characters ≈ ¾ word
- Affects: cost, context limits, response length

### Temperature
| 0.0-0.3 | 0.8-1.0 |
|---------|---------|
| Factual, Code | Creative |
| Deterministic | Random |

### RAG Flow
```
Query → Embed → Vector Search → Retrieve → FM + Context → Response
```

### Hallucination Mitigation
**RAG** (Retrieval Augmented Generation) = Primary solution!

---

## 📊 Key Metrics

### Classification
| Metric | When to Use |
|--------|-------------|
| **Precision** | False positives costly (spam) |
| **Recall** | False negatives costly (disease) |
| **F1** | Imbalanced data |
| **Accuracy** | Balanced classes only |

### Text Generation
| ROUGE | Summarization |
| BLEU | Translation |
| Perplexity | Language quality |

---

## 🛡️ Responsible AI

### Bias Mitigation
- **Pre**: Re-sampling, Augmentation
- **In**: Fairness constraints
- **Post**: Threshold adjustment

### Guardrails Features
Content filters → Denied topics → PII handling → Grounding check

---

## 🔐 Security Quick Reference

### Shared Responsibility
| AWS | Customer |
|-----|----------|
| Infrastructure | Data, Access |
| Platform security | Guardrails config |
| Model hosting | Prompt engineering |

### Key Services
| Service | Purpose |
|---------|---------|
| **IAM** | Access control |
| **KMS** | Encryption |
| **Macie** | PII detection |
| **CloudTrail** | Audit logs |
| **Guardrails** | Content filtering |

---

## ⭐ Top 10 Services to Know

1. **Amazon Bedrock** - FMs, Knowledge Bases, Agents, Guardrails
2. **Amazon SageMaker** - Clarify, Model Monitor, Training
3. **Amazon Q** - Developer & Business AI assistants
4. **AWS IAM** - Access management
5. **Amazon Comprehend** - NLP, sentiment, entities
6. **Amazon Lex** - Chatbots
7. **Amazon Rekognition** - Image/video analysis
8. **Amazon A2I** - Human review workflows
9. **AWS Glue** - ETL, data preparation
10. **Amazon OpenSearch** - Vector database

---

## ✅ Exam Day Tips

1. **No penalty for guessing** - Answer everything!
2. **Domain 3 = 28%** - Focus on RAG, prompts, fine-tuning
3. **"When NOT to use AI"** - Common trap question
4. **Match services to use cases** - Know what each service does
5. **Responsible AI** - Know bias, fairness, explainability terms
