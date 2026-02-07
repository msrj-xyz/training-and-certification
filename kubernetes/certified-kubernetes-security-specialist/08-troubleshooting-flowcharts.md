# CKS Security Troubleshooting Flowcharts

## Security Incident Response

```mermaid
flowchart TD
    A[Security Alert] --> B{Alert Source?}
    
    B -->|Falco| C[Runtime Issue]
    C --> C1[kubectl logs -n falco -l app=falco]
    C1 --> C2{What Detected?}
    C2 -->|Shell spawned| C3[Investigate or<br>terminate container]
    C2 -->|File access| C4[Check sensitive files<br>accessed]
    C2 -->|Network| C5[Check outbound<br>connections]
    
    B -->|Audit Log| D[API Activity]
    D --> D1[Check /var/log/kubernetes/audit.log]
    D1 --> D2{Suspicious User?}
    D2 -->|Yes| D3[Revoke access<br>Delete RoleBinding]
    D2 -->|No| D4{Abnormal Actions?}
    D4 -->|Yes| D5[Add NetworkPolicy<br>or tighten RBAC]
    
    B -->|Image CVE| E[Vulnerability]
    E --> E1{Severity?}
    E1 -->|Critical| E2[Patch immediately<br>Rebuild image]
    E1 -->|High| E3[Schedule patch soon]
    E1 -->|Lower| E4[Add to backlog]
    
    style A fill:#ff6b6b
    style C3 fill:#51cf66
    style C4 fill:#51cf66
    style C5 fill:#51cf66
    style D3 fill:#51cf66
    style D5 fill:#51cf66
    style E2 fill:#51cf66
    style E3 fill:#ffd43b
    style E4 fill:#74c0fc
```

---

## RBAC Troubleshooting

```mermaid
flowchart TD
    A[Access Denied] --> B[kubectl auth can-i]
    
    B --> C{Permission?}
    C -->|Denied| D[Check Bindings]
    D --> D1{RoleBinding<br>Exists?}
    D1 -->|No| D2[Create RoleBinding]
    D1 -->|Yes| D3{Correct Subject?}
    D3 -->|No| D4[Fix user/group/SA]
    D3 -->|Yes| D5{Correct Role?}
    D5 -->|No| D6[Fix roleRef]
    D5 -->|Yes| D7{Right Namespace?}
    D7 -->|No| D8[Use ClusterRoleBinding<br>or fix namespace]
    
    C -->|Allowed but fails| E[Check Role]
    E --> E1{Verb Correct?}
    E1 -->|No| E2[Add required verb]
    E1 -->|Yes| E3{Resource Correct?}
    E3 -->|No| E4[Add resource]
    E3 -->|Yes| E5{API Group?}
    E5 -->|Wrong| E6[Fix apiGroups]
    
    style A fill:#ff6b6b
    style D2 fill:#51cf66
    style D4 fill:#51cf66
    style D6 fill:#51cf66
    style D8 fill:#51cf66
    style E2 fill:#51cf66
    style E4 fill:#51cf66
    style E6 fill:#51cf66
```

---

## NetworkPolicy Troubleshooting

```mermaid
flowchart TD
    A[Network Blocked] --> B{NetworkPolicy<br>Exists?}
    
    B -->|No| C[All Traffic Allowed]
    C --> C1{Need Restriction?}
    C1 -->|Yes| C2[Create Default Deny<br>then Allow rules]
    
    B -->|Yes| D[Check Policy]
    D --> D1{Ingress Issue?}
    D1 -->|Yes| E[Check Ingress Rules]
    E --> E1{podSelector<br>Match?}
    E1 -->|No| E2[Fix pod labels]
    E1 -->|Yes| E3{namespaceSelector?}
    E3 -->|Needed| E4[Add namespaceSelector]
    E3 -->|OK| E5{Ports Correct?}
    E5 -->|No| E6[Fix port spec]
    
    D1 -->|No| F{Egress Issue?}
    F -->|Yes| G[Check Egress Rules]
    G --> G1{DNS Allowed?}
    G1 -->|No| G2[Add DNS rule<br>port 53/UDP]
    G1 -->|Yes| G3{Destination<br>Allowed?}
    G3 -->|No| G4[Add egress rule<br>for destination]
    
    style A fill:#ff6b6b
    style C2 fill:#51cf66
    style E2 fill:#51cf66
    style E4 fill:#51cf66
    style E6 fill:#51cf66
    style G2 fill:#51cf66
    style G4 fill:#51cf66
```

---

## Image Security Troubleshooting

```mermaid
flowchart TD
    A[Image Security Issue] --> B{Issue Type?}
    
    B -->|CVE Found| C[Trivy Results]
    C --> C1{Fix Available?}
    C1 -->|Yes| C2[Update base image<br>or dependency]
    C1 -->|No| C3{Critical?}
    C3 -->|Yes| C4[Find alternative<br>package/image]
    C3 -->|No| C5[Add to .trivyignore<br>with justification]
    
    B -->|Untrusted Registry| D[Policy Check]
    D --> D1{Gatekeeper<br>Installed?}
    D1 -->|No| D2[Install OPA/Gatekeeper]
    D1 -->|Yes| D3{Constraint<br>Exists?}
    D3 -->|No| D4[Create K8sAllowedRepos]
    D3 -->|Yes| D5[Update repos list]
    
    B -->|Image Too Large| E[Size Issue]
    E --> E1{Multi-stage?}
    E1 -->|No| E2[Use multi-stage build]
    E1 -->|Yes| E3{Distroless?}
    E3 -->|No| E4[Switch to distroless<br>or alpine]
    
    style A fill:#ff6b6b
    style C2 fill:#51cf66
    style C4 fill:#51cf66
    style C5 fill:#ffd43b
    style D2 fill:#51cf66
    style D4 fill:#51cf66
    style D5 fill:#51cf66
    style E2 fill:#51cf66
    style E4 fill:#51cf66
```

---

## Pod Security Troubleshooting

```mermaid
flowchart TD
    A[Pod Security Violation] --> B{PSS Level?}
    
    B -->|Restricted| C[Check Violations]
    C --> C1{Privileged?}
    C1 -->|Yes| C2[Remove privileged: true]
    C1 -->|No| C3{Running as Root?}
    C3 -->|Yes| C4[Set runAsNonRoot: true<br>runAsUser: 1000]
    C3 -->|No| C5{Privilege<br>Escalation?}
    C5 -->|Yes| C6[Set allowPrivilegeEscalation: false]
    C5 -->|No| C7{Capabilities?}
    C7 -->|Too Many| C8[Drop ALL, add needed]
    
    B -->|Baseline| D[Check Baseline Violations]
    D --> D1{hostNetwork?}
    D1 -->|Yes| D2[Remove hostNetwork<br>or use exemption]
    D1 -->|No| D3{hostPID/<br>hostIPC?}
    D3 -->|Yes| D4[Remove host namespaces]
    
    style A fill:#ff6b6b
    style C2 fill:#51cf66
    style C4 fill:#51cf66
    style C6 fill:#51cf66
    style C8 fill:#51cf66
    style D2 fill:#51cf66
    style D4 fill:#51cf66
```

---

## Quick Security Commands

| Check | Command |
|-------|---------|
| **RBAC test** | `kubectl auth can-i create pods --as <user>` |
| **List permissions** | `kubectl auth can-i --list --as <user>` |
| **Find cluster-admin** | `kubectl get clusterrolebindings -o json \| jq '.items[] \| select(.roleRef.name=="cluster-admin")'` |
| **Scan image** | `trivy image --severity HIGH,CRITICAL <image>` |
| **Falco logs** | `kubectl logs -n falco -l app=falco -f` |
| **Audit logs** | `cat /var/log/kubernetes/audit.log \| jq` |
| **Secret access** | `kubectl auth can-i get secrets --as <user>` |
| **Check PSS** | `kubectl get ns --show-labels` |
| **NetworkPolicies** | `kubectl get networkpolicy -A` |
| **kube-bench** | `kube-bench run --targets master` |
| **AppArmor status** | `aa-status` |
