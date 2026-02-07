# GitLab Certified Security Associate - Quick Reference Card

## Exam Format
| Aspect | Details |
|--------|---------|
| **Part 1** | 14 multiple-choice (60 min, 80% to pass) |
| **Part 2** | 14 hands-on tasks (120 min) |
| **Retakes** | Part 1: Unlimited, Part 2: Limited |

---

## Enable All Security Scanners

```yaml
include:
  - template: Security/SAST.gitlab-ci.yml
  - template: Security/Dependency-Scanning.gitlab-ci.yml
  - template: Security/Container-Scanning.gitlab-ci.yml
  - template: Security/Secret-Detection.gitlab-ci.yml
  - template: Security/License-Scanning.gitlab-ci.yml
  - template: DAST.gitlab-ci.yml
```

---

## Security Scanner Quick Reference

| Scanner | Purpose | Stage | Requirements |
|---------|---------|-------|--------------|
| **SAST** | Source code analysis | test | Source code |
| **DAST** | Running app testing | dast | Deployed URL |
| **Dependency** | Package vulnerabilities | test | Dependencies |
| **Container** | Image vulnerabilities | test | Docker image |
| **Secret** | Exposed secrets | .pre | Git repository |
| **License** | License compliance | test | Dependencies |

---

## OWASP Top 10 (2021)

1. **A01** - Broken Access Control
2. **A02** - Cryptographic Failures
3. **A03** - Injection (SQL, Command, XSS)
4. **A04** - Insecure Design
5. **A05** - Security Misconfiguration
6. **A06** - Vulnerable Components
7. **A07** - Authentication Failures
8. **A08** - Software/Data Integrity
9. **A09** - Logging Failures
10. **A10** - SSRF

---

## Severity Levels

| Severity | CVSS Score | Action | SLA |
|----------|------------|--------|-----|
| **Critical** | 9.0-10.0 | Fix immediately | 24h |
| **High** | 7.0-8.9 | Fix within days | 3d |
| **Medium** | 4.0-6.9 | Fix within weeks | 2w |
| **Low** | 0.1-3.9 | Fix when possible | 1m |

---

## SAST Configuration

```yaml
include:
  - template: Security/SAST.gitlab-ci.yml

variables:
  SAST_EXCLUDED_PATHS: "spec, test, tests, tmp"
  SAST_CONFIDENCE_LEVEL: 2  # 1=Low, 2=Medium, 3=High
```

### Common SAST Vulnerabilities

- **SQL Injection (CWE-89)** - Unsanitized SQL queries
- **XSS (CWE-79)** - Unescaped user input
- **Command Injection (CWE-78)** - Unsanitized shell commands
- **Path Traversal (CWE-22)** - Unvalidated file paths
- **Hardcoded Secrets (CWE-798)** - Credentials in code
- **Weak Crypto (CWE-327)** - MD5, SHA1, DES

---

## DAST Configuration

```yaml
include:
  - template: DAST.gitlab-ci.yml

variables:
  DAST_WEBSITE: "https://staging.example.com"
  DAST_FULL_SCAN_ENABLED: "true"
  
  # Authentication
  DAST_AUTH_URL: "https://staging.example.com/login"
  DAST_USERNAME: "testuser"
  DAST_PASSWORD: "$DAST_PASSWORD"
  DAST_USERNAME_FIELD: "username"
  DAST_PASSWORD_FIELD: "password"
```

---

## Dependency Scanning

```yaml
include:
  - template: Security/Dependency-Scanning.gitlab-ci.yml

variables:
  DS_EXCLUDED_PATHS: "spec, test, tests, tmp"
  DS_REMEDIATE: "true"  # Auto-create MR for fixes
```

### Check Dependencies

```bash
# npm
npm audit
npm audit fix

# pip
pip-audit

# bundler
bundle audit

# maven
mvn dependency-check:check
```

---

## Container Scanning

```yaml
include:
  - template: Security/Container-Scanning.gitlab-ci.yml

variables:
  CS_IMAGE: $CI_REGISTRY_IMAGE:$CI_COMMIT_SHA
  CS_SEVERITY_THRESHOLD: "HIGH"
```

### Secure Dockerfile

```dockerfile
FROM node:18-alpine
WORKDIR /app
RUN apk update && apk upgrade
COPY package*.json ./
RUN npm ci --only=production
COPY . .
RUN adduser -D appuser
USER appuser
EXPOSE 3000
CMD ["node", "server.js"]
```

---

## Secret Detection

```yaml
include:
  - template: Security/Secret-Detection.gitlab-ci.yml

variables:
  SECRET_DETECTION_HISTORIC_SCAN: "true"
  SECRET_DETECTION_EXCLUDED_PATHS: "tests/, docs/"
```

### Common Secrets

- AWS Access Keys: `AKIA[0-9A-Z]{16}`
- GitHub Tokens: `ghp_[a-zA-Z0-9]{36}`
- GitLab Tokens: `glpat-[a-zA-Z0-9_-]{20}`
- Private SSH Keys: `-----BEGIN RSA PRIVATE KEY-----`
- API Keys: `api_key=...`
- Database URLs: `postgres://user:pass@host`

---

## License Compliance

```yaml
include:
  - template: Security/License-Scanning.gitlab-ci.yml
```

### License Categories

```yaml
# ✅ Permissive (Business-friendly)
- MIT
- Apache-2.0
- BSD-2-Clause, BSD-3-Clause
- ISC

# ⚠️ Copyleft (Restrictive)
- GPL-3.0
- AGPL-3.0
- LGPL-3.0

# ✅ Weak Copyleft
- MPL-2.0
- EPL-2.0
```

---

## Security Policies

### Scan Execution Policy

```yaml
scan_execution_policy:
- name: Enforce Security Scans
  enabled: true
  rules:
  - type: pipeline
    branches: [main]
  actions:
  - scan: sast
  - scan: dependency_scanning
  - scan: container_scanning
```

### Scan Result Policy

```yaml
scan_result_policy:
- name: Block Critical Vulnerabilities
  enabled: true
  rules:
  - type: scan_finding
    severity_levels: [critical]
    vulnerabilities_allowed: 0
  actions:
  - type: require_approval
    approvals_required: 1
```

---

## Vulnerability Management

### Vulnerability States

```yaml
Detected → Confirmed → Resolved
         ↓
      Dismissed (false positive)
```

### Dismiss Reasons

- False positive
- Used in tests only
- Protected by other controls
- Risk accepted
- Not applicable

---

## Security Dashboard

### Access Dashboards

```yaml
# Project Dashboard
Project > Security & Compliance > Vulnerability Report

# Group Dashboard
Group > Security & Compliance > Vulnerability Report

# MR Security Widget
Merge Request > Security tab

# Pipeline Security
Pipeline > Security tab
```

### Key Metrics

- **MTTD**: Mean Time to Detect
- **MTTR**: Mean Time to Remediate
- **Backlog**: Unresolved vulnerabilities
- **Coverage**: % of projects with scanners
- **Remediation Rate**: % of vulnerabilities fixed

---

## Complete Security Pipeline

```yaml
stages:
  - .pre
  - build
  - test
  - security
  - deploy

# Secret detection (pre-commit)
include:
  - template: Security/Secret-Detection.gitlab-ci.yml

# Build image
build:
  stage: build
  image: docker:latest
  services:
    - docker:dind
  script:
    - docker build -t $CI_REGISTRY_IMAGE:$CI_COMMIT_SHA .
    - docker push $CI_REGISTRY_IMAGE:$CI_COMMIT_SHA

# SAST
sast:
  stage: test
  include:
    - template: Security/SAST.gitlab-ci.yml

# Dependency scanning
dependency_scanning:
  stage: test
  include:
    - template: Security/Dependency-Scanning.gitlab-ci.yml

# Container scanning
container_scanning:
  stage: security
  include:
    - template: Security/Container-Scanning.gitlab-ci.yml
  variables:
    CS_IMAGE: $CI_REGISTRY_IMAGE:$CI_COMMIT_SHA

# License compliance
license_scanning:
  stage: test
  include:
    - template: Security/License-Scanning.gitlab-ci.yml

# DAST (after deploy)
dast:
  stage: security
  include:
    - template: DAST.gitlab-ci.yml
  variables:
    DAST_WEBSITE: "https://staging.example.com"
```

---

## Exam Day Checklist

### Part 1 (Written)
- ✅ Know all scanner types and purposes
- ✅ Understand OWASP Top 10
- ✅ Know severity levels and SLAs
- ✅ Understand vulnerability lifecycle
- ✅ Know security policy types
- ✅ Understand license categories
- ✅ Know common CWE IDs
- ✅ Understand scanner configuration

### Part 2 (Hands-on)
- ✅ Enable all security scanners
- ✅ Configure DAST with authentication
- ✅ Create security policies
- ✅ Manage vulnerabilities (dismiss, resolve)
- ✅ Configure license policies
- ✅ Use security dashboard
- ✅ Create issues from vulnerabilities
- ✅ Export security reports

---

## Critical Reminders

| Topic | Remember |
|-------|----------|
| **SAST** | Include Security/SAST.gitlab-ci.yml |
| **DAST** | Requires deployed application URL |
| **Dependency** | Auto-detects package managers |
| **Container** | Requires built Docker image |
| **Secret** | Scans git history if enabled |
| **License** | MIT/Apache good, GPL restrictive |
| **Policies** | Scan execution vs scan result |
| **Severity** | Critical=24h, High=3d, Medium=2w, Low=1m |
| **States** | Detected, Confirmed, Dismissed, Resolved |
| **Dashboard** | Project, Group, MR, Pipeline views |

---

## Common Commands

```bash
# Check vulnerabilities
npm audit
pip-audit
bundle audit
trivy image myapp:latest

# Update dependencies
npm update
pip install --upgrade package
bundle update

# Scan with Trivy
trivy image --severity HIGH,CRITICAL myapp:latest

# Remove secrets from history
git filter-repo --path path/to/secret --invert-paths
```

---

## Resources During Exam

- ✅ https://docs.gitlab.com/ee/user/application_security/
- ✅ https://docs.gitlab.com/ee/user/application_security/sast/
- ✅ https://docs.gitlab.com/ee/user/application_security/dast/
- ✅ https://owasp.org/www-project-top-ten/

---

## Final Tips

1. **Enable scanners early** - Include templates
2. **Understand scanner requirements** - DAST needs URL, Container needs image
3. **Know vulnerability workflow** - Detect → Triage → Fix → Verify
4. **Practice hands-on** - Enable scanners, configure policies
5. **Use documentation** - Allowed during exam
6. **Manage time** - Don't spend too long on one task
7. **Verify results** - Check security dashboard
8. **Know severity SLAs** - Critical 24h, High 3d
9. **Understand policies** - Scan execution vs scan result
10. **Stay calm** - You've got this! 🔒

---

**Good luck with your certification! 🎉**
