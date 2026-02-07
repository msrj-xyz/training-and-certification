# Domain 1: Cluster Architecture, Installation & Configuration (25%)

## Task 1.1: Manage Role-Based Access Control (RBAC) ⭐

### RBAC Components

| Component | Scope | Description |
|-----------|-------|-------------|
| **Role** | Namespace | Permissions within a namespace |
| **ClusterRole** | Cluster | Cluster-wide permissions |
| **RoleBinding** | Namespace | Binds Role to subjects |
| **ClusterRoleBinding** | Cluster | Binds ClusterRole to subjects |

### RBAC Subjects

| Subject | Description |
|---------|-------------|
| **User** | Human user (external auth) |
| **Group** | Set of users |
| **ServiceAccount** | Pod identity |

---

### Create Role (Imperative)

```bash
# Create Role with specific verbs
kubectl create role pod-reader \
  --verb=get,list,watch \
  --resource=pods \
  -n development

# Create ClusterRole
kubectl create clusterrole node-reader \
  --verb=get,list,watch \
  --resource=nodes
```

### Create RoleBinding (Imperative)

```bash
# Bind Role to user
kubectl create rolebinding read-pods \
  --role=pod-reader \
  --user=jane \
  -n development

# Bind ClusterRole to ServiceAccount
kubectl create clusterrolebinding admin-binding \
  --clusterrole=cluster-admin \
  --serviceaccount=kube-system:admin-sa
```

---

### Role YAML Example

```yaml
apiVersion: rbac.authorization.k8s.io/v1
kind: Role
metadata:
  namespace: development
  name: pod-manager
rules:
- apiGroups: [""]
  resources: ["pods"]
  verbs: ["get", "list", "watch", "create", "delete"]
- apiGroups: ["apps"]
  resources: ["deployments"]
  verbs: ["get", "list"]
```

### Verify RBAC

```bash
# Check if user can perform action
kubectl auth can-i create pods --as jane -n development

# Check all permissions for user
kubectl auth can-i --list --as jane -n development

# Check ServiceAccount permissions
kubectl auth can-i get pods --as system:serviceaccount:default:my-sa
```

> **Exam Tip:** Use `kubectl auth can-i` to verify RBAC permissions quickly!

---

## Task 1.2: Use kubeadm to Install a Basic Cluster ⭐

### Pre-requisites

| Requirement | Details |
|-------------|---------|
| **OS** | Ubuntu, CentOS, RHEL |
| **Memory** | 2GB+ per node |
| **CPU** | 2+ cores for control plane |
| **Network** | Unique hostname, MAC, product_uuid |
| **Swap** | Disabled |
| **Ports** | 6443, 2379-2380, 10250-10252 |

### Initialize Control Plane

```bash
# Initialize cluster
sudo kubeadm init \
  --pod-network-cidr=10.244.0.0/16 \
  --apiserver-advertise-address=<CONTROL_PLANE_IP>

# Configure kubectl for current user
mkdir -p $HOME/.kube
sudo cp -i /etc/kubernetes/admin.conf $HOME/.kube/config
sudo chown $(id -u):$(id -g) $HOME/.kube/config
```

### Join Worker Nodes

```bash
# On worker nodes (token from kubeadm init output)
sudo kubeadm join <CONTROL_PLANE_IP>:6443 \
  --token <TOKEN> \
  --discovery-token-ca-cert-hash sha256:<HASH>

# Generate new join token if expired
kubeadm token create --print-join-command
```

### Install CNI (Pod Network)

```bash
# Flannel
kubectl apply -f https://github.com/flannel-io/flannel/releases/latest/download/kube-flannel.yml

# Calico
kubectl apply -f https://docs.projectcalico.org/manifests/calico.yaml

# Weave
kubectl apply -f https://github.com/weaveworks/weave/releases/download/v2.8.1/weave-daemonset-k8s.yaml
```

---

## Task 1.3: Manage Cluster Lifecycle ⭐

### Cluster Upgrade Process

```bash
# 1. Upgrade kubeadm
sudo apt-get update
sudo apt-get install -y kubeadm=1.34.0-00

# 2. Plan upgrade
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

### Upgrade Order

1. **Control Plane** nodes first
2. **Worker** nodes after all control planes

> **Exam Tip:** Always drain before upgrading kubelet!

---

## Task 1.4: Implement etcd Backup and Restore ⭐⭐

### etcd Backup

```bash
# Backup etcd
ETCDCTL_API=3 etcdctl snapshot save /backup/etcd-snapshot.db \
  --endpoints=https://127.0.0.1:2379 \
  --cacert=/etc/kubernetes/pki/etcd/ca.crt \
  --cert=/etc/kubernetes/pki/etcd/server.crt \
  --key=/etc/kubernetes/pki/etcd/server.key

# Verify backup
ETCDCTL_API=3 etcdctl snapshot status /backup/etcd-snapshot.db --write-out=table
```

### etcd Restore

```bash
# Restore from snapshot
ETCDCTL_API=3 etcdctl snapshot restore /backup/etcd-snapshot.db \
  --data-dir=/var/lib/etcd-restored \
  --endpoints=https://127.0.0.1:2379 \
  --cacert=/etc/kubernetes/pki/etcd/ca.crt \
  --cert=/etc/kubernetes/pki/etcd/server.crt \
  --key=/etc/kubernetes/pki/etcd/server.key

# Update etcd manifest to use new data-dir
sudo vi /etc/kubernetes/manifests/etcd.yaml
# Change --data-dir to /var/lib/etcd-restored
# Change hostPath volume to /var/lib/etcd-restored
```

### etcd Locations

| File | Path |
|------|------|
| **etcd manifest** | /etc/kubernetes/manifests/etcd.yaml |
| **CA cert** | /etc/kubernetes/pki/etcd/ca.crt |
| **Server cert** | /etc/kubernetes/pki/etcd/server.crt |
| **Server key** | /etc/kubernetes/pki/etcd/server.key |
| **Data directory** | /var/lib/etcd |

---

## Task 1.5: Manage High-Availability Control Plane ⭐

### HA Architecture

| Component | Replicas | Purpose |
|-----------|----------|---------|
| **kube-apiserver** | Multiple | API endpoint |
| **etcd** | 3 or 5 | Distributed store |
| **kube-controller-manager** | Multiple (leader election) | Controllers |
| **kube-scheduler** | Multiple (leader election) | Pod scheduling |

### HA topologies

| Topology | Description |
|----------|-------------|
| **Stacked etcd** | etcd on control plane nodes |
| **External etcd** | Separate etcd cluster |

---

## Task 1.6: Use Helm and Kustomize ⭐

### Helm Commands

```bash
# Add repository
helm repo add bitnami https://charts.bitnami.com/bitnami
helm repo update

# Search charts
helm search repo nginx

# Install chart
helm install my-nginx bitnami/nginx -n web --create-namespace

# Install with values
helm install my-app ./my-chart -f values.yaml

# Upgrade release
helm upgrade my-nginx bitnami/nginx -n web

# List releases
helm list -A

# Uninstall
helm uninstall my-nginx -n web
```

### Kustomize Commands

```bash
# View kustomized output
kubectl kustomize ./overlays/production

# Apply kustomization
kubectl apply -k ./overlays/production

# Create kustomization.yaml
kubectl kustomize init
```

### Kustomize Structure

```
├── base/
│   ├── deployment.yaml
│   ├── service.yaml
│   └── kustomization.yaml
└── overlays/
    ├── development/
    │   └── kustomization.yaml
    └── production/
        └── kustomization.yaml
```

---

## Task 1.7: Understand Extension Interfaces (CNI, CSI, CRI)

| Interface | Purpose | Examples |
|-----------|---------|----------|
| **CNI** | Container Network Interface | Calico, Flannel, Weave, Cilium |
| **CSI** | Container Storage Interface | AWS EBS, GCE PD, Ceph, NFS |
| **CRI** | Container Runtime Interface | containerd, CRI-O |

### CNI Configuration

```bash
# CNI config location
/etc/cni/net.d/

# CNI binaries
/opt/cni/bin/
```

---

## Task 1.8: CRDs and Operators

### Custom Resource Definitions

```bash
# List CRDs
kubectl get crd

# Describe CRD
kubectl describe crd <crd-name>

# Get custom resources
kubectl get <custom-resource-name>
```

### CRD YAML Example

```yaml
apiVersion: apiextensions.k8s.io/v1
kind: CustomResourceDefinition
metadata:
  name: databases.example.com
spec:
  group: example.com
  versions:
    - name: v1
      served: true
      storage: true
      schema:
        openAPIV3Schema:
          type: object
          properties:
            spec:
              type: object
              properties:
                size:
                  type: integer
  scope: Namespaced
  names:
    plural: databases
    singular: database
    kind: Database
    shortNames:
      - db
```

---

## Exam Tips for Domain 1

1. **RBAC** - Master imperative commands for Role/RoleBinding
2. **kubeadm** - Know init, join, upgrade process
3. **etcd backup/restore** - High-frequency exam topic!
4. **ETCDCTL_API=3** - Always set for etcdctl commands
5. **Drain before upgrade** - Required for kubelet upgrades
6. **CNI** - Install after kubeadm init
7. **Helm** - Install, upgrade, list, uninstall commands
8. **Kustomize** - kubectl apply -k for overlays
9. **kubeconfig** - Copy admin.conf after kubeadm init
10. **Token expiry** - kubeadm token create --print-join-command
