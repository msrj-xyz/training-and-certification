# Domain 3: Services & Networking (20%)

## Task 3.1: Understand Service Types ⭐

### Service Types Overview

| Type | Description | Use Case |
|------|-------------|----------|
| **ClusterIP** | Internal cluster IP (default) | Internal communication |
| **NodePort** | Expose on node port (30000-32767) | External access via node |
| **LoadBalancer** | Cloud provider LB | Production external access |
| **ExternalName** | DNS CNAME record | External service mapping |

---

### Create Services (Imperative)

```bash
# ClusterIP service
kubectl create service clusterip nginx --tcp=80:80

# NodePort service
kubectl create service nodeport nginx --tcp=80:80 --node-port=30080

# Expose deployment as service
kubectl expose deployment nginx --port=80 --target-port=80 --type=ClusterIP

# Expose with NodePort
kubectl expose deployment nginx --port=80 --type=NodePort

# Expose with specific name
kubectl expose deployment nginx --port=80 --name=nginx-svc
```

### ClusterIP Service YAML

```yaml
apiVersion: v1
kind: Service
metadata:
  name: nginx-service
spec:
  type: ClusterIP
  selector:
    app: nginx
  ports:
  - protocol: TCP
    port: 80
    targetPort: 80
```

### NodePort Service YAML

```yaml
apiVersion: v1
kind: Service
metadata:
  name: nginx-nodeport
spec:
  type: NodePort
  selector:
    app: nginx
  ports:
  - protocol: TCP
    port: 80
    targetPort: 80
    nodePort: 30080  # Optional: 30000-32767
```

### LoadBalancer Service YAML

```yaml
apiVersion: v1
kind: Service
metadata:
  name: nginx-lb
spec:
  type: LoadBalancer
  selector:
    app: nginx
  ports:
  - protocol: TCP
    port: 80
    targetPort: 80
```

### Headless Service (ClusterIP: None)

```yaml
apiVersion: v1
kind: Service
metadata:
  name: nginx-headless
spec:
  clusterIP: None
  selector:
    app: nginx
  ports:
  - port: 80
```

> **Exam Tip:** Headless services are used for StatefulSets to get stable DNS names!

---

## Task 3.2: Understand Pod Networking ⭐

### Pod Networking Model

| Principle | Description |
|-----------|-------------|
| **Pod-to-Pod** | All pods can communicate without NAT |
| **Node-to-Pod** | All nodes can communicate with all pods |
| **Pod IP** | Each pod gets unique IP |
| **Same Pod** | Containers share network namespace |

### Check Pod Networking

```bash
# Get pod IP
kubectl get pod nginx -o wide

# Test connectivity
kubectl exec -it busybox -- ping <pod-ip>

# Check DNS resolution
kubectl exec -it busybox -- nslookup nginx-service
```

---

## Task 3.3: Network Policies ⭐⭐

### Network Policy Components

| Component | Description |
|-----------|-------------|
| **podSelector** | Target pods for policy |
| **policyTypes** | Ingress, Egress, or both |
| **ingress** | Incoming traffic rules |
| **egress** | Outgoing traffic rules |

### Default Deny All Ingress

```yaml
apiVersion: networking.k8s.io/v1
kind: NetworkPolicy
metadata:
  name: deny-all-ingress
  namespace: production
spec:
  podSelector: {}
  policyTypes:
  - Ingress
```

### Default Deny All Egress

```yaml
apiVersion: networking.k8s.io/v1
kind: NetworkPolicy
metadata:
  name: deny-all-egress
  namespace: production
spec:
  podSelector: {}
  policyTypes:
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
  name: allow-from-monitoring
spec:
  podSelector:
    matchLabels:
      app: api
  policyTypes:
  - Ingress
  ingress:
  - from:
    - namespaceSelector:
        matchLabels:
          name: monitoring
```

### Allow Egress to Specific CIDR

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
        cidr: 10.0.0.0/8
        except:
        - 10.0.1.0/24
    ports:
    - protocol: TCP
      port: 443
```

> **Exam Tip:** If no NetworkPolicy selects a pod, all traffic is allowed!

---

## Task 3.4: Ingress Controllers and Resources ⭐

### Ingress Controller

```bash
# Install nginx ingress controller
kubectl apply -f https://raw.githubusercontent.com/kubernetes/ingress-nginx/controller-v1.8.0/deploy/static/provider/cloud/deploy.yaml

# Verify controller
kubectl get pods -n ingress-nginx
kubectl get svc -n ingress-nginx
```

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

### Ingress with Multiple Paths

```yaml
apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  name: multi-path-ingress
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
            name: users-service
            port:
              number: 80
      - path: /products
        pathType: Prefix
        backend:
          service:
            name: products-service
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
            name: app-service
            port:
              number: 80
```

### Path Types

| Type | Behavior |
|------|----------|
| **Prefix** | Matches URL path prefix |
| **Exact** | Exact path match |
| **ImplementationSpecific** | Controller-dependent |

---

## Task 3.5: Gateway API

### Gateway

```yaml
apiVersion: gateway.networking.k8s.io/v1
kind: Gateway
metadata:
  name: my-gateway
spec:
  gatewayClassName: nginx
  listeners:
  - name: http
    port: 80
    protocol: HTTP
```

### HTTPRoute

```yaml
apiVersion: gateway.networking.k8s.io/v1
kind: HTTPRoute
metadata:
  name: http-route
spec:
  parentRefs:
  - name: my-gateway
  hostnames:
  - "app.example.com"
  rules:
  - matches:
    - path:
        type: PathPrefix
        value: /api
    backendRefs:
    - name: api-service
      port: 80
```

---

## Task 3.6: CoreDNS Configuration ⭐

### CoreDNS Components

| Component | Description |
|-----------|-------------|
| **Deployment** | kube-dns in kube-system |
| **Service** | kube-dns (ClusterIP) |
| **ConfigMap** | coredns (Corefile) |

### View CoreDNS Config

```bash
# Get CoreDNS pods
kubectl get pods -n kube-system -l k8s-app=kube-dns

# View Corefile
kubectl get configmap coredns -n kube-system -o yaml
```

### DNS Records

| Record Type | Format |
|-------------|--------|
| **Service** | service.namespace.svc.cluster.local |
| **Pod** | pod-ip.namespace.pod.cluster.local |
| **Headless** | pod-name.service.namespace.svc.cluster.local |

### Test DNS Resolution

```bash
# Test service DNS
kubectl run test-dns --image=busybox --rm -it --restart=Never -- nslookup nginx-service

# Test FQDN
kubectl exec -it busybox -- nslookup nginx-service.default.svc.cluster.local
```

### Custom DNS Config in Pod

```yaml
spec:
  dnsPolicy: None
  dnsConfig:
    nameservers:
    - 8.8.8.8
    searches:
    - my.dns.search.suffix
    options:
    - name: ndots
      value: "2"
```

---

## Task 3.7: Host Networking Configuration

### Host Network Pod

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: host-network-pod
spec:
  hostNetwork: true
  containers:
  - name: nginx
    image: nginx
```

### CNI Configuration

```bash
# CNI config location
ls /etc/cni/net.d/

# CNI binaries
ls /opt/cni/bin/

# View current CNI config
cat /etc/cni/net.d/10-flannel.conflist
```

---

## Useful Networking Commands

```bash
# Get all services
kubectl get svc -A

# Get endpoints
kubectl get endpoints nginx-service

# Describe service
kubectl describe svc nginx-service

# Get ingress
kubectl get ingress -A

# Get network policies
kubectl get networkpolicy -A

# Test connectivity
kubectl run test --image=busybox --rm -it --restart=Never -- wget -O- http://nginx-service:80
```

---

## Exam Tips for Domain 3

1. **Service Types** - Know ClusterIP, NodePort, LoadBalancer differences
2. **Expose command** - `kubectl expose deployment` for quick service creation
3. **Network Policies** - Empty podSelector = all pods in namespace
4. **No NetworkPolicy** - All traffic allowed by default
5. **Ingress** - Requires ingress controller installed
6. **IngressClassName** - Required in newer versions
7. **CoreDNS** - service.namespace.svc.cluster.local format
8. **Headless Service** - clusterIP: None for StatefulSets
9. **Endpoints** - Verify service-to-pod mapping
10. **DNS Testing** - Use busybox for nslookup/wget tests
