# Domain 5: Networking and Content Delivery (18%)

## Task 5.1: Implement and Optimize Networking Features

### VPC Components

| Component | Description |
|-----------|-------------|
| **VPC** | Virtual network in AWS |
| **Subnet** | IP address range in AZ |
| **Route Table** | Traffic routing rules |
| **Internet Gateway** | Internet access |
| **NAT Gateway** | Outbound internet for private subnets |
| **Security Group** | Instance-level firewall (stateful) |
| **Network ACL** | Subnet-level firewall (stateless) |
| **Elastic IP** | Static public IP address |

---

### Subnet Types

| Type | Description | Route |
|------|-------------|-------|
| **Public** | Direct internet access | 0.0.0.0/0 → IGW |
| **Private** | No direct internet access | 0.0.0.0/0 → NAT GW |
| **Isolated** | No internet access | No default route |

#### Subnet CIDR Planning
| CIDR | Usable IPs |
|------|------------|
| /28 | 11 (16 - 5 reserved) |
| /27 | 27 |
| /26 | 59 |
| /25 | 123 |
| /24 | 251 |
| /23 | 507 |
| /22 | 1019 |

> **Exam Tip:** AWS reserves 5 IPs per subnet (first 4 + last 1)!

---

### Security Groups vs Network ACLs

| Feature | Security Group | Network ACL |
|---------|----------------|-------------|
| **Level** | Instance (ENI) | Subnet |
| **State** | Stateful | Stateless |
| **Rules** | Allow only | Allow and Deny |
| **Evaluation** | All rules | Rules in order |
| **Default** | Deny all inbound | Allow all |
| **Association** | Multiple per instance | One per subnet |

---

### NAT Solutions

| Solution | Description | Use Case |
|----------|-------------|----------|
| **NAT Gateway** | AWS managed, highly available | Production workloads |
| **NAT Instance** | Self-managed EC2 | Cost optimization |
| **Egress-only IGW** | IPv6 outbound only | IPv6 subnets |

#### NAT Gateway Characteristics
- AZ-specific (deploy in each AZ for HA)
- 5 Gbps baseline, scales to 100 Gbps
- No security groups (use NACLs)
- Cannot be associated with security group
- Use Elastic IP

---

### Private Connectivity

| Service | Description | Use Case |
|---------|-------------|----------|
| **VPC Peering** | Connect two VPCs | Simple VPC-to-VPC |
| **Transit Gateway** | Hub for multiple VPCs | Multi-VPC connectivity |
| **VPC Endpoints** | Private AWS service access | S3, DynamoDB, etc. |
| **PrivateLink** | Private service access | Third-party services |
| **Site-to-Site VPN** | Encrypted tunnel to on-prem | Hybrid connectivity |
| **Direct Connect** | Dedicated connection | High bandwidth, consistent |
| **Client VPN** | Remote user access | Remote workforce |

---

### VPC Endpoints

| Type | Description | Services |
|------|-------------|----------|
| **Gateway Endpoint** | Route table entry | S3, DynamoDB |
| **Interface Endpoint** | ENI in subnet | Most AWS services |
| **Gateway Load Balancer Endpoint** | GWLB targets | Third-party appliances |

> **Exam Tip:** Gateway endpoints are free, Interface endpoints incur hourly charge!

---

### Transit Gateway

| Feature | Description |
|---------|-------------|
| **Hub-and-spoke** | Central connectivity hub |
| **Route Tables** | Control traffic between attachments |
| **Attachments** | VPCs, VPNs, Direct Connect |
| **Peering** | Cross-region Transit Gateway |
| **Multicast** | Multicast traffic support |

---

### Network Protection Services

| Service | Purpose |
|---------|---------|
| **AWS WAF** | Web application firewall |
| **AWS Shield** | DDoS protection |
| **AWS Network Firewall** | VPC network protection |
| **Route 53 Resolver DNS Firewall** | DNS query filtering |
| **Security Groups** | Instance-level filtering |
| **Network ACLs** | Subnet-level filtering |

---

### Cost Optimization

| Strategy | Description |
|----------|-------------|
| **NAT Gateway consolidation** | Reduce number of NAT Gateways |
| **Gateway Endpoints** | Free S3/DynamoDB access |
| **VPC Endpoint Policies** | Control endpoint access |
| **Data transfer pricing** | Use same-AZ for internal traffic |
| **PrivateLink vs Peering** | Evaluate data transfer costs |

---

## Task 5.2: Configure DNS and Content Delivery

### Amazon Route 53

| Feature | Description |
|---------|-------------|
| **Hosted Zone** | Container for DNS records |
| **Record Types** | A, AAAA, CNAME, MX, TXT, etc. |
| **Alias Records** | AWS-specific, point to AWS resources |
| **Health Checks** | Monitor endpoint availability |
| **Routing Policies** | Traffic distribution methods |

---

### Route 53 Record Types

| Type | Description |
|------|-------------|
| **A** | IPv4 address |
| **AAAA** | IPv6 address |
| **CNAME** | Canonical name (not for apex) |
| **Alias** | AWS resource (works for apex) |
| **MX** | Mail server |
| **TXT** | Text record |
| **NS** | Name server |
| **SOA** | Start of authority |

> **Exam Tip:** CNAME cannot be used for zone apex (example.com). Use Alias instead!

---

### Route 53 Routing Policies (Review)

| Policy | Health Check | Use Case |
|--------|--------------|----------|
| **Simple** | No | Single resource |
| **Failover** | Yes (required) | DR scenarios |
| **Geolocation** | Optional | Location-based content |
| **Geoproximity** | Optional | Traffic shifting |
| **Latency** | Optional | Performance optimization |
| **Multivalue** | Optional | Multiple healthy endpoints |
| **Weighted** | Optional | Load distribution |

---

### Route 53 Resolver

| Feature | Description |
|---------|-------------|
| **Inbound Endpoints** | On-prem to VPC DNS resolution |
| **Outbound Endpoints** | VPC to on-prem DNS resolution |
| **DNS Firewall** | Block/allow DNS queries |
| **Query Logging** | Log DNS queries |

---

### Amazon CloudFront

| Component | Description |
|-----------|-------------|
| **Distribution** | CDN configuration |
| **Origins** | Content sources (S3, ALB, custom) |
| **Edge Locations** | Content cache points |
| **Behaviors** | Path-based settings |
| **Cache Policies** | TTL and key configuration |

#### CloudFront Features
| Feature | Description |
|---------|-------------|
| **Origin Shield** | Additional caching layer |
| **Lambda@Edge** | Edge compute |
| **CloudFront Functions** | Lightweight edge compute |
| **Field-level Encryption** | Encrypt specific fields |
| **Signed URLs/Cookies** | Private content access |

---

### AWS Global Accelerator

| Feature | Description |
|---------|-------------|
| **Static IPs** | Two anycast IPs |
| **Endpoints** | ALB, NLB, EC2, Elastic IP |
| **Health Checks** | Route to healthy endpoints |
| **Traffic Dials** | Control traffic percentage |

#### CloudFront vs Global Accelerator
| Feature | CloudFront | Global Accelerator |
|---------|------------|-------------------|
| **Use Case** | Cacheable content | Non-HTTP, gaming, IoT |
| **Protocol** | HTTP/HTTPS | TCP/UDP |
| **Caching** | Yes | No |
| **Static IP** | No | Yes |

---

## Task 5.3: Troubleshoot Network Connectivity

### Common Connectivity Issues

| Issue | Check |
|-------|-------|
| **No internet access** | Route table, IGW, NAT Gateway |
| **Cannot reach instance** | Security groups, NACLs |
| **Timeout** | Route tables, NACLs (ephemeral ports) |
| **Connection refused** | Service not running, wrong port |
| **DNS resolution fails** | VPC DNS settings, DHCP options |

---

### Troubleshooting Tools

| Tool | Purpose |
|------|---------|
| **VPC Flow Logs** | Network traffic logging |
| **VPC Reachability Analyzer** | Path analysis |
| **CloudWatch Metrics** | Network performance |
| **Traffic Mirroring** | Packet capture |
| **Route Analyzer** | Transit Gateway route analysis |

---

### VPC Flow Logs

| Field | Description |
|-------|-------------|
| **version** | Flow log version |
| **account-id** | AWS account |
| **interface-id** | ENI ID |
| **srcaddr/dstaddr** | Source/destination IP |
| **srcport/dstport** | Source/destination port |
| **protocol** | IANA protocol number |
| **packets/bytes** | Traffic volume |
| **action** | ACCEPT or REJECT |

> **Exam Tip:** REJECT in flow logs usually means security group or NACL blocking!

---

### VPC Reachability Analyzer

| Feature | Description |
|---------|-------------|
| **Path Analysis** | Analyze connectivity between resources |
| **Insights** | Identify blocking components |
| **Hop-by-hop** | Show route through VPC |
| **No packets sent** | Configuration analysis only |

---

### CloudFront Troubleshooting

| Issue | Solution |
|-------|----------|
| **Stale content** | Invalidate cache, check TTL |
| **Access denied** | Check origin access, signed URLs |
| **High latency** | Check origin response, enable compression |
| **Error pages** | Configure custom error responses |

---

### Hybrid and Private Connectivity Issues

| Issue | Check |
|-------|-------|
| **VPN tunnel down** | Phase 1/2 settings, routing |
| **Direct Connect issues** | Virtual interface, BGP |
| **VPC Endpoint not working** | Endpoint policy, security groups |
| **Peering traffic fails** | Route tables on both sides |

---

### Network Monitoring

| Service | Metrics |
|---------|---------|
| **CloudWatch** | NetworkIn/Out, PacketsIn/Out |
| **VPC Flow Logs** | Traffic logs |
| **ELB Access Logs** | Request logs |
| **CloudFront Logs** | Access logs |
| **WAF Logs** | Request sampling |

---

## Exam Tips for Domain 5

1. **Security Groups vs NACLs:**
   - SG = stateful, allow only, instance level
   - NACL = stateless, allow/deny, subnet level
   - NACL needs ephemeral port rules (1024-65535)
2. **NAT Gateway:**
   - AZ-specific, deploy in each AZ for HA
   - No security groups, control with NACLs
   - 5 Gbps baseline, up to 100 Gbps
3. **VPC Endpoints:**
   - Gateway = S3, DynamoDB only (FREE)
   - Interface = most other services (cost per hour)
4. **Route 53 records:**
   - Alias = zone apex (example.com), AWS resources
   - CNAME = cannot be zone apex
5. **VPC Flow Logs analysis:**
   - REJECT = blocked by SG or NACL
   - Check both inbound and outbound rules
6. **VPC Reachability Analyzer:**
   - No packets sent, configuration analysis only
   - Identifies blocking components
7. **CloudFront vs Global Accelerator:**
   - CloudFront = HTTP/HTTPS, caching
   - Global Accelerator = TCP/UDP, static IP, no caching
8. **Transit Gateway:**
   - Hub for multiple VPCs and VPNs
   - Route tables control traffic
   - Cross-region peering available
9. **Subnet sizing:**
   - AWS reserves 5 IPs per subnet
   - /28 = 11 usable, /24 = 251 usable
10. **DNS troubleshooting:**
    - Check VPC DNS settings (enableDnsHostnames, enableDnsSupport)
    - DHCP option sets for custom DNS
11. **Private connectivity options:**
    - VPC Peering = simple, non-transitive
    - Transit Gateway = scalable, transitive
    - PrivateLink = expose services privately
12. **Hybrid connectivity:**
    - Site-to-Site VPN = quick, encrypted, internet-dependent
    - Direct Connect = consistent, higher bandwidth, takes weeks

