# AWS Advanced Networking Specialty - Quick Reference Card

## Domain Weights
| Domain | Weight |
|--------|--------|
| D1 - Network Design | 30% |
| D2 - Network Implementation | 26% |
| D3 - Network Management | 20% |
| D4 - Network Security | 24% |

---

## Connectivity Options

| Need | Solution |
|------|----------|
| Multi-VPC hub | **Transit Gateway** |
| 1:1 VPC connection | **VPC Peering** |
| Service access | **PrivateLink** |
| AWS service access | **VPC Endpoints** |
| Dedicated connection | **Direct Connect** |
| Quick encrypted | **Site-to-Site VPN** |
| Global acceleration | **Global Accelerator** |

---

## Direct Connect Quick Reference

| Component | Purpose |
|-----------|---------|
| **Dedicated** | Full port (1/10/100 Gbps) |
| **Hosted** | Shared via partner |
| **Private VIF** | VPC access |
| **Public VIF** | AWS public services |
| **Transit VIF** | Transit Gateway |
| **DX Gateway** | Multi-region access |
| **LAG** | Link aggregation (max 4) |
| **MACsec** | Layer 2 encryption |

---

## Transit Gateway Quick Reference

| Feature | Description |
|---------|-------------|
| **Attachments** | VPC, VPN, DX, Peering, Connect |
| **Route Tables** | Segment traffic |
| **Max Bandwidth** | 50 Gbps per VPC |
| **Connect** | SD-WAN (GRE + BGP) |
| **Multicast** | Supported |
| **MTU** | 8500 bytes |

---

## Load Balancer Selection

| Requirement | Choose |
|-------------|--------|
| HTTP routing | **ALB** |
| Static IP | **NLB** |
| Lowest latency | **NLB** |
| Network appliances | **GWLB** |
| Lambda target | **ALB** |
| PrivateLink | **NLB** or **GWLB** |

---

## Route 53 Routing Policies

| Policy | Use Case |
|--------|----------|
| Simple | Single resource |
| Weighted | A/B testing |
| Latency | Lowest latency |
| Failover | DR |
| Geolocation | Location-based |
| Geoproximity | Traffic shifting |
| Multivalue | Multiple healthy |

---

## Route 53 Resolver

| Endpoint | Direction |
|----------|-----------|
| **Inbound** | On-prem → AWS |
| **Outbound** | AWS → On-prem |
| **Forward Rules** | Conditional forwarding |

---

## Security Controls

| Layer | Control |
|-------|---------|
| Edge | WAF, Shield, CloudFront |
| VPC | Network Firewall |
| Subnet | NACLs |
| Instance | Security Groups |

---

## Security Groups vs NACLs

| Aspect | SG | NACL |
|--------|-----|------|
| Level | Instance | Subnet |
| State | Stateful | Stateless |
| Rules | Allow | Allow + Deny |
| Evaluation | All | In order |

---

## BGP Path Selection (Order)

1. Highest Weight
2. Highest Local Preference
3. Locally originated
4. Shortest AS Path
5. Lowest Origin
6. Lowest MED
7. eBGP over iBGP
8. Lowest Router ID

---

## MTU Reference

| Path | MTU |
|------|-----|
| VPC internal | 9001 (jumbo) |
| Transit Gateway | 8500 |
| VPC Peering (same region) | 9001 |
| Internet | 1500 |
| VPN | 1500 |
| Direct Connect | 9001 |

---

## Network Interfaces

| Type | Use Case |
|------|----------|
| **ENI** | Standard |
| **ENA** | High throughput |
| **EFA** | HPC, ML |

---

## Encryption Options

| Layer | Solution |
|-------|----------|
| Layer 2 | **MACsec** (DX) |
| Layer 3 | **IPsec** (VPN) |
| Layer 4 | **TLS** (NLB) |
| Layer 7 | **HTTPS** (ALB, CloudFront) |

---

## VPC Flow Logs

| Field | Common Values |
|-------|---------------|
| action | ACCEPT, REJECT |
| protocol | 6 (TCP), 17 (UDP), 1 (ICMP) |
| Destinations | CloudWatch, S3, Kinesis |

---

## Exam Day Reminders

1. **Transit Gateway** = hub-and-spoke, transitive routing
2. **VPC Peering** = no transitive, no overlap
3. **Private VIF** = VPC access via VGW
4. **Transit VIF** = TGW access
5. **MACsec** = 10G/100G dedicated DX only
6. **NLB** = static IP, preserve source IP
7. **GWLB** = GENEVE, transparent
8. **Resolver Inbound** = queries coming TO AWS
9. **Jumbo frames** = 9001 in VPC, 8500 via TGW
10. **AS Path Prepending** = make path less preferred
