# Certified Kubernetes Security Specialist (CKS)

## Exam Information

| Aspect | Details |
|--------|---------|
| **Exam Code** | CKS |
| **Level** | Specialist |
| **Duration** | 2 hours |
| **Questions** | 15-20 performance-based tasks |
| **Passing Score** | 67% |
| **Format** | Command-line, hands-on |
| **Cost** | $445 USD |
| **Kubernetes Version** | v1.34 |
| **Validity** | 2 years |
| **Prerequisite** | CKA certification (must be active) |
| **Retakes** | 1 free retake included |

---

## Domain Weightings

| Domain | Weight | Focus Areas |
|--------|--------|-------------|
| **Domain 1** | 15% | Cluster Setup |
| **Domain 2** | 15% | Cluster Hardening |
| **Domain 3** | 10% | System Hardening |
| **Domain 4** | 20% | Minimize Microservice Vulnerabilities |
| **Domain 5** | 20% | Supply Chain Security |
| **Domain 6** | 20% | Monitoring, Logging, and Runtime Security |

---

## Target Candidate Profile

- Passed CKA certification (prerequisite)
- Experience with Kubernetes security
- Understanding of container security concepts
- Knowledge of Linux security tools
- Familiarity with security scanning tools

---

## Exam Environment

### Technical Details
| Aspect | Details |
|--------|---------|
| **Base System** | Linux (hostname: base) |
| **Access Method** | SSH to designated hosts |
| **Elevated Privileges** | `sudo -i` available |
| **Pre-installed Tools** | kubectl (k alias), yq, curl, wget |
| **Browser** | PSI Secure Browser |

### Security Tools Available
- Trivy (image scanning)
- Falco (runtime security)
- AppArmor profiles
- seccomp profiles
- OPA/Gatekeeper

---

## Resources Allowed During Exam

| Resource | URL |
|----------|-----|
| **Kubernetes Docs** | https://kubernetes.io/docs |
| **Kubernetes Blog** | https://kubernetes.io/blog |
| **Trivy Docs** | https://aquasecurity.github.io/trivy |
| **Falco Docs** | https://falco.org/docs |
| **AppArmor Docs** | https://gitlab.com/apparmor |
| **etcd Docs** | https://etcd.io/docs |

> **⚠️ Warning:** Only ONE browser tab allowed for documentation!

---

## Key Security Concepts

### Defense in Depth Layers

| Layer | Tools/Controls |
|-------|----------------|
| **Cluster** | RBAC, Network Policies, Admission Controllers |
| **Node** | AppArmor, seccomp, syscall filtering |
| **Container** | Security Context, Read-only rootfs |
| **Image** | Image scanning, Base image hardening |
| **Network** | Network Policies, mTLS, Ingress TLS |
| **Runtime** | Falco, Audit logs, Immutable containers |

### Security Principles

| Principle | Application |
|-----------|-------------|
| **Least Privilege** | Minimal RBAC, drop capabilities |
| **Defense in Depth** | Multiple security layers |
| **Zero Trust** | Verify everything, trust nothing |
| **Immutability** | Read-only containers |
| **Separation** | Network segmentation |

---

## Keyboard Shortcuts (Exam)

| Action | Shortcut |
|--------|----------|
| **Copy (Terminal)** | Ctrl+Shift+C |
| **Paste (Terminal)** | Ctrl+Shift+V |
| **Copy (Desktop Apps)** | Ctrl+C |
| **Paste (Desktop Apps)** | Ctrl+V |
| **Close Tab Alternative** | Ctrl+Alt+W |
| **Locate Cursor** | Ctrl+Alt+K |

---

## Exam Tips Summary

1. **CIS Benchmarks** - Know kube-bench for compliance checks
2. **RBAC** - Minimize permissions, audit access
3. **Network Policies** - Default deny, explicit allow
4. **Pod Security Standards** - Restricted, Baseline, Privileged
5. **Image Scanning** - Trivy for CVE detection
6. **Runtime Security** - Falco for syscall monitoring
7. **AppArmor/seccomp** - Kernel-level hardening
8. **Secrets Management** - Encrypt at rest, RBAC
9. **Audit Logging** - Enable and configure
10. **Supply Chain** - Image signing, allowed registries
