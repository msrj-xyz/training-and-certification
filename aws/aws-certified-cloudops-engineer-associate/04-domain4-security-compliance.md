# Domain 4: Security and Compliance (16%)

## Task 4.1: Implement and Manage Security Tools and Policies

### AWS Identity and Access Management (IAM)

| Component | Description |
|-----------|-------------|
| **Users** | Individual identities |
| **Groups** | Collection of users |
| **Roles** | Temporary credentials for services/users |
| **Policies** | JSON documents defining permissions |
| **Identity Providers** | External identity federation |

---

### IAM Policy Types

| Type | Description | Use Case |
|------|-------------|----------|
| **Identity-based** | Attached to users/groups/roles | Grant permissions to principals |
| **Resource-based** | Attached to resources | Allow cross-account access |
| **Permission Boundary** | Maximum permissions limit | Delegate administration |
| **Service Control Policy (SCP)** | Organization-level limits | Account guardrails |
| **Session Policy** | Temporary credentials limit | Reduce role permissions |

---

### IAM Policy Structure

| Element | Description | Required |
|---------|-------------|----------|
| **Version** | Policy language version | Yes (use "2012-10-17") |
| **Statement** | Array of individual statements | Yes |
| **Effect** | Allow or Deny | Yes |
| **Action** | Service actions | Yes |
| **Resource** | AWS resources | Yes |
| **Condition** | When policy applies | No |

#### Common Conditions
| Key | Example |
|-----|---------|
| **aws:SourceIp** | Restrict by IP address |
| **aws:MultiFactorAuthPresent** | Require MFA |
| **aws:RequestedRegion** | Restrict regions |
| **aws:PrincipalOrgID** | Organization membership |
| **s3:x-amz-acl** | S3 ACL conditions |

---

### IAM Features

#### Password Policies
- Minimum length
- Character requirements
- Password expiration
- Prevent password reuse
- Allow users to change password

#### Multi-Factor Authentication (MFA)
| Type | Description |
|------|-------------|
| **Virtual MFA** | Authenticator apps |
| **Hardware MFA** | Physical device |
| **U2F Security Key** | FIDO-compliant device |

> **Exam Tip:** MFA can be required for sensitive actions using policy conditions!

---

### Federated Identity

| Method | Description |
|--------|-------------|
| **SAML 2.0** | Enterprise identity providers |
| **Web Identity** | Social providers (Google, Facebook) |
| **IAM Identity Center** | Centralized access management |
| **Custom Federation** | Custom identity broker |

---

### IAM Troubleshooting Tools

| Tool | Purpose |
|------|---------|
| **IAM Policy Simulator** | Test policies before deployment |
| **IAM Access Analyzer** | Identify unintended access |
| **AWS CloudTrail** | Audit API calls |
| **Credential Report** | User credential status |
| **Access Advisor** | Service access history |

---

### AWS Trusted Advisor

| Category | Checks |
|----------|--------|
| **Cost Optimization** | Unused resources, reservations |
| **Performance** | Overprovisioned resources |
| **Security** | Exposed access keys, MFA usage |
| **Fault Tolerance** | Backup, Multi-AZ |
| **Service Limits** | Approaching quota limits |

#### Security Checks
- S3 Bucket Permissions
- Security Groups - Unrestricted Access
- IAM Use
- MFA on Root Account
- Exposed Access Keys

---

### Multi-Account Strategies

| Service | Purpose |
|---------|---------|
| **AWS Organizations** | Account management and consolidation |
| **AWS Control Tower** | Landing zone setup |
| **Service Control Policies (SCPs)** | Permission guardrails |
| **AWS RAM** | Resource sharing |
| **Cross-Account Roles** | Access across accounts |

#### SCP Characteristics
- Do not grant permissions
- Define maximum available permissions
- Apply to all users in account (except management account)
- Inheritance from parent OUs

---

### Compliance Enforcement

| Method | Purpose |
|--------|---------|
| **AWS Config Rules** | Configuration compliance |
| **SCPs** | Restrict regions, services |
| **IAM Policies** | Permission boundaries |
| **AWS Control Tower** | Guardrails |
| **AWS Audit Manager** | Compliance evidence |

---

## Task 4.2: Protect Data and Infrastructure

### Data Classification

| Level | Description | Controls |
|-------|-------------|----------|
| **Public** | No impact if disclosed | Basic |
| **Internal** | Limited disclosure | Access controls |
| **Confidential** | Business sensitive | Encryption, audit |
| **Restricted** | Highly sensitive | Full controls, MFA |

---

### Encryption at Rest

| Service | Encryption Method |
|---------|-------------------|
| **S3** | SSE-S3, SSE-KMS, SSE-C, client-side |
| **EBS** | AWS managed key or CMK |
| **RDS** | AWS managed key or CMK |
| **DynamoDB** | AWS managed or CMK |
| **EFS** | AWS managed key or CMK |

#### AWS KMS Key Types
| Type | Description |
|------|-------------|
| **AWS Managed** | AWS creates and manages |
| **Customer Managed (CMK)** | You create and manage |
| **AWS Owned** | Shared across accounts |

#### KMS Operations
| Operation | Description |
|-----------|-------------|
| **Encrypt** | Encrypt data |
| **Decrypt** | Decrypt data |
| **GenerateDataKey** | Create data encryption key |
| **Re-encrypt** | Re-encrypt with new key |

> **Exam Tip:** Envelope encryption uses data keys for large data!

---

### Encryption in Transit

| Method | Use Case |
|--------|----------|
| **TLS/SSL** | HTTPS, database connections |
| **VPN** | Site-to-site connectivity |
| **AWS PrivateLink** | Private service access |

#### AWS Certificate Manager (ACM)
| Feature | Description |
|---------|-------------|
| **Public Certificates** | Free, auto-renewed |
| **Private Certificates** | Internal PKI |
| **Import** | Bring your own certificate |
| **Integration** | ELB, CloudFront, API Gateway |

---

### Secrets Management

| Service | Use Case |
|---------|----------|
| **AWS Secrets Manager** | Database credentials, API keys |
| **SSM Parameter Store** | Configuration, non-sensitive data |
| **AWS KMS** | Encryption keys |

#### Secrets Manager Features
- Automatic rotation
- Cross-account sharing
- Audit with CloudTrail
- Integration with RDS, Redshift, DocumentDB

---

### Security Services

| Service | Purpose |
|---------|---------|
| **Amazon GuardDuty** | Threat detection |
| **AWS Security Hub** | Security posture dashboard |
| **Amazon Inspector** | Vulnerability scanning |
| **Amazon Macie** | S3 sensitive data discovery |
| **AWS Config** | Configuration compliance |

---

### Amazon GuardDuty

| Feature | Description |
|---------|-------------|
| **Findings** | Security issues detected |
| **Data Sources** | CloudTrail, VPC Flow Logs, DNS |
| **Threat Intelligence** | AWS and third-party feeds |
| **S3 Protection** | S3 data access threats |
| **EKS Protection** | Kubernetes audit logs |

---

### AWS Security Hub

| Feature | Description |
|---------|-------------|
| **Standards** | CIS, PCI DSS, AWS Foundational |
| **Aggregation** | Findings from multiple services |
| **Automation** | EventBridge integration |
| **Cross-account** | Organization-wide view |

---

### Amazon Inspector

| Feature | Description |
|---------|-------------|
| **EC2 Scanning** | Vulnerability assessment |
| **Container Scanning** | ECR image vulnerabilities |
| **Lambda Scanning** | Function code vulnerabilities |
| **Continuous** | Automatic rescanning |

---

### AWS Config

| Component | Description |
|-----------|-------------|
| **Configuration Items** | Resource state snapshot |
| **Configuration History** | Timeline of changes |
| **Rules** | Compliance evaluation |
| **Remediation** | Automatic fixes |
| **Aggregator** | Multi-account/region view |

#### Config Rule Types
| Type | Description |
|------|-------------|
| **AWS Managed** | Pre-built rules |
| **Custom** | Lambda-based rules |
| **Periodic** | Run on schedule |
| **Configuration Change** | Trigger on change |

---

## Exam Tips for Domain 4

1. **IAM policy evaluation order:**
   - Explicit Deny → Explicit Allow → Implicit Deny
   - Deny always wins
2. **Permission Boundaries:**
   - Maximum permissions, NOT grants
   - Used to delegate administration safely
3. **Service Control Policies (SCPs):**
   - Do NOT apply to management account
   - Do NOT grant permissions, only restrict
   - Inherited from parent OUs
4. **IAM Access Analyzer:**
   - Find unintended external access
   - Policy validation before deployment
5. **KMS key types:**
   - AWS Managed = AWS creates/manages
   - Customer Managed (CMK) = you control
   - AWS Owned = shared, invisible
6. **Envelope encryption:**
   - Data key encrypts data
   - Master key encrypts data key
   - GenerateDataKey API
7. **Secrets Manager benefits:**
   - Automatic rotation (Lambda-based)
   - Cross-account sharing
   - Native RDS/Redshift integration
8. **Security Hub:**
   - Aggregates findings from GuardDuty, Inspector, Macie
   - CIS, PCI DSS, AWS Foundational standards
   - EventBridge integration for automation
9. **AWS Config:**
   - Configuration compliance evaluation
   - Auto-remediation with SSM Automation
   - Aggregator for multi-account view
10. **GuardDuty data sources:**
    - CloudTrail, VPC Flow Logs, DNS logs
    - S3 protection, EKS audit logs
11. **ACM certificates:**
    - Free for AWS services (ELB, CloudFront, API Gateway)
    - Auto-renewal for AWS-issued certs
12. **MFA enforcement:**
    - Use aws:MultiFactorAuthPresent condition
    - Required for sensitive operations

