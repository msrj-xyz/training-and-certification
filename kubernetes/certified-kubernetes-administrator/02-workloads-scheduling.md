# Domain 2: Workloads & Scheduling (15%)

## Task 2.1: Understand Deployments and Rolling Updates ⭐

### Create Deployment (Imperative)

```bash
# Create deployment
kubectl create deployment nginx --image=nginx:1.23 --replicas=3

# Create with port
kubectl create deployment web --image=nginx --port=80

# Generate YAML
kubectl create deployment nginx --image=nginx --dry-run=client -o yaml > deploy.yaml
```

### Deployment YAML

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: nginx-deployment
spec:
  replicas: 3
  selector:
    matchLabels:
      app: nginx
  strategy:
    type: RollingUpdate
    rollingUpdate:
      maxSurge: 1
      maxUnavailable: 1
  template:
    metadata:
      labels:
        app: nginx
    spec:
      containers:
      - name: nginx
        image: nginx:1.23
        ports:
        - containerPort: 80
        resources:
          requests:
            memory: "64Mi"
            cpu: "250m"
          limits:
            memory: "128Mi"
            cpu: "500m"
```

---

### Rolling Updates

```bash
# Update image
kubectl set image deployment/nginx nginx=nginx:1.24

# Check rollout status
kubectl rollout status deployment/nginx

# View rollout history
kubectl rollout history deployment/nginx

# Rollback to previous
kubectl rollout undo deployment/nginx

# Rollback to specific revision
kubectl rollout undo deployment/nginx --to-revision=2

# Pause/Resume rollout
kubectl rollout pause deployment/nginx
kubectl rollout resume deployment/nginx
```

### Update Strategies

| Strategy | Description |
|----------|-------------|
| **RollingUpdate** | Gradual replacement (default) |
| **Recreate** | Delete all, then create new |

---

## Task 2.2: Configure Applications with ConfigMaps ⭐

### Create ConfigMap (Imperative)

```bash
# From literal values
kubectl create configmap my-config \
  --from-literal=key1=value1 \
  --from-literal=key2=value2

# From file
kubectl create configmap app-config --from-file=config.properties

# From directory
kubectl create configmap configs --from-file=./configs/

# From env file
kubectl create configmap env-config --from-env-file=app.env
```

### ConfigMap YAML

```yaml
apiVersion: v1
kind: ConfigMap
metadata:
  name: app-config
data:
  DATABASE_HOST: "mysql.default.svc"
  DATABASE_PORT: "3306"
  config.json: |
    {
      "environment": "production",
      "debug": false
    }
```

### Use ConfigMap in Pod

```yaml
# As environment variables
spec:
  containers:
  - name: app
    image: myapp
    envFrom:
    - configMapRef:
        name: app-config

# Specific keys as env
    env:
    - name: DB_HOST
      valueFrom:
        configMapKeyRef:
          name: app-config
          key: DATABASE_HOST

# As volume
    volumeMounts:
    - name: config-volume
      mountPath: /etc/config
  volumes:
  - name: config-volume
    configMap:
      name: app-config
```

---

## Task 2.3: Configure Applications with Secrets ⭐

### Create Secret (Imperative)

```bash
# Generic secret from literals
kubectl create secret generic db-secret \
  --from-literal=username=admin \
  --from-literal=password=supersecret

# From file
kubectl create secret generic tls-secret --from-file=./certs/

# Docker registry secret
kubectl create secret docker-registry regcred \
  --docker-server=docker.io \
  --docker-username=user \
  --docker-password=pass \
  --docker-email=email@example.com

# TLS secret
kubectl create secret tls tls-secret \
  --cert=path/to/cert.pem \
  --key=path/to/key.pem
```

### Secret Types

| Type | Usage |
|------|-------|
| **Opaque** | Arbitrary data (default) |
| **kubernetes.io/tls** | TLS certificates |
| **kubernetes.io/dockerconfigjson** | Docker registry auth |
| **kubernetes.io/basic-auth** | Basic authentication |
| **kubernetes.io/service-account-token** | SA token |

### Use Secret in Pod

```yaml
# As environment variables
spec:
  containers:
  - name: app
    image: myapp
    envFrom:
    - secretRef:
        name: db-secret

# Specific key as env
    env:
    - name: DB_PASSWORD
      valueFrom:
        secretKeyRef:
          name: db-secret
          key: password

# As volume
    volumeMounts:
    - name: secret-volume
      mountPath: /etc/secrets
      readOnly: true
  volumes:
  - name: secret-volume
    secret:
      secretName: db-secret
```

> **Exam Tip:** Secrets are base64 encoded, NOT encrypted by default!

---

## Task 2.4: Scale Applications ⭐

### Manual Scaling

```bash
# Scale deployment
kubectl scale deployment nginx --replicas=5

# Scale multiple
kubectl scale deployment nginx web api --replicas=3

# Scale with condition
kubectl scale deployment nginx --replicas=3 --current-replicas=5
```

### Horizontal Pod Autoscaler (HPA)

```bash
# Create HPA
kubectl autoscale deployment nginx \
  --min=2 \
  --max=10 \
  --cpu-percent=80

# Get HPA
kubectl get hpa

# Delete HPA
kubectl delete hpa nginx
```

### HPA YAML

```yaml
apiVersion: autoscaling/v2
kind: HorizontalPodAutoscaler
metadata:
  name: nginx-hpa
spec:
  scaleTargetRef:
    apiVersion: apps/v1
    kind: Deployment
    name: nginx
  minReplicas: 2
  maxReplicas: 10
  metrics:
  - type: Resource
    resource:
      name: cpu
      target:
        type: Utilization
        averageUtilization: 80
```

---

## Task 2.5: Understand Static Pods ⭐

### Static Pod Characteristics

| Aspect | Detail |
|--------|--------|
| **Location** | /etc/kubernetes/manifests |
| **Management** | kubelet only (no API server) |
| **Mirror Pod** | Created in API server (read-only) |
| **Naming** | pod-name-nodename |

### Create Static Pod

```bash
# Find static pod path
cat /var/lib/kubelet/config.yaml | grep staticPodPath

# Create static pod manifest
cat << EOF > /etc/kubernetes/manifests/nginx-static.yaml
apiVersion: v1
kind: Pod
metadata:
  name: nginx-static
spec:
  containers:
  - name: nginx
    image: nginx
EOF
```

> **Exam Tip:** Control plane components (apiserver, etcd, etc.) are static pods!

---

## Task 2.6: Pod Admission and Scheduling ⭐

### Node Selectors

```yaml
spec:
  nodeSelector:
    disktype: ssd
    environment: production
```

### Node Affinity

```yaml
spec:
  affinity:
    nodeAffinity:
      requiredDuringSchedulingIgnoredDuringExecution:
        nodeSelectorTerms:
        - matchExpressions:
          - key: disktype
            operator: In
            values:
            - ssd
      preferredDuringSchedulingIgnoredDuringExecution:
      - weight: 1
        preference:
          matchExpressions:
          - key: zone
            operator: In
            values:
            - us-west-1a
```

### Taints and Tolerations

```bash
# Add taint to node
kubectl taint nodes node1 key=value:NoSchedule

# Remove taint
kubectl taint nodes node1 key=value:NoSchedule-

# View taints
kubectl describe node node1 | grep Taint
```

### Toleration in Pod

```yaml
spec:
  tolerations:
  - key: "key"
    operator: "Equal"
    value: "value"
    effect: "NoSchedule"
  # Tolerate all taints
  - operator: "Exists"
```

### Taint Effects

| Effect | Behavior |
|--------|----------|
| **NoSchedule** | Pods won't be scheduled |
| **PreferNoSchedule** | Soft preference |
| **NoExecute** | Evict existing pods too |

---

## Task 2.7: Resource Limits and Requests

### Pod Resources

```yaml
spec:
  containers:
  - name: app
    image: myapp
    resources:
      requests:
        memory: "64Mi"
        cpu: "250m"
      limits:
        memory: "128Mi"
        cpu: "500m"
```

### Resource Quota

```yaml
apiVersion: v1
kind: ResourceQuota
metadata:
  name: compute-quota
  namespace: development
spec:
  hard:
    requests.cpu: "4"
    requests.memory: 8Gi
    limits.cpu: "8"
    limits.memory: 16Gi
    pods: "10"
```

### LimitRange

```yaml
apiVersion: v1
kind: LimitRange
metadata:
  name: default-limits
  namespace: development
spec:
  limits:
  - default:
      cpu: "500m"
      memory: "256Mi"
    defaultRequest:
      cpu: "100m"
      memory: "128Mi"
    type: Container
```

---

## Task 2.8: Init Containers

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: app-pod
spec:
  initContainers:
  - name: init-db
    image: busybox
    command: ['sh', '-c', 'until nslookup db; do sleep 2; done']
  - name: init-config
    image: busybox
    command: ['sh', '-c', 'cp /config/* /app-config/']
    volumeMounts:
    - name: config
      mountPath: /app-config
  containers:
  - name: app
    image: myapp
    volumeMounts:
    - name: config
      mountPath: /app-config
  volumes:
  - name: config
    emptyDir: {}
```

---

## Exam Tips for Domain 2

1. **Deployments** - Master set image, rollout commands
2. **ConfigMaps** - --from-literal, --from-file options
3. **Secrets** - base64 encoded, not encrypted
4. **Scaling** - kubectl scale, kubectl autoscale
5. **Static Pods** - /etc/kubernetes/manifests
6. **Node Affinity** - required vs preferred
7. **Taints** - NoSchedule, PreferNoSchedule, NoExecute
8. **Resources** - requests vs limits
9. **Init Containers** - Run before main containers
10. **Labels** - Critical for selectors and scheduling
