# Domain 6: Security and Compliance (17%)

## Task 6.1: IAM and Access Management ⭐

### IAM Policy Types

| Type | Description | Use Case |
|------|-------------|----------|
| **Identity-based** | Attached to users/roles | User permissions |
| **Resource-based** | Attached to resources | Cross-account access |
| **Permission Boundary** | Maximum permissions | Delegate administration |
| **Session Policy** | Temporary restrictions | Assumed roles |
| **SCP** | Organization guardrails | Account restrictions |

---

### IAM Policy Evaluation

1. Explicit Deny → Deny
2. SCP evaluation
3. Permission Boundary
4. Identity-based + Resource-based
5. Default → Implicit Deny

> **Exam Tip:** Explicit DENY always wins, regardless of any ALLOW!

---

### Cross-Account Access

| Method | Use Case |
|--------|----------|
| **AssumeRole** | Temporary credentials |
| **Resource Policy** | S3, KMS, Lambda |
| **AWS RAM** | Resource sharing |
| **Organizations** | Trusted access |

---

### AWS IAM Identity Center (SSO)

| Feature | Description |
|---------|-------------|
| **Identity Source** | AD, external IdP |
| **Permission Sets** | Managed policies |
| **Account Assignments** | Multi-account access |
| **ABAC** | Attribute-based access |

---

## Task 6.2: Secrets and Encryption

### AWS Secrets Manager ⭐

| Feature | Description |
|---------|-------------|
| **Secret Storage** | Encrypted credentials |
| **Automatic Rotation** | Lambda-based rotation |
| **RDS Integration** | Database credential rotation |
| **Cross-account** | Resource policy sharing |
| **Versioning** | Secret versions |

---

### AWS KMS ⭐

| Key Type | Description |
|----------|-------------|
| **AWS Managed** | AWS creates and manages |
| **Customer Managed** | Customer controls |
| **Customer-owned** | Own keys in CloudHSM |
| **AWS Owned** | Shared across accounts |

#### Key Policy vs IAM Policy
| Aspect | Key Policy | IAM Policy |
|--------|-----------|------------|
| **Scope** | Single key | All keys in account |
| **Required** | Yes (always) | Optional |
| **Cross-account** | Must allow | Can reference |

---

### Encryption Patterns

| Pattern | Description |
|---------|-------------|
| **Envelope Encryption** | Data key + CMK |
| **Multi-region Keys** | Same key across regions |
| **Key Rotation** | Automatic yearly |
| **Bring Your Own Key** | Import external keys |

---

## Task 6.3: Security Services

### Amazon GuardDuty

| Finding Type | Description |
|--------------|-------------|
| **Reconnaissance** | Port scanning, API discovery |
| **Instance Compromise** | Malware, Bitcoin mining |
| **Account Compromise** | Unusual API, credential exfiltration |
| **Bucket Compromise** | S3 data exfiltration |

---

### AWS Security Hub ⭐

| Feature | Description |
|---------|-------------|
| **Standards** | CIS, PCI DSS, NIST |
| **Findings** | Aggregated from services |
| **Insights** | Trend analysis |
| **Automations** | EventBridge integration |
| **Cross-account** | Organization aggregation |

---

### Amazon Inspector

| Assessment | Description |
|------------|-------------|
| **Network** | Open ports, connectivity |
| **Host** | CVE vulnerabilities |
| **Container** | ECR image scanning |
| **Lambda** | Function vulnerabilities |

---

### Amazon Macie

| Feature | Description |
|---------|-------------|
| **Sensitive Data** | PII, financial data |
| **S3 Discovery** | Bucket analysis |
| **Custom Identifiers** | Regex patterns |
| **Automation** | Scheduled jobs |

---

## Task 6.4: Network Security

### Security Groups vs NACLs

| Feature | Security Groups | NACLs |
|---------|-----------------|-------|
| **Level** | Instance | Subnet |
| **State** | Stateful | Stateless |
| **Rules** | Allow only | Allow and Deny |
| **Evaluation** | All rules | Order matters |
| **Default** | Deny all inbound | Allow all |

---

### AWS WAF

| Component | Description |
|-----------|-------------|
| **Web ACL** | Rule container |
| **Rules** | Match conditions |
| **Rule Groups** | Managed or custom |
| **Rate Limiting** | DDoS protection |
| **IP Sets** | Allow/block lists |

---

### AWS Network Firewall

| Feature | Description |
|---------|-------------|
| **Stateful Rules** | Protocol inspection |
| **Stateless Rules** | Packet filtering |
| **Domain Filtering** | Allow/block domains |
| **Suricata** | IDS/IPS rules |

---

## Task 6.5: Compliance and Governance

### AWS Organizations

| Feature | Description |
|---------|-------------|
| **SCPs** | Service Control Policies |
| **OUs** | Organizational Units |
| **Consolidated Billing** | Single payment |
| **Tag Policies** | Enforce tagging |
| **AI Services Opt-out** | Control AI data usage |

---

### AWS Control Tower

| Feature | Description |
|---------|-------------|
| **Landing Zone** | Multi-account setup |
| **Guardrails** | Preventive and detective |
| **Account Factory** | Provision new accounts |
| **Dashboard** | Compliance overview |

---

### AWS Config Rules

| Rule Type | Description |
|-----------|-------------|
| **AWS Managed** | Pre-built rules |
| **Custom (Lambda)** | Custom evaluation |
| **Conformance Packs** | Rule bundles |

#### Common Security Rules
- `encrypted-volumes`
- `s3-bucket-ssl-requests-only`
- `iam-password-policy`
- `root-account-mfa-enabled`
- `access-keys-rotated`

---

## Task 6.6: CI/CD Security

### Secure Pipeline Practices

| Practice | Implementation |
|----------|----------------|
| **Secrets** | Secrets Manager, Parameter Store |
| **Artifact Signing** | CodeSigner |
| **SAST** | CodeGuru, third-party |
| **DAST** | Inspector, third-party |
| **Container Scanning** | ECR, Inspector |
| **Least Privilege** | Pipeline IAM roles |

---

### Container Security

| Layer | Tool |
|-------|------|
| **Image Scanning** | ECR, Inspector |
| **Runtime** | GuardDuty EKS |
| **Network** | Security Groups, NACLs |
| **Secrets** | Secrets Manager, EKS Secrets |
| **Registry** | ECR with encryption |

---

## Exam Tips for Domain 6

1. **IAM evaluation order:**
   - Explicit Deny → SCPs → Permission Boundary → Policies
   - Implicit Deny by default
2. **Policy types:**
   - Identity-based = on principal
   - Resource-based = cross-account
   - SCP = organization guardrails
3. **Secrets Manager vs Parameter Store:**
   - Secrets Manager = auto-rotation, RDS
   - Parameter Store = config, free tier
4. **KMS key types:**
   - CMK = customer control
   - AWS managed = simpler
   - Multi-region = DR
5. **Security Hub:**
   - Aggregates findings
   - Compliance standards
   - Organization-wide
6. **GuardDuty:**
   - Threat detection
   - No agent needed
   - Machine learning based
7. **Cross-account access:**
   - AssumeRole = temporary
   - Resource policy = direct
   - RAM = service resources
8. **SCPs:**
   - Don't grant permissions
   - Restrict allowed actions
   - Deny always applies
9. **Container security:**
   - ECR scanning
   - GuardDuty for EKS
   - Secrets in Secrets Manager
10. **Network security:**
    - Security Groups = stateful
    - NACLs = stateless, ordered
    - WAF = Layer 7 protection
