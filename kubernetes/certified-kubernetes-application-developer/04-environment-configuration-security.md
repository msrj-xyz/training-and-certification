# Domain 4: Application Environment, Configuration, and Security (25%)

## Task 4.1: Discover and Use Resources that Extend Kubernetes ⭐

### Custom Resource Definitions (CRDs)

```bash
# List CRDs
kubectl get crd

# View CRD details
kubectl describe crd certificates.cert-manager.io

# Get custom resources
kubectl get certificates -A
kubectl get <crd-name> -A
```

### CRD Example

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
                engine:
                  type: string
  scope: Namespaced
  names:
    plural: databases
    singular: database
    kind: Database
    shortNames:
      - db
```

### Use Custom Resource

```yaml
apiVersion: example.com/v1
kind: Database
metadata:
  name: my-database
spec:
  size: 10
  engine: mysql
```

---

## Task 4.2: Understand Authentication, Authorization, and Admission Control ⭐

### RBAC Overview

| Component | Scope | Description |
|-----------|-------|-------------|
| **Role** | Namespace | Permissions in namespace |
| **ClusterRole** | Cluster | Cluster-wide permissions |
| **RoleBinding** | Namespace | Binds Role to subjects |
| **ClusterRoleBinding** | Cluster | Binds ClusterRole to subjects |

### Create Role

```bash
kubectl create role pod-reader \
  --verb=get,list,watch \
  --resource=pods \
  -n development
```

### Create RoleBinding

```bash
kubectl create rolebinding read-pods \
  --role=pod-reader \
  --user=jane \
  -n development
```

### Check Permissions

```bash
# Can I do this?
kubectl auth can-i create pods
kubectl auth can-i delete deployments -n production

# As another user
kubectl auth can-i create pods --as jane

# List all permissions
kubectl auth can-i --list
kubectl auth can-i --list --as jane
```

---

## Task 4.3: Resource Requests and Limits ⭐⭐

### Pod Resources

```yaml
spec:
  containers:
  - name: app
    image: myapp
    resources:
      requests:
        memory: "128Mi"
        cpu: "250m"
      limits:
        memory: "256Mi"
        cpu: "500m"
```

### Resource Units

| Unit | CPU | Memory |
|------|-----|--------|
| **m** | 1000m = 1 CPU | - |
| **Mi** | - | Mebibytes |
| **Gi** | - | Gibibytes |

### ResourceQuota

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
    pods: "20"
    configmaps: "10"
    secrets: "10"
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
    max:
      cpu: "2"
      memory: "1Gi"
    min:
      cpu: "50m"
      memory: "64Mi"
    type: Container
```

---

## Task 4.4: ConfigMaps ⭐⭐

### Create ConfigMap

```bash
# From literals
kubectl create configmap app-config \
  --from-literal=DATABASE_HOST=mysql \
  --from-literal=DATABASE_PORT=3306

# From file
kubectl create configmap nginx-config --from-file=nginx.conf

# From env file
kubectl create configmap env-config --from-env-file=app.env

# From directory
kubectl create configmap configs --from-file=./configs/
```

### ConfigMap YAML

```yaml
apiVersion: v1
kind: ConfigMap
metadata:
  name: app-config
data:
  DATABASE_HOST: mysql
  DATABASE_PORT: "3306"
  config.json: |
    {
      "debug": false,
      "log_level": "info"
    }
```

### Use ConfigMap

```yaml
# As environment variables (all keys)
spec:
  containers:
  - name: app
    image: myapp
    envFrom:
    - configMapRef:
        name: app-config

# Specific key as env
    env:
    - name: DB_HOST
      valueFrom:
        configMapKeyRef:
          name: app-config
          key: DATABASE_HOST

# As volume
    volumeMounts:
    - name: config
      mountPath: /etc/config
  volumes:
  - name: config
    configMap:
      name: app-config
      items:
      - key: config.json
        path: app-config.json
```

---

## Task 4.5: Create and Consume Secrets ⭐⭐

### Create Secret

```bash
# Generic secret
kubectl create secret generic db-secret \
  --from-literal=username=admin \
  --from-literal=password=supersecret

# From file
kubectl create secret generic tls-certs \
  --from-file=tls.crt \
  --from-file=tls.key

# TLS secret
kubectl create secret tls tls-secret \
  --cert=tls.crt \
  --key=tls.key

# Docker registry secret
kubectl create secret docker-registry regcred \
  --docker-server=docker.io \
  --docker-username=user \
  --docker-password=pass
```

### Secret Types

| Type | Purpose |
|------|---------|
| **Opaque** | Arbitrary data (default) |
| **kubernetes.io/tls** | TLS certificates |
| **kubernetes.io/dockerconfigjson** | Docker registry |
| **kubernetes.io/basic-auth** | Basic authentication |

### Use Secret

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
    - name: secrets
      mountPath: /etc/secrets
      readOnly: true
  volumes:
  - name: secrets
    secret:
      secretName: db-secret
```

### ImagePullSecrets

```yaml
spec:
  imagePullSecrets:
  - name: regcred
  containers:
  - name: app
    image: private-registry.io/myapp:v1
```

---

## Task 4.6: ServiceAccounts ⭐

### Create ServiceAccount

```bash
kubectl create serviceaccount app-sa -n production
```

### Use in Pod

```yaml
spec:
  serviceAccountName: app-sa
  automountServiceAccountToken: false  # Disable auto-mount
  containers:
  - name: app
    image: myapp
```

### ServiceAccount with RBAC

```bash
# Create SA
kubectl create sa app-sa

# Create Role
kubectl create role pod-reader --verb=get,list --resource=pods

# Bind Role to SA
kubectl create rolebinding app-sa-binding \
  --role=pod-reader \
  --serviceaccount=default:app-sa
```

---

## Task 4.7: Security Context and Capabilities ⭐

### Pod Security Context

```yaml
spec:
  securityContext:
    runAsUser: 1000
    runAsGroup: 3000
    fsGroup: 2000
    runAsNonRoot: true
```

### Container Security Context

```yaml
spec:
  containers:
  - name: app
    image: myapp
    securityContext:
      runAsUser: 1000
      runAsNonRoot: true
      readOnlyRootFilesystem: true
      allowPrivilegeEscalation: false
      capabilities:
        drop:
          - ALL
        add:
          - NET_BIND_SERVICE
```

### Linux Capabilities

| Capability | Purpose |
|------------|---------|
| **NET_BIND_SERVICE** | Bind to low ports (<1024) |
| **SYS_TIME** | Modify system clock |
| **NET_ADMIN** | Network administration |
| **SYS_ADMIN** | Many admin operations |

---

## Exam Tips for Domain 4

1. **ConfigMap** - --from-literal, --from-file options
2. **Secret** - Base64 encoded, not encrypted
3. **envFrom** - Load all keys as env vars
4. **valueFrom** - Load specific key
5. **Volume mount** - Mount as files
6. **ServiceAccount** - Pod identity
7. **automountServiceAccountToken** - Disable when not needed
8. **securityContext** - runAsNonRoot, readOnlyRootFilesystem
9. **Capabilities** - Drop ALL, add specific
10. **RBAC** - kubectl auth can-i for testing
