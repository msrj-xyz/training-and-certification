# Certified Kubernetes Administrator (CKA)

## Exam Information

| Aspect | Details |
|--------|---------|
| **Exam Code** | CKA |
| **Level** | Professional |
| **Duration** | 2 hours |
| **Questions** | 15-20 performance-based tasks |
| **Passing Score** | 66% |
| **Format** | Command-line, hands-on |
| **Cost** | $445 USD |
| **Kubernetes Version** | v1.34 |
| **Validity** | 2 years |
| **Retakes** | 1 free retake included |

---

## Domain Weightings

| Domain | Weight | Focus Areas |
|--------|--------|-------------|
| **Domain 1** | 25% | Cluster Architecture, Installation & Configuration |
| **Domain 2** | 15% | Workloads & Scheduling |
| **Domain 3** | 20% | Services & Networking |
| **Domain 4** | 10% | Storage |
| **Domain 5** | 30% | Troubleshooting |

---

## Target Candidate Profile

- Kubernetes administrators with hands-on experience
- Understanding of Linux command line
- Experience managing containerized applications
- Knowledge of cluster lifecycle management
- Familiarity with monitoring and logging

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

### Pre-configured Features
- kubectl with `k` alias and Bash autocompletion
- yq for YAML processing
- curl and wget for testing web services
- man pages for documentation

---

## Keyboard Shortcuts

| Action | Shortcut |
|--------|----------|
| **Copy (Terminal)** | Ctrl+Shift+C |
| **Paste (Terminal)** | Ctrl+Shift+V |
| **Copy (Desktop Apps)** | Ctrl+C |
| **Paste (Desktop Apps)** | Ctrl+V |
| **Close Tab Alternative** | Ctrl+Alt+W (instead of Ctrl+W) |
| **Locate Cursor** | Ctrl+Alt+K |

> **⚠️ Warning:** Do NOT reboot the base node! INSERT key is disabled.

---

## Resources Allowed During Exam

| Resource | URL |
|----------|-----|
| **Kubernetes Docs** | https://kubernetes.io/docs |
| **Kubernetes Blog** | https://kubernetes.io/blog |
| **Helm Docs** | https://helm.sh/docs |
| **Trivy Docs** | https://aquasecurity.github.io/trivy |
| **Falco Docs** | https://falco.org/docs |
| **etcd Docs** | https://etcd.io/docs |

> **Exam Tip:** Only ONE browser tab allowed for documentation!

---

## Exam Day Preparation

### Before the Exam
1. Test PSI Secure Browser installation
2. Prepare government-issued ID
3. Clear desk and workspace
4. Stable internet connection (minimum 1 Mbps)
5. Webcam and microphone working

### During the Exam
1. Read questions carefully
2. Use `k` alias for kubectl
3. Switch contexts when required
4. Check namespace in each question
5. Use `--dry-run=client -o yaml` for manifests

---

## Killer.sh Simulator

| Feature | Details |
|---------|---------|
| **Access** | 2 simulation attempts included |
| **Duration** | 36 hours per attempt |
| **Questions** | 17 questions per session |
| **Scoring** | Graded results provided |

---

## Exam Tips Summary

1. **Master imperative commands** - faster than writing YAML
2. **Use kubectl explain** - quick reference for fields
3. **Set namespace context** - avoid -n flag mistakes
4. **Practice with time limits** - 2 hours goes fast
5. **Know vim basics** - essential for editing
6. **Use kubectl aliases** - k, kgp, kgs, etc.
7. **Check context/namespace** - every question
8. **Skip and return** - don't get stuck
9. **Read carefully** - specific requirements matter
10. **Practice etcd backup/restore** - common topic
