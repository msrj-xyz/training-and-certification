# Domain 4: Monitoring and Logging (15%)

## Task 4.1: Amazon CloudWatch ⭐

### CloudWatch Components

| Component | Description |
|-----------|-------------|
| **Metrics** | Time-series data points |
| **Alarms** | Threshold-based alerts |
| **Logs** | Log data collection |
| **Dashboards** | Visualization |
| **Events/EventBridge** | Event routing |
| **Insights** | Log and metric analysis |
| **Synthetics** | Canary testing |
| **RUM** | Real User Monitoring |
| **Evidently** | Feature flags, A/B testing |

---

### CloudWatch Metrics

| Metric Type | Description |
|-------------|-------------|
| **Standard** | Default AWS metrics |
| **High-Resolution** | Sub-minute (1 second) |
| **Custom** | Application metrics |
| **Embedded Metric Format** | Logs → Metrics |

#### Metric Resolution
| Resolution | Retention |
|------------|-----------|
| **1 second** | 3 hours |
| **60 seconds** | 15 days |
| **5 minutes** | 63 days |
| **1 hour** | 455 days |

---

### CloudWatch Alarms ⭐

| Alarm State | Description |
|-------------|-------------|
| **OK** | Metric within threshold |
| **ALARM** | Metric breached threshold |
| **INSUFFICIENT_DATA** | Not enough data |

#### Alarm Actions
| Action | Description |
|--------|-------------|
| **SNS** | Notification |
| **Auto Scaling** | Scale in/out |
| **EC2 Actions** | Stop, terminate, reboot |
| **Systems Manager** | OpsItems, Incident Manager |

#### Composite Alarms
- Combine multiple alarms
- AND/OR logic
- Reduce alarm noise
- Complex monitoring scenarios

---

### CloudWatch Logs ⭐

| Component | Description |
|-----------|-------------|
| **Log Groups** | Collection of log streams |
| **Log Streams** | Sequence of log events |
| **Retention** | 1 day to 10 years |
| **Metric Filters** | Extract metrics from logs |
| **Subscription Filters** | Stream to destinations |

#### Log Destinations
| Destination | Use Case |
|-------------|----------|
| **Kinesis Streams** | Real-time processing |
| **Kinesis Firehose** | Load to S3, OpenSearch |
| **Lambda** | Custom processing |
| **OpenSearch** | Analysis, visualization |

---

### CloudWatch Logs Insights ⭐

| Feature | Description |
|---------|-------------|
| **Query Language** | SQL-like syntax |
| **Aggregations** | stats, sum, avg, count |
| **Pattern Detection** | find patterns |
| **Visualization** | Time-series, bar charts |
| **Cross-log Group** | Query multiple groups |

#### Example Queries
```
# Find errors
fields @timestamp, @message
| filter @message like /ERROR/
| sort @timestamp desc
| limit 20

# Top 10 IPs
fields @timestamp, clientIp
| stats count(*) as requests by clientIp
| sort requests desc
| limit 10

# Latency percentiles
fields @timestamp, latency
| stats avg(latency), pct(latency, 95), pct(latency, 99)
```

---

## Task 4.2: AWS X-Ray ⭐

### X-Ray Components

| Component | Description |
|-----------|-------------|
| **Trace** | End-to-end request |
| **Segment** | Service processing unit |
| **Subsegment** | External calls, details |
| **Service Map** | Visual dependency graph |
| **Annotations** | Indexed trace metadata |
| **Metadata** | Non-indexed data |

---

### X-Ray Integration

| Service | Integration Method |
|---------|-------------------|
| **Lambda** | Active tracing (built-in) |
| **API Gateway** | X-Ray tracing option |
| **ECS** | X-Ray sidecar container |
| **EC2** | X-Ray daemon |
| **Elastic Beanstalk** | Configuration option |

---

### X-Ray Sampling Rules

| Type | Description |
|------|-------------|
| **Default** | First request/second + 5% |
| **Custom** | Service, URL, method based |
| **Reservoir** | Fixed requests/second |
| **Rate** | Percentage of remaining |

---

## Task 4.3: Application Insights and Observability

### Amazon Managed Grafana

| Feature | Description |
|---------|-------------|
| **Data Sources** | CloudWatch, Prometheus, X-Ray |
| **Dashboards** | Custom visualizations |
| **Alerts** | Notification channels |
| **Annotations** | Event markers |

---

### Amazon Managed Service for Prometheus

| Feature | Description |
|---------|-------------|
| **PromQL** | Prometheus Query Language |
| **Remote Write** | Ingest metrics |
| **Alert Manager** | Alerting |
| **Integration** | EKS, Grafana |

---

### Container Insights

| Feature | Description |
|---------|-------------|
| **ECS Metrics** | Cluster, service, task metrics |
| **EKS Metrics** | Cluster, node, pod metrics |
| **Logs** | Container stdout/stderr |
| **Enhanced Observability** | Detailed resource metrics |

---

## Task 4.4: Infrastructure Monitoring

### AWS Config

| Feature | Description |
|---------|-------------|
| **Configuration Recorder** | Track resource changes |
| **Rules** | Compliance evaluation |
| **Remediation** | Auto-fix non-compliant |
| **Conformance Packs** | Rule bundles |
| **Aggregator** | Multi-account view |

---

### AWS CloudTrail ⭐

| Event Type | Description |
|------------|-------------|
| **Management Events** | Control plane operations |
| **Data Events** | Data plane operations (S3, Lambda) |
| **Insights Events** | Unusual API activity |

#### CloudTrail Integration
| Integration | Purpose |
|-------------|---------|
| **S3** | Long-term storage |
| **CloudWatch Logs** | Real-time analysis |
| **EventBridge** | Event-driven response |
| **Athena** | SQL analysis |

---

### VPC Flow Logs

| Field | Description |
|-------|-------------|
| **srcaddr** | Source IP |
| **dstaddr** | Destination IP |
| **srcport** | Source port |
| **dstport** | Destination port |
| **action** | ACCEPT, REJECT |

---

## Exam Tips for Domain 4

1. **CloudWatch metrics:**
   - High-resolution = 1 second
   - Standard = 1 minute (free tier)
   - Custom = PutMetricData API
2. **CloudWatch alarms:**
   - Composite = combine alarms
   - Actions = SNS, Auto Scaling, EC2
   - INSUFFICIENT_DATA = too new
3. **Logs Insights:**
   - SQL-like query language
   - fields, filter, stats, sort
   - Cross-log group queries
4. **Metric filters:**
   - Extract metrics from logs
   - Pattern matching
   - Create CloudWatch metrics
5. **X-Ray tracing:**
   - Sampling = control costs
   - Annotations = indexed, searchable
   - Service Map = dependencies
6. **Container Insights:**
   - ECS, EKS monitoring
   - Enhanced observability
   - Performance logs
7. **CloudTrail:**
   - Management = control plane
   - Data = S3, Lambda operations
   - Insights = unusual activity
8. **Log destinations:**
   - Kinesis = real-time
   - S3 = archival
   - OpenSearch = analysis
9. **Managed Prometheus/Grafana:**
   - Kubernetes monitoring
   - PromQL queries
   - Custom dashboards
10. **VPC Flow Logs:**
    - Network traffic analysis
    - Security monitoring
    - Troubleshooting
