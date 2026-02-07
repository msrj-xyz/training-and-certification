# Domain 6: Monitoring, Logging, and Runtime Security (20%)

## Task 6.1: Perform Behavioral Analytics of Syscall Process ⭐⭐

### Falco - Runtime Security

```bash
# Install Falco
helm repo add falcosecurity https://falcosecurity.github.io/charts
helm install falco falcosecurity/falco \
  --namespace falco --create-namespace

# Check Falco status
kubectl get pods -n falco

# View Falco logs
kubectl logs -n falco -l app=falco -f
```

### Falco Default Rules

| Rule Type | Example |
|-----------|---------|
| **Shell spawned** | Shell in container |
| **File access** | Sensitive file read |
| **Network** | Unexpected outbound connection |
| **Privilege** | Privilege escalation attempt |
| **Container drift** | New process in container |

### Falco Rule Example

```yaml
- rule: Terminal shell in container
  desc: Detect shell spawned in container
  condition: >
    spawned_process and
    container and
    shell_procs and
    proc.tty != 0
  output: >
    Shell spawned in container 
    (user=%user.name container=%container.name 
    shell=%proc.name parent=%proc.pname)
  priority: WARNING
  tags: [container, shell]
```

### Custom Falco Rule

```yaml
# /etc/falco/rules.d/custom-rules.yaml
- rule: Detect kubectl exec
  desc: Detect execution of kubectl exec
  condition: >
    spawned_process and 
    proc.name in (kubectl) and
    proc.args contains "exec"
  output: >
    kubectl exec detected (user=%user.name command=%proc.cmdline)
  priority: WARNING

- rule: Sensitive file access
  desc: Detect access to sensitive files
  condition: >
    open_read and 
    container and
    fd.name in (/etc/shadow, /etc/passwd, /etc/kubernetes/pki/*)
  output: >
    Sensitive file accessed (file=%fd.name container=%container.name)
  priority: CRITICAL
```

### Falco Priority Levels

| Priority | Severity |
|----------|----------|
| **EMERGENCY** | System unusable |
| **ALERT** | Immediate action needed |
| **CRITICAL** | Critical condition |
| **ERROR** | Error condition |
| **WARNING** | Warning condition |
| **NOTICE** | Normal important event |
| **INFO** | Informational |
| **DEBUG** | Debug information |

---

## Task 6.2: Detect Threats Within Infrastructure ⭐

### Monitor Suspicious Activities

| Activity | Detection |
|----------|-----------|
| **Crypto mining** | High CPU, network to mining pools |
| **Data exfiltration** | Large outbound transfers |
| **Lateral movement** | Unusual internal connections |
| **Privilege escalation** | sudo, setuid usage |
| **Container escape** | Access to host paths |

### Falco Rules for Common Threats

```yaml
# Crypto mining detection
- rule: Detect crypto miners
  desc: Detect crypto mining execution
  condition: >
    spawned_process and container and
    (proc.name in (minerd, cpuminer, xmrig) or
    proc.args contains "stratum+tcp")
  output: Crypto miner detected (container=%container.name proc=%proc.cmdline)
  priority: CRITICAL

# Reverse shell detection
- rule: Detect reverse shell
  desc: Detect reverse shell
  condition: >
    spawned_process and container and
    ((proc.name in (bash, sh) and proc.args contains "-i") or
    fd.type = ipv4 and evt.type in (connect))
  output: Possible reverse shell (container=%container.name)
  priority: CRITICAL
```

---

## Task 6.3: Ensure Immutability of Containers ⭐

### Read-Only Root Filesystem

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: immutable-pod
spec:
  containers:
  - name: app
    image: nginx
    securityContext:
      readOnlyRootFilesystem: true
    volumeMounts:
    - name: tmp
      mountPath: /tmp
    - name: var-run
      mountPath: /var/run
    - name: var-cache-nginx
      mountPath: /var/cache/nginx
  volumes:
  - name: tmp
    emptyDir: {}
  - name: var-run
    emptyDir: {}
  - name: var-cache-nginx
    emptyDir: {}
```

### Immutability Best Practices

| Practice | Implementation |
|----------|----------------|
| **Read-only rootfs** | readOnlyRootFilesystem: true |
| **No privilege escalation** | allowPrivilegeEscalation: false |
| **Drop capabilities** | capabilities.drop: ALL |
| **No writable volumes** | Use emptyDir for temp only |
| **Immutable image tags** | Use SHA256 digests |

### Use Image Digest

```yaml
spec:
  containers:
  - name: app
    image: nginx@sha256:abc123...  # Immutable reference
```

### Detect Container Drift

```yaml
# Falco rule for detecting new executables
- rule: Container drift detected
  desc: New executable run in container that wasn't in image
  condition: >
    spawned_process and
    container and
    not proc.name in (container.image.known_processes)
  output: Container drift (container=%container.name proc=%proc.name)
  priority: WARNING
```

---

## Task 6.4: Use Audit Logs to Monitor Access ⭐⭐

### Enable Kubernetes Audit Logging

```yaml
# Add to kube-apiserver.yaml
spec:
  containers:
  - command:
    - kube-apiserver
    - --audit-log-path=/var/log/kubernetes/audit.log
    - --audit-log-maxage=30
    - --audit-log-maxbackup=10
    - --audit-log-maxsize=100
    - --audit-policy-file=/etc/kubernetes/audit/policy.yaml
```

### Audit Policy

```yaml
# /etc/kubernetes/audit/policy.yaml
apiVersion: audit.k8s.io/v1
kind: Policy
rules:
  # Log all requests at Metadata level
  - level: Metadata
    resources:
    - group: ""
      resources: ["secrets", "configmaps"]

  # Log pod execution at RequestResponse level
  - level: RequestResponse
    resources:
    - group: ""
      resources: ["pods/exec", "pods/attach"]

  # Log authentication failures
  - level: Request
    users: ["system:anonymous"]
    verbs: ["*"]

  # Don't log read-only endpoints
  - level: None
    nonResourceURLs:
    - /healthz*
    - /version
    - /readyz

  # Default: log at Metadata level
  - level: Metadata
```

### Audit Levels

| Level | What's Logged |
|-------|---------------|
| **None** | Don't log |
| **Metadata** | Request metadata only |
| **Request** | Metadata + request body |
| **RequestResponse** | Metadata + request + response |

### Audit Log Analysis

```bash
# View audit logs
cat /var/log/kubernetes/audit.log | jq .

# Find failed authentication
cat /var/log/kubernetes/audit.log | jq 'select(.responseStatus.code >= 400)'

# Find secrets access
cat /var/log/kubernetes/audit.log | jq 'select(.objectRef.resource == "secrets")'

# Find pod exec
cat /var/log/kubernetes/audit.log | jq 'select(.objectRef.subresource == "exec")'
```

### Common Audit Queries

```bash
# Who accessed secrets?
jq 'select(.objectRef.resource=="secrets") | {user: .user.username, secret: .objectRef.name}' audit.log

# Failed API calls
jq 'select(.responseStatus.code >= 400) | {user: .user.username, verb: .verb, resource: .objectRef.resource, code: .responseStatus.code}' audit.log

# Pod deletions
jq 'select(.verb=="delete" and .objectRef.resource=="pods") | {user: .user.username, pod: .objectRef.name, ns: .objectRef.namespace}' audit.log
```

---

## Task 6.5: Investigate Security Events ⭐

### Investigation Workflow

```
1. Alert Triggered (Falco/Audit)
    ↓
2. Identify Affected Resources
    ↓
3. Collect Evidence
    ↓
4. Analyze Behavior
    ↓
5. Contain Threat
    ↓
6. Remediate
```

### Evidence Collection

```bash
# Get pod details
kubectl get pod <pod> -o yaml > pod-evidence.yaml

# Get logs
kubectl logs <pod> > pod-logs.txt
kubectl logs <pod> --previous > pod-logs-previous.txt

# Check events
kubectl get events --sort-by='.lastTimestamp' > events.txt

# Describe pod
kubectl describe pod <pod> > pod-describe.txt
```

### Container Forensics

```bash
# Exec into container (if still running)
kubectl exec -it <pod> -- /bin/sh

# Check running processes
ps aux

# Check network connections
netstat -tulpn
ss -tulpn

# Check mounted filesystems
mount

# Check environment variables
env
```

### Incident Response Actions

| Action | Command |
|--------|---------|
| **Isolate pod** | Delete pod or scale to 0 |
| **Network isolation** | Apply deny NetworkPolicy |
| **Preserve evidence** | Copy logs, describe resources |
| **Rotate credentials** | Delete and recreate secrets |

---

## Task 6.6: Configure Logging ⭐

### Container Logging

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: logging-pod
spec:
  containers:
  - name: app
    image: nginx
    # Logs go to stdout/stderr
  - name: sidecar-logger
    image: busybox
    args: [/bin/sh, -c, 'tail -f /var/log/app/*.log']
    volumeMounts:
    - name: logs
      mountPath: /var/log/app
  volumes:
  - name: logs
    emptyDir: {}
```

### Centralized Logging Stack

| Component | Purpose |
|-----------|---------|
| **Fluentd/Fluent Bit** | Log collection |
| **Elasticsearch** | Log storage |
| **Kibana** | Log visualization |

---

## Exam Tips for Domain 6

1. **Falco** - Primary runtime security tool
2. **Falco rules** - Condition, output, priority
3. **Audit logs** - Enable with policy file
4. **Audit levels** - None, Metadata, Request, RequestResponse
5. **readOnlyRootFilesystem** - Container immutability
6. **Image digests** - sha256 for immutable references
7. **jq queries** - Parse audit logs
8. **Evidence collection** - Logs, describe, events
9. **Container drift** - Detect new processes
10. **Investigation** - Identify, collect, analyze, contain
