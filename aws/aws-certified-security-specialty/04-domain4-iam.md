# Domain 4: Identity and Access Management (20%)

## Task 4.1: Design and Implement Authentication Strategies

### AWS Identity Services ⭐

| Service | Use Case |
|---------|----------|
| **IAM** | AWS users, roles, policies |
| **IAM Identity Center** | Workforce SSO |
| **Cognito User Pools** | App user authentication |
| **Cognito Identity Pools** | Federated access to AWS |
| **Directory Service** | Active Directory |

---

### IAM Identity Center (SSO) ⭐

| Feature | Description |
|---------|-------------|
| **Identity Source** | Built-in, AD, external IdP |
| **Permission Sets** | Pre-defined access |
| **Multi-Account** | Assign across organization |
| **SAML/OIDC** | Federation support |
| **MFA** | Enforced authentication |

---

### Amazon Cognito

| Component | Purpose |
|-----------|---------|
| **User Pools** | User directory, authentication |
| **Identity Pools** | Federated access to AWS resources |
| **Hosted UI** | Pre-built sign-in pages |
| **MFA** | SMS, TOTP |
| **Advanced Security** | Compromised credentials, risk-based |

#### Cognito User Pool Features
- Password policies
- MFA (SMS, TOTP)
- Email/phone verification
- Social identity federation
- SAML/OIDC federation
- Lambda triggers

---

### Multi-Factor Authentication ⭐

| MFA Type | Description |
|----------|-------------|
| **Virtual MFA** | TOTP apps |
| **Hardware MFA** | Physical tokens |
| **SMS MFA** | Text message (Cognito) |
| **FIDO2/WebAuthn** | Passkeys |
| **U2F** | Security keys |

---

### Temporary Credentials ⭐

| Mechanism | Use Case |
|-----------|----------|
| **AssumeRole** | Cross-account, service access |
| **AssumeRoleWithSAML** | SAML federation |
| **AssumeRoleWithWebIdentity** | OIDC/social federation |
| **GetSessionToken** | MFA-protected access |
| **GetFederationToken** | Temporary user credentials |

#### STS Best Practices
- Use short session duration
- Scope down with session policies
- Use external ID for cross-account
- Enable MFA for sensitive operations

---

### AWS Directory Service

| Type | Use Case |
|------|----------|
| **AWS Managed AD** | Full AD functionality |
| **AD Connector** | Proxy to on-prem AD |
| **Simple AD** | Basic AD features |

---

### S3 Presigned URLs ⭐

| Feature | Description |
|---------|-------------|
| **Purpose** | Temporary access to S3 objects |
| **Expiration** | Configurable (seconds to hours) |
| **Permissions** | GET or PUT |
| **Signature** | Based on creator's credentials |

> **Exam Tip:** Presigned URL permissions are limited by creator's permissions!

---

## Task 4.2: Design and Implement Authorization Strategies

### IAM Policy Types ⭐

| Policy Type | Attachment | Purpose |
|-------------|------------|---------|
| **Identity-based** | User, Group, Role | Define permissions |
| **Resource-based** | S3, KMS, SQS, etc. | Cross-account access |
| **Permission Boundary** | User, Role | Maximum permissions |
| **Session Policy** | AssumeRole | Further restrict session |
| **SCP** | OU, Account | Organization guardrails |

---

### Policy Evaluation Logic ⭐

```
1. Explicit Deny → DENY
2. SCP (if applicable) → Continue or DENY
3. Permission Boundary (if applicable) → Continue or DENY
4. Session Policy (if applicable) → Continue or DENY
5. Identity Policy → Continue or DENY
6. Resource Policy → ALLOW or DENY
7. Default → DENY
```

> **Exam Tip:** Explicit Deny ALWAYS wins!

---

### ABAC vs RBAC

| Approach | Description | Scalability |
|----------|-------------|-------------|
| **RBAC** | Role-based access | Permissions per role |
| **ABAC** | Attribute-based (tags) | Dynamic, scales better |

#### ABAC Example
```json
{
  "Condition": {
    "StringEquals": {
      "aws:ResourceTag/Project": "${aws:PrincipalTag/Project}"
    }
  }
}
```

---

### Permission Boundaries ⭐

| Feature | Description |
|---------|-------------|
| **Purpose** | Maximum permissions envelope |
| **Attachment** | IAM users and roles |
| **Effect** | Intersection with identity policy |
| **Use Case** | Delegated administration |

---

### Cross-Account Access ⭐

| Method | Use Case |
|--------|----------|
| **IAM Role Trust** | Assume role from another account |
| **Resource Policy** | Direct resource access |
| **AWS RAM** | Share resources |
| **Organizations** | Cross-account within org |

#### Trust Policy for Cross-Account
```json
{
  "Principal": {
    "AWS": "arn:aws:iam::123456789012:root"
  },
  "Condition": {
    "StringEquals": {
      "sts:ExternalId": "UniqueExternalId"
    }
  }
}
```

> **Exam Tip:** Use External ID to prevent confused deputy problem!

---

### IAM Roles Anywhere

| Feature | Description |
|---------|-------------|
| **Purpose** | IAM roles for on-premises workloads |
| **Authentication** | X.509 certificates |
| **Trust Anchor** | CA certificate (Private CA or external) |
| **Profile** | Role and session settings |

---

### Amazon Verified Permissions

| Feature | Description |
|---------|-------------|
| **Purpose** | Fine-grained application authorization |
| **Policy Language** | Cedar |
| **Schema** | Define entities and actions |
| **Integration** | Cognito, Lambda, API Gateway |

---

### IAM Analysis Tools ⭐

| Tool | Purpose |
|------|---------|
| **IAM Access Analyzer** | External access findings |
| **IAM Policy Simulator** | Test policy decisions |
| **Access Advisor** | Last accessed services |
| **Credential Report** | User credential status |

#### Access Analyzer Features
- External access findings
- Policy validation
- Policy generation
- Unused access analysis
- Cross-account and cross-org findings

---

### Troubleshooting Authorization

| Issue | Tool/Method |
|-------|-------------|
| **Access Denied** | Policy Simulator, CloudTrail |
| **Unexpected Access** | Access Analyzer |
| **Permission Creep** | Access Advisor |
| **Policy Errors** | Access Analyzer validation |

---

## Exam Tips for Domain 4

1. **Explicit Deny** always wins in policy evaluation
2. **Permission Boundary** = max permissions envelope
3. **External ID** = prevent confused deputy
4. **ABAC** = tag-based, scales better than RBAC
5. **IAM Identity Center** = centralized SSO
6. **Cognito User Pools** = app authentication
7. **Cognito Identity Pools** = federated AWS access
8. **Access Analyzer** = find external access
9. **Presigned URLs** = temporary S3 access
10. **Roles Anywhere** = IAM for on-premises
