# CKA Quick Reference Card

## Domain Weights
| Domain | Weight |
|--------|--------|
| D1 - Cluster Architecture | 25% |
| D2 - Workloads & Scheduling | 15% |
| D3 - Services & Networking | 20% |
| D4 - Storage | 10% |
| D5 - Troubleshooting | 30% |

---

## Cluster & RBAC

| Need | Command |
|------|---------|
| Create Role | `kubectl create role <name> --verb=get,list --resource=pods` |
| Create RoleBinding | `kubectl create rolebinding <name> --role=<role> --user=<user>` |
| Check permissions | `kubectl auth can-i create pods --as <user>` |
| Join cluster | `kubeadm token create --print-join-command` |
| Upgrade cluster | `kubeadm upgrade apply v1.34.0` |

### etcd Backup/Restore

```bash
# Backup
ETCDCTL_API=3 etcdctl snapshot save /backup/snapshot.db \
  --endpoints=https://127.0.0.1:2379 \
  --cacert=/etc/kubernetes/pki/etcd/ca.crt \
  --cert=/etc/kubernetes/pki/etcd/server.crt \
  --key=/etc/kubernetes/pki/etcd/server.key

# Restore
ETCDCTL_API=3 etcdctl snapshot restore /backup/snapshot.db \
  --data-dir=/var/lib/etcd-restored
```

---

## Workloads

| Need | Command |
|------|---------|
| Create deployment | `kubectl create deployment <name> --image=<image>` |
| Update image | `kubectl set image deployment/<name> <container>=<image>` |
| Scale | `kubectl scale deployment/<name> --replicas=3` |
| Rollback | `kubectl rollout undo deployment/<name>` |
| Create ConfigMap | `kubectl create configmap <name> --from-literal=key=value` |
| Create Secret | `kubectl create secret generic <name> --from-literal=key=value` |

### Scheduling

| Concept | Key |
|---------|-----|
| **nodeSelector** | Simple node selection |
| **nodeAffinity** | Advanced node selection |
| **Taints** | Repel pods from nodes |
| **Tolerations** | Allow pods on tainted nodes |
| **Static Pods** | /etc/kubernetes/manifests |

---

## Services & Networking

| Service Type | Usage |
|--------------|-------|
| **ClusterIP** | Internal (default) |
| **NodePort** | External via node port |
| **LoadBalancer** | Cloud LB |
| **Headless** | clusterIP: None |

### Quick Commands

```bash
# Expose deployment
kubectl expose deployment <name> --port=80 --type=ClusterIP

# Test DNS
kubectl run test --image=busybox --rm -it -- nslookup <service>

# Test connectivity
kubectl run test --image=busybox --rm -it -- wget -O- <service>:<port>
```

### Network Policy Default Deny

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

### DNS Format
- **Service**: `<svc>.<ns>.svc.cluster.local`
- **Pod**: `<pod-ip>.<ns>.pod.cluster.local`

---

## Storage

| Access Mode | Abbrev | Meaning |
|-------------|--------|---------|
| ReadWriteOnce | RWO | Single node RW |
| ReadOnlyMany | ROX | Multi node RO |
| ReadWriteMany | RWX | Multi node RW |

| Reclaim Policy | Action |
|----------------|--------|
| Retain | Keep PV data |
| Delete | Delete PV data |

### Quick PVC

```yaml
apiVersion: v1
kind: PersistentVolumeClaim
metadata:
  name: my-pvc
spec:
  accessModes: ["ReadWriteOnce"]
  resources:
    requests:
      storage: 5Gi
  storageClassName: standard
```

---

## Troubleshooting

### Pod States

| Status | Check |
|--------|-------|
| **Pending** | Resources, taints, PVC |
| **ImagePullBackOff** | Image name, secrets |
| **CrashLoopBackOff** | Logs --previous |
| **Error** | Logs, describe |

### Quick Debug Commands

```bash
kubectl describe pod <pod>           # Check events
kubectl logs <pod> --previous        # Crashed container
kubectl exec -it <pod> -- sh         # Shell access
kubectl get events --sort-by=.lastTimestamp
```

### Control Plane

| Component | Location |
|-----------|----------|
| apiserver | /etc/kubernetes/manifests |
| scheduler | /etc/kubernetes/manifests |
| controller-manager | /etc/kubernetes/manifests |
| etcd | /etc/kubernetes/manifests |
| kubelet | systemctl status kubelet |

### Node Issues

```bash
systemctl status kubelet
journalctl -u kubelet -f
crictl ps                  # Container runtime
```

---

## Imperative Generation

```bash
# Always add for YAML generation
--dry-run=client -o yaml > file.yaml

# Examples
kubectl run nginx --image=nginx --dry-run=client -o yaml > pod.yaml
kubectl create deployment nginx --image=nginx --dry-run=client -o yaml > deploy.yaml
kubectl create service clusterip nginx --tcp=80:80 --dry-run=client -o yaml > svc.yaml
```

---

## Keyboard Shortcuts (Exam)

| Action | Shortcut |
|--------|----------|
| Copy (Terminal) | Ctrl+Shift+C |
| Paste (Terminal) | Ctrl+Shift+V |
| Close Tab Alt | Ctrl+Alt+W |
| Locate Cursor | Ctrl+Alt+K |

---

## Exam Day Checklist

1. ✅ Set context: `kubectl config use-context <context>`
2. ✅ Set namespace: `kubectl config set-context --current --namespace=<ns>`
3. ✅ Use `k` alias for kubectl
4. ✅ Use `--dry-run=client -o yaml` for manifests
5. ✅ Check namespace in each question
6. ✅ Use `kubectl explain` for quick docs
7. ✅ Skip hard questions, return later
8. ✅ Validate with `kubectl get` after apply
9. ✅ Read questions carefully
10. ✅ Practice etcd backup/restore!

---

## Critical Reminders

| Topic | Remember |
|-------|----------|
| **etcd** | ETCDCTL_API=3 always! |
| **Drain** | --ignore-daemonsets |
| **Static Pods** | /etc/kubernetes/manifests |
| **kubelet** | journalctl -u kubelet |
| **NetworkPolicy** | Empty selector = all pods |
| **Secrets** | base64 encoded, NOT encrypted |
| **Ingress** | Requires controller installed |
| **Headless** | clusterIP: None |
| **PV/PVC** | Capacity, accessMode, storageClass must match |
| **Upgrade** | Control plane first, then workers |
