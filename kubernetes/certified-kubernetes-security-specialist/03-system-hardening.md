# Domain 3: System Hardening (10%)

## Task 3.1: Minimize Host OS Footprint ⭐

### Host Security Best Practices

| Practice | Implementation |
|----------|----------------|
| **Minimal OS** | Use minimal base images |
| **Remove packages** | Uninstall unnecessary tools |
| **Disable services** | Stop unused systemd services |
| **Read-only filesystems** | Mount as read-only where possible |
| **Regular updates** | Keep OS patched |

### Reduce Attack Surface

```bash
# List installed packages
dpkg -l  # Debian/Ubuntu
rpm -qa  # RHEL/CentOS

# Remove unnecessary packages
apt-get remove --purge <package>

# Disable unnecessary services
systemctl disable <service>
systemctl stop <service>

# List running services
systemctl list-units --type=service --state=running
```

### Secure SSH Configuration

```bash
# /etc/ssh/sshd_config
PermitRootLogin no
PasswordAuthentication no
PubkeyAuthentication yes
PermitEmptyPasswords no
X11Forwarding no
AllowUsers admin
MaxAuthTries 3
```

---

## Task 3.2: Limit Node Access ⭐

### IAM for Node Access

| Cloud | Service |
|-------|---------|
| **AWS** | IAM roles for EC2 |
| **GCP** | Service Accounts |
| **Azure** | Managed Identity |

### Principle of Least Privilege

```bash
# Restrict kubectl access
chmod 600 ~/.kube/config

# Use RBAC for node access
kubectl create clusterrole node-reader \
  --verb=get,list,watch \
  --resource=nodes
```

### Network Segmentation

| Network | Purpose |
|---------|---------|
| **Control Plane** | API server, etcd, scheduler |
| **Worker Nodes** | Application workloads |
| **Pod Network** | Container communication |
| **Service Network** | ClusterIP services |

---

## Task 3.3: Use Kernel Hardening Tools ⭐⭐

### AppArmor

```bash
# Check AppArmor status
aa-status
apparmor_status

# List loaded profiles
aa-status --profiles

# Load profile
apparmor_parser -r /etc/apparmor.d/<profile>

# Set profile to enforce mode
aa-enforce /etc/apparmor.d/<profile>
```

### AppArmor Profile Example

```
#include <tunables/global>

profile k8s-apparmor-deny-write flags=(attach_disconnected) {
  #include <abstractions/base>

  file,
  
  # Deny all file writes
  deny /** w,
  
  # Allow specific paths
  /var/run/secrets/kubernetes.io/** r,
  /etc/passwd r,
  /etc/group r,
}
```

### Apply AppArmor to Pod

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: apparmor-pod
  annotations:
    container.apparmor.security.beta.kubernetes.io/nginx: localhost/k8s-apparmor-deny-write
spec:
  containers:
  - name: nginx
    image: nginx
```

---

### seccomp (Secure Computing Mode)

```bash
# Check seccomp support
grep SECCOMP /boot/config-$(uname -r)

# Default seccomp profile location
/var/lib/kubelet/seccomp/
```

### seccomp Profile Example

```json
{
  "defaultAction": "SCMP_ACT_ERRNO",
  "architectures": [
    "SCMP_ARCH_X86_64",
    "SCMP_ARCH_X86",
    "SCMP_ARCH_X32"
  ],
  "syscalls": [
    {
      "names": [
        "accept4",
        "epoll_wait",
        "pselect6",
        "futex",
        "madvise",
        "read",
        "write",
        "close",
        "arch_prctl",
        "sched_yield",
        "nanosleep"
      ],
      "action": "SCMP_ACT_ALLOW"
    }
  ]
}
```

### Apply seccomp to Pod

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: seccomp-pod
spec:
  securityContext:
    seccompProfile:
      type: Localhost
      localhostProfile: profiles/audit.json
  containers:
  - name: nginx
    image: nginx
```

### seccomp Profile Types

| Type | Description |
|------|-------------|
| **RuntimeDefault** | Container runtime default profile |
| **Unconfined** | No seccomp (not recommended) |
| **Localhost** | Custom profile on node |

```yaml
# Use RuntimeDefault (recommended)
spec:
  securityContext:
    seccompProfile:
      type: RuntimeDefault
```

---

## Task 3.4: Minimize External Access to Network ⭐

### Firewall Rules (iptables)

```bash
# Block outbound to metadata service
iptables -A OUTPUT -d 169.254.169.254 -j DROP

# Allow only specific ports
iptables -A INPUT -p tcp --dport 22 -j ACCEPT
iptables -A INPUT -p tcp --dport 6443 -j ACCEPT
iptables -A INPUT -j DROP
```

### Network Policy for External Access

```yaml
apiVersion: networking.k8s.io/v1
kind: NetworkPolicy
metadata:
  name: deny-external-egress
spec:
  podSelector: {}
  policyTypes:
  - Egress
  egress:
  - to:
    - ipBlock:
        cidr: 10.0.0.0/8  # Internal only
    - ipBlock:
        cidr: 172.16.0.0/12
    - ipBlock:
        cidr: 192.168.0.0/16
```

---

## Exam Tips for Domain 3

1. **Minimal OS** - Remove unnecessary packages/services
2. **SSH hardening** - Disable root login, use keys
3. **AppArmor** - Kernel security module for access control
4. **seccomp** - Syscall filtering
5. **RuntimeDefault** - Use container runtime's seccomp profile
6. **Profile location** - /var/lib/kubelet/seccomp/
7. **aa-status** - Check AppArmor status
8. **Annotations** - AppArmor uses annotations
9. **securityContext** - seccomp uses securityContext
10. **Least privilege** - Minimal permissions everywhere
