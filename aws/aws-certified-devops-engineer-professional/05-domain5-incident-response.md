# Domain 5: Incident and Event Response (14%)

## Task 5.1: Event-Driven Architecture

### Amazon EventBridge ⭐

| Component | Description |
|-----------|-------------|
| **Event Bus** | Event routing channel |
| **Rules** | Event matching patterns |
| **Targets** | Event destinations |
| **Schema Registry** | Event structure discovery |
| **Archive** | Event replay capability |
| **Pipes** | Point-to-point integration |

#### Event Bus Types
| Type | Description |
|------|-------------|
| **Default** | AWS service events |
| **Custom** | Application events |
| **Partner** | SaaS integrations |

---

### EventBridge Patterns

| Pattern | Description |
|---------|-------------|
| **Source** | Event source matching |
| **Detail-type** | Event type matching |
| **Detail** | Content-based filtering |

```json
{
  "source": ["aws.ec2"],
  "detail-type": ["EC2 Instance State-change Notification"],
  "detail": {
    "state": ["stopped", "terminated"]
  }
}
```

---

### Event-Driven Responses

| Trigger | Target | Use Case |
|---------|--------|----------|
| **EC2 State Change** | Lambda | Auto-remediation |
| **Config Rule** | SSM Automation | Compliance fix |
| **GuardDuty Finding** | SNS, Lambda | Security response |
| **CloudWatch Alarm** | SNS, Auto Scaling | Scaling |
| **CodePipeline State** | SNS, Lambda | Notification |

---

## Task 5.2: Automated Remediation

### Systems Manager Automation ⭐

| Feature | Description |
|---------|-------------|
| **Runbooks** | Automation documents |
| **Built-in Actions** | AWS API actions |
| **Parameters** | Dynamic inputs |
| **Branching** | Conditional logic |
| **Approval** | Human-in-the-loop |
| **Rate Control** | Concurrency limits |

#### Common Automation Runbooks
| Runbook | Purpose |
|---------|---------|
| `AWS-RestartEC2Instance` | Restart instances |
| `AWS-StopEC2Instance` | Stop instances |
| `AWS-CreateSnapshot` | EBS snapshots |
| `AWS-UpdateCloudFormationStack` | Stack updates |
| `AWS-EnableCloudTrail` | Enable CloudTrail |
| `AWS-ConfigureS3BucketLogging` | Enable S3 logging |

---

### AWS Config Auto Remediation

| Component | Description |
|-----------|-------------|
| **Rules** | Compliance evaluation |
| **Remediation Action** | SSM document to execute |
| **Auto Remediation** | Automatic on non-compliance |
| **Manual Remediation** | One-click fix |
| **Retry** | Configurable retries |

---

### Lambda Event Processing

| Pattern | Description |
|---------|-------------|
| **Synchronous** | Request-response |
| **Asynchronous** | Fire-and-forget |
| **Event Source Mapping** | Poll-based (SQS, Kinesis) |

#### Error Handling
| Feature | Description |
|---------|-------------|
| **DLQ** | Dead letter queue |
| **Destinations** | Success/failure routing |
| **Retry** | Async retries (2 times) |
| **Reserved Concurrency** | Throttling |

---

## Task 5.3: Incident Management

### AWS Systems Manager Incident Manager

| Component | Description |
|-----------|-------------|
| **Response Plans** | Incident procedures |
| **Contacts** | Escalation paths |
| **Engagements** | Notification methods |
| **Runbooks** | Automation during incident |
| **Analysis** | Post-incident review |
| **Timeline** | Incident history |

---

### CloudWatch Alarm Actions

| Action Type | Description |
|-------------|-------------|
| **SNS** | Notify teams |
| **Auto Scaling** | Scale resources |
| **EC2** | Stop, terminate, recover |
| **Incident Manager** | Create incident |
| **OpsCenter** | Create OpsItem |

---

### Health Dashboard and Events

| Type | Description |
|------|-------------|
| **Service Health** | AWS service status |
| **Account Health** | Account-specific events |
| **Scheduled Changes** | Maintenance events |
| **Notifications** | EventBridge integration |

---

## Task 5.4: Root Cause Analysis

### AWS X-Ray Analysis

| Feature | Use Case |
|---------|----------|
| **Trace Analysis** | Request flow issues |
| **Error Analysis** | Error patterns |
| **Latency Analysis** | Performance bottlenecks |
| **Service Map** | Dependency issues |

---

### CloudWatch Contributor Insights

| Feature | Description |
|---------|-------------|
| **Top Contributors** | Highest impact entities |
| **Rules** | Log group analysis |
| **Time Series** | Trend analysis |
| **Integration** | Dashboard widgets |

---

### Amazon Detective

| Feature | Description |
|---------|-------------|
| **Behavior Graph** | Entity relationships |
| **Findings Analysis** | GuardDuty integration |
| **VPC Flow Analysis** | Network investigation |
| **CloudTrail Analysis** | API investigation |

---

## Exam Tips for Domain 5

1. **EventBridge patterns:**
   - Rules match events
   - Targets receive events
   - Archive = replay events
2. **Event-driven remediation:**
   - Config Rule → SSM Automation
   - CloudWatch Alarm → Lambda
   - GuardDuty → Step Functions
3. **SSM Automation:**
   - Built-in runbooks
   - Custom runbooks
   - Rate control = safety
4. **Lambda error handling:**
   - DLQ for async failures
   - Destinations for routing
   - Reserved concurrency = throttle
5. **Incident Manager:**
   - Response plans
   - Escalation contacts
   - Runbook integration
6. **Config remediation:**
   - Auto vs manual
   - SSM document execution
   - Retry configuration
7. **Root cause analysis:**
   - X-Ray for distributed tracing
   - Contributor Insights for patterns
   - Detective for security
8. **Health Dashboard:**
   - Service vs account health
   - EventBridge integration
   - Scheduled maintenance
9. **Cross-account events:**
   - Event bus rules
   - Resource-based policies
   - Same region requirement
10. **Alarm best practices:**
    - Composite alarms = reduce noise
    - Multiple notification channels
    - Document runbook in alarm description
