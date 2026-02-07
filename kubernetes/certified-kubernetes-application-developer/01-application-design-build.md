# Domain 1: Application Design and Build (20%)

## Task 1.1: Define, Build and Modify Container Images ⭐

### Dockerfile Best Practices

```dockerfile
# Multi-stage build example
FROM golang:1.21 AS builder
WORKDIR /app
COPY go.mod go.sum ./
RUN go mod download
COPY . .
RUN CGO_ENABLED=0 GOOS=linux go build -o main .

# Runtime stage
FROM alpine:3.19
RUN adduser -D -u 1000 appuser
WORKDIR /app
COPY --from=builder /app/main .
USER appuser
EXPOSE 8080
CMD ["./main"]
```

### Key Dockerfile Instructions

| Instruction | Purpose |
|-------------|---------|
| **FROM** | Base image |
| **WORKDIR** | Set working directory |
| **COPY** | Copy files into image |
| **RUN** | Execute commands |
| **ENV** | Set environment variables |
| **EXPOSE** | Document port |
| **USER** | Run as non-root |
| **ENTRYPOINT** | Container executable |
| **CMD** | Default arguments |

### Build and Push Images

```bash
# Build image
docker build -t myapp:v1 .

# Tag image
docker tag myapp:v1 registry.example.com/myapp:v1

# Push to registry
docker push registry.example.com/myapp:v1

# Build with different Dockerfile
docker build -f Dockerfile.prod -t myapp:prod .
```

---

## Task 1.2: Choose Appropriate Workload Resources ⭐⭐

### Workload Types

| Workload | Use Case |
|----------|----------|
| **Deployment** | Stateless apps, rolling updates |
| **StatefulSet** | Stateful apps, stable identity |
| **DaemonSet** | One pod per node (agents) |
| **Job** | Run to completion |
| **CronJob** | Scheduled tasks |
| **ReplicaSet** | Managed by Deployment |

### Deployment

```bash
# Create deployment
kubectl create deployment nginx --image=nginx:1.24 --replicas=3

# Generate YAML
kubectl create deployment nginx --image=nginx --dry-run=client -o yaml > deploy.yaml
```

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: web-app
spec:
  replicas: 3
  selector:
    matchLabels:
      app: web
  template:
    metadata:
      labels:
        app: web
    spec:
      containers:
      - name: web
        image: nginx:1.24
        ports:
        - containerPort: 80
```

### StatefulSet

```yaml
apiVersion: apps/v1
kind: StatefulSet
metadata:
  name: mysql
spec:
  serviceName: mysql
  replicas: 3
  selector:
    matchLabels:
      app: mysql
  template:
    metadata:
      labels:
        app: mysql
    spec:
      containers:
      - name: mysql
        image: mysql:8.0
        volumeMounts:
        - name: data
          mountPath: /var/lib/mysql
  volumeClaimTemplates:
  - metadata:
      name: data
    spec:
      accessModes: ["ReadWriteOnce"]
      resources:
        requests:
          storage: 10Gi
```

### DaemonSet

```yaml
apiVersion: apps/v1
kind: DaemonSet
metadata:
  name: fluentd
spec:
  selector:
    matchLabels:
      name: fluentd
  template:
    metadata:
      labels:
        name: fluentd
    spec:
      containers:
      - name: fluentd
        image: fluentd:latest
```

### Job

```bash
# Create job
kubectl create job backup --image=busybox -- /bin/sh -c "echo backup done"
```

```yaml
apiVersion: batch/v1
kind: Job
metadata:
  name: backup-job
spec:
  completions: 3
  parallelism: 2
  backoffLimit: 4
  template:
    spec:
      restartPolicy: Never
      containers:
      - name: backup
        image: busybox
        command: ["/bin/sh", "-c", "echo backup done"]
```

### CronJob

```bash
# Create cronjob
kubectl create cronjob daily-backup --image=busybox --schedule="0 2 * * *" -- /bin/sh -c "echo backup"
```

```yaml
apiVersion: batch/v1
kind: CronJob
metadata:
  name: daily-backup
spec:
  schedule: "0 2 * * *"  # 2 AM daily
  jobTemplate:
    spec:
      template:
        spec:
          restartPolicy: OnFailure
          containers:
          - name: backup
            image: busybox
            command: ["/bin/sh", "-c", "echo daily backup"]
```

---

## Task 1.3: Multi-Container Pod Patterns ⭐⭐

### Init Container

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: init-pod
spec:
  initContainers:
  - name: init-db
    image: busybox
    command: ['sh', '-c', 'until nslookup db-service; do sleep 2; done']
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

### Sidecar Container

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: sidecar-pod
spec:
  containers:
  - name: app
    image: nginx
    volumeMounts:
    - name: logs
      mountPath: /var/log/nginx
  - name: log-shipper
    image: fluentd
    volumeMounts:
    - name: logs
      mountPath: /var/log/nginx
      readOnly: true
  volumes:
  - name: logs
    emptyDir: {}
```

### Ambassador Pattern

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: ambassador-pod
spec:
  containers:
  - name: app
    image: myapp
    env:
    - name: DB_HOST
      value: "localhost"
  - name: ambassador
    image: haproxy
    ports:
    - containerPort: 3306
```

### Adapter Pattern

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: adapter-pod
spec:
  containers:
  - name: app
    image: legacy-app
    volumeMounts:
    - name: logs
      mountPath: /var/log
  - name: adapter
    image: log-adapter
    volumeMounts:
    - name: logs
      mountPath: /var/log
      readOnly: true
```

---

## Task 1.4: Utilize Persistent and Ephemeral Volumes ⭐

### emptyDir (Ephemeral)

```yaml
volumes:
- name: cache
  emptyDir:
    sizeLimit: 500Mi
    medium: Memory  # Use RAM
```

### PersistentVolumeClaim

```yaml
apiVersion: v1
kind: PersistentVolumeClaim
metadata:
  name: app-pvc
spec:
  accessModes:
    - ReadWriteOnce
  resources:
    requests:
      storage: 5Gi
  storageClassName: standard
```

### Pod with PVC

```yaml
spec:
  containers:
  - name: app
    image: myapp
    volumeMounts:
    - name: data
      mountPath: /data
  volumes:
  - name: data
    persistentVolumeClaim:
      claimName: app-pvc
```

---

## Exam Tips for Domain 1

1. **Multi-stage Dockerfile** - Smaller, secure images
2. **Deployment** - Most common workload type
3. **StatefulSet** - Databases, stable network identity
4. **Job vs CronJob** - One-time vs scheduled
5. **Init containers** - Run before main containers
6. **Sidecar** - Log shipping, proxies
7. **emptyDir** - Ephemeral, share data between containers
8. **PVC** - Persistent storage
9. **--dry-run=client -o yaml** - Generate templates
10. **Container patterns** - Know all three
