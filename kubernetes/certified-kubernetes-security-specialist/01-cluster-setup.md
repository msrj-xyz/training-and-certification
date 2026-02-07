# Domain 1: Cluster Setup (15%)

## Task 1.1: Use Network Policies to Restrict Cluster-Level Access ⭐

### Default Deny All Traffic

```yaml
apiVersion: networking.k8s.io/v1
kind: NetworkPolicy
metadata:
  name: deny-all
  namespace: production
spec:
  podSelector: {}
  policyTypes:
  - Ingress
  - Egress
```

### Allow Specific Ingress

```yaml
apiVersion: networking.k8s.io/v1
kind: NetworkPolicy
metadata:
  name: allow-frontend-to-backend
  namespace: production
spec:
  podSelector:
    matchLabels:
      app: backend
  policyTypes:
  - Ingress
  ingress:
  - from:
    - podSelector:
        matchLabels:
          app: frontend
    ports:
    - protocol: TCP
      port: 8080
```

### Allow DNS Egress

```yaml
apiVersion: networking.k8s.io/v1
kind: NetworkPolicy
metadata:
  name: allow-dns
  namespace: production
spec:
  podSelector: {}
  policyTypes:
  - Egress
  egress:
  - to:
    - namespaceSelector:
        matchLabels:
          kubernetes.io/metadata.name: kube-system
    ports:
    - protocol: UDP
      port: 53
```

### Namespace Isolation

```yaml
apiVersion: networking.k8s.io/v1
kind: NetworkPolicy
metadata:
  name: deny-from-other-namespaces
  namespace: production
spec:
  podSelector: {}
  ingress:
  - from:
    - podSelector: {}  # Only from same namespace
```

> **Exam Tip:** NetworkPolicy requires a CNI that supports it (Calico, Cilium, Weave)!

---

## Task 1.2: Use CIS Benchmark to Review Kubernetes Security ⭐⭐

### Run kube-bench

```bash
# Run kube-bench on control plane
kube-bench run --targets master

# Run on worker node
kube-bench run --targets node

# Run all tests
kube-bench run

# Run specific section
kube-bench run --targets master --check 1.2

# Output as JSON
kube-bench run --json
```

### CIS Benchmark Categories

| Section | Target |
|---------|--------|
| **1.x** | Control Plane Components |
| **2.x** | etcd |
| **3.x** | Control Plane Configuration |
| **4.x** | Worker Nodes |
| **5.x** | Policies |

### Key Security Configurations

| Component | Key Settings |
|-----------|--------------|
| **API Server** | --anonymous-auth=false, --authorization-mode=RBAC |
| **etcd** | Encrypted, TLS enabled |
| **kubelet** | --anonymous-auth=false, --authorization-mode=Webhook |
| **Controller Manager** | --use-service-account-credentials=true |
| **Scheduler** | --profiling=false |

### API Server Security Flags

```bash
# Check current flags
cat /etc/kubernetes/manifests/kube-apiserver.yaml | grep -E "anonymous|authorization|audit"

# Recommended settings
--anonymous-auth=false
--authorization-mode=Node,RBAC
--enable-admission-plugins=NodeRestriction,PodSecurityAdmission
--audit-log-path=/var/log/kubernetes/audit.log
--audit-log-maxage=30
--audit-log-maxbackup=10
--audit-log-maxsize=100
--encryption-provider-config=/etc/kubernetes/enc/enc.yaml
```

---

## Task 1.3: Properly Set Up Ingress with TLS ⭐

### Create TLS Secret

```bash
# Generate self-signed certificate
openssl req -x509 -nodes -days 365 -newkey rsa:2048 \
  -keyout tls.key -out tls.crt \
  -subj "/CN=app.example.com"

# Create TLS secret
kubectl create secret tls ingress-tls \
  --cert=tls.crt \
  --key=tls.key \
  -n production
```

### Ingress with TLS

```yaml
apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  name: secure-ingress
  namespace: production
  annotations:
    nginx.ingress.kubernetes.io/ssl-redirect: "true"
    nginx.ingress.kubernetes.io/force-ssl-redirect: "true"
spec:
  ingressClassName: nginx
  tls:
  - hosts:
    - app.example.com
    secretName: ingress-tls
  rules:
  - host: app.example.com
    http:
      paths:
      - path: /
        pathType: Prefix
        backend:
          service:
            name: app-service
            port:
              number: 80
```

---

## Task 1.4: Protect Node Metadata and Endpoints ⭐

### Block Cloud Metadata Access

```yaml
apiVersion: networking.k8s.io/v1
kind: NetworkPolicy
metadata:
  name: deny-cloud-metadata
  namespace: default
spec:
  podSelector: {}
  policyTypes:
  - Egress
  egress:
  - to:
    - ipBlock:
        cidr: 0.0.0.0/0
        except:
        - 169.254.169.254/32  # AWS/GCP metadata
        - 100.100.100.200/32  # Alibaba metadata
```

### kubelet Security

```yaml
# /var/lib/kubelet/config.yaml
authentication:
  anonymous:
    enabled: false
  webhook:
    enabled: true
authorization:
  mode: Webhook
readOnlyPort: 0  # Disable unauthenticated read-only port
```

### Disable kubelet Read-Only Port

```bash
# Check read-only port
curl -k https://<node-ip>:10250/pods
curl http://<node-ip>:10255/pods  # Should be disabled

# Verify in kubelet config
cat /var/lib/kubelet/config.yaml | grep readOnlyPort
```

---

## Task 1.5: Verify Platform Binaries Before Deployment ⭐

### Verify Binary Checksums

```bash
# Download kubectl
curl -LO "https://dl.k8s.io/release/v1.34.0/bin/linux/amd64/kubectl"

# Download checksum
curl -LO "https://dl.k8s.io/release/v1.34.0/bin/linux/amd64/kubectl.sha256"

# Verify
echo "$(cat kubectl.sha256)  kubectl" | sha256sum --check
# Expected: kubectl: OK

# Alternative method
sha256sum kubectl | awk '{print $1}'
cat kubectl.sha256
# Compare manually
```

### Verify API Server Binary

```bash
# Get current binary hash
sha256sum /usr/local/bin/kube-apiserver

# Compare with official release
# Download from: https://github.com/kubernetes/kubernetes/releases
```

---

## Task 1.6: Minimize External Access to Network ⭐

### Restrict API Server Access

```bash
# Limit API server to specific IPs
# In kube-apiserver manifest:
--advertise-address=<INTERNAL_IP>
--bind-address=0.0.0.0

# Or use firewall rules
iptables -A INPUT -p tcp --dport 6443 -s 10.0.0.0/8 -j ACCEPT
iptables -A INPUT -p tcp --dport 6443 -j DROP
```

### NodePort Security

```yaml
# Limit NodePort range in API server
--service-node-port-range=30000-32767

# Or disable NodePort entirely via admission controller
```

---

## Exam Tips for Domain 1

1. **NetworkPolicy** - Default deny, then allow specific
2. **kube-bench** - CIS benchmark tool for compliance
3. **TLS Ingress** - Always use HTTPS, force redirect
4. **Metadata blocking** - Block 169.254.169.254
5. **kubelet** - Disable anonymous auth, read-only port
6. **Binary verification** - sha256sum for checksums
7. **API Server flags** - Know security-related flags
8. **Anonymous auth** - Should be disabled
9. **Authorization mode** - Use RBAC, not AlwaysAllow
10. **Audit logging** - Enable for compliance
