# Domain 2: Application Deployment (20%)

## Task 2.1: Deployment Strategies ⭐⭐

### Rolling Update (Default)

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: web-app
spec:
  replicas: 4
  strategy:
    type: RollingUpdate
    rollingUpdate:
      maxSurge: 1        # Max pods above replicas
      maxUnavailable: 1  # Max pods unavailable
  template:
    spec:
      containers:
      - name: web
        image: myapp:v2
```

### Recreate Strategy

```yaml
spec:
  strategy:
    type: Recreate  # Delete all, then create new
```

### Blue/Green Deployment

```bash
# Blue deployment (current)
kubectl create deployment blue --image=myapp:v1

# Green deployment (new version)
kubectl create deployment green --image=myapp:v2

# Switch service to green
kubectl patch svc myapp -p '{"spec":{"selector":{"app":"green"}}}'
```

### Canary Deployment

```yaml
# Main deployment (90% traffic)
apiVersion: apps/v1
kind: Deployment
metadata:
  name: myapp-stable
spec:
  replicas: 9
  selector:
    matchLabels:
      app: myapp
      version: stable
---
# Canary deployment (10% traffic)
apiVersion: apps/v1
kind: Deployment
metadata:
  name: myapp-canary
spec:
  replicas: 1
  selector:
    matchLabels:
      app: myapp
      version: canary
---
# Service selects both
apiVersion: v1
kind: Service
metadata:
  name: myapp
spec:
  selector:
    app: myapp  # Matches both versions
```

---

## Task 2.2: Rolling Updates and Rollbacks ⭐⭐

### Update Deployment

```bash
# Update image
kubectl set image deployment/nginx nginx=nginx:1.25

# Edit deployment
kubectl edit deployment nginx

# Patch deployment
kubectl patch deployment nginx -p '{"spec":{"template":{"spec":{"containers":[{"name":"nginx","image":"nginx:1.25"}]}}}}'
```

### Rollout Commands

```bash
# Check rollout status
kubectl rollout status deployment/nginx

# View rollout history
kubectl rollout history deployment/nginx

# View specific revision
kubectl rollout history deployment/nginx --revision=2

# Rollback to previous version
kubectl rollout undo deployment/nginx

# Rollback to specific revision
kubectl rollout undo deployment/nginx --to-revision=1

# Pause rollout
kubectl rollout pause deployment/nginx

# Resume rollout
kubectl rollout resume deployment/nginx

# Restart deployment (rolling restart)
kubectl rollout restart deployment/nginx
```

### Record Changes

```bash
# Record change in history
kubectl set image deployment/nginx nginx=nginx:1.25 --record

# Or use annotation
kubectl annotate deployment nginx kubernetes.io/change-cause="Updated to v1.25"
```

---

## Task 2.3: Use Helm Package Manager ⭐⭐

### Helm Basics

```bash
# Add repository
helm repo add bitnami https://charts.bitnami.com/bitnami
helm repo update

# Search charts
helm search repo nginx
helm search hub wordpress

# Show chart info
helm show chart bitnami/nginx
helm show values bitnami/nginx
```

### Install Charts

```bash
# Install with default values
helm install my-nginx bitnami/nginx

# Install in namespace
helm install my-nginx bitnami/nginx -n web --create-namespace

# Install with custom values
helm install my-nginx bitnami/nginx -f values.yaml

# Install with set values
helm install my-nginx bitnami/nginx \
  --set replicaCount=3 \
  --set service.type=NodePort

# Dry run (preview)
helm install my-nginx bitnami/nginx --dry-run
```

### Manage Releases

```bash
# List releases
helm list
helm list -A  # All namespaces

# Get release info
helm status my-nginx
helm get values my-nginx
helm get manifest my-nginx

# Upgrade release
helm upgrade my-nginx bitnami/nginx --set replicaCount=5

# Upgrade or install
helm upgrade --install my-nginx bitnami/nginx

# Rollback release
helm rollback my-nginx 1

# Uninstall
helm uninstall my-nginx
```

### Helm History

```bash
# View history
helm history my-nginx

# Rollback to revision
helm rollback my-nginx 2
```

---

## Task 2.4: Use Kustomize ⭐

### Kustomize Structure

```
├── base/
│   ├── kustomization.yaml
│   ├── deployment.yaml
│   └── service.yaml
└── overlays/
    ├── development/
    │   └── kustomization.yaml
    └── production/
        └── kustomization.yaml
```

### Base kustomization.yaml

```yaml
# base/kustomization.yaml
apiVersion: kustomize.config.k8s.io/v1beta1
kind: Kustomization
resources:
  - deployment.yaml
  - service.yaml
```

### Overlay kustomization.yaml

```yaml
# overlays/production/kustomization.yaml
apiVersion: kustomize.config.k8s.io/v1beta1
kind: Kustomization
resources:
  - ../../base
namespace: production
namePrefix: prod-
commonLabels:
  env: production
replicas:
  - name: myapp
    count: 5
images:
  - name: myapp
    newTag: v2.0.0
```

### Kustomize Commands

```bash
# View kustomized output
kubectl kustomize ./overlays/production

# Apply kustomization
kubectl apply -k ./overlays/production

# Delete kustomization
kubectl delete -k ./overlays/production
```

### Kustomize Patches

```yaml
# overlays/production/kustomization.yaml
patches:
  - patch: |-
      - op: replace
        path: /spec/replicas
        value: 10
    target:
      kind: Deployment
      name: myapp
```

---

## Deployment Strategy Comparison

| Strategy | Downtime | Rollback | Traffic Control |
|----------|----------|----------|-----------------|
| **Rolling Update** | No | Easy | No |
| **Recreate** | Yes | Easy | No |
| **Blue/Green** | No | Instant | Full |
| **Canary** | No | Easy | Partial |

---

## Exam Tips for Domain 2

1. **Rolling Update** - Default strategy, know maxSurge/maxUnavailable
2. **Recreate** - Use for incompatible versions
3. **Blue/Green** - Two deployments, switch service selector
4. **Canary** - Partial traffic to new version
5. **kubectl rollout** - status, history, undo, restart
6. **Helm install** - -f for values, --set for inline
7. **Helm upgrade** - --install for create-or-update
8. **Kustomize** - kubectl apply -k
9. **Record changes** - --record or annotations
10. **Revision history** - rollout history for debugging
