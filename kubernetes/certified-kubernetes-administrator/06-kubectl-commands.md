# kubectl Commands Cheatsheet

## Essential Aliases (Exam Setup)

```bash
# These are pre-configured in exam environment
alias k=kubectl
complete -o default -F __start_kubectl k

# Additional useful aliases
alias kgp='kubectl get pods'
alias kgs='kubectl get svc'
alias kgn='kubectl get nodes'
alias kd='kubectl describe'
alias kl='kubectl logs'
alias ke='kubectl exec -it'
```

---

## Context and Configuration

```bash
# View current context
kubectl config current-context

# List all contexts
kubectl config get-contexts

# Switch context
kubectl config use-context <context-name>

# Set default namespace
kubectl config set-context --current --namespace=<namespace>

# View kubeconfig
kubectl config view
```

---

## Resource Management

### Get Resources

```bash
# Get pods
kubectl get pods
kubectl get pods -o wide
kubectl get pods -A                    # All namespaces
kubectl get pods -n <namespace>
kubectl get pods -l app=nginx          # By label
kubectl get pods --show-labels
kubectl get pods -o yaml               # YAML output
kubectl get pods -o json               # JSON output

# Get all resources
kubectl get all
kubectl get all -A

# Get specific resource types
kubectl get nodes
kubectl get svc
kubectl get deployments
kubectl get configmaps
kubectl get secrets
kubectl get pv
kubectl get pvc
kubectl get networkpolicies
kubectl get ingress
```

### Watch Resources

```bash
kubectl get pods -w
kubectl get events -w
```

---

## Imperative Commands (Exam Speed!)

### Create Resources

```bash
# Pod
kubectl run nginx --image=nginx
kubectl run nginx --image=nginx --port=80
kubectl run nginx --image=nginx --dry-run=client -o yaml > pod.yaml

# Deployment
kubectl create deployment nginx --image=nginx
kubectl create deployment nginx --image=nginx --replicas=3

# Service
kubectl create service clusterip nginx --tcp=80:80
kubectl create service nodeport nginx --tcp=80:80 --node-port=30080

# Expose deployment
kubectl expose deployment nginx --port=80 --type=ClusterIP
kubectl expose deployment nginx --port=80 --type=NodePort
kubectl expose pod nginx --port=80 --name=nginx-svc

# ConfigMap
kubectl create configmap my-config --from-literal=key=value
kubectl create configmap my-config --from-file=config.properties

# Secret
kubectl create secret generic my-secret --from-literal=password=secret
kubectl create secret tls tls-secret --cert=cert.pem --key=key.pem

# ServiceAccount
kubectl create serviceaccount my-sa

# Role
kubectl create role pod-reader --verb=get,list,watch --resource=pods

# RoleBinding
kubectl create rolebinding read-pods --role=pod-reader --user=jane

# ClusterRole
kubectl create clusterrole node-reader --verb=get,list,watch --resource=nodes

# ClusterRoleBinding
kubectl create clusterrolebinding node-read --clusterrole=node-reader --user=admin

# Namespace
kubectl create namespace dev

# Job
kubectl create job my-job --image=busybox -- /bin/sh -c "echo hello"

# CronJob
kubectl create cronjob my-cron --image=busybox --schedule="*/5 * * * *" -- /bin/sh -c "date"
```

---

## Update Resources

```bash
# Update image
kubectl set image deployment/nginx nginx=nginx:1.24

# Scale deployment
kubectl scale deployment nginx --replicas=5

# Autoscale
kubectl autoscale deployment nginx --min=2 --max=10 --cpu-percent=80

# Edit resource
kubectl edit deployment nginx

# Patch resource
kubectl patch deployment nginx -p '{"spec":{"replicas":3}}'

# Label
kubectl label pod nginx env=prod
kubectl label pod nginx env-               # Remove label

# Annotate
kubectl annotate pod nginx description="My pod"

# Rollout management
kubectl rollout status deployment/nginx
kubectl rollout history deployment/nginx
kubectl rollout undo deployment/nginx
kubectl rollout undo deployment/nginx --to-revision=2
kubectl rollout pause deployment/nginx
kubectl rollout resume deployment/nginx
kubectl rollout restart deployment/nginx
```

---

## Delete Resources

```bash
# Delete resource
kubectl delete pod nginx
kubectl delete deployment nginx
kubectl delete svc nginx

# Delete by label
kubectl delete pods -l app=nginx

# Delete all pods in namespace
kubectl delete pods --all -n dev

# Force delete
kubectl delete pod nginx --force --grace-period=0

# Delete from file
kubectl delete -f pod.yaml
```

---

## Describe and Explain

```bash
# Describe (detailed info + events)
kubectl describe pod nginx
kubectl describe node worker-1
kubectl describe svc nginx

# Explain (API documentation)
kubectl explain pod
kubectl explain pod.spec
kubectl explain pod.spec.containers
kubectl explain deployment.spec.strategy
```

---

## Logs and Debugging

```bash
# Logs
kubectl logs nginx
kubectl logs nginx -c container-name      # Specific container
kubectl logs nginx --previous              # Previous container
kubectl logs nginx -f                      # Follow
kubectl logs nginx --tail=100              # Last 100 lines
kubectl logs nginx --since=1h              # Last hour
kubectl logs -l app=nginx                  # By label
kubectl logs nginx --all-containers

# Exec into container
kubectl exec nginx -- ls /
kubectl exec -it nginx -- /bin/sh
kubectl exec -it nginx -c container -- bash

# Port forward
kubectl port-forward pod/nginx 8080:80
kubectl port-forward svc/nginx 8080:80

# Copy files
kubectl cp nginx:/etc/nginx/nginx.conf ./nginx.conf
kubectl cp ./file.txt nginx:/tmp/

# Debug
kubectl debug pod/nginx -it --image=busybox
kubectl debug node/worker-1 -it --image=busybox
```

---

## Advanced Queries

### JSONPath

```bash
# Get pod IPs
kubectl get pods -o jsonpath='{.items[*].status.podIP}'

# Get node names
kubectl get nodes -o jsonpath='{.items[*].metadata.name}'

# Get container images
kubectl get pods -o jsonpath='{.items[*].spec.containers[*].image}'

# Custom columns
kubectl get pods -o custom-columns=NAME:.metadata.name,STATUS:.status.phase
```

### Sorting and Filtering

```bash
# Sort by name
kubectl get pods --sort-by=.metadata.name

# Sort by creation time
kubectl get pods --sort-by=.metadata.creationTimestamp

# Field selector
kubectl get pods --field-selector status.phase=Running
kubectl get pods --field-selector spec.nodeName=worker-1
kubectl get events --field-selector type=Warning
```

---

## Node Operations

```bash
# Cordon (mark unschedulable)
kubectl cordon node-1

# Uncordon (mark schedulable)
kubectl uncordon node-1

# Drain (evict pods)
kubectl drain node-1 --ignore-daemonsets --delete-emptydir-data

# Taint
kubectl taint nodes node-1 key=value:NoSchedule
kubectl taint nodes node-1 key=value:NoSchedule-   # Remove
```

---

## RBAC Verification

```bash
# Check permissions
kubectl auth can-i create pods
kubectl auth can-i delete deployments --as jane
kubectl auth can-i get pods --as system:serviceaccount:default:my-sa

# List all permissions
kubectl auth can-i --list
kubectl auth can-i --list --as jane
```

---

## Dry Run and Apply

```bash
# Generate YAML (client-side)
kubectl create deployment nginx --image=nginx --dry-run=client -o yaml > deploy.yaml

# Validate (server-side)
kubectl apply -f deploy.yaml --dry-run=server

# Apply
kubectl apply -f deploy.yaml

# Apply directory
kubectl apply -f ./manifests/

# Apply from URL
kubectl apply -f https://example.com/manifest.yaml

# Apply with Kustomize
kubectl apply -k ./overlays/production/
```

---

## Resource Information

```bash
# API resources
kubectl api-resources

# API versions
kubectl api-versions

# Get resource by name
kubectl get pod nginx -o yaml
kubectl get deploy nginx -o json
```

---

## Quick Tips for Exam

| Task | Command |
|------|---------|
| **Generate YAML** | `--dry-run=client -o yaml` |
| **Quick pod** | `kubectl run nginx --image=nginx` |
| **Quick deployment** | `kubectl create deployment nginx --image=nginx` |
| **Expose** | `kubectl expose deployment nginx --port=80` |
| **Check docs** | `kubectl explain <resource>` |
| **Events** | `kubectl describe <resource>` (bottom) |
| **Logs** | `kubectl logs <pod> --previous` |
| **Debug** | `kubectl exec -it <pod> -- sh` |
| **Save YAML** | `> file.yaml` redirect output |
| **Edit inline** | `kubectl edit <resource>` |
