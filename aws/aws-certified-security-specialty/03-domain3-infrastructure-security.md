# Domain 3: Infrastructure Security (18%)

## Task 3.1: Security Controls for Network Edge Services

### AWS Edge Security Services ⭐

| Service | Purpose |
|---------|---------|
| **CloudFront** | CDN, edge caching |
| **AWS WAF** | Web application firewall |
| **AWS Shield** | DDoS protection |
| **Global Accelerator** | Global traffic acceleration |
| **Route 53** | DNS, routing |

---

### AWS WAF ⭐

| Component | Description |
|-----------|-------------|
| **Web ACLs** | Container for rules |
| **Rules** | Match conditions and actions |
| **Rule Groups** | Reusable rule collections |
| **Managed Rules** | AWS and partner rules |

#### WAF Rule Types
| Type | Use Case |
|------|----------|
| **Rate-based** | DDoS, brute force protection |
| **IP Match** | Block/allow specific IPs |
| **Geo Match** | Country-based filtering |
| **Regex Pattern** | Custom pattern matching |
| **SQL Injection** | SQLi protection |
| **XSS** | Cross-site scripting protection |
| **Bot Control** | Bot management |

#### WAF Managed Rules ⭐
| Rule Group | Protection |
|------------|------------|
| **Core Rule Set (CRS)** | OWASP Top 10 |
| **Known Bad Inputs** | Common exploits |
| **SQL Database** | SQLi protection |
| **Linux/POSIX** | LFI, command injection |
| **Bot Control** | Bot detection |
| **Account Takeover** | Credential stuffing |

---

### AWS Shield ⭐

| Tier | Features |
|------|----------|
| **Standard** | Free, L3/L4 protection |
| **Advanced** | DRT support, L7 protection, cost protection |

#### Shield Advanced Features
- 24/7 DDoS Response Team (DRT)
- Cost protection for scaling
- Advanced reporting
- WAF rules at no extra cost
- Health-based detection
- Application layer protection

---

### CloudFront Security

| Feature | Purpose |
|---------|---------|
| **Origin Access Control (OAC)** | Secure S3 access |
| **Field-Level Encryption** | Encrypt sensitive fields |
| **Signed URLs/Cookies** | Restrict access |
| **HTTPS Enforcement** | Viewer and origin policies |
| **Security Headers** | CSP, HSTS, etc. |

---

### OWASP Top 10 Protection

| Threat | AWS Protection |
|--------|----------------|
| **Injection** | WAF SQLi/XSS rules |
| **Broken Auth** | Cognito, WAF ATP |
| **Sensitive Data** | ACM, KMS encryption |
| **XXE** | WAF managed rules |
| **Broken Access** | IAM, Cognito |
| **Misconfiguration** | Config, Security Hub |
| **XSS** | WAF XSS rules |
| **Insecure Deserialization** | Inspector, code review |
| **Vulnerable Components** | Inspector, ECR scanning |
| **Logging Failures** | CloudTrail, CloudWatch |

---

## Task 3.2: Security Controls for Compute Workloads

### EC2 Security ⭐

| Control | Implementation |
|---------|----------------|
| **Golden AMIs** | EC2 Image Builder, hardened images |
| **Instance Profiles** | IAM roles for EC2 |
| **Security Groups** | Instance-level firewall |
| **SSM Session Manager** | Secure SSH alternative |
| **Inspector** | Vulnerability scanning |
| **Patch Manager** | Automated patching |

#### EC2 Image Builder
| Feature | Purpose |
|---------|---------|
| **Image Pipeline** | Automated AMI creation |
| **Components** | Build and test scripts |
| **Recipes** | AMI configuration |
| **Distribution** | Cross-account, cross-region |
| **STIG Hardening** | Security baseline |

---

### Container Security

| Layer | Controls |
|-------|----------|
| **Registry** | ECR image scanning, IAM |
| **Image** | Minimal base images, no secrets |
| **Runtime** | GuardDuty, Falco |
| **Network** | Security groups, network policies |
| **Secrets** | Secrets Manager, SSM Parameters |

#### ECR Security Features
- Image scanning (Inspector integration)
- Image tag immutability
- Lifecycle policies
- Cross-account access
- Encryption at rest

---

### Lambda Security

| Control | Implementation |
|---------|----------------|
| **Execution Role** | Least privilege IAM |
| **VPC** | Deploy in VPC if needed |
| **Environment Variables** | Encrypted with KMS |
| **Code Signing** | Verify source |
| **Inspector** | Vulnerability scanning |
| **Layers** | Shared dependencies |

---

### Secure Administrative Access ⭐

| Method | Description |
|--------|-------------|
| **Session Manager** | No SSH keys, IAM-based |
| **EC2 Instance Connect** | Temporary SSH keys |
| **RDP via Fleet Manager** | Windows remote access |
| **Bastion Host** | Jump server pattern |

> **Exam Tip:** Session Manager eliminates need for SSH bastion hosts!

---

### Pipeline Security

| Tool | Purpose |
|------|---------|
| **CodeGuru Security** | SAST scanning |
| **Amazon Inspector** | Dependency scanning |
| **Amazon Q Developer** | Security recommendations |
| **SAST/DAST** | Static/dynamic analysis |
| **SCA** | Software composition analysis |

---

### GenAI Security (OWASP LLM Top 10)

| Threat | Protection |
|--------|------------|
| **Prompt Injection** | Input validation, guardrails |
| **Data Leakage** | Output filtering |
| **Training Data Poisoning** | Data validation |
| **Model Denial of Service** | Rate limiting |
| **Insecure Output** | Output sanitization |

---

## Task 3.3: Network Security Controls

### VPC Security ⭐

| Control | Layer | Direction |
|---------|-------|-----------|
| **Security Groups** | Instance | Stateful |
| **Network ACLs** | Subnet | Stateless |
| **Network Firewall** | VPC | Stateful/Stateless |
| **Traffic Mirroring** | ENI | Capture packets |

#### Security Groups vs NACLs
| Aspect | Security Groups | NACLs |
|--------|-----------------|-------|
| **Level** | Instance/ENI | Subnet |
| **State** | Stateful | Stateless |
| **Rules** | Allow only | Allow and Deny |
| **Evaluation** | All rules | In order |
| **Default** | Deny all inbound | Allow all |

---

### AWS Network Firewall ⭐

| Feature | Description |
|---------|-------------|
| **Stateful Rules** | 5-tuple, domain, Suricata |
| **Stateless Rules** | Simple packet filtering |
| **Rule Actions** | Pass, Drop, Alert |
| **Domain Filtering** | Allow/deny domains |
| **TLS Inspection** | Decrypt and inspect |
| **Centralized** | With Firewall Manager |

---

### Network Segmentation

| Pattern | Implementation |
|---------|----------------|
| **North/South** | VPC boundaries, NAT |
| **East/West** | Security groups, Network Firewall |
| **Micro-segmentation** | Security groups per tier |
| **Isolated Subnets** | No internet access |

---

### Hybrid Connectivity Security

| Connection | Security Features |
|------------|-------------------|
| **Site-to-Site VPN** | IPsec encryption |
| **Direct Connect** | MACsec (layer 2 encryption) |
| **Client VPN** | Certificate-based auth |
| **Verified Access** | Zero-trust access |

#### AWS Verified Access ⭐
| Feature | Description |
|---------|-------------|
| **Zero Trust** | Continuous verification |
| **Identity Provider** | IAM Identity Center, OIDC |
| **Device Posture** | Trust providers |
| **Application Access** | Per-application policies |
| **No VPN Required** | Direct secure access |

---

### Network Access Analysis

| Tool | Purpose |
|------|---------|
| **Network Access Analyzer** | Verify access paths |
| **VPC Reachability Analyzer** | Path diagnostics |
| **Inspector Network Findings** | Open port analysis |
| **VPC Flow Logs** | Traffic analysis |

---

## Exam Tips for Domain 3

1. **WAF** = Layer 7, web protection (SQLi, XSS)
2. **Shield Standard** = free L3/L4 DDoS
3. **Shield Advanced** = DRT, L7, cost protection
4. **Network Firewall** = VPC-level stateful inspection
5. **Security Groups** = stateful, instance-level
6. **NACLs** = stateless, subnet-level
7. **Session Manager** = no SSH keys, audit logged
8. **Verified Access** = zero-trust, no VPN needed
9. **EC2 Image Builder** = hardened AMIs
10. **Inspector** = scan EC2, Lambda, ECR for vulnerabilities
