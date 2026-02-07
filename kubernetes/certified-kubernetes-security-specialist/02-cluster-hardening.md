# Domain 2: Cluster Hardening (15%)

## Task 2.1: Restrict Access to Kubernetes API ⭐

### API Server Access Control

| Method | Description |
|--------|-------------|
| **Authentication** | Who can access |
| **Authorization** | What they can do |
| **Admission Control** | Additional validation |

### Authentication Methods

```bash
# Check current auth methods
kubectl config view

# API Server authentication flags
--client-ca-file=/etc/kubernetes/pki/ca.crt
--enable-bootstrap-token-auth=true
--authentication-token-webhook-config-file=...
```

### Disable Anonymous Authentication

```yaml
# In kube-apiserver.yaml
spec:
  containers:
  - command:
    - kube-apiserver
    - --anonymous-auth=false
```

### API Server Audit

```bash
# Check API access
kubectl auth can-i --list

# Check as specific user
kubectl auth can-i create pods --as system:anonymous
```

---

## Task 2.2: Use Role-Based Access Control (RBAC) ⭐⭐

### RBAC Best Practices

| Practice | Implementation |
|----------|----------------|
| **Least Privilege** | Minimal permissions needed |
| **No cluster-admin** | Avoid broad admin access |
| **Namespace scoping** | Use Role over ClusterRole |
| **Service Account** | Dedicated SA per workload |

### Create Restricted Role

```bash
# Create role with minimal permissions
kubectl create role pod-reader \
  --verb=get,list,watch \
  --resource=pods \
  -n production

# Bind to user
kubectl create rolebinding pod-read-binding \
  --role=pod-reader \
  --user=developer \
  -n production
```

### Audit RBAC Permissions

```bash
# Check user permissions
kubectl auth can-i --list --as developer

# Check specific action
kubectl auth can-i delete pods --as developer -n production

# Check ServiceAccount permissions
kubectl auth can-i create deployments \
  --as system:serviceaccount:default:my-sa
```

### Find Overly Permissive Roles

```bash
# Find ClusterRoleBindings with cluster-admin
kubectl get clusterrolebindings -o json | jq -r '
  .items[] | 
  select(.roleRef.name=="cluster-admin") | 
  .metadata.name'

# Find roles with wildcard permissions
kubectl get roles,clusterroles -A -o yaml | grep -B5 '"*"'
```

### Restrict Role Example

```yaml
apiVersion: rbac.authorization.k8s.io/v1
kind: Role
metadata:
  name: deployment-manager
  namespace: production
rules:
- apiGroups: ["apps"]
  resources: ["deployments"]
  verbs: ["get", "list", "watch", "create", "update", "patch"]
  # Note: No "delete" verb - restrict destructive actions
- apiGroups: [""]
  resources: ["pods"]
  verbs: ["get", "list", "watch"]
  # Read-only for pods
```

---

## Task 2.3: Exercise Caution with Service Accounts ⭐⭐

### Disable Auto-mounting Service Account Token

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: secure-pod
spec:
  automountServiceAccountToken: false
  containers:
  - name: app
    image: nginx
```

### Disable at ServiceAccount Level

```yaml
apiVersion: v1
kind: ServiceAccount
metadata:
  name: restricted-sa
automountServiceAccountToken: false
```

### Create Dedicated ServiceAccount

```bash
# Create SA
kubectl create serviceaccount app-sa -n production

# Use in pod
kubectl patch deployment myapp -n production -p \
  '{"spec":{"template":{"spec":{"serviceAccountName":"app-sa"}}}}'
```

### ServiceAccount Token Security

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: secure-pod
spec:
  serviceAccountName: app-sa
  automountServiceAccountToken: true
  containers:
  - name: app
    image: nginx
    volumeMounts:
    - name: token
      mountPath: /var/run/secrets/kubernetes.io/serviceaccount
      readOnly: true
  volumes:
  - name: token
    projected:
      sources:
      - serviceAccountToken:
          path: token
          expirationSeconds: 3600  # Short-lived token
          audience: api
```

### Find Pods Using Default SA

```bash
# Find pods with default service account
kubectl get pods -A -o json | jq -r '
  .items[] | 
  select(.spec.serviceAccountName=="default" or .spec.serviceAccountName==null) |
  "\(.metadata.namespace)/\(.metadata.name)"'
```

---

## Task 2.4: Update Kubernetes Frequently ⭐

### Check Current Version

```bash
# Check cluster version
kubectl version

# Check node versions
kubectl get nodes -o wide

# Check component versions
kubectl get pods -n kube-system -o jsonpath='{range .items[*]}{.metadata.name}{"\t"}{.spec.containers[*].image}{"\n"}{end}'
```

### Upgrade Process

```bash
# 1. Upgrade kubeadm
sudo apt-get update
sudo apt-get install -y kubeadm=1.34.0-00

# 2. Check upgrade plan
sudo kubeadm upgrade plan

# 3. Apply upgrade (control plane)
sudo kubeadm upgrade apply v1.34.0

# 4. Drain node
kubectl drain <node-name> --ignore-daemonsets --delete-emptydir-data

# 5. Upgrade kubelet and kubectl
sudo apt-get install -y kubelet=1.34.0-00 kubectl=1.34.0-00
sudo systemctl daemon-reload
sudo systemctl restart kubelet

# 6. Uncordon node
kubectl uncordon <node-name>
```

### CVE Monitoring

| Resource | URL |
|----------|-----|
| **Kubernetes Security** | https://kubernetes.io/docs/reference/issues-security/ |
| **CVE Database** | https://cve.mitre.org |
| **GitHub Security** | https://github.com/kubernetes/kubernetes/security |

---

## Task 2.5: Restrict Network Access to Control Plane ⭐

### API Server Network Restrictions

```bash
# Limit advertised address
--advertise-address=10.0.0.1

# Bind to specific interface
--bind-address=0.0.0.0
```

### Control Plane Firewall Rules

| Port | Component | Access |
|------|-----------|--------|
| 6443 | API Server | Authorized clients |
| 2379-2380 | etcd | Control plane only |
| 10250 | kubelet | Control plane only |
| 10259 | Scheduler | Localhost only |
| 10257 | Controller Manager | Localhost only |

---

## Exam Tips for Domain 2

1. **RBAC** - Least privilege, audit with `kubectl auth can-i`
2. **Anonymous auth** - Always disable in production
3. **Service Accounts** - Don't use default, disable auto-mount
4. **Token projection** - Use short-lived tokens
5. **cluster-admin** - Avoid binding to users
6. **Wildcard permissions** - Find and remove "*" verbs
7. **Version updates** - Keep Kubernetes current
8. **Network access** - Restrict control plane ports
9. **Authorization mode** - Use Node,RBAC
10. **Audit permissions** - Regular RBAC review
