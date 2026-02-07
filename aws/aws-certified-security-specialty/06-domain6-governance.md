# Domain 6: Security Foundations and Governance (14%)

## Task 6.1: Centrally Deploy and Manage AWS Accounts

### AWS Organizations ⭐

| Component | Description |
|-----------|-------------|
| **Root** | Top-level container |
| **OUs** | Organizational Units |
| **Accounts** | Member accounts |
| **Management Account** | Billing, organization management |
| **Policies** | SCPs, Tag policies, etc. |

---

### Organization Policies ⭐

| Policy Type | Purpose |
|-------------|---------|
| **Service Control Policies (SCPs)** | Permission guardrails |
| **Resource Control Policies (RCPs)** | Resource-level controls |
| **Tag Policies** | Enforce tagging standards |
| **AI Services Opt-out** | Disable AI data sharing |
| **Backup Policies** | Centralized backup rules |

#### SCP Characteristics
- Do NOT grant permissions
- Define maximum permissions
- Apply to all principals in account
- Management account is exempt
- Deny always wins

---

### SCP Strategies ⭐

| Strategy | Description |
|----------|-------------|
| **Deny List** | Allow all, explicit deny |
| **Allow List** | Deny all, explicit allow |

#### Common SCP Use Cases
| SCP | Purpose |
|-----|---------|
| **Region Restriction** | Limit to specific regions |
| **Service Restriction** | Block unused services |
| **Prevent Root Actions** | Restrict root user |
| **Require Encryption** | Enforce KMS usage |
| **Prevent CloudTrail Disable** | Protect audit logs |

---

### AWS Control Tower ⭐

| Component | Description |
|-----------|-------------|
| **Landing Zone** | Multi-account environment |
| **Account Factory** | Automated provisioning |
| **Controls (Guardrails)** | Security and compliance |
| **Dashboard** | Compliance visibility |

#### Control Tower Control Types
| Type | Implementation | Behavior |
|------|----------------|----------|
| **Preventive** | SCP | Block actions |
| **Detective** | Config Rules | Alert on violations |
| **Proactive** | CloudFormation hooks | Check before deploy |

---

### Delegated Administrator

| Purpose | Services |
|---------|----------|
| **Centralized Management** | Security Hub, GuardDuty, Config |
| **Reduce Management Account Usage** | Run services from member account |
| **Access Control** | Org-wide visibility |

---

### Root User Management ⭐

| Best Practice | Implementation |
|---------------|----------------|
| **Strong Password** | Complex, unique |
| **Hardware MFA** | FIDO2, U2F |
| **Minimize Use** | Only for root-only tasks |
| **Centralized Root** | Control Tower root access |
| **Break-Glass** | Document emergency access |

#### Root-Only Tasks
- Change account settings
- Close account
- Change support plan
- Enable MFA Delete on S3
- Restore IAM permissions

---

## Task 6.2: Secure and Consistent Deployment

### Infrastructure as Code Security

| Tool | Purpose |
|------|---------|
| **CloudFormation Guard** | Policy-as-code validation |
| **cfn-lint** | Template linting |
| **CloudFormation StackSets** | Multi-account deployment |
| **Terraform Sentinel** | Policy enforcement |
| **AWS Config** | Drift detection |

---

### CloudFormation StackSets ⭐

| Feature | Description |
|---------|-------------|
| **Multi-Account** | Deploy across organization |
| **Multi-Region** | Deploy across regions |
| **Automatic Deployment** | New accounts |
| **Service-Managed** | Organizations integration |

---

### AWS Firewall Manager ⭐

| Resource | Management |
|----------|------------|
| **WAF Rules** | Centralized web protection |
| **Shield Advanced** | Centralized DDoS |
| **Security Groups** | Common rules |
| **Network Firewall** | Centralized VPC firewall |
| **Route 53 DNS Firewall** | DNS filtering |

---

### Resource Sharing

| Service | Purpose |
|---------|---------|
| **AWS RAM** | Share resources across accounts |
| **Service Catalog** | Approved products |
| **License Manager** | License management |

#### RAM Shareable Resources
- VPC Subnets
- Transit Gateway
- Route 53 Resolver Rules
- License Manager configurations
- AWS PrivateLink

---

### Tagging Strategy ⭐

| Tag Type | Purpose |
|----------|---------|
| **Cost Allocation** | CostCenter, Project |
| **Environment** | prod, dev, test |
| **Security** | DataClassification, Compliance |
| **Operations** | Owner, Contact |

#### Enforce Tags
| Method | Tool |
|--------|------|
| **Preventive** | SCPs, IAM policies |
| **Detective** | Tag policies, Config Rules |
| **Automated** | Lambda, EventBridge |

---

## Task 6.3: Evaluate Compliance of AWS Resources

### AWS Config ⭐

| Feature | Description |
|---------|-------------|
| **Configuration Recorder** | Track resource configs |
| **Rules** | Evaluate compliance |
| **Conformance Packs** | Rule bundles |
| **Aggregator** | Multi-account view |
| **Remediation** | Auto-fix via SSM |

#### Config Rule Types
| Type | Trigger |
|------|---------|
| **Change-triggered** | Resource change |
| **Periodic** | Scheduled interval |
| **Custom** | Lambda-based |

---

### Config Remediation

```
Non-compliant Resource → Config Rule → SSM Automation → Remediation
```

| Remediation Type | Description |
|------------------|-------------|
| **Manual** | Notification only |
| **Automatic** | SSM Automation document |
| **Retry** | Configurable retries |

---

### Security Hub Compliance ⭐

| Standard | Coverage |
|----------|----------|
| **AWS Foundational Security Best Practices** | 200+ controls |
| **CIS AWS Foundations** | CIS Benchmark |
| **PCI DSS** | Payment card industry |
| **NIST CSF** | Cybersecurity framework |

---

### AWS Audit Manager

| Feature | Description |
|---------|-------------|
| **Frameworks** | Pre-built compliance frameworks |
| **Assessments** | Collect audit evidence |
| **Controls** | Automated and manual |
| **Reports** | Audit-ready reports |

#### Supported Frameworks
- SOC 2
- PCI DSS
- HIPAA
- GDPR
- CIS Controls
- NIST

---

### AWS Artifact

| Feature | Description |
|---------|-------------|
| **Compliance Reports** | AWS audit reports |
| **Agreements** | BAA, NDA |
| **Access** | Download via console |

---

### Well-Architected Framework

| Pillar | Security Focus |
|--------|----------------|
| **Security** | IAM, detection, data protection |
| **Operational Excellence** | Logging, monitoring |
| **Reliability** | DR, recovery |
| **Performance** | Right-sizing |
| **Cost** | Optimization |
| **Sustainability** | Efficiency |

---

## Exam Tips for Domain 6

1. **SCPs** = guardrails, don't grant permissions
2. **Control Tower** = landing zone, guardrails
3. **Delegated Admin** = run security services from member account
4. **Config** = configuration compliance, remediation
5. **Firewall Manager** = centralized security policies
6. **RAM** = share resources across accounts
7. **StackSets** = multi-account IaC deployment
8. **Audit Manager** = collect compliance evidence
9. **Security Hub Standards** = CIS, AWS Foundational, PCI
10. **Tag Policies** = enforce tagging standards
