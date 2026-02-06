# Domain 3: Deployment, Provisioning, and Automation (22%)

## Task 3.1: Provision and Maintain Cloud Resources

### AMI Management

| Concept | Description |
|---------|-------------|
| **Amazon Machine Image (AMI)** | Template for EC2 instances |
| **AMI Components** | Root volume, launch permissions, block device mapping |
| **AMI Types** | EBS-backed, Instance store-backed |
| **AMI Sharing** | Private, Public, Specific accounts |

#### EC2 Image Builder
| Feature | Description |
|---------|-------------|
| **Image Pipeline** | Automated image creation workflow |
| **Recipe** | Components to install and configure |
| **Components** | Build and test components |
| **Distribution** | Regions and accounts to share |

#### Image Builder Workflow
1. Create base image recipe
2. Add build components
3. Add test components
4. Create distribution settings
5. Create pipeline with schedule
6. Execute pipeline

---

### AWS CloudFormation

| Component | Description |
|-----------|-------------|
| **Template** | JSON/YAML definition of resources |
| **Stack** | Collection of AWS resources |
| **StackSet** | Stacks across accounts/regions |
| **Change Set** | Preview changes before execution |
| **Drift Detection** | Identify manual changes |

#### Template Sections
| Section | Description | Required |
|---------|-------------|----------|
| **AWSTemplateFormatVersion** | Template version | No |
| **Description** | Template description | No |
| **Parameters** | Input values at runtime | No |
| **Mappings** | Key-value lookups | No |
| **Conditions** | Conditional resource creation | No |
| **Resources** | AWS resources to create | **Yes** |
| **Outputs** | Return values | No |

#### Intrinsic Functions
| Function | Purpose |
|----------|---------|
| **!Ref** | Reference parameter or resource |
| **!GetAtt** | Get resource attribute |
| **!Sub** | String substitution |
| **!Join** | Join strings |
| **!If** | Conditional value |
| **!FindInMap** | Lookup in mappings |
| **!ImportValue** | Import exported value |

---

### CloudFormation StackSets

| Feature | Description |
|---------|-------------|
| **Administrator Account** | Creates and manages StackSets |
| **Target Accounts** | Accounts where stacks deploy |
| **Self-managed** | Use IAM roles you create |
| **Service-managed** | AWS Organizations integration |
| **Concurrent Accounts** | Max simultaneous deployments |
| **Failure Tolerance** | Max failures before rollback |

---

### AWS Cloud Development Kit (CDK)

| Concept | Description |
|---------|-------------|
| **Constructs** | Building blocks (L1, L2, L3) |
| **Stacks** | Deployable unit |
| **Apps** | Container for stacks |
| **Synthesis** | Generate CloudFormation templates |

#### CDK Commands
| Command | Purpose |
|---------|---------|
| `cdk init` | Initialize new project |
| `cdk synth` | Generate CloudFormation template |
| `cdk deploy` | Deploy stack |
| `cdk diff` | Show changes |
| `cdk destroy` | Delete stack |

---

### Deployment Issues and Troubleshooting

#### CloudFormation Common Errors
| Error | Cause | Solution |
|-------|-------|----------|
| **InsufficientCapacity** | No capacity in AZ | Try different AZ/instance type |
| **LimitExceeded** | Service limit reached | Request limit increase |
| **AccessDenied** | IAM permissions | Check IAM policies |
| **DependencyError** | Resource dependency issue | Check DependsOn |
| **UPDATE_ROLLBACK_FAILED** | Rollback cannot complete | Skip resources or manual fix |

#### Subnet Sizing Issues
| Problem | Solution |
|---------|----------|
| **CIDR too small** | Plan for growth, use larger CIDR |
| **IP exhaustion** | Add secondary CIDR, use larger subnets |
| **No available IPs** | Check ENI usage, release unused |

---

### AWS Resource Access Manager (RAM)

| Feature | Description |
|---------|-------------|
| **Purpose** | Share resources across accounts |
| **Sharable Resources** | Subnets, Transit Gateways, Route 53 Resolver rules, etc. |
| **Resource Shares** | Collection of shared resources |
| **Principals** | Accounts, OUs, Organization |

---

### Deployment Strategies

| Strategy | Description | Risk |
|----------|-------------|------|
| **All-at-once** | Deploy to all targets | High |
| **Rolling** | Deploy in batches | Medium |
| **Rolling with batch** | Deploy batch, test, continue | Medium |
| **Immutable** | New instances, swap | Low |
| **Blue/Green** | Parallel environment | Lowest |
| **Canary** | Small percentage first | Low |

---

### Third-Party Tools

#### Terraform
| Concept | Description |
|---------|-------------|
| **State** | Track resource state |
| **Providers** | AWS, Azure, GCP, etc. |
| **Modules** | Reusable configurations |
| **Plan** | Preview changes |
| **Apply** | Execute changes |

#### Git Integration
| Tool | Purpose |
|------|---------|
| **AWS CodeCommit** | Git repository hosting |
| **GitHub** | External Git hosting |
| **GitLab** | External Git hosting |

---

## Task 3.2: Automate Management of Resources

### AWS Systems Manager

| Feature | Description |
|---------|-------------|
| **Fleet Manager** | Manage instances at scale |
| **Session Manager** | Secure shell without SSH |
| **Run Command** | Execute commands remotely |
| **Patch Manager** | Automate patching |
| **State Manager** | Maintain instance configuration |
| **Automation** | Runbook automation |
| **Parameter Store** | Configuration data storage |
| **Inventory** | Collect instance metadata |
| **Maintenance Windows** | Schedule maintenance tasks |

---

### Systems Manager Components

#### Session Manager
| Feature | Benefit |
|---------|---------|
| **No SSH/RDP** | No inbound ports required |
| **Logging** | CloudWatch and S3 logging |
| **IAM control** | Fine-grained access |
| **Cross-platform** | Linux and Windows |

#### Run Command
| Feature | Description |
|---------|-------------|
| **Documents** | Predefined or custom scripts |
| **Rate Control** | Concurrent or error threshold |
| **Targets** | Tags, resource groups, manually |
| **Output** | S3, CloudWatch Logs |

#### Patch Manager
| Component | Description |
|-----------|-------------|
| **Patch Baseline** | Rules for patch approval |
| **Patch Group** | Instances to patch together |
| **Maintenance Window** | Schedule for patching |
| **Compliance** | Patch status reporting |

---

### Parameter Store vs Secrets Manager

| Feature | Parameter Store | Secrets Manager |
|---------|-----------------|-----------------|
| **Cost** | Free tier available | Per-secret charge |
| **Rotation** | Manual | Automatic |
| **Cross-account** | No | Yes |
| **Max size** | 8 KB (Advanced) | 64 KB |
| **Best for** | Configuration | Credentials |

---

### Event-Driven Automation

#### Amazon EventBridge Patterns
| Pattern | Example |
|---------|---------|
| **AWS Events** | EC2 state change |
| **Scheduled** | Cron or rate expression |
| **Custom Events** | Application events |
| **Partner Events** | SaaS partner events |

#### Lambda Triggers
| Service | Event Type |
|---------|------------|
| **S3** | Object created/deleted |
| **DynamoDB** | Stream records |
| **SNS** | Messages |
| **SQS** | Messages |
| **CloudWatch Events** | Events |
| **API Gateway** | HTTP requests |

---

### S3 Event Notifications

| Destination | Use Case |
|-------------|----------|
| **Lambda** | Data processing |
| **SNS** | Fan-out notifications |
| **SQS** | Queue for processing |
| **EventBridge** | Advanced routing |

#### Event Types
- s3:ObjectCreated:*
- s3:ObjectRemoved:*
- s3:ObjectRestore:*
- s3:Replication:*

---

### Automation Use Cases

| Scenario | Solution |
|----------|----------|
| **Patch instances** | Systems Manager Patch Manager |
| **Rotate secrets** | Secrets Manager + Lambda |
| **Process uploads** | S3 Event → Lambda |
| **Remediate findings** | EventBridge → Lambda |
| **Scheduled tasks** | EventBridge scheduled rules |
| **Instance bootstrap** | CloudFormation + cfn-init |

---

## Exam Tips for Domain 3

1. **EC2 Image Builder** - Automate AMI creation pipeline
2. **CloudFormation StackSets** - Multi-account/region deployments
3. **CDK** - Know it synthesizes to CloudFormation
4. **Systems Manager** - Core service for operations
5. **Session Manager** - No SSH keys, no inbound ports
6. **Patch Manager** - Patch baselines and maintenance windows
7. **Parameter Store** - Configuration; Secrets Manager - credentials
8. **EventBridge** - Central event routing for automation
