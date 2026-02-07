# Domain 5: Data Protection (18%)

## Task 5.1: Controls for Data in Transit

### TLS/SSL Enforcement ⭐

| Service | TLS Configuration |
|---------|-------------------|
| **ALB** | Security policies (TLS 1.2+) |
| **CloudFront** | Origin protocol, viewer policy |
| **API Gateway** | TLS 1.2 minimum |
| **RDS** | require_ssl parameter |
| **S3** | aws:SecureTransport condition |

#### Enforce HTTPS on S3
```json
{
  "Condition": {
    "Bool": {
      "aws:SecureTransport": "false"
    }
  },
  "Effect": "Deny"
}
```

---

### Private Connectivity ⭐

| Service | Purpose |
|---------|---------|
| **VPC Endpoints** | Private AWS service access |
| **PrivateLink** | Private endpoint services |
| **Client VPN** | Remote user VPN |
| **Site-to-Site VPN** | Site connectivity |
| **Verified Access** | Zero-trust application access |

#### VPC Endpoint Types
| Type | Services | Cost |
|------|----------|------|
| **Gateway** | S3, DynamoDB | Free |
| **Interface** | Most AWS services | Per hour + data |

---

### Inter-Service Encryption

| Scenario | Solution |
|----------|----------|
| **EKS node-to-node** | Encryption in transit |
| **EMR** | In-transit encryption |
| **SageMaker** | Inter-container encryption |
| **Nitro Instances** | Hardware-level encryption |
| **ElastiCache** | In-transit encryption |

---

### Certificate Management ⭐

| Service | Purpose |
|---------|---------|
| **ACM** | Public SSL/TLS certificates |
| **ACM Private CA** | Private certificates |
| **Import** | Bring your own certificates |

#### ACM Features
- Auto-renewal
- Free public certificates
- Integration with ALB, CloudFront, API Gateway
- DNS or email validation

---

## Task 5.2: Controls for Data at Rest

### AWS KMS ⭐

| Concept | Description |
|---------|-------------|
| **CMK (KMS Key)** | Customer master key |
| **Data Key** | Encrypt actual data |
| **Envelope Encryption** | CMK encrypts data key |
| **Key Policy** | Access to key |
| **Grants** | Temporary key access |

#### KMS Key Types
| Type | Management | Rotation |
|------|------------|----------|
| **AWS Managed** | AWS | Automatic (annual) |
| **Customer Managed** | Customer | Configurable |
| **Customer Managed (Imported)** | Customer | Manual |
| **External Key Store** | CloudHSM/External | Manual |

---

### Key Policies vs IAM Policies ⭐

| Aspect | Key Policy | IAM Policy |
|--------|------------|------------|
| **Location** | On the key | On principal |
| **Required** | Yes (every key) | Optional |
| **Cross-account** | Required for external | Additional layer |

> **Exam Tip:** Key policy MUST allow access; IAM policy alone is not sufficient!

---

### KMS Grants

| Feature | Description |
|---------|-------------|
| **Purpose** | Temporary key access |
| **Programmatic** | Created via API |
| **No policy change** | Doesn't modify key policy |
| **Revocable** | Can be retired/revoked |

---

### AWS CloudHSM ⭐

| Feature | KMS | CloudHSM |
|---------|-----|----------|
| **Management** | AWS | Customer |
| **Tenancy** | Shared | Dedicated |
| **Compliance** | FIPS 140-2 L2 | FIPS 140-2 L3 |
| **Integration** | Native AWS | Custom apps |
| **Key Access** | AWS can help | Customer only |

---

### Encryption Options by Service

| Service | Server-Side | Client-Side |
|---------|-------------|-------------|
| **S3** | SSE-S3, SSE-KMS, SSE-C | SDK encryption |
| **EBS** | KMS encryption | N/A |
| **RDS** | KMS encryption | N/A |
| **DynamoDB** | AWS owned, KMS | DynamoDB Encryption Client |
| **EFS** | KMS encryption | N/A |

#### S3 Encryption Types ⭐
| Type | Key Management | Use Case |
|------|----------------|----------|
| **SSE-S3** | AWS managed | Default encryption |
| **SSE-KMS** | Customer managed | Audit, rotation |
| **SSE-C** | Customer provided | Full control |
| **CSE** | Client-side | End-to-end encryption |

---

### Data Integrity Protection

| Mechanism | AWS Service |
|-----------|-------------|
| **S3 Object Lock** | WORM (Write Once Read Many) |
| **S3 Glacier Vault Lock** | Compliance retention |
| **Versioning** | Object history |
| **Code Signing** | Lambda, ECR |
| **Checksums** | Data validation |

#### S3 Object Lock ⭐
| Mode | Description |
|------|-------------|
| **Governance** | Can be overridden with permission |
| **Compliance** | Cannot be overridden (even root) |
| **Legal Hold** | Indefinite retention |

---

### Backup and Recovery

| Service | Purpose |
|---------|---------|
| **AWS Backup** | Centralized backup |
| **Data Lifecycle Manager** | EBS snapshot automation |
| **S3 Replication** | Cross-region/account copy |
| **RDS Snapshots** | Database backup |
| **DataSync** | Secure data transfer |

---

## Task 5.3: Protect Confidential Data and Secrets

### AWS Secrets Manager ⭐

| Feature | Description |
|---------|-------------|
| **Automatic Rotation** | Lambda-based rotation |
| **RDS Integration** | Native rotation for databases |
| **Cross-Account** | Resource-based policy |
| **Versioning** | Secret versions |
| **Encryption** | KMS integration |

#### Secrets Manager vs Parameter Store
| Feature | Secrets Manager | Parameter Store |
|---------|-----------------|-----------------|
| **Rotation** | Built-in | Manual/Lambda |
| **Cost** | Per secret | Free tier |
| **Size** | 64 KB | 8 KB (standard) |
| **Database Integration** | Native | Manual |

---

### Data Masking and Protection

| Service | Capability |
|---------|------------|
| **CloudWatch Logs** | Data protection policies |
| **SNS** | Message data protection |
| **Macie** | PII discovery |
| **Glue DataBrew** | Data masking |

---

### Cross-Region Key Management

| Scenario | Solution |
|----------|----------|
| **Multi-region encryption** | KMS multi-region keys |
| **Cross-region replication** | Separate keys per region |
| **Disaster recovery** | Replicate key material |

#### Multi-Region KMS Keys
| Feature | Description |
|---------|-------------|
| **Primary Key** | Source key |
| **Replica Keys** | Copies in other regions |
| **Key ID** | Same across regions |
| **Failover** | Promote replica to primary |

---

### Imported Key Material

| Feature | Description |
|---------|-------------|
| **BYOK** | Bring Your Own Key |
| **Rotation** | Manual (must reimport) |
| **Deletion** | Set expiration |
| **Requirements** | 256-bit symmetric |

---

### External Key Store

| Feature | Description |
|---------|-------------|
| **Purpose** | Keys stored outside AWS |
| **Backing** | CloudHSM or external HSM |
| **Control** | Full customer control |
| **Compliance** | Data sovereignty |

---

## Exam Tips for Domain 5

1. **KMS Key Policy** = required, IAM alone not enough
2. **SSE-KMS** = audit trail, key rotation
3. **CloudHSM** = FIPS 140-2 L3, dedicated HSM
4. **Secrets Manager** = auto-rotation for databases
5. **S3 Object Lock Compliance** = even root can't delete
6. **Multi-region keys** = same key ID across regions
7. **Envelope encryption** = CMK encrypts data key
8. **aws:SecureTransport** = enforce HTTPS
9. **ACM** = free public certs, auto-renewal
10. **VPC Endpoints** = private service access
