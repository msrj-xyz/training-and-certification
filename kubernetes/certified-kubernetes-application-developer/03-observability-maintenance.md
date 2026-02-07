# Domain 3: Application Observability and Maintenance (15%)

## Task 3.1: Understand API Deprecations ⭐

### Check API Versions

```bash
# List available API versions
kubectl api-versions

# List API resources with versions
kubectl api-resources -o wide

# Check specific resource API
kubectl explain deployment

# Check deprecation warnings
kubectl get deployment nginx --warnings
```

### Deprecated API Migration

| Old API | New API | Resource |
|---------|---------|----------|
| extensions/v1beta1 | apps/v1 | Deployment |
| extensions/v1beta1 | networking.k8s.io/v1 | Ingress |
| networking.k8s.io/v1beta1 | networking.k8s.io/v1 | Ingress |
| batch/v1beta1 | batch/v1 | CronJob |

### Convert Manifest

```bash
# Use kubectl convert (if available)
kubectl convert -f old-deployment.yaml --output-version apps/v1

# Manual: Update apiVersion in YAML
sed -i 's|extensions/v1beta1|apps/v1|g' deployment.yaml
```

---

## Task 3.2: Implement Probes and Health Checks ⭐⭐

### Probe Types

| Probe | Purpose | When Checked |
|-------|---------|--------------|
| **Liveness** | Is container alive? | Continuously |
| **Readiness** | Can container serve traffic? | Continuously |
| **Startup** | Has container started? | Initial startup |

### Liveness Probe

```yaml
spec:
  containers:
  - name: app
    image: myapp
    livenessProbe:
      httpGet:
        path: /healthz
        port: 8080
      initialDelaySeconds: 10
      periodSeconds: 5
      timeoutSeconds: 3
      failureThreshold: 3
```

### Readiness Probe

```yaml
spec:
  containers:
  - name: app
    image: myapp
    readinessProbe:
      httpGet:
        path: /ready
        port: 8080
      initialDelaySeconds: 5
      periodSeconds: 5
      successThreshold: 1
      failureThreshold: 3
```

### Startup Probe

```yaml
spec:
  containers:
  - name: app
    image: myapp
    startupProbe:
      httpGet:
        path: /startup
        port: 8080
      initialDelaySeconds: 0
      periodSeconds: 10
      failureThreshold: 30  # 30 * 10 = 300s to start
```

### Probe Methods

```yaml
# HTTP GET
httpGet:
  path: /health
  port: 8080
  httpHeaders:
  - name: Custom-Header
    value: MyValue

# TCP Socket
tcpSocket:
  port: 3306

# Exec Command
exec:
  command:
  - cat
  - /tmp/healthy

# gRPC (K8s 1.24+)
grpc:
  port: 50051
```

### Probe Parameters

| Parameter | Default | Description |
|-----------|---------|-------------|
| **initialDelaySeconds** | 0 | Wait before first probe |
| **periodSeconds** | 10 | How often to probe |
| **timeoutSeconds** | 1 | Probe timeout |
| **successThreshold** | 1 | Min consecutive success |
| **failureThreshold** | 3 | Min consecutive failures |

---

## Task 3.3: Use Built-in CLI Tools for Monitoring ⭐

### kubectl top (Metrics Server Required)

```bash
# Node resources
kubectl top nodes

# Pod resources
kubectl top pods
kubectl top pods -A
kubectl top pods --containers

# Sort by CPU/Memory
kubectl top pods --sort-by=cpu
kubectl top pods --sort-by=memory
```

### Resource Usage Analysis

```bash
# Check resource requests/limits
kubectl describe pod <pod> | grep -A5 "Requests\|Limits"

# Get pods with resource info
kubectl get pods -o custom-columns=NAME:.metadata.name,CPU:.spec.containers[*].resources.requests.cpu,MEM:.spec.containers[*].resources.requests.memory
```

---

## Task 3.4: Utilize Container Logs ⭐⭐

### View Logs

```bash
# Basic logs
kubectl logs <pod-name>

# Specific container
kubectl logs <pod-name> -c <container-name>

# Follow logs (stream)
kubectl logs <pod-name> -f

# Previous container (crashed)
kubectl logs <pod-name> --previous

# Tail logs
kubectl logs <pod-name> --tail=100

# Since time
kubectl logs <pod-name> --since=1h
kubectl logs <pod-name> --since=30m

# With timestamps
kubectl logs <pod-name> --timestamps

# All containers in pod
kubectl logs <pod-name> --all-containers

# By label selector
kubectl logs -l app=nginx

# Multiple pods
kubectl logs -l app=nginx --max-log-requests=10
```

### Log Aggregation Pattern

```yaml
# Sidecar for log shipping
apiVersion: v1
kind: Pod
metadata:
  name: app-with-logging
spec:
  containers:
  - name: app
    image: myapp
    volumeMounts:
    - name: logs
      mountPath: /var/log/app
  - name: log-shipper
    image: fluentd
    volumeMounts:
    - name: logs
      mountPath: /var/log/app
      readOnly: true
  volumes:
  - name: logs
    emptyDir: {}
```

---

## Task 3.5: Debugging in Kubernetes ⭐⭐

### Debug Commands

```bash
# Describe pod (check Events)
kubectl describe pod <pod-name>

# Get pod details
kubectl get pod <pod-name> -o yaml

# Get events
kubectl get events --sort-by='.lastTimestamp'
kubectl get events --field-selector involvedObject.name=<pod-name>

# Exec into container
kubectl exec -it <pod-name> -- /bin/sh
kubectl exec -it <pod-name> -c <container> -- bash

# Debug with ephemeral container
kubectl debug <pod-name> -it --image=busybox

# Debug copy of pod
kubectl debug <pod-name> -it --copy-to=debug-pod --image=busybox

# Port forward for testing
kubectl port-forward <pod-name> 8080:80
kubectl port-forward svc/<service-name> 8080:80
```

### Common Debug Images

| Image | Use Case |
|-------|----------|
| **busybox** | Basic shell, wget, nslookup |
| **nicolaka/netshoot** | Network debugging |
| **curlimages/curl** | HTTP testing |
| **alpine** | Lightweight Linux |

### Check Container Status

```bash
# Container states
kubectl get pod <pod> -o jsonpath='{.status.containerStatuses[*].state}'

# Exit codes
kubectl get pod <pod> -o jsonpath='{.status.containerStatuses[*].lastState.terminated.exitCode}'

# Restart count
kubectl get pod <pod> -o jsonpath='{.status.containerStatuses[*].restartCount}'
```

---

## Exam Tips for Domain 3

1. **API deprecations** - Know current stable APIs (apps/v1, networking.k8s.io/v1)
2. **Liveness probe** - Restart if fails
3. **Readiness probe** - Remove from service if fails
4. **Startup probe** - For slow-starting containers
5. **kubectl logs --previous** - Debug crashed containers
6. **kubectl describe** - Check Events section
7. **kubectl top** - Requires metrics-server
8. **Port forward** - Test services locally
9. **Ephemeral debug containers** - kubectl debug
10. **Probe parameters** - initialDelaySeconds is key
