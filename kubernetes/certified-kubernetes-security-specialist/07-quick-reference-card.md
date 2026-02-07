# CKS Quick Reference Card

## Domain Weights
| Domain | Weight |
|--------|--------|
| D1 - Cluster Setup | 15% |
| D2 - Cluster Hardening | 15% |
| D3 - System Hardening | 10% |
| D4 - Minimize Microservice Vulnerabilities | 20% |
| D5 - Supply Chain Security | 20% |
| D6 - Monitoring, Logging, Runtime Security | 20% |

---

## Security Tools

| Tool | Purpose |
|------|---------|
| **kube-bench** | CIS benchmark scanning |
| **Trivy** | Image vulnerability scanning |
| **Kubesec** | YAML security analysis |
| **Falco** | Runtime syscall monitoring |
| **OPA/Gatekeeper** | Policy enforcement |
| **AppArmor** | Kernel security profiles |
| **seccomp** | Syscall filtering |

---

## Network Security

### Default Deny Policy
```yaml
apiVersion: networking.k8s.io/v1
kind: NetworkPolicy
metadata:
  name: deny-all
spec:
  podSelector: {}
  policyTypes:
  - Ingress
  - Egress
```

### Block Metadata Service
```yaml
egress:
- to:
  - ipBlock:
      cidr: 0.0.0.0/0
      except:
      - 169.254.169.254/32
```

---

## Pod Security

### Restricted Security Context
```yaml
securityContext:
  runAsNonRoot: true
  runAsUser: 1000
  readOnlyRootFilesystem: true
  allowPrivilegeEscalation: false
  capabilities:
    drop: ["ALL"]
  seccompProfile:
    type: RuntimeDefault
```

### Pod Security Standards (PSS)
```bash
kubectl label ns production \
  pod-security.kubernetes.io/enforce=restricted
```

---

## Secrets Encryption

### Encryption Config
```yaml
apiVersion: apiserver.config.k8s.io/v1
kind: EncryptionConfiguration
resources:
- resources: ["secrets"]
  providers:
  - aescbc:
      keys:
      - name: key1
        secret: <BASE64_32_BYTE_KEY>
  - identity: {}
```

### Generate Key
```bash
head -c 32 /dev/urandom | base64
```

---

## Image Security

### Trivy Scan
```bash
trivy image --severity HIGH,CRITICAL nginx:1.24
trivy image --ignore-unfixed nginx:1.24
```

### Kubesec Scan
```bash
kubesec scan pod.yaml
```

---

## Runtime Security

### Falco Key Commands
```bash
# View Falco alerts
kubectl logs -n falco -l app=falco -f

# Custom rules location
/etc/falco/rules.d/
```

### Audit Logging
```yaml
# kube-apiserver flags
--audit-log-path=/var/log/kubernetes/audit.log
--audit-policy-file=/etc/kubernetes/audit/policy.yaml
```

---

## RBAC Quick Commands

```bash
# Check permissions
kubectl auth can-i create pods --as <user>
kubectl auth can-i --list --as <user>

# Create role
kubectl create role pod-reader \
  --verb=get,list,watch --resource=pods

# Create binding
kubectl create rolebinding read-pods \
  --role=pod-reader --user=jane
```

---

## ServiceAccount Security

```yaml
# Disable auto-mount
automountServiceAccountToken: false

# Short-lived token
volumes:
- name: token
  projected:
    sources:
    - serviceAccountToken:
        expirationSeconds: 3600
```

---

## AppArmor & seccomp

### AppArmor Annotation
```yaml
annotations:
  container.apparmor.security.beta.kubernetes.io/<container>: localhost/<profile>
```

### seccomp in SecurityContext
```yaml
securityContext:
  seccompProfile:
    type: RuntimeDefault  # or Localhost
    localhostProfile: profiles/audit.json
```

---

## Key File Locations

| File | Path |
|------|------|
| **API Server manifest** | /etc/kubernetes/manifests/kube-apiserver.yaml |
| **Encryption config** | /etc/kubernetes/enc/enc.yaml |
| **Audit policy** | /etc/kubernetes/audit/policy.yaml |
| **Audit logs** | /var/log/kubernetes/audit.log |
| **seccomp profiles** | /var/lib/kubelet/seccomp/ |
| **AppArmor profiles** | /etc/apparmor.d/ |
| **Falco rules** | /etc/falco/rules.d/ |
| **kubelet config** | /var/lib/kubelet/config.yaml |

---

## Exam Day Checklist

1. ✅ **CIS Benchmarks** - kube-bench for compliance
2. ✅ **Network Policies** - Default deny, explicit allow
3. ✅ **RBAC** - Least privilege, no cluster-admin
4. ✅ **ServiceAccounts** - Disable auto-mount, dedicated SAs
5. ✅ **Secrets** - Encrypt at rest
6. ✅ **Pod Security** - Restricted PSS, security context
7. ✅ **Image scanning** - Trivy for vulnerabilities
8. ✅ **AppArmor/seccomp** - Kernel hardening
9. ✅ **Falco** - Runtime monitoring
10. ✅ **Audit logs** - Enable and configure

---

## Critical Reminders

| Topic | Remember |
|-------|----------|
| **NetworkPolicy** | Requires CNI support (Calico, Cilium) |
| **Trivy** | Primary image scanner |
| **Falco** | Runtime syscall monitoring |
| **PSS Labels** | enforce, warn, audit |
| **readOnlyRootFilesystem** | Use emptyDir for writable paths |
| **allowPrivilegeEscalation** | Always false |
| **capabilities** | Drop ALL, add only needed |
| **ServiceAccount** | Don't use default |
| **Audit levels** | None < Metadata < Request < RequestResponse |
| **Encryption** | aescbc provider for secrets |

---

## Quick Security Checklist

| ✓ | Control |
|---|---------|
| ☐ | Anonymous auth disabled |
| ☐ | RBAC enabled |
| ☐ | Network policies applied |
| ☐ | Secrets encrypted at rest |
| ☐ | Pod security context configured |
| ☐ | Images scanned for vulnerabilities |
| ☐ | Audit logging enabled |
| ☐ | Runtime monitoring (Falco) active |
| ☐ | ServiceAccounts restricted |
| ☐ | TLS for ingress |
