# Domain 2: Incident Response (14%)

## Task 2.1: Design and Test Incident Response Plan

### Incident Response Lifecycle ⭐

| Phase | Activities |
|-------|------------|
| **Preparation** | Playbooks, tools, access provisioning |
| **Detection** | Monitoring, alerting, triage |
| **Containment** | Isolate affected resources |
| **Eradication** | Remove threat, patch vulnerabilities |
| **Recovery** | Restore services, validate |
| **Lessons Learned** | Post-incident review, improve |

---

### AWS Incident Response Tools

| Tool | Purpose |
|------|---------|
| **Systems Manager OpsCenter** | Runbooks, operational issues |
| **Systems Manager Automation** | Automated remediation |
| **Step Functions** | Orchestrate response workflows |
| **Lambda** | Custom automation |
| **EventBridge** | Event-driven response |
| **AWS Backup** | Restore from backup |

---

### Runbook Components

| Component | Description |
|-----------|-------------|
| **Detection** | How to identify incident |
| **Containment** | Isolation procedures |
| **Investigation** | Data collection steps |
| **Remediation** | Fix procedures |
| **Recovery** | Restoration steps |
| **Communication** | Notification process |

---

### Pre-Incident Preparation ⭐

| Activity | AWS Service |
|----------|-------------|
| **Provisioning access** | IAM break-glass roles |
| **Deploy security tools** | GuardDuty, Security Hub |
| **Minimize blast radius** | VPC isolation, SCPs |
| **Shield Advanced** | DDoS protection, response team |
| **AWS Backup** | Automated backups |
| **Forensic account** | Isolated analysis environment |

---

### Testing Incident Response

| Method | AWS Service |
|--------|-------------|
| **Chaos engineering** | AWS Fault Injection Service (FIS) |
| **DR testing** | AWS Resilience Hub |
| **Tabletop exercises** | Manual simulation |
| **Purple teaming** | Attack simulation |

---

### Automated Remediation Patterns

| Trigger | Action |
|---------|--------|
| **GuardDuty finding** | EventBridge → Lambda (isolate instance) |
| **Config non-compliance** | SSM Automation (remediate) |
| **Security Hub finding** | Step Functions workflow |
| **CloudWatch Alarm** | Auto Scaling (terminate instance) |

```
GuardDuty Finding → EventBridge Rule → Lambda → Isolate EC2
                                              → Snapshot EBS
                                              → Notify SOC
```

---

## Task 2.2: Respond to Security Events

### Forensic Data Collection ⭐

| Data Type | Collection Method |
|-----------|-------------------|
| **Memory** | EC2 memory dump (before termination) |
| **Disk** | EBS snapshot |
| **Logs** | CloudTrail, VPC Flow Logs |
| **Network** | VPC Traffic Mirroring |
| **Metadata** | Instance metadata, tags |

#### Forensic Best Practices
- Create EBS snapshots immediately
- Isolate instance (modify security group)
- DO NOT terminate until analyzed
- Use separate forensic account
- Maintain chain of custody

---

### Amazon Detective ⭐

| Feature | Description |
|---------|-------------|
| **Graph Analysis** | Visualize relationships |
| **Timeline** | Event sequence |
| **Data Sources** | CloudTrail, VPC Flow, GuardDuty |
| **Investigation** | Pivot from GuardDuty findings |
| **Scope** | Assess impact breadth |

---

### Containment Strategies

| Scenario | Containment Action |
|----------|-------------------|
| **Compromised EC2** | Isolate via SG, take snapshot |
| **Stolen credentials** | Revoke sessions, rotate keys |
| **S3 exposure** | Update bucket policy, block public |
| **DDoS attack** | Enable Shield Advanced, WAF rules |
| **Compromised Lambda** | Update function permissions |

#### EC2 Containment Steps
1. Modify security group (isolate)
2. Create EBS snapshot
3. Capture memory (if possible)
4. Preserve CloudTrail logs
5. Analyze in forensic account

---

### Credential Compromise Response

| Action | Command/Method |
|--------|----------------|
| **Invalidate sessions** | IAM → Revoke session policies |
| **Deactivate access keys** | IAM → Deactivate keys |
| **Rotate credentials** | Create new, delete old |
| **Check CloudTrail** | Analyze API activity |
| **Review IAM policies** | Remove excess permissions |

---

### Recovery Actions

| Activity | AWS Service |
|----------|-------------|
| **Restore from backup** | AWS Backup |
| **Redeploy infrastructure** | CloudFormation, Terraform |
| **Patch vulnerabilities** | SSM Patch Manager |
| **Rotate all credentials** | Secrets Manager |
| **Update security controls** | WAF rules, SCPs |

---

### Root Cause Analysis

| Tool | Use Case |
|------|----------|
| **Amazon Detective** | Graph-based investigation |
| **CloudTrail Insights** | Unusual API activity |
| **CloudWatch Logs Insights** | Log correlation |
| **Athena** | S3 log queries |
| **OpenSearch** | Full-text search |

---

## Exam Tips for Domain 2

1. **Forensic order**: Snapshot EBS → Isolate → Analyze
2. **DON'T terminate** until forensics complete
3. **Detective** = graph-based investigation from GuardDuty
4. **Isolation** = change security group to deny all
5. **Credential compromise** = revoke sessions + rotate keys
6. **Step Functions** = orchestrate complex IR workflows
7. **EventBridge + Lambda** = automated response pattern
8. **AWS Backup** = restore from known good state
9. **Forensic account** = isolated analysis environment
10. **Chain of custody** = document all actions
