# AWS Security Specialty - Quick Reference Card

## Domain Weights
| Domain | Weight |
|--------|--------|
| D1 - Detection | 16% |
| D2 - Incident Response | 14% |
| D3 - Infrastructure Security | 18% |
| D4 - Identity & Access | 20% |
| D5 - Data Protection | 18% |
| D6 - Governance | 14% |

---

## Detection & Monitoring

| Need | Solution |
|------|----------|
| Threat detection | **GuardDuty** |
| Aggregated findings | **Security Hub** |
| S3 sensitive data | **Macie** |
| Vulnerability scan | **Inspector** |
| Investigation | **Detective** |
| API audit | **CloudTrail** |
| Security data lake | **Security Lake** |

---

## Incident Response

| Action | Solution |
|--------|----------|
| Isolate EC2 | Modify Security Group |
| Preserve evidence | EBS Snapshot |
| Automated remediation | EventBridge + Lambda |
| Investigation | Amazon Detective |
| Runbooks | SSM Automation |
| Credential compromise | Revoke sessions, rotate keys |

---

## Network Security

| Need | Solution |
|------|----------|
| Web protection | **WAF** |
| DDoS protection | **Shield** |
| VPC firewall | **Network Firewall** |
| Zero-trust access | **Verified Access** |
| Centralized rules | **Firewall Manager** |

---

## Authentication

| Need | Solution |
|------|----------|
| Workforce SSO | **IAM Identity Center** |
| App users | **Cognito User Pools** |
| Federated AWS access | **Cognito Identity Pools** |
| AD integration | **Directory Service** |
| On-prem IAM | **Roles Anywhere** |

---

## Authorization

| Concept | Key Point |
|---------|----------|
| Policy evaluation | Explicit Deny wins |
| Permission boundary | Max permissions envelope |
| Cross-account | Trust policy + External ID |
| ABAC | Tag-based access control |
| Find external access | **IAM Access Analyzer** |

---

## Encryption & Keys

| Need | Solution |
|------|----------|
| Key management | **KMS** |
| Dedicated HSM | **CloudHSM** |
| Secret rotation | **Secrets Manager** |
| Certificates | **ACM** |
| Private CA | **ACM Private CA** |

### KMS Key Types
| Type | Use Case |
|------|----------|
| AWS Managed | Default, AWS controls |
| Customer Managed | Custom policies, rotation |
| External Key Store | Full customer control |

### S3 Encryption
| Type | Key Management |
|------|----------------|
| SSE-S3 | AWS managed |
| SSE-KMS | Customer managed, audit |
| SSE-C | Customer provided |

---

## Governance

| Need | Solution |
|------|----------|
| Multi-account mgmt | **Organizations** |
| Landing zone | **Control Tower** |
| Permission guardrails | **SCPs** |
| Config compliance | **AWS Config** |
| Centralized firewall | **Firewall Manager** |
| Audit evidence | **Audit Manager** |

---

## Quick Policy Rules

| Rule | Meaning |
|------|---------|
| **Explicit Deny** | Always wins |
| **Key Policy** | Required for KMS access |
| **SCP** | Max permissions for account |
| **Permission Boundary** | Max for IAM entity |
| **External ID** | Prevents confused deputy |

---

## Critical Security Controls

| Control | Service |
|---------|---------|
| MFA | IAM, Identity Center, Cognito |
| Encryption at rest | KMS, SSE |
| Encryption in transit | TLS, VPN |
| Logging | CloudTrail, VPC Flow Logs |
| Least privilege | IAM policies |

---

## Exam Day Reminders

1. **GuardDuty** = detects, doesn't prevent
2. **Shield Standard** = free, L3/L4 DDoS
3. **WAF** = Layer 7, web app protection
4. **KMS Key Policy** = required for access
5. **Explicit Deny** = always wins
6. **Session Manager** = no SSH keys needed
7. **SSE-KMS** = audit trail via CloudTrail
8. **S3 Object Lock Compliance** = immutable
9. **SCPs** = don't grant, only restrict
10. **Detective** = root cause investigation
