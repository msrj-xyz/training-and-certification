# Domain 2: Network Implementation (26%)

## Task 2.1: Implement Hybrid Connectivity

### Direct Connect Implementation ⭐

#### Physical Connection Setup
| Step | Description |
|------|-------------|
| 1. **Request Connection** | Console or API |
| 2. **LOA-CFA** | Letter of Authorization |
| 3. **Cross-Connect** | At colocation facility |
| 4. **Create VIF** | Private, Public, or Transit |
| 5. **Configure BGP** | On customer router |

#### Direct Connect Resiliency ⭐
| Level | Description | Cost |
|-------|-------------|------|
| **Maximum** | 2+ locations, 2+ connections each | $$$$ |
| **High** | 2 locations, 1+ connection each | $$$ |
| **Development** | 1 location, 1+ connection | $ |

#### LAG (Link Aggregation Group)
| Feature | Value |
|---------|-------|
| **Max Links** | 4 |
| **Min Active** | 1 (configurable) |
| **Same Speed** | Required |
| **Same Location** | Required |

---

### VPN Implementation

#### Site-to-Site VPN Configuration
| Component | Customer Side | AWS Side |
|-----------|---------------|----------|
| **Gateway** | Customer Gateway | VGW or TGW |
| **Public IP** | CGW IP address | VPN endpoints |
| **BGP ASN** | Customer ASN | 64512 or custom |
| **Tunnels** | 2 (HA) | 2 endpoints |

#### Accelerated VPN
| Feature | Description |
|---------|-------------|
| **Uses** | Global Accelerator |
| **Benefit** | Reduced latency |
| **Route** | AWS backbone instead of internet |
| **Endpoints** | Edge locations |

---

### Transit Gateway Implementation ⭐

#### TGW Attachments
| Type | Use Case |
|------|----------|
| **VPC** | Connect VPCs |
| **VPN** | Site-to-Site VPN |
| **Direct Connect** | Transit VIF |
| **Peering** | Inter-region TGW |
| **Connect** | SD-WAN, GRE |

#### TGW Route Tables
| Concept | Description |
|---------|-------------|
| **Association** | Attachment → Route Table |
| **Propagation** | Routes learned from attachment |
| **Static Routes** | Manually added routes |
| **Blackhole** | Drop traffic |

#### Transit Gateway Connect
| Feature | Description |
|---------|-------------|
| **Protocol** | GRE + BGP |
| **Use Case** | SD-WAN integration |
| **Bandwidth** | Up to 5 Gbps per Connect peer |
| **Peers** | Up to 4 per Connect attachment |

---

### BGP Configuration ⭐

#### Customer Router Configuration
```
router bgp <customer-asn>
  neighbor <aws-peer-ip> remote-as <aws-asn>
  network <on-prem-prefix>
```

#### BGP Attributes for Traffic Engineering
| Goal | Attribute | Action |
|------|-----------|--------|
| **Prefer path** | Local Preference | Higher = preferred |
| **Avoid path** | AS Path Prepend | Longer path = less preferred |
| **Influence AWS** | MED | Lower = preferred |

---

## Task 2.2: Implement Multi-VPC Connectivity

### VPC Peering Implementation

| Step | Action |
|------|--------|
| 1 | Create peering request (requester VPC) |
| 2 | Accept peering request (accepter VPC) |
| 3 | Update route tables (both VPCs) |
| 4 | Update security groups |

#### VPC Peering Rules
- No transitive peering
- No overlapping CIDR
- Cross-region supported
- Cross-account supported
- No edge-to-edge routing

---

### Transit Gateway Implementation ⭐

| Step | Action |
|------|--------|
| 1 | Create Transit Gateway |
| 2 | Create VPC attachments |
| 3 | Create/update route tables |
| 4 | Update VPC route tables |

#### TGW Routing Concepts
| Concept | Description |
|---------|-------------|
| **Default Route Table** | All attachments use by default |
| **Custom Route Tables** | Segment traffic |
| **Route Propagation** | Auto-learn routes |
| **Prefix Lists** | Reference shared prefixes |

---

### PrivateLink Implementation ⭐

#### As Service Provider
| Step | Action |
|------|--------|
| 1 | Create NLB for service |
| 2 | Create VPC Endpoint Service |
| 3 | (Optional) Require acceptance |
| 4 | Share service name |

#### As Service Consumer
| Step | Action |
|------|--------|
| 1 | Create Interface Endpoint |
| 2 | Wait for acceptance |
| 3 | Use endpoint DNS name |

---

### VPC Sharing (AWS RAM) ⭐

| Feature | Description |
|---------|-------------|
| **Share** | Subnets across accounts |
| **Owner** | Controls VPC/subnet |
| **Participant** | Creates resources in subnet |
| **Organizations** | Required for sharing |

---

## Task 2.3: Implement DNS Architectures

### Route 53 Private Hosted Zone

| Step | Action |
|------|--------|
| 1 | Create private hosted zone |
| 2 | Associate with VPCs |
| 3 | Create DNS records |
| 4 | Test resolution |

#### Cross-Account PHZ Association
| Method | Description |
|--------|-------------|
| **CLI** | create-vpc-association-authorization |
| **RAM** | Share via Resource Access Manager |

---

### Route 53 Resolver Rules ⭐

| Rule Type | Use Case |
|-----------|----------|
| **Forward** | Send to specific DNS servers |
| **System** | Use default Route 53 behavior |
| **Recursive** | (AWS internal) |

#### Hybrid DNS Architecture
```
On-premises DNS ◄──────► Outbound Endpoint ──► VPC
                                              ▲
On-premises Apps ──────► Inbound Endpoint ────┘
```

---

### DNSSEC Implementation

| Step | Action |
|------|--------|
| 1 | Enable DNSSEC signing in Route 53 |
| 2 | Create KSK (Key-Signing Key) |
| 3 | Create DS record at registrar |
| 4 | Enable DNSSEC validation |

---

## Task 2.4: Network Automation

### Infrastructure as Code

| Tool | Use Case |
|------|----------|
| **CloudFormation** | AWS native IaC |
| **CDK** | Programmatic IaC |
| **Terraform** | Multi-cloud IaC |
| **CLI/SDK** | Scripting |

#### CloudFormation for Networking
```yaml
Resources:
  MyVPC:
    Type: AWS::EC2::VPC
    Properties:
      CidrBlock: 10.0.0.0/16
      EnableDnsHostnames: true
      EnableDnsSupport: true
```

---

### Event-Driven Automation

| Trigger | Action |
|---------|--------|
| **VPC Flow Log** | CloudWatch → Lambda → Block IP |
| **Route change** | EventBridge → SNS notification |
| **DX down** | EventBridge → Failover to VPN |
| **Unhealthy target** | ALB → Replace instance |

---

### Reachability Analyzer

| Use Case | Benefit |
|----------|---------|
| **Pre-deployment** | Validate design |
| **Troubleshooting** | Identify block points |
| **Compliance** | Verify segmentation |
| **Automation** | CI/CD network validation |

---

## Exam Tips for Domain 2

1. **Direct Connect setup** = LOA-CFA → cross-connect → VIF → BGP
2. **DX resiliency** = 2 locations for high availability
3. **LAG** = same speed, same location, max 4 links
4. **TGW attachments** = VPC, VPN, DX Transit VIF, Peering, Connect
5. **TGW Connect** = SD-WAN integration, GRE + BGP
6. **VPC Peering** = no transitive, no overlap
7. **PrivateLink** = NLB required for endpoint service
8. **Resolver rules** = Forward rules for hybrid DNS
9. **RAM** = share subnets, TGW, Resolver rules
10. **CloudFormation StackSets** = multi-account/region deployment
