# Domain 5: Troubleshooting (30%)

## Task 5.1: Troubleshoot Cluster and Node Issues ⭐⭐

### Check Cluster Health

```bash
# Cluster info
kubectl cluster-info

# Component statuses (deprecated but useful)
kubectl get componentstatuses

# Check all nodes
kubectl get nodes -o wide

# Node details
kubectl describe node <node-name>
```

### Node Conditions

| Condition | Healthy Value | Issue When |
|-----------|---------------|------------|
| **Ready** | True | False/Unknown |
| **MemoryPressure** | False | True |
| **DiskPressure** | False | True |
| **PIDPressure** | False | True |
| **NetworkUnavailable** | False | True |

### Node Not Ready Troubleshooting

```bash
# Check kubelet status
sudo systemctl status kubelet

# Check kubelet logs
sudo journalctl -u kubelet -f

# Restart kubelet
sudo systemctl restart kubelet

# Check kubelet config
cat /var/lib/kubelet/config.yaml
```

### Common Node Issues

| Issue | Check | Solution |
|-------|-------|----------|
| **kubelet not running** | systemctl status kubelet | Restart kubelet |
| **Container runtime** | crictl ps | Restart containerd |
| **Network plugin** | CNI config | Check /etc/cni/net.d |
| **Disk full** | df -h | Clean up disk |
| **Memory exhausted** | free -m | Add memory/evict pods |

---

## Task 5.2: Troubleshoot Control Plane Components ⭐⭐

### Control Plane Locations

| Component | Type | Location |
|-----------|------|----------|
| **kube-apiserver** | Static Pod | /etc/kubernetes/manifests |
| **kube-controller-manager** | Static Pod | /etc/kubernetes/manifests |
| **kube-scheduler** | Static Pod | /etc/kubernetes/manifests |
| **etcd** | Static Pod | /etc/kubernetes/manifests |
| **kubelet** | Service | systemd service |

### Check Control Plane Pods

```bash
# Get control plane pods
kubectl get pods -n kube-system

# Check apiserver
kubectl logs kube-apiserver-<master> -n kube-system

# Check scheduler
kubectl logs kube-scheduler-<master> -n kube-system

# Check controller-manager
kubectl logs kube-controller-manager-<master> -n kube-system

# Check etcd
kubectl logs etcd-<master> -n kube-system
```

### Static Pod Troubleshooting

```bash
# Check manifests directory
ls /etc/kubernetes/manifests/

# View apiserver manifest
cat /etc/kubernetes/manifests/kube-apiserver.yaml

# Check for syntax errors
kubectl apply --dry-run=server -f /etc/kubernetes/manifests/kube-apiserver.yaml

# Check kubelet logs for static pod issues
journalctl -u kubelet | grep -i error
```

### etcd Troubleshooting

```bash
# Check etcd health
ETCDCTL_API=3 etcdctl endpoint health \
  --endpoints=https://127.0.0.1:2379 \
  --cacert=/etc/kubernetes/pki/etcd/ca.crt \
  --cert=/etc/kubernetes/pki/etcd/server.crt \
  --key=/etc/kubernetes/pki/etcd/server.key

# Check etcd member list
ETCDCTL_API=3 etcdctl member list \
  --endpoints=https://127.0.0.1:2379 \
  --cacert=/etc/kubernetes/pki/etcd/ca.crt \
  --cert=/etc/kubernetes/pki/etcd/server.crt \
  --key=/etc/kubernetes/pki/etcd/server.key
```

---

## Task 5.3: Troubleshoot Application Failures ⭐

### Pod Status Investigation

| Status | Meaning | Action |
|--------|---------|--------|
| **Pending** | Not scheduled | Check resources, taints |
| **ImagePullBackOff** | Image pull failed | Check image name, registry |
| **CrashLoopBackOff** | Container crashing | Check logs, command |
| **Error** | Container error | Check logs |
| **Running** | Container running | App-level debugging |
| **Completed** | Job finished | Normal for Jobs |

### Pod Debugging Commands

```bash
# Describe pod (events at bottom)
kubectl describe pod <pod-name>

# Get pod logs
kubectl logs <pod-name>

# Logs for previous container (crashed)
kubectl logs <pod-name> --previous

# Logs for specific container
kubectl logs <pod-name> -c <container-name>

# Follow logs
kubectl logs <pod-name> -f

# Logs with timestamps
kubectl logs <pod-name> --timestamps
```

### Debug Running Container

```bash
# Exec into container
kubectl exec -it <pod-name> -- /bin/sh

# Exec specific container
kubectl exec -it <pod-name> -c <container> -- /bin/sh

# Run debug command
kubectl exec <pod-name> -- cat /etc/config/app.conf
```

### Debug Pod (Ephemeral Containers)

```bash
# Attach debug container to running pod
kubectl debug <pod-name> -it --image=busybox

# Copy pod with debug container
kubectl debug <pod-name> -it --image=busybox --copy-to=debug-pod

# Debug node
kubectl debug node/<node-name> -it --image=busybox
```

---

## Task 5.4: Troubleshoot Container Logging ⭐

### Container Output Streams

| Stream | Default | Purpose |
|--------|---------|---------|
| **stdout** | Console | Standard output |
| **stderr** | Console | Error output |

### Log Aggregation

```bash
# All pod logs in namespace
kubectl logs -n <namespace> -l app=myapp

# Logs from all containers in pod
kubectl logs <pod-name> --all-containers

# Tail logs
kubectl logs <pod-name> --tail=100

# Since timestamp
kubectl logs <pod-name> --since=1h
```

### Check Container Status

```bash
# Detailed pod info
kubectl get pod <pod-name> -o yaml

# Container states
kubectl get pod <pod-name> -o jsonpath='{.status.containerStatuses[*].state}'

# Exit codes
kubectl get pod <pod-name> -o jsonpath='{.status.containerStatuses[*].lastState.terminated.exitCode}'
```

---

## Task 5.5: Troubleshoot Services and Networking ⭐⭐

### Service Troubleshooting

```bash
# Check service
kubectl get svc <service-name>

# Check endpoints (pods backing service)
kubectl get endpoints <service-name>

# Describe service
kubectl describe svc <service-name>
```

### Common Service Issues

| Issue | Symptom | Solution |
|-------|---------|----------|
| **No endpoints** | Endpoints: none | Check selector matches pod labels |
| **Wrong port** | Connection refused | Verify targetPort |
| **Pod not ready** | Missing from endpoints | Check readiness probe |

### Network Debugging

```bash
# Test DNS resolution
kubectl run test-dns --image=busybox --rm -it --restart=Never -- nslookup <service-name>

# Test service connectivity
kubectl run test-net --image=busybox --rm -it --restart=Never -- wget -O- <service-name>:<port>

# Test pod connectivity
kubectl run test-pod --image=busybox --rm -it --restart=Never -- ping <pod-ip>

# Check NetworkPolicies
kubectl get networkpolicy -A
```

### CoreDNS Troubleshooting

```bash
# Check CoreDNS pods
kubectl get pods -n kube-system -l k8s-app=kube-dns

# Check CoreDNS logs
kubectl logs -n kube-system -l k8s-app=kube-dns

# Test DNS from pod
kubectl exec -it <pod> -- nslookup kubernetes.default.svc.cluster.local
```

---

## Task 5.6: Monitor Resource Usage ⭐

### Resource Monitoring

```bash
# Node resource usage (requires metrics-server)
kubectl top nodes

# Pod resource usage
kubectl top pods

# Pod resource usage in namespace
kubectl top pods -n <namespace>

# Container-level usage
kubectl top pods --containers
```

### Install Metrics Server

```bash
kubectl apply -f https://github.com/kubernetes-sigs/metrics-server/releases/latest/download/components.yaml
```

### Resource Analysis

```bash
# Check resource requests/limits
kubectl describe pod <pod-name> | grep -A5 "Requests\|Limits"

# Get pods sorted by CPU
kubectl top pods --sort-by=cpu

# Get pods sorted by memory
kubectl top pods --sort-by=memory
```

---

## Troubleshooting Decision Tree

### Pod Not Starting

```
Pod Pending?
├── No nodes available? → Check node status, taints
├── Insufficient resources? → Scale cluster or reduce requests
├── No matching PV? → Check storage class, PVC
└── nodeSelector/affinity? → Check node labels

Pod ImagePullBackOff?
├── Image exists? → Verify image name/tag
├── Private registry? → Check imagePullSecrets
└── Network issue? → Check node connectivity

Pod CrashLoopBackOff?
├── Check logs → kubectl logs --previous
├── Check command → Verify entrypoint/args
├── Check probes → Adjust liveness probe
└── Check resources → Increase memory limits
```

### Service Not Working

```
No Endpoints?
├── Selector matches? → Check pod labels
├── Pods running? → Check pod status
└── Pods ready? → Check readiness probes

Cannot Connect?
├── Service exists? → kubectl get svc
├── DNS resolves? → nslookup test
├── Port correct? → Check targetPort
└── NetworkPolicy? → Check policies
```

---

## Useful Troubleshooting Commands Summary

```bash
# Quick health check
kubectl get nodes
kubectl get pods -A | grep -v Running
kubectl get events --sort-by='.lastTimestamp'

# System pods
kubectl get pods -n kube-system

# Recent events
kubectl get events -n <namespace> --sort-by=.lastTimestamp

# All resources in namespace
kubectl get all -n <namespace>

# Cluster events
kubectl get events -A --field-selector type=Warning
```

---

## Exam Tips for Domain 5

1. **kubectl describe** - Always check Events section at bottom
2. **kubectl logs --previous** - For crashed container logs
3. **Pending pods** - Check resources, taints, PVC binding
4. **CrashLoopBackOff** - Check logs, command, resources
5. **ImagePullBackOff** - Check image name, imagePullSecrets
6. **No endpoints** - Selector doesn't match pod labels
7. **Static pods** - Check /etc/kubernetes/manifests
8. **kubelet issues** - journalctl -u kubelet
9. **Control plane** - Logs in kube-system namespace
10. **etcd** - Always use ETCDCTL_API=3
