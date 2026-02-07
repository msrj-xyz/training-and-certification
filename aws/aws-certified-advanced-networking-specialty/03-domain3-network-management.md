# Domain 3: Network Management and Operation (20%)

## Task 3.1: Maintain Routing and Connectivity

### BGP Route Management ⭐

#### Route Advertisement
| From | To | Method |
|------|-------|--------|
| **On-prem** | AWS | BGP advertise to VGW/TGW |
| **AWS VPC** | On-prem | BGP propagation |
| **VPC** | TGW | Route propagation |

#### Route Limits
| Resource | Limit |
|----------|-------|
| **VPC routes** | 50 static, 100 propagated |
| **TGW routes** | 10,000 per route table |
| **DX Gateway** | 20 routes from on-prem |
| **VGW per VPC** | 100 propagated routes |

---

### Direct Connect Management

#### DX Connection States
| State | Description |
|-------|-------------|
| **Ordering** | Requesting connection |
| **Pending** | Awaiting LOA-CFA |
| **Available** | Ready for VIFs |
| **Down** | Connection issue |
| **Deleted** | Terminated |

#### VIF States
| State | Description |
|-------|-------------|
| **Confirming** | Awaiting confirmation |
| **Verifying** | Verifying configuration |
| **Available** | Ready for traffic |
| **Down** | BGP session down |

---

### Transit Gateway Management ⭐

#### TGW Routing Best Practices
| Practice | Description |
|----------|-------------|
| **Segmentation** | Separate route tables per use case |
| **Blackholes** | Drop unwanted traffic |
| **Prefix Lists** | Managed route references |
| **Propagation** | Auto-learn VPC CIDRs |

#### TGW Network Manager
| Feature | Purpose |
|---------|---------|
| **Global View** | Cross-region topology |
| **Events** | Track topology changes |
| **Insights** | Connectivity issues |
| **Route Analyzer** | Validate paths |

---

### PrivateLink Management

#### Endpoint Service Quotas
| Resource | Limit |
|----------|-------|
| **Endpoints per VPC** | 50 |
| **Endpoint services per Region** | 15 |
| **Subnets per endpoint** | 1 per AZ |

---

## Task 3.2: Monitor and Troubleshoot

### VPC Flow Logs Analysis ⭐

#### Common Flow Log Queries (Athena)
```sql
-- Top talkers
SELECT srcaddr, SUM(bytes) as total_bytes
FROM vpc_flow_logs
WHERE action = 'ACCEPT'
GROUP BY srcaddr
ORDER BY total_bytes DESC
LIMIT 10;

-- Rejected traffic
SELECT srcaddr, dstaddr, dstport, COUNT(*) as count
FROM vpc_flow_logs
WHERE action = 'REJECT'
GROUP BY srcaddr, dstaddr, dstport
ORDER BY count DESC;
```

#### Flow Log Fields (v5)
| Field | Description |
|-------|-------------|
| **pkt-src-aws-service** | Source AWS service |
| **pkt-dst-aws-service** | Destination AWS service |
| **flow-direction** | Ingress or egress |
| **traffic-path** | Path identifier |

---

### VPC Traffic Mirroring

| Component | Description |
|-----------|-------------|
| **Source** | ENI to mirror |
| **Target** | NLB or ENI |
| **Filter** | Traffic selection rules |
| **Session** | Source + Target + Filter |

#### Traffic Mirroring Use Cases
- IDS/IPS analysis
- Compliance monitoring
- Troubleshooting
- Content inspection

---

### Reachability Analyzer ⭐

| Use Case | Analysis |
|----------|----------|
| **Connectivity test** | Source to destination reachability |
| **Block detection** | Identify blocking component |
| **Pre-deployment** | Validate configuration |
| **Compliance** | Verify network segmentation |

#### Analyzed Components
- Route tables
- Security groups
- Network ACLs
- VPC endpoints
- VPC peering
- Transit Gateway

---

### CloudWatch Network Metrics

| Metric | Service | Purpose |
|--------|---------|---------|
| **NetworkIn/Out** | EC2 | Instance throughput |
| **PacketsIn/Out** | EC2 | Packet count |
| **ConnectionEstablished** | DX | DX connection status |
| **VirtualInterfaceBpsEgress** | DX | VIF throughput |
| **BytesProcessed** | NLB | Load balancer traffic |

---

### Troubleshooting Common Issues

| Issue | Cause | Resolution |
|-------|-------|------------|
| **No connectivity** | Route table | Check routes |
| **Intermittent** | Security group | Check inbound/outbound |
| **Slow performance** | MTU mismatch | Enable jumbo frames |
| **DNS failure** | Resolver | Check endpoints |
| **BGP down** | Peer config | Verify ASN, peer IP |

---

## Task 3.3: Optimize Network Performance

### Network Interface Types ⭐

| Interface | Description | Use Case |
|-----------|-------------|----------|
| **ENI** | Standard network interface | General purpose |
| **ENA** | Enhanced Networking Adapter | High throughput |
| **EFA** | Elastic Fabric Adapter | HPC, ML training |

#### ENA Features
- Up to 100 Gbps
- Lower latency
- Higher PPS
- Required for larger instances

#### EFA Features
- OS-bypass
- MPI communication
- NCCL for ML
- HPC workloads only

---

### MTU and Jumbo Frames ⭐

| Path | MTU |
|------|-----|
| **Within VPC** | 9001 (jumbo) |
| **Internet** | 1500 |
| **Direct Connect** | 9001 (private VIF) |
| **VPN** | 1500 |
| **VPC Peering** | 9001 (same region) |
| **TGW** | 8500 |

> **Exam Tip:** Path MTU Discovery (PMTUD) requires ICMP to work!

---

### VPC and Subnet Optimization

#### Secondary CIDR
| Limit | Value |
|-------|-------|
| **Max CIDRs per VPC** | 5 (extendable to 50) |
| **CIDR size** | /16 to /28 |
| **IPv6** | /56 per VPC |

#### IP Address Depletion Solutions
| Solution | Description |
|----------|-------------|
| **Secondary CIDR** | Add more IP space |
| **Larger subnets** | Plan for growth |
| **IPv6** | Dual-stack |
| **PrivateLink** | Reduce need for peering |

---

### Data Transfer Cost Optimization

| Transfer Type | Cost |
|---------------|------|
| **Same AZ (private IP)** | Free |
| **Cross-AZ** | $ |
| **Same Region (public IP)** | $ |
| **Cross-Region** | $$ |
| **Internet** | $$$ |
| **Direct Connect** | Variable |

#### Cost Optimization Strategies
| Strategy | Savings |
|----------|---------|
| **VPC Endpoints** | Avoid NAT Gateway data costs |
| **PrivateLink** | Direct service access |
| **Transit Gateway** | Centralized NAT |
| **Data compression** | Reduce transfer volume |

---

### Route 53 Optimization

#### High Availability DNS
| Feature | Purpose |
|---------|---------|
| **Health checks** | Failover unhealthy endpoints |
| **Failover routing** | Active-passive |
| **Latency routing** | Route to closest |
| **Multivalue** | Return healthy endpoints |

---

## Exam Tips for Domain 3

1. **VPC Flow Logs** = version 5 has AWS service fields
2. **Traffic Mirroring** = packet-level capture to NLB/ENI
3. **Reachability Analyzer** = pre-deployment validation
4. **Network Manager** = global TGW topology view
5. **ENA** = high throughput, low latency
6. **EFA** = HPC, OS-bypass, MPI
7. **Jumbo frames** = 9001 MTU within VPC
8. **TGW MTU** = 8500 bytes
9. **Secondary CIDR** = solve IP depletion
10. **VPC Endpoints** = reduce data transfer costs
