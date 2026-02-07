# Domain 5: Services and Networking (20%)

## Task 5.1: Demonstrate Understanding of NetworkPolicies ⭐

### Default Deny All

```yaml
apiVersion: networking.k8s.io/v1
kind: NetworkPolicy
metadata:
  name: deny-all
  namespace: production
spec:
  podSelector: {}
  policyTypes:
  - Ingress
  - Egress
```

### Allow Specific Ingress

```yaml
apiVersion: networking.k8s.io/v1
kind: NetworkPolicy
metadata:
  name: allow-frontend
  namespace: production
spec:
  podSelector:
    matchLabels:
      app: backend
  policyTypes:
  - Ingress
  ingress:
  - from:
    - podSelector:
        matchLabels:
          app: frontend
    ports:
    - protocol: TCP
      port: 8080
```

### Allow from Namespace

```yaml
apiVersion: networking.k8s.io/v1
kind: NetworkPolicy
metadata:
  name: allow-monitoring
spec:
  podSelector:
    matchLabels:
      app: api
  ingress:
  - from:
    - namespaceSelector:
        matchLabels:
          name: monitoring
```

### Allow Egress to External

```yaml
apiVersion: networking.k8s.io/v1
kind: NetworkPolicy
metadata:
  name: allow-external
spec:
  podSelector:
    matchLabels:
      app: web
  policyTypes:
  - Egress
  egress:
  - to:
    - ipBlock:
        cidr: 0.0.0.0/0
        except:
        - 10.0.0.0/8
    ports:
    - protocol: TCP
      port: 443
```

### Allow DNS Egress

```yaml
egress:
- to:
  - namespaceSelector: {}
    podSelector:
      matchLabels:
        k8s-app: kube-dns
  ports:
  - protocol: UDP
    port: 53
```

> **Exam Tip:** Without any NetworkPolicy, all traffic is allowed!

---

## Task 5.2: Provide and Troubleshoot Access via Services ⭐⭐

### Service Types

| Type | Description | Access |
|------|-------------|--------|
| **ClusterIP** | Internal only (default) | Within cluster |
| **NodePort** | Expose on node port | node-ip:30000-32767 |
| **LoadBalancer** | Cloud provider LB | External IP |
| **ExternalName** | DNS CNAME | External service |

### Create Services

```bash
# ClusterIP
kubectl create service clusterip nginx --tcp=80:80

# NodePort
kubectl create service nodeport nginx --tcp=80:80 --node-port=30080

# Expose deployment
kubectl expose deployment nginx --port=80 --target-port=80
kubectl expose deployment nginx --port=80 --type=NodePort
kubectl expose deployment nginx --port=80 --type=LoadBalancer

# Expose pod
kubectl expose pod nginx --port=80 --name=nginx-svc
```

### ClusterIP Service

```yaml
apiVersion: v1
kind: Service
metadata:
  name: backend-svc
spec:
  type: ClusterIP
  selector:
    app: backend
  ports:
  - port: 80
    targetPort: 8080
```

### NodePort Service

```yaml
apiVersion: v1
kind: Service
metadata:
  name: web-svc
spec:
  type: NodePort
  selector:
    app: web
  ports:
  - port: 80
    targetPort: 80
    nodePort: 30080  # Optional: 30000-32767
```

### LoadBalancer Service

```yaml
apiVersion: v1
kind: Service
metadata:
  name: api-svc
spec:
  type: LoadBalancer
  selector:
    app: api
  ports:
  - port: 80
    targetPort: 8080
```

### Headless Service

```yaml
apiVersion: v1
kind: Service
metadata:
  name: db-headless
spec:
  clusterIP: None
  selector:
    app: database
  ports:
  - port: 3306
```

### Troubleshoot Services

```bash
# Check service
kubectl get svc <service-name>
kubectl describe svc <service-name>

# Check endpoints
kubectl get endpoints <service-name>

# Test DNS
kubectl run test --image=busybox --rm -it --restart=Never -- nslookup <service-name>

# Test connectivity
kubectl run test --image=busybox --rm -it --restart=Never -- wget -O- <service-name>:<port>
```

### Common Service Issues

| Issue | Check | Solution |
|-------|-------|----------|
| **No endpoints** | Selector labels | Fix pod labels to match |
| **Connection refused** | targetPort | Verify container port |
| **Timeout** | NetworkPolicy | Check network rules |
| **Pod not ready** | Readiness probe | Fix probe or app |

---

## Task 5.3: Use Ingress to Expose Applications ⭐⭐

### Simple Ingress

```yaml
apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  name: simple-ingress
  annotations:
    nginx.ingress.kubernetes.io/rewrite-target: /
spec:
  ingressClassName: nginx
  rules:
  - host: app.example.com
    http:
      paths:
      - path: /
        pathType: Prefix
        backend:
          service:
            name: app-service
            port:
              number: 80
```

### Multi-Path Ingress

```yaml
apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  name: multi-path
spec:
  ingressClassName: nginx
  rules:
  - host: api.example.com
    http:
      paths:
      - path: /users
        pathType: Prefix
        backend:
          service:
            name: users-svc
            port:
              number: 80
      - path: /orders
        pathType: Prefix
        backend:
          service:
            name: orders-svc
            port:
              number: 80
```

### Multi-Host Ingress

```yaml
apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  name: multi-host
spec:
  ingressClassName: nginx
  rules:
  - host: app1.example.com
    http:
      paths:
      - path: /
        pathType: Prefix
        backend:
          service:
            name: app1-svc
            port:
              number: 80
  - host: app2.example.com
    http:
      paths:
      - path: /
        pathType: Prefix
        backend:
          service:
            name: app2-svc
            port:
              number: 80
```

### Ingress with TLS

```yaml
apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  name: tls-ingress
spec:
  ingressClassName: nginx
  tls:
  - hosts:
    - app.example.com
    secretName: tls-secret
  rules:
  - host: app.example.com
    http:
      paths:
      - path: /
        pathType: Prefix
        backend:
          service:
            name: app-svc
            port:
              number: 80
```

### Path Types

| Type | Behavior |
|------|----------|
| **Prefix** | URL path prefix match |
| **Exact** | Exact path match |
| **ImplementationSpecific** | Controller-specific |

### Ingress Annotations (nginx)

```yaml
annotations:
  nginx.ingress.kubernetes.io/rewrite-target: /
  nginx.ingress.kubernetes.io/ssl-redirect: "true"
  nginx.ingress.kubernetes.io/proxy-body-size: "10m"
  nginx.ingress.kubernetes.io/proxy-read-timeout: "60"
```

---

## DNS in Kubernetes

### DNS Record Formats

| Resource | DNS Format |
|----------|------------|
| **Service** | `<svc>.<ns>.svc.cluster.local` |
| **Pod** | `<pod-ip>.<ns>.pod.cluster.local` |
| **Headless** | `<pod-name>.<svc>.<ns>.svc.cluster.local` |

### Test DNS

```bash
# Full DNS name
kubectl run test --image=busybox --rm -it --restart=Never -- \
  nslookup nginx-svc.default.svc.cluster.local

# Short name (same namespace)
kubectl run test --image=busybox --rm -it --restart=Never -- \
  nslookup nginx-svc
```

---

## Exam Tips for Domain 5

1. **NetworkPolicy** - Empty selector = all pods
2. **No NetworkPolicy** - All traffic allowed by default
3. **Service types** - ClusterIP, NodePort, LoadBalancer
4. **kubectl expose** - Quick service creation
5. **Endpoints** - Check service-to-pod mapping
6. **Ingress** - Requires controller (nginx, traefik)
7. **ingressClassName** - Required in newer versions
8. **Path types** - Prefix vs Exact
9. **TLS secret** - Must have tls.crt and tls.key
10. **DNS** - service.namespace.svc.cluster.local
