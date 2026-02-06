# Domain 4: ML Solution Monitoring, Maintenance, and Security (24%)

## Task 4.1: Monitor Model Inference

### Types of Drift

| Drift Type | Description | Detection |
|------------|-------------|-----------|
| **Data Drift** | Input data distribution changes | Compare to baseline |
| **Model Drift** | Model performance degrades | Monitor metrics |
| **Concept Drift** | Target relationship changes | Monitor predictions |
| **Feature Drift** | Feature distributions shift | Statistical tests |

> **Exam Tip:** Data drift affects input; concept drift affects the target relationship!

---

### SageMaker Model Monitor

| Monitor Type | Description |
|--------------|-------------|
| **Data Quality** | Monitor input data distribution |
| **Model Quality** | Monitor model accuracy |
| **Bias Drift** | Monitor fairness metrics |
| **Feature Attribution** | Monitor feature importance |

#### Model Monitor Components
- **Baseline** - Establish expected behavior
- **Schedule** - Monitoring frequency
- **Constraints** - Acceptable thresholds
- **Statistics** - Data statistics
- **Violations** - Detected issues

#### Model Monitor Setup
1. Create baseline from training data
2. Configure data capture on endpoint
3. Create monitoring schedule
4. Define constraints and thresholds
5. Set up alerts for violations

---

### Data Capture

| Feature | Description |
|---------|-------------|
| **Input Capture** | Log inference inputs |
| **Output Capture** | Log inference outputs |
| **Sampling** | Capture percentage |
| **S3 Storage** | Captured data location |

---

### A/B Testing and Shadow Variants

| Testing Method | Description |
|----------------|-------------|
| **A/B Testing** | Split traffic between variants |
| **Shadow Variant** | Parallel inference, no traffic |
| **Canary Release** | Gradual traffic shift |
| **Production Variants** | Multiple model versions |

#### Traffic Shifting
- Control traffic percentage per variant
- Monitor latency and errors
- Gradual rollout or rollback
- CloudWatch metrics per variant

---

### SageMaker Clarify for Monitoring

| Capability | Description |
|------------|-------------|
| **Bias Drift** | Monitor fairness over time |
| **Explainability Drift** | Feature importance changes |
| **Continuous Monitoring** | Scheduled checks |
| **Alerts** | CloudWatch integration |

---

## Task 4.2: Monitor Infrastructure and Costs

### Key Performance Metrics

| Metric Category | Metrics |
|-----------------|---------|
| **Latency** | ModelLatency, OverheadLatency |
| **Throughput** | InvocationsPerInstance |
| **Availability** | Invocation4XXErrors, Invocation5XXErrors |
| **Utilization** | CPUUtilization, MemoryUtilization, GPUUtilization |

---

### CloudWatch for ML

| Component | Description |
|-----------|-------------|
| **Metrics** | SageMaker endpoint metrics |
| **Logs** | Training and inference logs |
| **Alarms** | Threshold-based alerts |
| **Dashboards** | Custom visualizations |
| **Insights** | Log query analysis |
| **Lambda Insights** | Serverless monitoring |

#### Key CloudWatch Metrics
- Invocations
- InvocationsPerInstance
- ModelLatency
- OverheadLatency
- ModelSetupTime
- CPUUtilization
- MemoryUtilization

---

### AWS X-Ray

| Feature | Description |
|---------|-------------|
| **Distributed Tracing** | End-to-end request tracing |
| **Service Map** | Visual dependency map |
| **Latency Analysis** | Identify bottlenecks |
| **Error Analysis** | Root cause identification |

---

### CloudTrail for ML

| Feature | Description |
|---------|-------------|
| **API Logging** | All SageMaker API calls |
| **Audit Trail** | Who did what, when |
| **Compliance** | Regulatory requirements |
| **Retraining Triggers** | Based on API events |

---

### Cost Management

| Tool | Purpose |
|------|---------|
| **AWS Cost Explorer** | Analyze spending patterns |
| **AWS Budgets** | Set spending limits |
| **Billing Dashboard** | Current spending |
| **Trusted Advisor** | Cost optimization recommendations |
| **Resource Tagging** | Cost allocation |

---

### Instance Right-sizing

| Tool | Description |
|------|-------------|
| **SageMaker Inference Recommender** | Recommend instance types |
| **AWS Compute Optimizer** | EC2 right-sizing |
| **Load Testing** | Performance testing |

#### Inference Recommender
- Benchmarks model on different instances
- Recommends optimal configuration
- Considers latency and cost
- Default and custom benchmarks

---

### Cost Optimization Strategies

| Strategy | Description |
|----------|-------------|
| **Spot Instances** | Training cost reduction |
| **Savings Plans** | Committed use discounts |
| **Right-sizing** | Optimal instance selection |
| **Serverless** | Pay per inference |
| **Multi-Model Endpoints** | Consolidate resources |
| **Resource Tagging** | Track and allocate costs |
| **Auto Scaling** | Scale with demand |

---

## Task 4.3: Secure AWS Resources

### IAM for ML

| Component | Description |
|-----------|-------------|
| **Execution Role** | SageMaker job permissions |
| **User Policies** | Developer access |
| **Resource Policies** | S3 bucket policies |
| **Service Control Policies** | Organization guardrails |

#### Common SageMaker Permissions
- sagemaker:CreateTrainingJob
- sagemaker:CreateEndpoint
- sagemaker:CreateModel
- sagemaker:InvokeEndpoint
- s3:GetObject, s3:PutObject
- logs:CreateLogGroup

---

### SageMaker Role Manager

| Feature | Description |
|---------|-------------|
| **Pre-built Personas** | Data scientist, ML admin, etc. |
| **Custom Roles** | Tailored permissions |
| **Least Privilege** | Minimal required access |
| **Audit Support** | Role documentation |

---

### Network Security

| Component | Description |
|-----------|-------------|
| **VPC** | Isolated network |
| **Private Subnets** | No public internet |
| **Security Groups** | Instance-level firewall |
| **VPC Endpoints** | Private AWS service access |
| **PrivateLink** | Private endpoint connectivity |

#### VPC Endpoint Types
- **Interface Endpoints** - SageMaker API, ECR, CloudWatch
- **Gateway Endpoints** - S3, DynamoDB

---

### Encryption

| Type | Service |
|------|---------|
| **At Rest (S3)** | SSE-S3, SSE-KMS |
| **At Rest (EBS)** | KMS encryption |
| **In Transit** | TLS/SSL |
| **Model Artifacts** | KMS encryption |
| **Notebook Volumes** | KMS encryption |

---

### Data Protection

| Service | Purpose |
|---------|---------|
| **AWS KMS** | Key management |
| **AWS Secrets Manager** | API keys, credentials |
| **Amazon Macie** | S3 sensitive data discovery |
| **VPC Flow Logs** | Network traffic logging |

---

### Compliance and Auditing

| Tool | Purpose |
|------|---------|
| **CloudTrail** | API audit trail |
| **AWS Config** | Configuration compliance |
| **Security Hub** | Security posture |
| **GuardDuty** | Threat detection |
| **IAM Access Analyzer** | Permission analysis |

---

### CI/CD Security Best Practices

| Practice | Description |
|----------|-------------|
| **Secrets Management** | Use Secrets Manager |
| **Least Privilege** | Minimal IAM permissions |
| **Code Scanning** | Security vulnerability checks |
| **Artifact Signing** | Verify model integrity |
| **Network Isolation** | VPC for build environments |
| **Audit Logging** | Log all pipeline actions |

---

### SageMaker Security Features

| Feature | Description |
|---------|-------------|
| **Inter-Container Encryption** | Encrypt distributed training |
| **Network Isolation** | Disable internet access |
| **Root Access** | Disable for notebooks |
| **Lifecycle Configurations** | Secure notebook setup |
| **VPC Mode** | Run in private VPC |

---

## Exam Tips for Domain 4

1. **Model Monitor types:**
   - Data Quality = input distribution drift
   - Model Quality = accuracy degradation
   - Bias Drift = fairness over time
   - Feature Attribution = importance changes
2. **Drift types:**
   - Data drift = input distribution changes
   - Concept drift = target relationship changes
   - Know the difference!
3. **SageMaker Inference Recommender:**
   - Benchmarks on different instances
   - Recommends optimal config
   - Balances latency vs cost
4. **Cost optimization:**
   - Spot instances = 70-90% training savings
   - Savings Plans = committed use
   - Right-sizing with Inference Recommender
   - Resource tagging for allocation
5. **IAM for SageMaker:**
   - Execution Role = job permissions to AWS
   - sagemaker:InvokeEndpoint = inference permission
   - Least privilege principle
   - SageMaker Role Manager = pre-built personas
6. **VPC configuration:**
   - VPC Endpoints = private API access
   - Gateway Endpoints = S3, DynamoDB (free)
   - Interface Endpoints = other services
7. **Encryption:**
   - KMS for model artifacts
   - KMS for EBS volumes
   - TLS for in-transit
   - Inter-container encryption for distributed
8. **Audit and compliance:**
   - CloudTrail = all SageMaker API calls
   - AWS Config = configuration compliance
   - Security Hub = aggregated findings
9. **Data Capture:**
   - Input/output logging
   - Sampling percentage config
   - S3 destination
10. **A/B Testing:**
    - Production variants = traffic split
    - Shadow variant = parallel, no traffic
    - Canary = gradual rollout

