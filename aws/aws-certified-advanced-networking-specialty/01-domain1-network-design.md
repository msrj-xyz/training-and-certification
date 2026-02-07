# Domain 1: Network Design (30%)

## Task 1.1: Edge Network Services

### Amazon CloudFront ⭐

| Feature | Description |
|---------|-------------|
| **Edge Locations** | 400+ global PoPs |
| **Origin Types** | S3, ALB, EC2, custom HTTP |
| **Caching** | TTL-based, cache behaviors |
| **Security** | OAC, signed URLs, field encryption |
| **Lambda@Edge** | Request/response modification |
| **CloudFront Functions** | Lightweight edge compute |

#### CloudFront Use Cases
| Use Case | Configuration |
|----------|---------------|
| **Static content** | S3 origin with OAC |
| **Dynamic content** | ALB/EC2 origin |
| **API acceleration** | API Gateway origin |
| **Live/VOD streaming** | Media services origin |

---

### AWS Global Accelerator ⭐

| Feature | Description |
|---------|-------------|
| **Static IPs** | 2 anycast IPs |
| **Protocol** | TCP, UDP |
| **Health Checks** | Endpoint health monitoring |
| **Traffic Dial** | Percentage-based routing |
| **Endpoint Groups** | Regional grouping |

#### CloudFront vs Global Accelerator
| Aspect | CloudFront | Global Accelerator |
|--------|------------|-------------------|
| **Content** | Cacheable | Non-cacheable |
| **Protocol** | HTTP/HTTPS | TCP/UDP |
| **IPs** | Dynamic | Static anycast |
| **Use Case** | Web content | Gaming, IoT, VoIP |

---

## Task 1.2: DNS Solutions

### Amazon Route 53 ⭐

| Feature | Description |
|---------|-------------|
| **Public Hosted Zone** | Internet-facing DNS |
| **Private Hosted Zone** | VPC-internal DNS |
| **Health Checks** | Endpoint monitoring |
| **Traffic Flow** | Visual policy editor |
| **DNSSEC** | DNS security |

#### Route 53 Routing Policies ⭐
| Policy | Use Case |
|--------|----------|
| **Simple** | Single resource |
| **Weighted** | Load distribution, A/B testing |
| **Latency** | Lowest latency routing |
| **Failover** | Active-passive DR |
| **Geolocation** | Location-based content |
| **Geoproximity** | Traffic shifting with bias |
| **Multivalue** | Multiple healthy endpoints |

---

### Route 53 Resolver ⭐

| Component | Direction | Purpose |
|-----------|-----------|---------|
| **Inbound Endpoint** | On-prem → AWS | Resolve AWS DNS |
| **Outbound Endpoint** | AWS → On-prem | Resolve on-prem DNS |
| **Resolver Rules** | Forwarding | Conditional forwarding |

#### Hybrid DNS Architecture
```
On-premises                          AWS
    │                                 │
    │   Inbound Endpoint ◄────────────┤ VPC queries on-prem
    │                                 │
    │   Outbound Endpoint ────────────► On-prem queries AWS
    │                                 │
```

> **Exam Tip:** Inbound = queries coming INTO AWS, Outbound = queries going OUT of AWS

---

### DNS Record Types

| Record | Purpose |
|--------|---------|
| **A** | IPv4 address |
| **AAAA** | IPv6 address |
| **CNAME** | Canonical name (alias) |
| **Alias** | AWS resource (free queries) |
| **MX** | Mail exchange |
| **TXT** | Text records (verification) |
| **NS** | Name servers |
| **SOA** | Start of authority |
| **PTR** | Reverse DNS |

---

## Task 1.3: Load Balancing

### Elastic Load Balancing Types ⭐

| Type | Layer | Protocol | Use Case |
|------|-------|----------|----------|
| **ALB** | 7 | HTTP/HTTPS | Web apps, microservices |
| **NLB** | 4 | TCP/UDP/TLS | Ultra-low latency, static IP |
| **GWLB** | 3 | GENEVE | Network appliances |
| **CLB** | 4/7 | TCP/HTTP | Legacy |

#### ALB Features
| Feature | Description |
|---------|-------------|
| **Path-based routing** | Route by URL path |
| **Host-based routing** | Route by hostname |
| **Weighted targets** | Traffic distribution |
| **Lambda targets** | Serverless backend |
| **OIDC auth** | Authenticate users |
| **Sticky sessions** | Session affinity |
| **Cross-zone** | Balance across AZs |

#### NLB Features
| Feature | Description |
|---------|-------------|
| **Static IP** | One IP per AZ |
| **Elastic IP** | Attach EIP |
| **Preserve source IP** | Client IP visible |
| **Ultra-low latency** | Millions of requests/sec |
| **TCP/UDP/TLS** | All layer 4 protocols |
| **PrivateLink** | Create endpoint services |

#### Gateway Load Balancer (GWLB) ⭐
| Feature | Description |
|---------|-------------|
| **GENEVE** | Encapsulation protocol |
| **Transparent** | Bump-in-the-wire |
| **Use Case** | Firewalls, IDS/IPS |
| **Appliances** | Third-party or AWS |

---

### Load Balancer Selection

| Requirement | Choose |
|-------------|--------|
| HTTP routing | **ALB** |
| Static IP | **NLB** |
| Lowest latency | **NLB** |
| Network appliances | **GWLB** |
| gRPC | **ALB** |
| WebSocket | **ALB** or **NLB** |
| PrivateLink service | **NLB** or **GWLB** |

---

## Task 1.4: Logging and Monitoring

### VPC Flow Logs ⭐

| Field | Description |
|-------|-------------|
| **version** | Log format version |
| **account-id** | AWS account |
| **interface-id** | ENI ID |
| **srcaddr/dstaddr** | IP addresses |
| **srcport/dstport** | Ports |
| **protocol** | TCP=6, UDP=17, ICMP=1 |
| **packets/bytes** | Traffic volume |
| **action** | ACCEPT or REJECT |
| **log-status** | OK, NODATA, SKIPDATA |

#### Flow Log Destinations
| Destination | Use Case |
|-------------|----------|
| **CloudWatch Logs** | Real-time analysis |
| **S3** | Long-term storage, Athena |
| **Kinesis Firehose** | Streaming to SIEM |

---

### Network Monitoring Tools

| Tool | Purpose |
|------|---------|
| **VPC Flow Logs** | Traffic metadata |
| **Traffic Mirroring** | Packet capture |
| **Reachability Analyzer** | Path analysis |
| **Network Manager** | Global topology |
| **CloudWatch** | Metrics, alarms |

---

## Task 1.5: Hybrid Connectivity Design

### AWS Direct Connect ⭐

| Component | Description |
|-----------|-------------|
| **Connection** | Physical link (1/10/100 Gbps) |
| **Dedicated** | Full port ownership |
| **Hosted** | Shared via partner |
| **LAG** | Link aggregation (up to 4) |
| **Virtual Interface (VIF)** | Logical connection |

#### VIF Types
| VIF Type | Purpose | BGP Peer |
|----------|---------|----------|
| **Private VIF** | VPC access | VGW |
| **Public VIF** | AWS public services | Public IPs |
| **Transit VIF** | Transit Gateway | TGW |

#### Direct Connect Gateway ⭐
- Connect VIFs to multiple regions
- Associate with VGWs or TGW
- Cross-region, cross-account
- Up to 10 VGWs per DX Gateway
- Up to 3 TGWs per DX Gateway

---

### Site-to-Site VPN

| Feature | Description |
|---------|-------------|
| **Tunnels** | 2 tunnels per connection |
| **Encryption** | IPsec (IKEv1, IKEv2) |
| **Bandwidth** | Up to 1.25 Gbps per tunnel |
| **Accelerated VPN** | Uses Global Accelerator |
| **ECMP** | Equal-cost multi-path |

---

### BGP Routing ⭐

| Attribute | Purpose | Lower/Higher Wins |
|-----------|---------|-------------------|
| **Weight** | Cisco-specific, local | Higher |
| **Local Preference** | iBGP, exit selection | Higher |
| **AS Path** | Path length | Shorter |
| **MED** | Suggest entry point | Lower |
| **Origin** | Route source | IGP > EGP > Incomplete |

#### BGP Path Selection Order
1. Highest Weight (Cisco)
2. Highest Local Preference
3. Locally originated
4. Shortest AS Path
5. Lowest Origin (IGP < EGP < ?)
6. Lowest MED
7. eBGP over iBGP
8. Lowest IGP cost
9. Lowest Router ID

---

## Task 1.6: Multi-VPC Connectivity Design

### Connectivity Options ⭐

| Option | Use Case | Scale |
|--------|----------|-------|
| **VPC Peering** | 1:1 connection | Point-to-point |
| **Transit Gateway** | Hub-and-spoke | Up to 5000 attachments |
| **PrivateLink** | Service access | Producer-consumer |
| **VPC Sharing** | Shared subnets | Within organization |

#### Transit Gateway ⭐
| Feature | Description |
|---------|-------------|
| **Attachments** | VPC, VPN, DX, TGW peering |
| **Route Tables** | Segment traffic |
| **Multicast** | Multicast support |
| **Network Manager** | Global view |
| **Inter-region Peering** | Cross-region connectivity |

#### VPC Peering vs Transit Gateway
| Aspect | VPC Peering | Transit Gateway |
|--------|-------------|-----------------|
| **Topology** | Full mesh | Hub-and-spoke |
| **Transitive** | No | Yes |
| **Scale** | n(n-1)/2 connections | Central hub |
| **Cost** | Data transfer only | Per attachment + data |
| **Bandwidth** | No limit | 50 Gbps per VPC |

---

### IP Address Overlap Solutions

| Scenario | Solution |
|----------|----------|
| **Overlapping CIDRs** | PrivateLink |
| **NAT for overlap** | Custom NAT instances |
| **Service access** | VPC Endpoints |

---

## Exam Tips for Domain 1

1. **CloudFront** = caching, Lambda@Edge, OAC for S3
2. **Global Accelerator** = static IPs, non-cacheable traffic
3. **Route 53 policies** = know all 7 routing types
4. **Resolver endpoints** = inbound (to AWS), outbound (from AWS)
5. **ALB** = Layer 7, path/host routing
6. **NLB** = Layer 4, static IP, ultra-low latency
7. **GWLB** = network appliances, GENEVE
8. **Direct Connect VIFs** = Private (VPC), Public (AWS), Transit (TGW)
9. **BGP** = AS Path prepending, MED, Local Preference
10. **Transit Gateway** = hub-and-spoke, transitive routing
