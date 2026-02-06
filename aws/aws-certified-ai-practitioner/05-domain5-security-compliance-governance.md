# Domain 5: Security, Compliance, and Governance for AI Solutions (14%)

## Task 5.1: Methods to Secure AI Systems

### AWS Security Services for AI

| Service | Purpose | AI-Specific Use |
|---------|---------|-----------------|
| **AWS IAM** | Identity and access management | Control who accesses AI resources |
| **AWS KMS** | Key management | Encrypt model data and artifacts |
| **Amazon Macie** | Data security and privacy | Detect PII in training data |
| **AWS PrivateLink** | Private connectivity | Access Bedrock without public internet |
| **AWS Secrets Manager** | Secrets management | Store API keys for AI services |
| **Amazon Inspector** | Vulnerability assessment | Scan AI workload infrastructure |

---

### AWS Shared Responsibility Model for AI

| AWS Responsibility | Customer Responsibility |
|--------------------|------------------------|
| Infrastructure security | Data protection and privacy |
| Physical security | Access management (IAM) |
| Network infrastructure | Application security |
| Service availability | Prompt/input validation |
| Foundation model hosting | Model selection for use case |
| Platform security | Content moderation decisions |

> **Exam Tip:** Customer is responsible for data, access, and how they use AI services!

#### Bedrock-Specific Shared Responsibility

| AWS Manages | Customer Manages |
|-------------|------------------|
| Model hosting infrastructure | Prompt engineering |
| API availability | Guardrails configuration |
| Network encryption | Data used with models |
| Service scaling | Knowledge base content |
| Model updates | Custom model training data |
| Regional availability | Compliance requirements |

---

### Security Considerations for AI Systems

| Consideration | Description | Best Practice |
|---------------|-------------|---------------|
| **Application Security** | Protect AI applications | Secure APIs, validate inputs |
| **Threat Detection** | Identify malicious activity | CloudWatch, GuardDuty monitoring |
| **Vulnerability Management** | Address security weaknesses | Regular scanning, patching |
| **Infrastructure Protection** | Secure underlying systems | VPC isolation, security groups |
| **Prompt Injection** | Malicious inputs in prompts | Input validation, guardrails |
| **Encryption at Rest** | Protect stored data | KMS encryption |
| **Encryption in Transit** | Protect data transfer | TLS/HTTPS |

---

### Prompt Injection Attacks

| Type | Description | Mitigation |
|------|-------------|------------|
| **Direct Injection** | Malicious instructions in user input | Input validation, guardrails |
| **Indirect Injection** | Hidden instructions in retrieved content | Content filtering, sanitization |
| **Jailbreaking** | Bypassing safety controls | Robust guardrails, model selection |
| **Goal Hijacking** | Redirecting model behavior | Clear system prompts, monitoring |

---

### Data Security Best Practices

| Practice | Description |
|----------|-------------|
| **Data Quality Assessment** | Verify data accuracy and completeness |
| **Privacy-Enhancing Technologies** | Anonymization, differential privacy |
| **Data Access Control** | Role-based access, least privilege |
| **Data Integrity** | Ensure data hasn't been tampered with |
| **Data Classification** | Categorize data sensitivity levels |
| **Data Encryption** | Protect data at rest and in transit |

#### Privacy-Enhancing Technologies (PETs)

| Technology | Description | Use Case |
|------------|-------------|----------|
| **Data Anonymization** | Remove identifying information | Training data privacy |
| **Differential Privacy** | Add noise to preserve privacy | Aggregate analysis |
| **Federated Learning** | Train on distributed data | Cross-organization ML |
| **Homomorphic Encryption** | Compute on encrypted data | Secure inference |
| **Synthetic Data** | Generate artificial data | Testing, training |
| **Data Masking** | Replace sensitive data | Development environments |

#### Encryption in AWS AI

| Type | Service | Key Management |
|------|---------|----------------|
| **At Rest** | S3, EBS, SageMaker | AWS KMS, CMK |
| **In Transit** | All services | TLS 1.2+ |
| **Model Artifacts** | SageMaker | KMS encryption |
| **Training Data** | S3 | Server-side encryption |
| **Bedrock Data** | Bedrock | AWS-managed keys |

---

### Source Citation and Data Lineage

| Concept | Description | AWS Tool |
|---------|-------------|----------|
| **Data Lineage** | Track data origin and transformations | AWS Glue Data Catalog |
| **Data Cataloging** | Organize and document data assets | AWS Glue, Lake Formation |
| **Model Cards** | Document model metadata and provenance | SageMaker Model Cards |
| **Source Attribution** | Credit data sources | RAG citations |

---

## Task 5.2: Governance and Compliance Regulations

### AWS Governance and Compliance Services

| Service | Purpose | AI-Specific Use |
|---------|---------|-----------------|
| **AWS Config** | Configuration compliance | Track AI resource configurations |
| **Amazon Inspector** | Vulnerability assessment | Scan AI infrastructure |
| **AWS Audit Manager** | Audit automation | Prepare for AI audits |
| **AWS Artifact** | Compliance documentation | Access AWS compliance reports |
| **AWS CloudTrail** | API activity logging | Track AI service usage |
| **AWS Trusted Advisor** | Best practice recommendations | Optimize AI deployments |

---

### Data Governance Strategies

| Strategy | Description | Implementation |
|----------|-------------|----------------|
| **Data Lifecycles** | Manage data from creation to deletion | Retention policies, archival |
| **Logging** | Record system activities | CloudWatch, CloudTrail |
| **Data Residency** | Control where data is stored | Region selection, data sovereignty |
| **Monitoring** | Track data access and usage | CloudWatch metrics |
| **Observation** | Insight into system behavior | CloudWatch Logs Insights |
| **Retention** | How long to keep data | Lifecycle policies |

---

### Data Lifecycle Management

| Stage | Description | AWS Service |
|-------|-------------|-------------|
| **Collection** | Gather data | S3, Kinesis |
| **Storage** | Store data securely | S3, RDS, DynamoDB |
| **Processing** | Transform and prepare | Glue, EMR |
| **Usage** | Train/inference | SageMaker, Bedrock |
| **Archival** | Long-term storage | S3 Glacier |
| **Deletion** | Secure removal | S3 lifecycle policies |

---

### Governance Protocols and Frameworks

| Protocol | Description |
|----------|-------------|
| **Policies** | Written rules for AI use |
| **Review Cadence** | Regular governance reviews |
| **Review Strategies** | Methods for evaluating compliance |
| **Governance Frameworks** | Structured approach to AI governance |
| **Transparency Standards** | Requirements for model transparency |
| **Team Training** | Educating teams on AI governance |

---

### Generative AI Security Scoping Matrix

The Generative AI Security Scoping Matrix helps organizations:
- Assess security requirements based on use case
- Identify appropriate controls
- Match security measures to risk level
- Ensure consistent security posture

---

### Compliance Considerations

| Consideration | Description |
|---------------|-------------|
| **Regulatory Requirements** | GDPR, HIPAA, PCI-DSS, SOC2 |
| **Industry Standards** | ISO 27001, NIST frameworks |
| **Data Privacy** | Personal data protection |
| **Model Documentation** | Required model information |
| **Audit Trails** | Evidence of compliance |
| **Third-Party Assessments** | External validation |

---

### AWS Compliance Programs Relevant to AI

| Program | Description | AI Relevance |
|---------|-------------|--------------|
| **SOC 1/2/3** | Security controls | AI service security |
| **ISO 27001** | Information security | AI data protection |
| **HIPAA** | Healthcare data | Healthcare AI applications |
| **PCI DSS** | Payment data | Financial AI applications |
| **GDPR** | EU data protection | European AI deployments |
| **FedRAMP** | US government | Government AI solutions |

---

### Monitoring and Logging for Compliance

| Service | What to Log |
|---------|-------------|
| **CloudTrail** | API calls to AI services |
| **CloudWatch** | Metrics, logs from AI workloads |
| **VPC Flow Logs** | Network traffic to/from AI resources |
| **S3 Access Logs** | Access to training/inference data |
| **Bedrock Logs** | Model invocations, inputs/outputs |

---

### Governance Best Practices

| Practice | Description |
|----------|-------------|
| **Clear Ownership** | Define who is responsible for AI governance |
| **Regular Reviews** | Periodic assessment of AI systems |
| **Documentation** | Maintain complete records |
| **Training** | Ensure team understands requirements |
| **Incident Response** | Plan for AI-related incidents |
| **Continuous Improvement** | Update policies based on learnings |

---

## Key AWS Services Summary

### Security Services

| Service | Quick Description |
|---------|-------------------|
| **IAM** | Who can do what |
| **KMS** | Encryption keys |
| **Macie** | Find sensitive data |
| **PrivateLink** | Private network access |
| **Inspector** | Find vulnerabilities |
| **Secrets Manager** | Store secrets |

### Governance Services

| Service | Quick Description |
|---------|-------------------|
| **Config** | Track configurations |
| **CloudTrail** | Audit API calls |
| **Audit Manager** | Prepare audits |
| **Artifact** | Compliance docs |
| **Trusted Advisor** | Best practices |

---

## Exam Tips for Domain 5

1. **Shared responsibility model:**
   - AWS = infrastructure, platform security
   - Customer = data, access, content moderation
   - Bedrock: You manage prompts, guardrails, training data
2. **Security services mapping:**
   - IAM = access control (who can do what)
   - KMS = encryption keys
   - Macie = detect PII in data
   - PrivateLink = private network access to Bedrock
   - Secrets Manager = store API keys
3. **Governance services:**
   - Config = track configurations
   - CloudTrail = audit API calls
   - Artifact = compliance documentation
   - Audit Manager = prepare for audits
4. **Prompt injection attacks:**
   - Direct = malicious user input
   - Indirect = hidden in retrieved content
   - Mitigation = input validation + Guardrails
5. **Data lifecycle stages:**
   - Collection → Storage → Processing → Usage → Archival → Deletion
6. **Privacy-Enhancing Technologies:**
   - Anonymization, differential privacy
   - Synthetic data, data masking
7. **Encryption:**
   - At rest = KMS
   - In transit = TLS 1.2+
8. **Logging for compliance:**
   - CloudTrail = all API calls
   - CloudWatch = metrics, logs
   - Bedrock logs = model invocations
9. **PrivateLink:** Keep Bedrock traffic off public internet
10. **Compliance programs:** SOC, ISO 27001, HIPAA, GDPR, PCI DSS

