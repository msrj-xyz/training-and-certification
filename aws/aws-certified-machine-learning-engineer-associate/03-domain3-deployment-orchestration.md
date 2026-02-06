# Domain 3: Deployment and Orchestration of ML Workflows (22%)

## Task 3.1: Select Deployment Infrastructure

### SageMaker Endpoint Types

| Endpoint Type | Description | Use Case |
|---------------|-------------|----------|
| **Real-time** | Persistent endpoint, low latency | Interactive applications |
| **Serverless** | Auto-scaling, pay-per-request | Variable/unpredictable traffic |
| **Asynchronous** | Queue-based, large payloads | Long-running inference |
| **Batch Transform** | Offline batch processing | Large datasets, no endpoint |

#### Real-time Endpoints
- Persistent instances (always-on)
- Low latency (<100ms typical)
- Manual or auto-scaling
- Choose instance type

#### Serverless Endpoints
- No instance management
- Pay per inference
- Cold start latency
- Memory configuration (1-6 GB)
- Max 15 minutes timeout

#### Asynchronous Endpoints
- S3 input/output
- Up to 1 hour timeout
- Auto-scaling to zero
- SNS notifications

#### Batch Transform
- No persistent endpoint
- Process entire dataset
- Cost-effective for large batches
- S3 input/output

> **Exam Tip:** Use Serverless for unpredictable traffic, Async for large payloads!

---

### Multi-Model and Multi-Container

| Deployment | Description | Use Case |
|------------|-------------|----------|
| **Multi-Model Endpoint** | Multiple models on one endpoint | Many similar models |
| **Multi-Container Endpoint** | Multiple containers in pipeline | Inference pipeline |
| **Serial Inference Pipeline** | Sequential container processing | Pre/post-processing |

---

### Deployment Targets

| Target | Description | Use Case |
|--------|-------------|----------|
| **SageMaker Endpoints** | Managed hosting | Standard ML inference |
| **AWS Lambda** | Serverless functions | Light models, low latency |
| **Amazon ECS** | Container orchestration | Custom containers |
| **Amazon EKS** | Kubernetes | Microservices architecture |
| **EC2** | Virtual machines | Full control |
| **Edge (SageMaker Neo)** | Edge devices | IoT, mobile |

---

### SageMaker Neo

| Feature | Description |
|---------|-------------|
| **Model Compilation** | Optimize for target hardware |
| **Hardware Targets** | CPU, GPU, edge devices |
| **Performance** | Up to 2x improvement |
| **Model Formats** | TensorFlow, PyTorch, MXNet, ONNX |
| **Inference Containers** | Pre-built optimized containers |

---

### Compute Resource Selection

| Instance Type | Description | Use Case |
|---------------|-------------|----------|
| **ml.m5** | General purpose | Balanced workloads |
| **ml.c5** | Compute optimized | CPU-intensive inference |
| **ml.p3/p4** | GPU (training) | Deep learning training |
| **ml.g4dn** | GPU (inference) | Cost-effective GPU inference |
| **ml.inf1** | Inferentia | High-throughput inference |
| **ml.trn1** | Trainium | Cost-effective training |

---

### Container Options

| Option | Description |
|--------|-------------|
| **Pre-built Containers** | AWS-maintained, framework-specific |
| **Script Mode** | Your code in AWS container |
| **BYOC** | Bring Your Own Container |
| **Extend** | Build on pre-built containers |

---

## Task 3.2: Create and Script Infrastructure

### Auto Scaling for SageMaker Endpoints

| Scaling Type | Description |
|--------------|-------------|
| **Target Tracking** | Maintain target metric value |
| **Step Scaling** | Scale based on CloudWatch alarms |
| **Scheduled Scaling** | Time-based scaling |

#### Scaling Metrics
- InvocationsPerInstance
- ModelLatency
- CPUUtilization
- MemoryUtilization
- GPUUtilization

---

### Infrastructure as Code

| Tool | Description | Use Case |
|------|-------------|----------|
| **CloudFormation** | YAML/JSON templates | Standard IaC |
| **AWS CDK** | Programmatic IaC | Python, TypeScript, Java |
| **SageMaker SDK** | Python SDK | ML-specific operations |

---

### Cost Optimization

| Strategy | Description |
|----------|-------------|
| **Spot Instances** | 70-90% savings for training |
| **Managed Spot Training** | SageMaker handles checkpoints |
| **Serverless Endpoints** | Pay-per-request pricing |
| **Multi-Model Endpoints** | Share resources |
| **Savings Plans** | Committed use discounts |
| **Auto-scaling to Zero** | Async endpoints only |

> **Exam Tip:** Managed Spot Training handles interruptions automatically!

---

### VPC Configuration

| Component | Description |
|-----------|-------------|
| **VPC Endpoints** | Private access to AWS services |
| **Private Subnets** | No internet access |
| **Security Groups** | Instance-level firewall |
| **NAT Gateway** | Outbound internet access |

---

## Task 3.3: CI/CD Pipelines

### AWS CI/CD Services

| Service | Role |
|---------|------|
| **CodeCommit** | Git repository |
| **CodeBuild** | Build and test |
| **CodeDeploy** | Deployment automation |
| **CodePipeline** | Pipeline orchestration |

---

### SageMaker Pipelines

| Component | Description |
|-----------|-------------|
| **Steps** | Individual pipeline tasks |
| **Parameters** | Runtime configuration |
| **Conditions** | Conditional execution |
| **Callback Steps** | Human approval, external calls |
| **Model Registry** | Register approved models |

#### Pipeline Step Types
- ProcessingStep (data prep)
- TrainingStep (model training)
- TuningStep (hyperparameter tuning)
- CreateModelStep (create model)
- TransformStep (batch inference)
- ConditionStep (branching)
- CallbackStep (external calls)
- RegisterModelStep (registry)

---

### Deployment Strategies

| Strategy | Description | Risk |
|----------|-------------|------|
| **Blue/Green** | Parallel environments, swap | Low |
| **Canary** | Small % first, then full | Low |
| **Linear** | Gradual traffic shift | Low |
| **All-at-once** | Immediate full deployment | High |
| **Rolling** | Incremental updates | Medium |

---

### ML Workflow Orchestration

| Tool | Description |
|------|-------------|
| **SageMaker Pipelines** | Native SageMaker workflow |
| **AWS Step Functions** | General workflow orchestration |
| **Amazon MWAA** | Managed Apache Airflow |
| **Amazon EventBridge** | Event-driven triggers |

---

### Git Workflow Patterns

| Pattern | Description |
|---------|-------------|
| **Gitflow** | Feature, develop, release branches |
| **GitHub Flow** | Main + feature branches |
| **Trunk-based** | Direct to main with feature flags |

---

### CI/CD for ML

| Stage | Tasks |
|-------|-------|
| **Source** | Code commit, data versioning |
| **Build** | Data validation, feature engineering |
| **Train** | Model training, tuning |
| **Evaluate** | Metrics validation, bias check |
| **Register** | Model registry, approval |
| **Deploy** | Staging, production |
| **Monitor** | Performance tracking |

---

### EventBridge for ML

| Trigger | Use Case |
|---------|----------|
| **S3 Events** | New data triggers retraining |
| **Scheduled** | Regular retraining |
| **Model Monitor** | Drift triggers retraining |
| **Pipeline Events** | Workflow automation |

---

## Exam Tips for Domain 3

1. **Endpoint type selection:**
   - Real-time = consistent traffic, low latency
   - Serverless = variable/unpredictable traffic
   - Asynchronous = large payloads (>6MB), long processing
   - Batch Transform = large datasets, no endpoint needed
2. **Serverless endpoints:**
   - Cold start trade-off
   - 1-6 GB memory config
   - Max 15 min timeout
   - Pay per inference
3. **Async endpoints:**
   - Up to 1 hour timeout
   - Can scale to zero
   - S3 input/output
   - SNS notifications
4. **Cost optimization:**
   - Managed Spot Training = 70-90% savings
   - Checkpoints handle interruptions
   - Multi-Model Endpoints = share resources
5. **SageMaker Pipelines:**
   - Native ML workflow orchestration
   - Step types: Processing, Training, Tuning, Transform
   - Callback steps for human approval
6. **CI/CD for ML:**
   - CodePipeline = orchestration
   - CodeBuild = build and test
   - Model Registry = version control
7. **Deployment strategies:**
   - Blue/Green = safest, full rollback
   - Canary = gradual traffic shift
   - Shadow = parallel inference, no traffic
8. **Auto Scaling:**
   - InvocationsPerInstance = common metric
   - Target tracking = maintain target value
   - Scale on latency or errors
9. **Instance selection:**
   - ml.g4dn = cost-effective GPU inference
   - ml.inf1 = Inferentia, optimized inference
   - ml.trn1 = Trainium, optimized training
10. **SageMaker Neo:**
    - Model compilation for hardware
    - Up to 2x performance improvement
    - Edge deployment support

