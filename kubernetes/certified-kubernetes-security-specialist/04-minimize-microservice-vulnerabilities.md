# Domain 4: Minimize Microservice Vulnerabilities (20%)

## Task 4.1: Set Up OS-Level Security Domains ⭐⭐

### Pod Security Standards (PSS)

| Level | Description |
|-------|-------------|
| **Privileged** | Unrestricted, for system pods |
| **Baseline** | Minimal restrictions, easy adoption |
| **Restricted** | Hardened, security best practices |

### Enforce Pod Security at Namespace Level

```bash
# Add labels to namespace
kubectl label namespace production \
  pod-security.kubernetes.io/enforce=restricted \
  pod-security.kubernetes.io/warn=restricted \
  pod-security.kubernetes.io/audit=restricted
```

### Namespace with PSS Labels

```yaml
apiVersion: v1
kind: Namespace
metadata:
  name: production
  labels:
    pod-security.kubernetes.io/enforce: restricted
    pod-security.kubernetes.io/enforce-version: latest
    pod-security.kubernetes.io/warn: restricted
    pod-security.kubernetes.io/warn-version: latest
    pod-security.kubernetes.io/audit: restricted
    pod-security.kubernetes.io/audit-version: latest
```

### PSS Modes

| Mode | Behavior |
|------|----------|
| **enforce** | Reject violating pods |
| **warn** | Allow but show warning |
| **audit** | Log violations to audit log |

---

## Task 4.2: Security Context ⭐⭐

### Pod Security Context

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: secure-pod
spec:
  securityContext:
    runAsUser: 1000
    runAsGroup: 3000
    fsGroup: 2000
    runAsNonRoot: true
    seccompProfile:
      type: RuntimeDefault
  containers:
  - name: app
    image: nginx
    securityContext:
      allowPrivilegeEscalation: false
      readOnlyRootFilesystem: true
      capabilities:
        drop:
          - ALL
```

### Security Context Fields

| Field | Description | Best Practice |
|-------|-------------|---------------|
| **runAsNonRoot** | Must run as non-root | true |
| **runAsUser** | Specific UID | Non-zero |
| **readOnlyRootFilesystem** | Immutable container | true |
| **allowPrivilegeEscalation** | Prevent sudo-like elevation | false |
| **capabilities** | Linux capabilities | Drop ALL |

### Drop All Capabilities

```yaml
securityContext:
  capabilities:
    drop:
      - ALL
    add:
      - NET_BIND_SERVICE  # Only if needed
```

---

## Task 4.3: Manage Kubernetes Secrets ⭐⭐

### Enable Encryption at Rest

```yaml
# /etc/kubernetes/enc/enc.yaml
apiVersion: apiserver.config.k8s.io/v1
kind: EncryptionConfiguration
resources:
  - resources:
      - secrets
    providers:
      - aescbc:
          keys:
            - name: key1
              secret: <BASE64_ENCODED_32_BYTE_KEY>
      - identity: {}
```

### Configure API Server

```yaml
# Add to kube-apiserver.yaml
spec:
  containers:
  - command:
    - kube-apiserver
    - --encryption-provider-config=/etc/kubernetes/enc/enc.yaml
    volumeMounts:
    - name: enc
      mountPath: /etc/kubernetes/enc
      readOnly: true
  volumes:
  - name: enc
    hostPath:
      path: /etc/kubernetes/enc
      type: DirectoryOrCreate
```

### Generate Encryption Key

```bash
# Generate 32-byte key
head -c 32 /dev/urandom | base64
```

### Re-encrypt Existing Secrets

```bash
# Force re-encryption of all secrets
kubectl get secrets --all-namespaces -o json | \
  kubectl replace -f -
```

### Secret Best Practices

| Practice | Implementation |
|----------|----------------|
| **Encrypt at rest** | EncryptionConfiguration |
| **RBAC for secrets** | Limit who can read secrets |
| **Rotate secrets** | Regular rotation policy |
| **External secrets** | Use external secret managers |
| **Don't log secrets** | Avoid printing in logs |

---

## Task 4.4: Use Container Runtime Sandboxes ⭐

### gVisor (runsc)

```yaml
apiVersion: node.k8s.io/v1
kind: RuntimeClass
metadata:
  name: gvisor
handler: runsc
```

### Kata Containers

```yaml
apiVersion: node.k8s.io/v1
kind: RuntimeClass
metadata:
  name: kata
handler: kata
```

### Use RuntimeClass in Pod

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: sandboxed-pod
spec:
  runtimeClassName: gvisor
  containers:
  - name: app
    image: nginx
```

### Runtime Sandboxes Comparison

| Runtime | Isolation | Overhead |
|---------|-----------|----------|
| **runc** | Process-level | Low |
| **gVisor** | User-space kernel | Medium |
| **Kata** | Lightweight VM | Higher |

---

## Task 4.5: Implement Pod-to-Pod Encryption (mTLS) ⭐

### Service Mesh Options

| Option | Description |
|--------|-------------|
| **Istio** | Full-featured service mesh |
| **Linkerd** | Lightweight service mesh |
| **Cilium** | eBPF-based networking with encryption |

### Cilium Encryption

```yaml
# Enable encryption in Cilium config
apiVersion: v1
kind: ConfigMap
metadata:
  name: cilium-config
  namespace: kube-system
data:
  enable-ipsec: "true"
  enable-wireguard: "true"
```

---

## Task 4.6: OPA Gatekeeper ⭐

### Install Gatekeeper

```bash
kubectl apply -f https://raw.githubusercontent.com/open-policy-agent/gatekeeper/release-3.14/deploy/gatekeeper.yaml
```

### Constraint Template

```yaml
apiVersion: templates.gatekeeper.sh/v1
kind: ConstraintTemplate
metadata:
  name: k8srequiredlabels
spec:
  crd:
    spec:
      names:
        kind: K8sRequiredLabels
      validation:
        openAPIV3Schema:
          type: object
          properties:
            labels:
              type: array
              items:
                type: string
  targets:
    - target: admission.k8s.gatekeeper.sh
      rego: |
        package k8srequiredlabels
        violation[{"msg": msg}] {
          provided := {label | input.review.object.metadata.labels[label]}
          required := {label | label := input.parameters.labels[_]}
          missing := required - provided
          count(missing) > 0
          msg := sprintf("Missing required labels: %v", [missing])
        }
```

### Constraint

```yaml
apiVersion: constraints.gatekeeper.sh/v1beta1
kind: K8sRequiredLabels
metadata:
  name: require-app-label
spec:
  match:
    kinds:
      - apiGroups: [""]
        kinds: ["Pod"]
  parameters:
    labels: ["app", "owner"]
```

---

## Exam Tips for Domain 4

1. **Pod Security Standards** - Restricted is most secure
2. **runAsNonRoot** - Essential security setting
3. **readOnlyRootFilesystem** - Immutable containers
4. **Drop ALL capabilities** - Then add only what's needed
5. **Encrypt secrets** - EncryptionConfiguration
6. **RuntimeClass** - gVisor, Kata for sandboxing
7. **OPA Gatekeeper** - Policy enforcement
8. **Security Context** - Pod and container levels
9. **PSS labels** - enforce, warn, audit
10. **allowPrivilegeEscalation** - Always false
