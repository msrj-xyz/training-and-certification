# Domain 4: Network Security, Compliance, and Governance (24%)

## Task 4.1: Network Security Controls

### Defense in Depth ⭐

| Layer | AWS Controls |
|-------|-------------|
| **Edge** | CloudFront, Shield, WAF, Global Accelerator |
| **VPC Perimeter** | Network Firewall, NAT Gateway |
| **Subnet** | Network ACLs |
| **Instance** | Security Groups |
| **Application** | Authentication, authorization |

---

### Security Groups vs Network ACLs ⭐

| Aspect | Security Groups | Network ACLs |
|--------|-----------------|--------------|
| **Level** | Instance/ENI | Subnet |
| **State** | Stateful | Stateless |
| **Rules** | Allow only | Allow and Deny |
| **Evaluation** | All rules | In order |
| **Default** | Deny all | Allow all |
| **Association** | Multiple per ENI | 1 per subnet |

---

### AWS Network Firewall ⭐

| Feature | Description |
|---------|-------------|
| **Deployment** | VPC-level |
| **Rules** | Stateless and stateful |
| **Stateful** | 5-tuple, domain, Suricata |
| **TLS Inspection** | Decrypt and inspect |
| **Logging** | CloudWatch, S3, Kinesis |

#### Network Firewall Rule Types
| Type | Use Case |
|------|----------|
| **Stateless** | Simple packet filtering |
| **Stateful 5-tuple** | Protocol, ports, IPs |
| **Stateful Domain** | Allow/block domains |
| **Stateful Suricata** | IDS/IPS rules |

#### Network Firewall Deployment Patterns
| Pattern | Description |
|---------|-------------|
| **Distributed** | Per-VPC deployment |
| **Centralized** | Inspection VPC |
| **Combined** | N/S centralized, E/W distributed |

---

### AWS WAF ⭐

| Component | Description |
|-----------|-------------|
| **Web ACL** | Container for rules |
| **Rules** | Match conditions + actions |
| **Rule Groups** | Managed or custom |
| **Rate-based** | DDoS/bot protection |

#### WAF Deployment Points
| Service | Integration |
|---------|-------------|
| **CloudFront** | Edge protection |
| **ALB** | Application protection |
| **API Gateway** | API protection |
| **AppSync** | GraphQL protection |
| **Cognito** | Auth protection |

---

### AWS Shield

| Tier | Features |
|------|----------|
| **Standard** | Free, L3/L4 DDoS protection |
| **Advanced** | 24/7 DRT, L7 protection, cost protection |

#### Shield Advanced Features
- DDoS Response Team (DRT)
- Real-time metrics
- Cost protection
- WAF included
- Health-based detection
- Application layer protection

---

### Gateway Load Balancer (GWLB) ⭐

| Feature | Description |
|---------|-------------|
| **Protocol** | GENEVE encapsulation |
| **Use Case** | Third-party appliances |
| **Deployment** | Security VPC |
| **Transparent** | Bump-in-the-wire |

#### GWLB Architecture
```
Internet → GWLB Endpoint → GWLB → Firewall Appliances → Target VPC
```

---

### VPC Endpoint Policies

| Endpoint Type | Policy Support |
|---------------|----------------|
| **Interface** | Yes |
| **Gateway (S3/DynamoDB)** | Yes |

#### Endpoint Policy Example
```json
{
  "Statement": [{
    "Effect": "Allow",
    "Principal": "*",
    "Action": "s3:GetObject",
    "Resource": "arn:aws:s3:::my-bucket/*"
  }]
}
```

---

### Inter-VPC Security

| Method | Security Control |
|--------|------------------|
| **VPC Peering** | Security groups reference |
| **Transit Gateway** | TGW route tables, blackholes |
| **PrivateLink** | Endpoint policies |
| **Network Firewall** | Centralized inspection |

---

## Task 4.2: Security Monitoring and Logging

### VPC Flow Logs for Security ⭐

#### Security Analysis Use Cases
| Analysis | Query/Filter |
|----------|--------------|
| **Rejected traffic** | action = 'REJECT' |
| **Top blocked IPs** | Group by srcaddr |
| **Port scanning** | Multiple dstport from same srcaddr |
| **Data exfiltration** | Large bytes to external |

---

### Traffic Mirroring for Security

| Use Case | Description |
|----------|-------------|
| **IDS/IPS** | Send to security appliance |
| **Forensics** | Capture for analysis |
| **Compliance** | Content inspection |
| **Threat hunting** | Deep packet inspection |

---

### Security Logging Services

| Service | Logs |
|---------|------|
| **CloudTrail** | API audit |
| **VPC Flow Logs** | Network traffic |
| **ALB Access Logs** | Load balancer requests |
| **CloudFront Logs** | CDN requests |
| **WAF Logs** | Web traffic |
| **Network Firewall Logs** | Firewall traffic |
| **Route 53 Query Logs** | DNS queries |

---

### AWS Firewall Manager ⭐

| Feature | Description |
|---------|-------------|
| **Centralized** | Cross-account policy |
| **Security Groups** | Common baseline |
| **WAF** | Managed WAF rules |
| **Shield** | Shield Advanced |
| **Network Firewall** | Centralized firewall |
| **DNS Firewall** | Route 53 Resolver |

#### Firewall Manager Prerequisites
- AWS Organizations
- AWS Config enabled
- Delegated administrator account

---

### Security Automation

| Trigger | Response |
|---------|----------|
| **GuardDuty finding** | Block IP via NACL |
| **WAF rule match** | Log and alert |
| **Flow Log anomaly** | Investigate with Detective |
| **Config violation** | Auto-remediate |

---

## Task 4.3: Network Encryption

### Encryption in Transit ⭐

| Service | Encryption Method |
|---------|-------------------|
| **ALB** | TLS termination |
| **NLB** | TLS passthrough or termination |
| **CloudFront** | Viewer + origin HTTPS |
| **API Gateway** | TLS only |
| **VPN** | IPsec (IKEv1/v2) |
| **Direct Connect** | MACsec (layer 2) |

---

### Direct Connect Encryption Options ⭐

| Option | Layer | Description |
|--------|-------|-------------|
| **MACsec** | Layer 2 | Native DX encryption |
| **VPN over DX** | Layer 3 | IPsec tunnel over private VIF |
| **Application** | Layer 7 | TLS/SSL |

#### MACsec (MAC Security)
| Feature | Description |
|---------|-------------|
| **Standard** | IEEE 802.1AE |
| **Supported** | 10 Gbps and 100 Gbps dedicated |
| **Keys** | CKN/CAK rotation |
| **Encryption** | GCM-AES-256 or GCM-AES-XPN-256 |

---

### VPN Encryption

| Component | Options |
|-----------|---------|
| **IKE** | IKEv1, IKEv2 |
| **Phase 1** | AES-128/256, SHA-256 |
| **Phase 2** | AES-128/256, SHA-256 |
| **DH Group** | 2, 14, 15, 16, 17, 18, 19-21, 22-24 |

---

### Certificate Management

| Service | Use Case |
|---------|----------|
| **ACM** | Public certificates (free) |
| **ACM Private CA** | Private certificates |
| **IAM** | Upload external certs |

#### ACM Features
- Auto-renewal
- Integration with ALB, CloudFront, API Gateway
- DNS or email validation
- Regional (except CloudFront = us-east-1)

---

### DNSSEC

| Feature | Description |
|---------|-------------|
| **Purpose** | Authenticate DNS responses |
| **Route 53** | Signing supported |
| **Validation** | Resolver validation |
| **Keys** | KSK, ZSK |

#### DNSSEC Implementation Steps
| Step | Action |
|------|--------|
| 1 | Enable DNSSEC signing |
| 2 | Create KSK in KMS |
| 3 | Add DS record to parent zone |
| 4 | Monitor with CloudWatch |

---

### Network Architecture Patterns

#### Perimeter VPC (Transit VPC)
```
Internet → Perimeter VPC → Network Firewall → Transit Gateway → Workload VPCs
```

#### Three-Tier Architecture
| Tier | Security |
|------|----------|
| **Web** | Public subnet, ALB, WAF |
| **Application** | Private subnet, SG only |
| **Database** | Isolated subnet, no internet |

---

## Exam Tips for Domain 4

1. **Security Groups** = stateful, allow only
2. **NACLs** = stateless, allow and deny, ordered
3. **Network Firewall** = Suricata rules, domain filtering
4. **GWLB** = third-party appliances, GENEVE
5. **Firewall Manager** = cross-account security policies
6. **MACsec** = Layer 2 encryption for DX (10G/100G)
7. **VPN over DX** = Layer 3 encryption option
8. **ACM** = free public certs, auto-renewal
9. **DNSSEC** = DNS authentication (not encryption)
10. **VPC Flow Logs** = security analysis, rejected traffic
