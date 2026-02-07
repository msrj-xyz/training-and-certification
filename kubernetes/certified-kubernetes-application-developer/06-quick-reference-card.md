# CKAD Quick Reference Card

## Domain Weights
| Domain | Weight |
|--------|--------|
| D1 - Application Design & Build | 20% |
| D2 - Application Deployment | 20% |
| D3 - Observability & Maintenance | 15% |
| D4 - Environment, Config & Security | 25% |
| D5 - Services & Networking | 20% |

---

## Workload Resources

| Resource | Use Case | Key Feature |
|----------|----------|-------------|
| **Deployment** | Stateless apps | Rolling updates |
| **StatefulSet** | Databases | Stable identity |
| **DaemonSet** | Node agents | One per node |
| **Job** | Batch tasks | Run to completion |
| **CronJob** | Scheduled | Cron schedule |

---

## Quick Imperative Commands

```bash
# Pod
kubectl run nginx --image=nginx --port=80

# Deployment
kubectl create deployment nginx --image=nginx --replicas=3

# Service
kubectl expose deployment nginx --port=80 --type=ClusterIP

# ConfigMap
kubectl create configmap app-config --from-literal=key=value

# Secret
kubectl create secret generic db-secret --from-literal=pass=secret

# Job
kubectl create job backup --image=busybox -- echo "done"

# CronJob
kubectl create cronjob daily --image=busybox --schedule="0 2 * * *" -- echo "run"
```

---

## Generate YAML

```bash
# Always add for templates
--dry-run=client -o yaml > resource.yaml

# Examples
kubectl run nginx --image=nginx --dry-run=client -o yaml > pod.yaml
kubectl create deployment web --image=nginx --dry-run=client -o yaml > deploy.yaml
```

---

## Probes Quick Reference

```yaml
# Liveness - restart if fails
livenessProbe:
  httpGet:
    path: /healthz
    port: 8080
  initialDelaySeconds: 10
  periodSeconds: 5

# Readiness - remove from service if fails
readinessProbe:
  httpGet:
    path: /ready
    port: 8080
  initialDelaySeconds: 5

# Startup - for slow starting apps
startupProbe:
  httpGet:
    path: /startup
    port: 8080
  failureThreshold: 30
  periodSeconds: 10
```

---

## ConfigMap & Secret Usage

```yaml
# Load all as env vars
envFrom:
- configMapRef:
    name: app-config
- secretRef:
    name: app-secret

# Load specific key
env:
- name: DB_HOST
  valueFrom:
    configMapKeyRef:
      name: app-config
      key: DATABASE_HOST

# Mount as volume
volumeMounts:
- name: config
  mountPath: /etc/config
volumes:
- name: config
  configMap:
    name: app-config
```

---

## Security Context

```yaml
securityContext:
  runAsUser: 1000
  runAsNonRoot: true
  readOnlyRootFilesystem: true
  allowPrivilegeEscalation: false
  capabilities:
    drop: ["ALL"]
```

---

## Multi-Container Patterns

| Pattern | Purpose | Example |
|---------|---------|---------|
| **Init** | Setup before main | Wait for DB |
| **Sidecar** | Support main | Log shipper |
| **Ambassador** | Proxy | DB proxy |
| **Adapter** | Transform | Log formatter |

---

## Debugging Commands

```bash
kubectl describe pod <pod>        # Events at bottom
kubectl logs <pod> --previous     # Crashed container
kubectl exec -it <pod> -- sh      # Shell access
kubectl port-forward <pod> 8080:80  # Local access
kubectl debug <pod> -it --image=busybox
```

---

## Service Quick Reference

| Type | Access | Port Range |
|------|--------|------------|
| **ClusterIP** | Internal | Any |
| **NodePort** | External (node) | 30000-32767 |
| **LoadBalancer** | External (LB) | Any |

```bash
# Expose deployment
kubectl expose deployment nginx --port=80 --type=ClusterIP

# Check endpoints
kubectl get endpoints <svc>

# Test service
kubectl run test --rm -it --image=busybox -- wget -O- <svc>:<port>
```

---

## Helm Commands

```bash
helm repo add bitnami https://charts.bitnami.com/bitnami
helm search repo nginx
helm install myapp bitnami/nginx -f values.yaml
helm upgrade myapp bitnami/nginx --set replicas=3
helm list
helm rollback myapp 1
helm uninstall myapp
```

---

## Rollout Commands

```bash
kubectl set image deployment/nginx nginx=nginx:1.25
kubectl rollout status deployment/nginx
kubectl rollout history deployment/nginx
kubectl rollout undo deployment/nginx
kubectl rollout restart deployment/nginx
```

---

## Exam Day Checklist

1. ✅ Use `k` alias for kubectl
2. ✅ `--dry-run=client -o yaml` for templates
3. ✅ Check namespace in each question
4. ✅ Use `kubectl explain` for field reference
5. ✅ `kubectl describe` for events
6. ✅ Skip hard questions, return later
7. ✅ Verify with `kubectl get` after apply
8. ✅ Practice multi-container patterns
9. ✅ Know all probe types
10. ✅ Master ConfigMap/Secret usage

---

## Critical Reminders

| Topic | Remember |
|-------|----------|
| **Deployment** | RollingUpdate is default |
| **Probes** | initialDelaySeconds is key |
| **ConfigMap** | --from-literal, --from-file |
| **Secret** | base64 encoded, NOT encrypted |
| **Service** | selector must match pod labels |
| **Ingress** | Requires controller installed |
| **NetworkPolicy** | No policy = all traffic allowed |
| **Resources** | requests = scheduling, limits = cap |
| **Security** | runAsNonRoot, readOnlyRootFilesystem |
| **Helm** | upgrade --install for upsert |
