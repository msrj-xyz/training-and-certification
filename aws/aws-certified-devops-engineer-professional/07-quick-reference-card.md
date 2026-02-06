# AWS DevOps Engineer Professional (DOP-C02) - Quick Reference Card

## Exam at a Glance
| | |
|---|---|
| **Duration** | 180 min | **Questions** | 75 (65 scored) |
| **Passing** | 750/1000 | **Level** | Professional ⚠️ |

## Domain Weights
```
Domain 1: SDLC Automation           ████████████████     22%
Domain 2: Config & IaC              ████████████         17%
Domain 3: Resilient Solutions       ██████████           15%
Domain 4: Monitoring & Logging      ██████████           15%
Domain 5: Incident Response         █████████            14%
Domain 6: Security & Compliance     ████████████         17%
```

---

## 🔥 Core CI/CD Services

| Service | Purpose |
|---------|---------|
| **CodePipeline** ⭐ | CI/CD orchestration |
| **CodeBuild** ⭐ | Build and test |
| **CodeDeploy** ⭐ | Deployment automation |
| **CodeArtifact** | Package management |
| **ECR** | Container registry |

---

## 🚀 Deployment Strategies

| Strategy | Risk | Rollback |
|----------|------|----------|
| **All-at-once** | High | Manual |
| **Rolling** | Medium | Manual |
| **Blue/Green** | Low | Instant ⭐ |
| **Canary** | Low | Fast |
| **Linear** | Low | Fast |

---

## 🏗️ IaC Essentials

| CloudFormation | CDK |
|----------------|-----|
| **StackSets** = Multi-account | **L1** = Raw Cfn |
| **Change Sets** = Preview | **L2** = Defaults |
| **Drift Detection** = Manual changes | **L3** = Patterns |
| **Nested Stacks** = Modular | **cdk synth** = Generate |

---

## 📊 Monitoring Quick Ref

| CloudWatch | X-Ray |
|------------|-------|
| **Logs Insights** = Query logs | **Traces** = E2E requests |
| **Metric Filters** = Logs → Metrics | **Service Map** = Dependencies |
| **Composite Alarms** = Combine | **Sampling** = Cost control |

---

## 🔐 Security Essentials

| Topic | Remember |
|-------|----------|
| **IAM** | Explicit DENY always wins |
| **SCPs** | Restrict, don't grant |
| **KMS** | Key Policy always required |
| **Secrets Manager** | Auto-rotation for RDS |
| **Cross-account** | AssumeRole + Key Policy |

---

## 🛡️ Security Services

| Service | Purpose |
|---------|---------|
| **GuardDuty** | Threat detection |
| **Security Hub** | Aggregated findings |
| **Inspector** | Vulnerability scanning |
| **Config** | Compliance rules |
| **Macie** | PII detection |

---

## ⚡ DR Strategies (RTO/RPO)

| Strategy | RTO | Cost |
|----------|-----|------|
| **Backup/Restore** | Hours | $ |
| **Pilot Light** | Minutes | $$ |
| **Warm Standby** | Minutes | $$$ |
| **Multi-Site** | Near-zero | $$$$ |

---

## ✅ Exam Day Reminders

1. **CodePipeline cross-account** = AssumeRole + KMS
2. **CodeDeploy Blue/Green** = instant rollback
3. **CloudFormation StackSets** = Organization deployment
4. **SSM Automation** = Runbooks for remediation
5. **Secrets Manager** = auto-rotation (RDS)
6. **Parameter Store** = free, no rotation
7. **GuardDuty** = no agent, ML-based
8. **Security Hub** = aggregates all security findings
9. **Lifecycle Hooks** = custom actions at scaling
10. **EventBridge** = event-driven automation
