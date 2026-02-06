# Domain 2: Configuration Management and IaC (17%)

## Task 2.1: AWS CloudFormation ⭐

### CloudFormation Components

| Component | Description |
|-----------|-------------|
| **Templates** | JSON/YAML infrastructure definitions |
| **Stacks** | Deployed template instances |
| **Stack Sets** | Multi-account/region deployments |
| **Change Sets** | Preview changes before deployment |
| **Nested Stacks** | Modular template design |
| **Drift Detection** | Identify manual changes |

---

### Template Sections

| Section | Description | Required |
|---------|-------------|----------|
| **AWSTemplateFormatVersion** | Template version | No |
| **Description** | Template description | No |
| **Parameters** | Input values | No |
| **Mappings** | Key-value lookups | No |
| **Conditions** | Conditional logic | No |
| **Resources** | AWS resources (only required) | **Yes** |
| **Outputs** | Return values | No |
| **Metadata** | Additional info | No |
| **Transform** | Macros (SAM, Include) | No |

---

### Intrinsic Functions

| Function | Purpose |
|----------|---------|
| `!Ref` | Reference parameters, resources |
| `!GetAtt` | Get resource attributes |
| `!Sub` | String substitution |
| `!Join` | Join strings |
| `!If` | Conditional values |
| `!Equals` | Equality comparison |
| `!FindInMap` | Lookup in mappings |
| `!ImportValue` | Cross-stack references |
| `!GetAZs` | Get availability zones |

---

### Stack Operations

| Operation | Description |
|-----------|-------------|
| **Create** | New stack from template |
| **Update** | Modify existing stack |
| **Delete** | Remove stack and resources |
| **Rollback** | Restore to previous state |
| **Drift Detection** | Check for manual changes |

#### Update Behaviors
| Behavior | Description |
|----------|-------------|
| **No Interruption** | Updates without disruption |
| **Some Interruption** | Brief service interruption |
| **Replacement** | Resource recreated (new physical ID) |

> **Exam Tip:** Know which updates cause replacement vs no interruption!

---

### CloudFormation StackSets ⭐

| Feature | Description |
|---------|-------------|
| **Multi-Account** | Deploy to Organization accounts |
| **Multi-Region** | Deploy across regions |
| **Self-Managed** | Manual target specification |
| **Service-Managed** | Automatic via Organizations |
| **Concurrent Deployments** | Parallel stack operations |
| **Failure Tolerance** | Continue despite failures |

---

### CloudFormation Advanced Features

| Feature | Description |
|---------|-------------|
| **Custom Resources** | Lambda for unsupported resources |
| **Macros** | Template transformation |
| **Dynamic References** | SSM, Secrets Manager |
| **Hooks** | Pre-create/update validation |
| **Registry** | Third-party resources |
| **Modules** | Reusable components |

#### Dynamic References
```yaml
# SSM Parameter
Password: '{{resolve:ssm-secure:my-password:1}}'

# Secrets Manager
Password: '{{resolve:secretsmanager:my-secret:SecretString:password}}'
```

---

## Task 2.2: AWS CDK ⭐

### CDK Concepts

| Concept | Description |
|---------|-------------|
| **Apps** | Root CDK construct |
| **Stacks** | Unit of deployment |
| **Constructs** | Cloud components |
| **L1** | CloudFormation resources (Cfn*) |
| **L2** | Higher-level constructs |
| **L3** | Patterns (opinionated) |

---

### CDK Commands

| Command | Purpose |
|---------|---------|
| `cdk init` | Initialize project |
| `cdk synth` | Generate CloudFormation |
| `cdk diff` | Preview changes |
| `cdk deploy` | Deploy stack |
| `cdk destroy` | Delete stack |
| `cdk bootstrap` | Prepare account/region |

---

## Task 2.3: AWS Systems Manager ⭐

### Systems Manager Capabilities

| Capability | Description |
|------------|-------------|
| **Run Command** | Execute commands remotely |
| **State Manager** | Maintain desired state |
| **Patch Manager** | Automate patching |
| **Automation** | Runbook execution |
| **Parameter Store** | Configuration storage |
| **Session Manager** | Secure shell access |
| **Inventory** | Track instance metadata |
| **Maintenance Windows** | Scheduled operations |

---

### SSM Parameter Store vs Secrets Manager

| Feature | Parameter Store | Secrets Manager |
|---------|-----------------|-----------------|
| **Cost** | Free (Standard) | Per secret |
| **Rotation** | Manual | Automatic |
| **Max Size** | 8 KB (Advanced) | 64 KB |
| **Cross-account** | No | Yes |
| **Versioning** | Yes | Yes |
| **Encryption** | Optional KMS | Always KMS |

> **Exam Tip:** Use Secrets Manager for database credentials (auto-rotation), Parameter Store for general config!

---

### SSM Documents

| Type | Purpose |
|------|---------|
| **Command** | Run Command, State Manager |
| **Automation** | Multi-step automation |
| **Session** | Session Manager config |
| **Policy** | Inventory, patch |
| **Package** | Distributor packages |

---

## Task 2.4: Configuration Management Tools

### AWS OpsWorks

| Type | Description |
|------|-------------|
| **OpsWorks Stacks** | Chef-based management |
| **OpsWorks for Chef Automate** | Managed Chef server |
| **OpsWorks for Puppet Enterprise** | Managed Puppet server |

---

### EC2 Image Builder

| Component | Description |
|-----------|-------------|
| **Recipes** | Build instructions |
| **Components** | Build/test actions |
| **Pipelines** | Automated workflows |
| **Distribution** | AMI distribution |
| **Infrastructure** | Build environment |

---

### AWS Proton

| Feature | Description |
|---------|-------------|
| **Templates** | Environment/service templates |
| **Environments** | Shared infrastructure |
| **Services** | Application deployments |
| **Components** | Service extensions |

---

## Task 2.5: Elastic Beanstalk

### Deployment Policies

| Policy | Description | Downtime |
|--------|-------------|----------|
| **All at Once** | Deploy to all | Yes |
| **Rolling** | Batch updates | Reduced |
| **Rolling with Batch** | Maintain capacity | No |
| **Immutable** | New instances | No |
| **Traffic Splitting** | Canary | No |
| **Blue/Green** | URL swap | No |

---

### Beanstalk Configuration

| File | Purpose |
|------|---------|
| `.ebextensions/*.config` | Environment customization |
| `.platform/hooks/` | Platform hooks |
| `Procfile` | Process commands |
| `env.yaml` | Environment manifest |

---

## Exam Tips for Domain 2

1. **CloudFormation essentials:**
   - Resources = only required section
   - Change Sets = preview before update
   - Drift Detection = find manual changes
2. **StackSets usage:**
   - Service-managed = Organizations OU
   - Self-managed = target accounts manually
   - Failure tolerance = concurrent deployments
3. **Intrinsic functions:**
   - !Ref = parameters and resource names
   - !GetAtt = resource attributes
   - !Sub = string substitution
   - !ImportValue = cross-stack
4. **Dynamic references:**
   - SSM secure parameters
   - Secrets Manager secrets
   - Real-time resolution
5. **CDK levels:**
   - L1 = raw CloudFormation (Cfn*)
   - L2 = sensible defaults
   - L3 = patterns (opinionated)
6. **Systems Manager:**
   - Run Command = ad-hoc execution
   - State Manager = desired state
   - Automation = runbooks
7. **Parameter Store vs Secrets Manager:**
   - Secrets Manager = rotation, RDS
   - Parameter Store = general config, free
8. **Custom Resources:**
   - Lambda-backed
   - Unsupported resources
   - Cleanup on delete
9. **Nested Stacks:**
   - Modular design
   - Reusable components
   - Parent-child relationship
10. **Beanstalk deployments:**
    - Traffic Splitting = canary
    - Immutable = safest
    - Blue/Green = URL swap
