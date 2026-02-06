# Domain 1: Monitoring, Logging, Analysis, Remediation, and Performance Optimization (22%)

## Task 1.1: Implement Metrics, Alarms, and Filters

### Amazon CloudWatch Core Components

| Component | Description | Key Points |
|-----------|-------------|------------|
| **Metrics** | Time-series data points | Standard (5-min) vs. Detailed (1-min) |
| **Alarms** | Trigger actions based on thresholds | OK, ALARM, INSUFFICIENT_DATA states |
| **Logs** | Log data from AWS services/apps | Log groups, log streams, retention |
| **Dashboards** | Visual display of metrics | Cross-account, cross-region support |
| **Events/EventBridge** | Event-driven automation | Rules, targets, event buses |
| **Insights** | Log query and analysis | CloudWatch Logs Insights |

---

### CloudWatch Metrics

| Metric Type | Description | Examples |
|-------------|-------------|----------|
| **Standard Metrics** | Built-in AWS metrics | CPUUtilization, NetworkIn/Out |
| **Custom Metrics** | User-published metrics | Application-specific data |
| **High-Resolution** | 1-second resolution metrics | Real-time monitoring |

#### Common EC2 Metrics (Default)
- CPUUtilization
- NetworkIn / NetworkOut
- DiskReadOps / DiskWriteOps
- StatusCheckFailed

> **Exam Tip:** Memory and disk space utilization require CloudWatch agent!

---

### CloudWatch Agent

| Feature | Description |
|---------|-------------|
| **Purpose** | Collect system-level and custom metrics |
| **Install locations** | EC2, on-premises, ECS, EKS |
| **Metrics collected** | Memory, disk, processes, custom logs |
| **Configuration** | JSON config file, SSM Parameter Store |

#### Agent Installation Steps
1. Download and install agent
2. Create agent configuration
3. Store config in SSM Parameter Store (optional)
4. Start agent with configuration

---

### CloudWatch Alarms

| Alarm Type | Description | Use Case |
|------------|-------------|----------|
| **Standard Alarm** | Single metric threshold | CPU > 80% for 5 minutes |
| **Composite Alarm** | Multiple alarms combined | CPU high AND disk low |
| **Anomaly Detection** | ML-based thresholds | Deviations from baseline |

#### Alarm States
| State | Meaning |
|-------|---------|
| **OK** | Metric within threshold |
| **ALARM** | Metric breached threshold |
| **INSUFFICIENT_DATA** | Not enough data to determine |

#### Alarm Actions
| Target | Description |
|--------|-------------|
| **SNS** | Send notifications |
| **Auto Scaling** | Scale in/out |
| **EC2** | Stop, terminate, reboot, recover |
| **Systems Manager** | Run automation |
| **EventBridge** | Trigger events |

---

### CloudWatch Logs

| Component | Description |
|-----------|-------------|
| **Log Group** | Collection of log streams |
| **Log Stream** | Sequence of log events |
| **Retention** | 1 day to never expire |
| **Metric Filter** | Create metrics from log data |
| **Subscription Filter** | Stream to Lambda, Kinesis, etc. |

### Log Sources
- EC2 instances (via CloudWatch agent)
- Lambda functions (automatic)
- VPC Flow Logs
- CloudTrail logs
- Route 53 query logs
- API Gateway logs
- ECS/EKS container logs

#### CloudWatch Logs Insights Query Examples
```
# Find errors in last hour
fields @timestamp, @message
| filter @message like /ERROR/
| sort @timestamp desc
| limit 100

# Count requests by status code
fields @timestamp, status
| stats count(*) by status

# Find slowest requests
fields @timestamp, @duration
| sort @duration desc
| limit 20

# Parse and aggregate
parse @message "* * * * * *" as ip, id, user, time, request, status
| stats count(*) by ip
```

> **Exam Tip:** Logs Insights uses a custom query language - familiarize with basic syntax!

---

### CloudWatch Dashboards

| Feature | Description |
|---------|-------------|
| **Widgets** | Metrics, logs, text, alarms |
| **Cross-account** | View metrics from multiple accounts |
| **Cross-region** | View metrics from multiple regions |
| **Sharing** | Share via public/private URLs |
| **Automatic** | Auto-generated dashboards available |

---

### Amazon EventBridge

| Component | Description |
|-----------|-------------|
| **Event Bus** | Receives and routes events |
| **Rules** | Match events to targets |
| **Targets** | Lambda, SNS, SQS, Step Functions, etc. |
| **Event Pattern** | JSON pattern matching |
| **Schedule** | Cron or rate expressions |

> **Exam Tip:** EventBridge is the successor to CloudWatch Events!

---

### AWS CloudTrail

| Feature | Description |
|---------|-------------|
| **Management Events** | Control plane operations |
| **Data Events** | Data plane operations (S3, Lambda) |
| **Insights** | Unusual activity detection |
| **Multi-region** | Trail applies to all regions |
| **Organization Trail** | All accounts in organization |

---

### Amazon Managed Service for Prometheus

| Feature | Description |
|---------|-------------|
| **Purpose** | Managed Prometheus for containers |
| **Use with** | EKS, ECS, self-managed Kubernetes |
| **Query** | PromQL compatible |
| **Visualization** | Grafana integration |

---

### SNS Notifications Integration

| Integration | Description |
|-------------|-------------|
| **CloudWatch Alarms** | Send alarm notifications |
| **Budget Alerts** | Cost threshold notifications |
| **EventBridge** | Event notifications |
| **CloudFormation** | Stack event notifications |

---

## Task 1.2: Identify and Remediate Issues

### Automated Remediation Strategies

| Service | Remediation Capability |
|---------|----------------------|
| **Auto Scaling** | Replace unhealthy instances |
| **CloudWatch** | Alarm actions (EC2 recovery) |
| **Systems Manager** | Automation runbooks |
| **EventBridge** | Trigger Lambda for remediation |
| **Config** | Auto-remediation rules |

---

### AWS Systems Manager Automation

| Feature | Description |
|---------|-------------|
| **Runbooks** | Predefined or custom automation documents |
| **Automation** | Execute runbooks against resources |
| **Rate Control** | Control execution speed |
| **Approval** | Optional approval steps |

#### Common Runbooks
- AWS-StopEC2Instance
- AWS-StartEC2Instance
- AWS-RebootEC2Instance
- AWS-RestartEC2InstanceWithApproval
- AWS-UpdateCloudFormationStack

---

### EventBridge Rule Troubleshooting

| Issue | Check |
|-------|-------|
| **Rule not triggering** | Event pattern matching |
| **Target not invoked** | Target permissions (IAM role) |
| **Event not received** | Event bus permissions |
| **Partial matching** | JSON structure accuracy |

---

## Task 1.3: Performance Optimization Strategies

### EC2 Performance Optimization

| Strategy | Description |
|----------|-------------|
| **Right-sizing** | Use Compute Optimizer |
| **Instance types** | Match workload requirements |
| **Placement groups** | Cluster, Spread, Partition |
| **Enhanced networking** | ENA for higher throughput |
| **Nitro System** | Latest generation instances |

#### EC2 Placement Groups
| Type | Description | Use Case |
|------|-------------|----------|
| **Cluster** | Low latency, single AZ | HPC, tightly coupled |
| **Spread** | Different hardware | High availability |
| **Partition** | Groups on different racks | Large distributed workloads |

---

### Amazon EBS Optimization

| Volume Type | Description | Use Case |
|-------------|-------------|----------|
| **gp3** | General purpose SSD | Most workloads |
| **gp2** | General purpose SSD (baseline IOPS) | Small databases |
| **io2 Block Express** | High-performance SSD | Mission-critical |
| **io1/io2** | Provisioned IOPS SSD | Databases |
| **st1** | Throughput optimized HDD | Big data |
| **sc1** | Cold HDD | Infrequent access |

#### EBS Performance Metrics
- VolumeReadOps / VolumeWriteOps
- VolumeReadBytes / VolumeWriteBytes
- VolumeQueueLength
- VolumeThroughputPercentage
- VolumeConsumedReadWriteOps

> **Exam Tip:** High queue length indicates I/O bottleneck!

---

### Amazon S3 Performance Optimization

| Strategy | Description |
|----------|-------------|
| **Transfer Acceleration** | Edge locations for faster upload |
| **Multipart Upload** | Parallel upload for large objects |
| **Byte-range Fetches** | Parallel download |
| **S3 Select** | Query data in-place |
| **Prefix Distribution** | Distribute objects across prefixes |
| **Lifecycle Policies** | Transition to appropriate storage class |

---

### Amazon RDS Performance

| Strategy | Tool/Feature |
|----------|--------------|
| **Performance Insights** | Database load analysis |
| **Enhanced Monitoring** | OS-level metrics |
| **RDS Proxy** | Connection pooling |
| **Read Replicas** | Offload read traffic |
| **Storage Auto Scaling** | Automatic storage increase |

#### Performance Insights Metrics
- Database Load (AAS - Average Active Sessions)
- Wait events
- Top SQL queries
- Top hosts

---

### Shared Storage Solutions

| Service | Protocol | Use Case |
|---------|----------|----------|
| **Amazon EFS** | NFS | Linux shared storage |
| **Amazon FSx for Windows** | SMB | Windows shared storage |
| **Amazon FSx for Lustre** | Lustre | HPC, ML workloads |
| **Amazon FSx for NetApp ONTAP** | NFS, SMB, iSCSI | Multi-protocol |
| **Amazon FSx for OpenZFS** | NFS | Linux, ZFS features |

#### EFS Lifecycle Policies
- Transition to IA after X days (7, 14, 30, 60, 90, 180, 365)
- Transition back to Standard on access

---

## Exam Tips for Domain 1

1. **CloudWatch Agent requirements:**
   - Memory metrics = requires agent
   - Disk space = requires agent
   - CPU, Network = default metrics (no agent)
2. **Alarm configurations:**
   - Composite alarms = reduce noise, combine conditions
   - Anomaly detection = ML-based thresholds
   - Missing data = treat as INSUFFICIENT_DATA by default
3. **EventBridge patterns:**
   - Preferred over CloudWatch Events
   - Supports partner events, scheduled rules
4. **Systems Manager Automation:**
   - Know common runbooks (AWS-StopEC2Instance, etc.)
   - Rate control prevents blast radius
5. **EBS volume types:**
   - gp3 = general purpose (most workloads)
   - io2 = high IOPS databases
   - st1 = throughput (big data)
   - High queue length = I/O bottleneck
6. **RDS Performance Insights:**
   - First tool for database performance issues
   - AAS (Average Active Sessions) key metric
7. **S3 optimization:**
   - Transfer Acceleration = long-distance uploads
   - Multipart upload = large files (>100MB)
8. **Placement Groups:**
   - Cluster = low latency, HPC
   - Spread = HA, max 7 per AZ
   - Partition = large distributed (HDFS, Kafka)
9. **CloudTrail:**
   - Management events = control plane
   - Data events = S3/Lambda data access
10. **Metric resolution:**
    - Standard = 5 minutes (free)
    - Detailed = 1 minute (extra cost)
    - High-resolution = 1 second (custom metrics)

