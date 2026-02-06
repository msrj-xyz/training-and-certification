# AWS ML Engineer Associate (MLA-C01) - Quick Reference Card

## Exam at a Glance
| | |
|---|---|
| **Duration** | 130 min | **Questions** | 65 (50 scored) |
| **Passing** | 720/1000 | **Format** | MC, MR, Ordering, Matching |

## Domain Weights
```
Domain 1: Data Preparation      ████████████████████   28%
Domain 2: Model Development     ██████████████████     26%
Domain 3: Deployment & CI/CD    ████████████████       22%
Domain 4: Monitoring & Security █████████████████      24%
```

---

## 🔥 Core SageMaker Services

| Service | Purpose |
|---------|---------|
| **Data Wrangler** | Visual data prep, 300+ transforms |
| **Feature Store** | Online (inference) / Offline (training) |
| **Ground Truth** | Data labeling, auto-labeling |
| **Clarify** | Bias detection + SHAP explainability |
| **Model Monitor** | Data/Model/Bias/Feature drift |
| **Pipelines** | ML workflow orchestration |
| **Model Registry** | Versioning + approval |
| **JumpStart** | Pre-trained models, foundation models |

---

## 📊 Built-in Algorithms

| Algorithm | Type | Use Case |
|-----------|------|----------|
| **XGBoost** | Classification/Regression | Structured data (DEFAULT) |
| **Linear Learner** | Classification/Regression | Linear relationships |
| **K-Means** | Clustering | Segmentation |
| **Random Cut Forest** | Anomaly | Fraud detection |
| **DeepAR** | Time Series | Forecasting |
| **BlazingText** | NLP | Text classification |
| **Image Classification** | CV | Image categorization |

---

## 🚀 Endpoint Types

| Type | Latency | Traffic | Timeout |
|------|---------|---------|---------|
| **Real-time** | Low | Consistent | N/A |
| **Serverless** | Cold start | Variable | 15 min |
| **Async** | Higher | Large payload | 1 hour |
| **Batch Transform** | Highest | Offline | N/A |

### Quick Selection
- Variable traffic → **Serverless**
- Large payloads (>6MB) → **Async**
- Offline processing → **Batch Transform**
- Low latency needed → **Real-time**

---

## 🔍 Model Monitor Types

| Monitor | What It Detects |
|---------|-----------------|
| **Data Quality** | Input distribution drift |
| **Model Quality** | Accuracy degradation |
| **Bias Drift** | Fairness changes |
| **Feature Attribution** | Importance changes |

### Drift Types
- **Data Drift** = input distribution changes
- **Concept Drift** = target relationship changes

---

## 💰 Cost Optimization

| Strategy | Savings |
|----------|---------|
| Managed Spot Training | 70-90% |
| Multi-Model Endpoints | Shared resources |
| Serverless Endpoints | Pay per inference |
| Async (scale to zero) | No idle cost |

---

## 🔐 Security Quick Reference

| Area | Service |
|------|---------|
| **Permissions** | IAM Execution Role |
| **Encryption** | KMS (rest), TLS (transit) |
| **Network** | VPC Endpoints |
| **Audit** | CloudTrail |
| **Secrets** | Secrets Manager |

---

## ✅ Exam Day Reminders

1. **XGBoost** = default for structured data
2. **Feature Store**: Online = inference, Offline = training
3. **Parquet** = preferred data format
4. **Bayesian** = most efficient hyperparameter tuning
5. **L1** = sparsity, **L2** = small weights
6. **Serverless endpoints** = cold start trade-off
7. **Model Monitor** = 4 types (Data, Model, Bias, Feature)
8. **Data drift ≠ Concept drift**
9. **VPC Endpoints** = private access to SageMaker
10. **Clarify** = bias AND explainability (SHAP)
