# Domain 4: Data Security and Governance (18%)

## Task 4.1: Apply Authentication and Authorization

### AWS IAM for Data Services

| Component | Description |
|-----------|-------------|
| **IAM Users** | Individual identities |
| **IAM Roles** | Assumed by services |
| **IAM Policies** | Permission documents |
| **Service Roles** | Glue, EMR, Lambda roles |
| **Resource Policies** | S3, KMS, Glue policies |

---

### IAM Policy Types for Data

| Type | Use Case |
|------|----------|
| **Identity-based** | Attach to users/roles |
| **Resource-based** | S3 bucket policies |
| **Permission Boundary** | Maximum permissions |
| **Session Policy** | Assumed role limits |
| **SCP** | Organization guardrails |

---

### AWS Lake Formation Permissions ⭐

| Feature | Description |
|---------|-------------|
| **Fine-grained Access** | Table/column/row level |
| **LF-Tags** | Tag-based access control |
| **Data Filters** | Row and cell filtering |
| **Cross-account** | Secure data sharing |
| **Grant/Revoke** | Permission management |

#### Lake Formation vs IAM
| Aspect | Lake Formation | IAM |
|--------|---------------|-----|
| **Granularity** | Column/row level | Bucket/prefix level |
| **Management** | Centralized | Distributed |
| **Cross-account** | Built-in | Complex setup |
| **Data Lake** | Designed for | Generic |

> **Exam Tip:** Lake Formation simplifies data lake security - use it instead of S3 bucket policies!

---

### Amazon Redshift Security

| Feature | Description |
|---------|-------------|
| **Role-based Access** | Database roles |
| **Row-Level Security (RLS)** | Filter rows by user |
| **Dynamic Data Masking** | Hide sensitive columns |
| **Column-Level Access** | GRANT on columns |
| **Federated Users** | IAM integration |

---

### Amazon Athena Security

| Feature | Description |
|---------|-------------|
| **Workgroups** | Isolate users and queries |
| **Query Result Encryption** | SSE-S3, SSE-KMS |
| **Lake Formation** | Fine-grained access |
| **Cost Control** | Query limits per workgroup |

---

## Task 4.2: Apply Data Encryption

### Encryption at Rest

| Service | Options |
|---------|---------|
| **S3** | SSE-S3, SSE-KMS, SSE-C |
| **Redshift** | KMS, HSM |
| **DynamoDB** | AWS managed, CMK |
| **Glue** | KMS for Data Catalog |
| **EMR** | EBS, EMRFS, HDFS encryption |
| **Kinesis** | Server-side encryption |

#### S3 Encryption Types
| Type | Key Management |
|------|----------------|
| **SSE-S3** | AWS managed |
| **SSE-KMS** | Customer managed keys |
| **SSE-C** | Customer-provided keys |
| **Client-side** | Encrypt before upload |

---

### AWS KMS for Data

| Feature | Description |
|---------|-------------|
| **CMK** | Customer managed keys |
| **Key Policies** | Who can use the key |
| **Grants** | Temporary permissions |
| **Key Rotation** | Automatic yearly rotation |
| **Multi-region** | Cross-region key access |

---

### Encryption in Transit

| Service | Method |
|---------|--------|
| **S3** | HTTPS (TLS) |
| **Redshift** | SSL connections |
| **RDS** | SSL/TLS |
| **EMR** | TLS for in-transit |
| **Kinesis** | HTTPS endpoint |

---

## Task 4.3: Manage PII and Sensitive Data

### Amazon Macie

| Feature | Description |
|---------|-------------|
| **Sensitive Data Discovery** | PII, financial data |
| **S3 Bucket Analysis** | Automated scanning |
| **Custom Data Identifiers** | Regex patterns |
| **Findings** | Security Hub integration |
| **Job Scheduling** | Periodic scans |

---

### Data Masking Techniques

| Technique | Description | Use Case |
|-----------|-------------|----------|
| **Tokenization** | Replace with token | Credit cards |
| **Hashing** | One-way encryption | Passwords |
| **Anonymization** | Remove identifiers | Analytics |
| **Pseudonymization** | Replace with pseudonym | Reversible |
| **Redaction** | Remove completely | Logs |
| **Data Masking** | Partial visibility | Display |

---

### AWS Services for PII

| Service | Capability |
|---------|-----------|
| **Macie** | S3 scanning |
| **Comprehend** | NLP-based detection |
| **Glue DataBrew** | PII masking |
| **Lake Formation** | Cell-level filtering |
| **Redshift** | Dynamic data masking |

---

## Task 4.4: Implement Governance and Compliance

### AWS Glue Data Catalog Governance

| Feature | Description |
|---------|-------------|
| **Resource Policies** | Cross-account access |
| **Encryption** | KMS encryption |
| **Audit** | CloudTrail logging |
| **Schema Registry** | Schema versioning |

---

### Data Governance Best Practices

| Practice | Implementation |
|----------|---------------|
| **Data Lineage** | Track data flow |
| **Data Classification** | Sensitivity levels |
| **Access Control** | Least privilege |
| **Audit Logging** | CloudTrail, Config |
| **Retention** | Lifecycle policies |
| **Data Quality** | Glue Data Quality |

---

### AWS Config for Data

| Feature | Description |
|---------|-------------|
| **Rules** | Compliance evaluation |
| **Remediation** | Auto-fix violations |
| **Conformance Packs** | Rule bundles |
| **Aggregator** | Multi-account view |

#### Common Data Config Rules
- `s3-bucket-ssl-requests-only`
- `s3-bucket-server-side-encryption-enabled`
- `redshift-cluster-configuration-check`
- `dynamodb-table-encryption-enabled`

---

### Audit and Logging

| Service | Logs |
|---------|------|
| **CloudTrail** | API calls |
| **S3 Access Logs** | Bucket access |
| **Redshift Audit Logs** | Connection, query logs |
| **VPC Flow Logs** | Network traffic |
| **CloudWatch Logs** | Application logs |

---

### Data Residency and Sovereignty

| Requirement | Solution |
|-------------|----------|
| **Regional Storage** | S3 regional buckets |
| **Cross-region Replication** | Controlled replication |
| **Data Transfer** | PrivateLink, VPC endpoints |
| **Compliance** | AWS Artifact |

---

## Task 4.5: Enable Logging

### CloudWatch Logs for Data Services

| Service | Log Location |
|---------|--------------|
| **Glue** | /aws-glue/jobs/logs-v2 |
| **EMR** | /aws-emr-containers/ |
| **Lambda** | /aws/lambda/{function} |
| **Kinesis Firehose** | CloudWatch Logs |
| **Step Functions** | CloudWatch Logs |

---

### VPC Endpoints for Data

| Service | Endpoint Type |
|---------|---------------|
| **S3** | Gateway endpoint (free) |
| **DynamoDB** | Gateway endpoint (free) |
| **Glue** | Interface endpoint |
| **Kinesis** | Interface endpoint |
| **Athena** | Interface endpoint |

> **Exam Tip:** Gateway endpoints (S3, DynamoDB) are free - use them for data security!

---

## Exam Tips for Domain 4

1. **Lake Formation essentials:**
   - Fine-grained access (column/row/cell)
   - LF-Tags = tag-based permissions
   - Replaces complex S3 policies
   - Cross-account data sharing
2. **Encryption requirements:**
   - SSE-KMS = audit trail, key control
   - SSE-S3 = simpler, less control
   - Always enable encryption at rest
3. **Macie capabilities:**
   - PII detection in S3
   - Custom data identifiers (regex)
   - Automated, scheduled scanning
   - Security Hub integration
4. **IAM for data services:**
   - Service roles = Glue, EMR, Lambda
   - Resource policies = S3, Glue Catalog
   - Least privilege principle
5. **Redshift security:**
   - Row-level security (RLS)
   - Dynamic data masking
   - Column-level GRANT
6. **Data governance:**
   - Data lineage tracking
   - Classification levels
   - Retention policies
   - Audit logging
7. **AWS Config:**
   - Compliance rules for data
   - Auto-remediation
   - Multi-account aggregation
8. **CloudTrail:**
   - All API activity
   - Data events = S3 object access
   - Management events = control plane
9. **VPC endpoints:**
   - Gateway = S3, DynamoDB (free)
   - Interface = other services
   - Private data access
10. **PII handling:**
    - Macie = detection
    - DataBrew = masking
    - Lake Formation = filtering
    - Redshift = dynamic masking
