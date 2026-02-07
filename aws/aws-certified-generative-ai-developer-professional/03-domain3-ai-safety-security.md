# Domain 3: AI Safety, Security, and Governance (20%)

## Task 3.1: Amazon Bedrock Guardrails ⭐

### Guardrails Components

| Component | Description |
|-----------|-------------|
| **Content Filters** | Block harmful content |
| **Denied Topics** | Prevent specific subjects |
| **Word Filters** | Block specific words/phrases |
| **PII Handling** | Detect and redact PII |
| **Contextual Grounding** | Reduce hallucinations |
| **App Integration** | Applied at API level |

---

### Content Filter Categories

| Category | Description | Threshold |
|----------|-------------|-----------|
| **Hate** | Discriminatory content | None/Low/Medium/High |
| **Insults** | Offensive language | None/Low/Medium/High |
| **Sexual** | Sexual content | None/Low/Medium/High |
| **Violence** | Violent content | None/Low/Medium/High |
| **Misconduct** | Illegal activities | None/Low/Medium/High |
| **Prompt Attacks** | Jailbreak attempts | Enable/Disable |

---

### PII Handling Options

| Action | Description |
|--------|-------------|
| **Block** | Reject request with PII |
| **Anonymize** | Replace with placeholder |
| **Mask** | Partial redaction |

#### PII Types Detected
- Names, addresses, phone numbers
- SSN, credit cards, bank accounts
- Email addresses, IP addresses
- Dates of birth, passport numbers

---

### Contextual Grounding

| Check | Description |
|-------|-------------|
| **Grounding** | Response based on provided context |
| **Relevance** | Response relevant to query |
| **Threshold** | Confidence score required |

> **Exam Tip:** Guardrails work on both input prompts AND output responses!

---

## Task 3.2: Responsible AI

### Responsible AI Principles

| Principle | Implementation |
|-----------|----------------|
| **Fairness** | Bias detection, diverse training |
| **Transparency** | Explainability, citations |
| **Privacy** | PII protection, data minimization |
| **Security** | Encryption, access control |
| **Accountability** | Audit trails, human oversight |
| **Safety** | Content filtering, guardrails |

---

### Bias Detection and Mitigation

| Type | Description | Mitigation |
|------|-------------|------------|
| **Selection Bias** | Unrepresentative data | Diverse data sources |
| **Measurement Bias** | Inconsistent labeling | Clear guidelines |
| **Algorithmic Bias** | Model amplification | Fairness constraints |
| **Confirmation Bias** | Human prejudice | Diverse reviewers |

---

### Human-in-the-Loop

| Pattern | Description |
|---------|-------------|
| **Review Queue** | Human reviews before action |
| **Escalation** | High-risk to humans |
| **Feedback Loop** | User corrections improve model |
| **Approval Workflow** | Critical decisions need approval |

---

### Defense-in-Depth Safety ⭐

| Layer | Tool | Purpose |
|-------|------|---------|
| **Pre-processing** | Amazon Comprehend | Filter input content |
| **Model-level** | Bedrock Guardrails | Content filtering |
| **Post-processing** | Lambda functions | Validate outputs |
| **API-level** | API Gateway | Response filtering |

#### Text-to-SQL Safety ⭐
| Control | Description |
|---------|-------------|
| **Schema Validation** | Validate SQL against schema |
| **Query Sanitization** | Prevent SQL injection |
| **Parameterized Queries** | Bind user inputs safely |
| **Deterministic Results** | Ensure consistent outputs |

> **Exam Tip:** Text-to-SQL requires extra validation layers - never trust FM-generated SQL directly!

---

### Advanced Threat Detection ⭐

| Threat | Mitigation |
|--------|------------|
| **Prompt Injection** | Input sanitization, content filters |
| **Jailbreak Attempts** | Guardrails prompt attack detection |
| **Data Exfiltration** | Output monitoring, PII filters |
| **Adversarial Inputs** | Safety classifiers, automated testing |

```python
# Prompt Injection Detection Pattern
def detect_injection(user_input):
    # Check for common injection patterns
    injection_patterns = [
        "ignore previous instructions",
        "disregard system prompt",
        "pretend you are"
    ]
    for pattern in injection_patterns:
        if pattern.lower() in user_input.lower():
            raise SecurityException("Potential injection detected")
```

---

## Task 3.3: Security

### IAM for Bedrock

| Permission | Description |
|------------|-------------|
| `bedrock:InvokeModel` | Call model inference |
| `bedrock:InvokeModelWithResponseStream` | Streaming inference |
| `bedrock:GetCustomModel` | Access custom models |
| `bedrock:CreateAgent` | Create agents |
| `bedrock:InvokeAgent` | Call agent |

#### Example IAM Policy
```json
{
  "Version": "2012-10-17",
  "Statement": [{
    "Effect": "Allow",
    "Action": [
      "bedrock:InvokeModel",
      "bedrock:InvokeModelWithResponseStream"
    ],
    "Resource": [
      "arn:aws:bedrock:*::foundation-model/anthropic.claude-3-sonnet*"
    ]
  }]
}
```

---

### Model Access Control

| Control | Description |
|---------|-------------|
| **Model Access** | Request access to FMs |
| **Custom Model Policies** | Share fine-tuned models |
| **Cross-Account** | Share models across accounts |
| **VPC Endpoints** | Private network access |

---

### Data Protection

| Layer | Implementation |
|-------|----------------|
| **At Rest** | KMS encryption |
| **In Transit** | TLS 1.2+ |
| **Tokenization** | Remove sensitive data |
| **Access Logging** | CloudTrail |

---

### VPC Configuration

| Component | Description |
|-----------|-------------|
| **VPC Endpoints** | Private Bedrock access |
| **PrivateLink** | No internet exposure |
| **Security Groups** | Network ACLs |
| **NAT Gateway** | Optional outbound |

---

## Task 3.4: Governance

### Model Governance

| Aspect | Implementation |
|--------|----------------|
| **Model Inventory** | Track all models in use |
| **Version Control** | Model and prompt versions |
| **Access Control** | Who can use which models |
| **Usage Tracking** | Monitor consumption |
| **Compliance** | Industry regulations |

---

### Audit and Logging

| Service | Data Captured |
|---------|---------------|
| **CloudTrail** | API calls, model invocations |
| **CloudWatch Logs** | Application logs |
| **Bedrock Logs** | Model I/O (optional) |
| **S3** | Long-term storage |

#### Bedrock Invocation Logging
```json
{
  "modelId": "anthropic.claude-3-sonnet",
  "inputTokenCount": 150,
  "outputTokenCount": 200,
  "timestamp": "2024-01-15T10:30:00Z"
}
```

---

### Compliance Considerations

| Regulation | Relevant Controls |
|------------|-------------------|
| **GDPR** | PII handling, data residency |
| **HIPAA** | PHI protection, encryption |
| **SOC 2** | Access control, monitoring |
| **PCI-DSS** | Card data protection |

---

### Model Cards & Documentation ⭐

| Component | Description |
|-----------|-------------|
| **SageMaker Model Cards** | Programmatic model documentation |
| **Intended Use** | Define appropriate use cases |
| **Limitations** | Document known issues |
| **Performance Metrics** | Record evaluation results |
| **Training Data** | Source and characteristics |

> **Exam Tip:** Model Cards provide transparency and support regulatory compliance!

---

### Data Lineage & Traceability ⭐

| Tool | Capability |
|------|------------|
| **AWS Glue Data Catalog** | Register data sources |
| **Metadata Tagging** | Source attribution in FM outputs |
| **CloudTrail** | API audit logging |
| **S3 Object Metadata** | Document timestamps, authorship |

#### Data Lineage Flow
```
Source Data → Glue Catalog → FM Processing → Output + Citations
     ↓                              ↓
 Metadata Tags                 Source Attribution
```

---

### Continuous Governance ⭐

| Control | Description |
|---------|-------------|
| **Bias Drift Monitoring** | Track fairness metrics over time |
| **Policy Violations** | Automated detection and alerts |
| **Token-level Redaction** | Sensitive data filtering |
| **AI Output Policy Filters** | Enforce content standards |

---

## Exam Tips for Domain 3

1. **Guardrails essentials:**
   - Apply to input AND output
   - Content filters = harm categories
   - Denied topics = custom blocking
   - PII = detect/mask/block
2. **Content filter thresholds:**
   - None → Low → Medium → High
   - Higher = more restrictive
   - Configure per category
3. **Contextual grounding:**
   - Reduces hallucinations
   - Checks if response matches context
   - Set confidence threshold
4. **PII handling:**
   - Block = reject request
   - Anonymize = replace
   - Multiple PII types supported
5. **Responsible AI:**
   - Fairness, transparency, privacy
   - Human oversight
   - Bias detection
6. **IAM best practices:**
   - Least privilege
   - Model-specific permissions
   - Resource ARN constraints
7. **Data protection:**
   - KMS for encryption
   - VPC endpoints for isolation
   - CloudTrail for audit
8. **VPC endpoints:**
   - Private Bedrock access
   - No internet required
   - PrivateLink integration
9. **Invocation logging:**
   - Optional but recommended
   - Captures I/O for audit
   - Store in S3
10. **Compliance mapping:**
    - GDPR = PII, data residency
    - HIPAA = encryption, access
    - Track all requirements
