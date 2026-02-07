# AWS Certified Advanced Networking - Specialty (ANS-C01)

## Exam Information

| Aspect | Details |
|--------|---------|
| **Exam Code** | ANS-C01 |
| **Level** | Specialty |
| **Duration** | 170 minutes |
| **Questions** | 65 questions |
| **Passing Score** | 750/1000 |
| **Format** | Multiple choice, multiple response |
| **Cost** | $300 USD |
| **Prerequisite** | 5+ years networking, 2+ years cloud experience |

---

## Domain Weightings

| Domain | Weight | Focus Areas |
|--------|--------|-------------|
| **Domain 1** | 30% | Network Design |
| **Domain 2** | 26% | Network Implementation |
| **Domain 3** | 20% | Network Management and Operation |
| **Domain 4** | 24% | Network Security, Compliance, Governance |

---

## Target Candidate Profile

- 5+ years of networking experience
- 2+ years of cloud and hybrid networking experience
- Deep understanding of AWS networking services
- Experience with hybrid connectivity (Direct Connect, VPN)
- BGP routing expertise
- Network security implementation experience

---

## Key AWS Networking Services

### Connectivity
| Service | Purpose |
|---------|---------|
| **VPC** | Virtual private cloud |
| **Direct Connect** | Dedicated private connection |
| **Site-to-Site VPN** | Encrypted tunnel over internet |
| **Transit Gateway** | Multi-VPC/hybrid hub |
| **VPC Peering** | 1:1 VPC connection |
| **PrivateLink** | Private service access |
| **Cloud WAN** | Global network management |

### DNS & Content Delivery
| Service | Purpose |
|---------|---------|
| **Route 53** | DNS service |
| **Route 53 Resolver** | Hybrid DNS |
| **CloudFront** | CDN |
| **Global Accelerator** | Global traffic acceleration |

### Load Balancing
| Service | Layer | Use Case |
|---------|-------|----------|
| **ALB** | Layer 7 | HTTP/HTTPS |
| **NLB** | Layer 4 | TCP/UDP, ultra-low latency |
| **GWLB** | Layer 3 | Network appliances |
| **CLB** | Layer 4/7 | Legacy |

### Security
| Service | Purpose |
|---------|---------|
| **Security Groups** | Instance-level firewall |
| **Network ACLs** | Subnet-level firewall |
| **Network Firewall** | VPC-level firewall |
| **WAF** | Web application firewall |
| **Shield** | DDoS protection |
| **Firewall Manager** | Centralized firewall rules |

### Monitoring
| Service | Purpose |
|---------|---------|
| **VPC Flow Logs** | Network traffic logging |
| **Traffic Mirroring** | Packet capture |
| **CloudWatch** | Metrics and alarms |
| **Reachability Analyzer** | Path analysis |
| **Network Manager** | Global network visibility |

---

## Key Networking Concepts

### OSI Model in AWS
| Layer | AWS Services |
|-------|-------------|
| **Layer 7** | ALB, WAF, CloudFront |
| **Layer 4** | NLB, Security Groups |
| **Layer 3** | GWLB, Network Firewall, Routing |
| **Layer 2** | Direct Connect VLANs |
| **Layer 1** | Direct Connect physical |

### IP Addressing
- VPC CIDR: /16 to /28
- Secondary CIDR: up to 5 blocks
- IPv6: /56 block per VPC
- Reserved IPs: 5 per subnet

### BGP Concepts
- ASN: Autonomous System Number
- AWS ASN: 64512 (default) or custom
- BGP Communities
- AS Path Prepending
- MED (Multi-Exit Discriminator)

---

## Exam Tips Summary

1. **Know Direct Connect deeply** - VIFs, LAG, resiliency
2. **Master Transit Gateway** - routing, attachments, peering
3. **Understand BGP** - attributes, path selection, communities
4. **Route 53 routing policies** - all 7 types
5. **Load balancer selection** - ALB vs NLB vs GWLB
6. **VPC Flow Logs** - fields, analysis
7. **Network Firewall** - rule types, stateful vs stateless
8. **Hybrid DNS** - Resolver endpoints, forwarding
9. **PrivateLink** - endpoint services, interface endpoints
10. **Network performance** - MTU, jumbo frames, ENA
