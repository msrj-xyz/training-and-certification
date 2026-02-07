# Domain 1: Security Fundamentals & DevSecOps

## Task 1.1: DevSecOps Principles ⭐⭐

### Shift-Left Security

| Traditional Security | DevSecOps (Shift-Left) |
|---------------------|------------------------|
| Security at the end | Security from the start |
| Manual security reviews | Automated security scanning |
| Separate security team | Everyone owns security |
| Slow feedback | Immediate feedback |
| Production vulnerabilities | Early vulnerability detection |

### DevSecOps Benefits

```yaml
# Security integrated into CI/CD
stages:
  - build
  - test
  - security  # Security stage
  - deploy

# Security runs automatically
include:
  - template: Security/SAST.gitlab-ci.yml
  - template: Security/Dependency-Scanning.gitlab-ci.yml
  - template: Security/Container-Scanning.gitlab-ci.yml
  - template: Security/Secret-Detection.gitlab-ci.yml
```

---

## Task 1.2: OWASP Top 10 ⭐⭐

### OWASP Top 10 (2021)

| Rank | Vulnerability | Description | GitLab Scanner |
|------|--------------|-------------|----------------|
| **A01** | Broken Access Control | Unauthorized access | SAST, DAST |
| **A02** | Cryptographic Failures | Weak encryption | SAST |
| **A03** | Injection | SQL, command injection | SAST, DAST |
| **A04** | Insecure Design | Flawed architecture | SAST |
| **A05** | Security Misconfiguration | Improper config | SAST, Container |
| **A06** | Vulnerable Components | Outdated dependencies | Dependency Scanning |
| **A07** | Authentication Failures | Weak authentication | SAST, DAST |
| **A08** | Software/Data Integrity | Supply chain attacks | Dependency, Container |
| **A09** | Logging Failures | Insufficient logging | SAST |
| **A10** | SSRF | Server-side request forgery | SAST, DAST |

### Example Vulnerabilities

```python
# A03: SQL Injection (Vulnerable)
def get_user(username):
    query = f"SELECT * FROM users WHERE username = '{username}'"
    return db.execute(query)

# Fixed with parameterized query
def get_user(username):
    query = "SELECT * FROM users WHERE username = ?"
    return db.execute(query, (username,))

# A03: Command Injection (Vulnerable)
import os
def ping_host(host):
    os.system(f"ping -c 1 {host}")

# Fixed with input validation
import subprocess
def ping_host(host):
    if not re.match(r'^[\w\.-]+$', host):
        raise ValueError("Invalid host")
    subprocess.run(["ping", "-c", "1", host])
```

---

## Task 1.3: Security Vulnerability Severities ⭐⭐

### Severity Levels

| Severity | CVSS Score | Description | Action Required |
|----------|------------|-------------|-----------------|
| **Critical** | 9.0-10.0 | Immediate exploitation risk | Fix immediately |
| **High** | 7.0-8.9 | Significant risk | Fix within days |
| **Medium** | 4.0-6.9 | Moderate risk | Fix within weeks |
| **Low** | 0.1-3.9 | Minor risk | Fix when possible |
| **Unknown** | N/A | Severity not determined | Investigate |

### CVSS (Common Vulnerability Scoring System)

```yaml
# CVSS v3.1 Metrics
Base Score:
  - Attack Vector (AV): Network, Adjacent, Local, Physical
  - Attack Complexity (AC): Low, High
  - Privileges Required (PR): None, Low, High
  - User Interaction (UI): None, Required
  - Scope (S): Unchanged, Changed
  - Impact: Confidentiality, Integrity, Availability

# Example: Critical SQL Injection
CVSS:3.1/AV:N/AC:L/PR:N/UI:N/S:C/C:H/I:H/A:H
Score: 10.0 (Critical)
```

---

## Task 1.4: Common Weakness Enumeration (CWE) ⭐

### Top CWEs

| CWE ID | Name | Description |
|--------|------|-------------|
| **CWE-79** | Cross-Site Scripting (XSS) | Improper input neutralization |
| **CWE-89** | SQL Injection | Improper SQL query construction |
| **CWE-20** | Improper Input Validation | Insufficient input validation |
| **CWE-78** | OS Command Injection | Improper command neutralization |
| **CWE-22** | Path Traversal | Improper path limitation |
| **CWE-352** | CSRF | Cross-site request forgery |
| **CWE-434** | Unrestricted File Upload | Dangerous file upload |
| **CWE-94** | Code Injection | Improper code neutralization |
| **CWE-798** | Hard-coded Credentials | Embedded credentials |
| **CWE-327** | Weak Cryptography | Broken crypto algorithm |

---

## Task 1.5: Security in SDLC ⭐⭐

### Secure SDLC Phases

```yaml
# 1. Planning & Requirements
- Threat modeling
- Security requirements
- Risk assessment

# 2. Design
- Security architecture review
- Secure design patterns
- Data flow diagrams

# 3. Development
- Secure coding practices
- Code reviews
- SAST scanning

# 4. Testing
- Security testing
- Penetration testing
- DAST scanning

# 5. Deployment
- Container scanning
- Configuration review
- Deployment validation

# 6. Maintenance
- Vulnerability monitoring
- Patch management
- Incident response
```

### GitLab Security in SDLC

```yaml
# Integrated security pipeline
stages:
  - .pre
  - build
  - test
  - security
  - deploy
  - .post

# Pre-commit: Secret detection
secret_detection:
  stage: .pre
  include:
    - template: Security/Secret-Detection.gitlab-ci.yml

# Build: SAST
sast:
  stage: test
  include:
    - template: Security/SAST.gitlab-ci.yml

# Build: Dependency scanning
dependency_scanning:
  stage: test
  include:
    - template: Security/Dependency-Scanning.gitlab-ci.yml

# Post-build: Container scanning
container_scanning:
  stage: security
  include:
    - template: Security/Container-Scanning.gitlab-ci.yml

# Post-deploy: DAST
dast:
  stage: security
  include:
    - template: DAST.gitlab-ci.yml
  variables:
    DAST_WEBSITE: "https://staging.example.com"
```

---

## Task 1.6: Security Best Practices ⭐⭐

### Secure Coding Principles

```python
# 1. Input Validation
def process_user_input(user_input):
    # Validate input
    if not isinstance(user_input, str):
        raise ValueError("Invalid input type")
    if len(user_input) > 100:
        raise ValueError("Input too long")
    if not re.match(r'^[a-zA-Z0-9_]+$', user_input):
        raise ValueError("Invalid characters")
    return user_input

# 2. Output Encoding
from html import escape
def display_user_content(content):
    return escape(content)  # Prevent XSS

# 3. Parameterized Queries
def get_user_by_email(email):
    query = "SELECT * FROM users WHERE email = ?"
    return db.execute(query, (email,))

# 4. Least Privilege
def read_file(filename):
    # Check permissions first
    if not has_permission(current_user, filename):
        raise PermissionError("Access denied")
    with open(filename, 'r') as f:
        return f.read()

# 5. Secure Random
import secrets
def generate_token():
    return secrets.token_urlsafe(32)  # Not random.random()

# 6. Error Handling
def process_payment(amount):
    try:
        payment_gateway.charge(amount)
    except PaymentError as e:
        # Log error securely
        logger.error(f"Payment failed: {e.code}")
        # Don't expose details to user
        raise UserError("Payment processing failed")
```

---

## Task 1.7: Security Compliance Standards ⭐

### Common Compliance Frameworks

| Framework | Focus | Requirements |
|-----------|-------|--------------|
| **PCI DSS** | Payment card data | Encryption, access control, monitoring |
| **HIPAA** | Healthcare data | Privacy, security, breach notification |
| **SOC 2** | Service organizations | Security, availability, confidentiality |
| **ISO 27001** | Information security | ISMS, risk management |
| **GDPR** | Data privacy | Consent, data protection, breach notification |
| **NIST** | Cybersecurity | Framework, controls, risk management |

### GitLab Compliance Features

```yaml
# Compliance framework
include:
  - project: 'compliance/pipeline-templates'
    ref: main
    file: '/compliance-template.yml'

# Audit events
# Settings > Security & Compliance > Audit Events

# Compliance dashboard
# Group > Security & Compliance > Compliance

# License compliance
include:
  - template: Security/License-Scanning.gitlab-ci.yml
```

---

## Task 1.8: Threat Modeling ⭐

### STRIDE Threat Model

| Threat | Description | Mitigation |
|--------|-------------|------------|
| **Spoofing** | Impersonation | Authentication, MFA |
| **Tampering** | Data modification | Integrity checks, signing |
| **Repudiation** | Deny actions | Logging, audit trails |
| **Information Disclosure** | Data exposure | Encryption, access control |
| **Denial of Service** | Service disruption | Rate limiting, redundancy |
| **Elevation of Privilege** | Unauthorized access | Least privilege, RBAC |

### Threat Modeling Process

```yaml
# 1. Identify Assets
- User data
- API keys
- Database
- Application code

# 2. Identify Threats
- SQL injection
- XSS attacks
- Data breaches
- DDoS attacks

# 3. Assess Risk
- Likelihood: High/Medium/Low
- Impact: High/Medium/Low
- Risk = Likelihood × Impact

# 4. Implement Controls
- Input validation
- Output encoding
- Authentication
- Encryption
- Monitoring

# 5. Verify Controls
- Security testing
- Penetration testing
- Code review
- Vulnerability scanning
```

---

## Task 1.9: Security Tools Ecosystem ⭐

### GitLab Security Scanners

```yaml
# All security scanners
include:
  # Static Analysis
  - template: Security/SAST.gitlab-ci.yml
  
  # Dynamic Analysis
  - template: DAST.gitlab-ci.yml
  
  # Dependency Analysis
  - template: Security/Dependency-Scanning.gitlab-ci.yml
  
  # Container Analysis
  - template: Security/Container-Scanning.gitlab-ci.yml
  
  # Secret Detection
  - template: Security/Secret-Detection.gitlab-ci.yml
  
  # License Compliance
  - template: Security/License-Scanning.gitlab-ci.yml
  
  # IaC Scanning
  - template: Security/SAST-IaC.gitlab-ci.yml
  
  # API Security
  - template: API-Fuzzing.gitlab-ci.yml
  
  # Coverage Fuzzing
  - template: Coverage-Fuzzing.gitlab-ci.yml
```

### Scanner Comparison

| Scanner | Type | Stage | Requirements |
|---------|------|-------|--------------|
| **SAST** | Static | test | Source code |
| **DAST** | Dynamic | security | Deployed app |
| **Dependency** | Static | test | Dependencies |
| **Container** | Static | security | Docker image |
| **Secret** | Static | .pre | Git repository |
| **License** | Static | test | Dependencies |
| **IaC** | Static | test | IaC files |
| **API Fuzzing** | Dynamic | fuzz | API spec |
| **Coverage Fuzzing** | Dynamic | fuzz | Instrumented code |

---

## Task 1.10: Security Metrics ⭐

### Key Security Metrics

```yaml
# Vulnerability Metrics
- Total vulnerabilities
- Vulnerabilities by severity
- Vulnerabilities by type
- Time to detect
- Time to remediate
- Vulnerability trends

# Scanning Metrics
- Scan coverage (% of projects)
- Scan frequency
- Scan success rate
- False positive rate
- Scanner performance

# Remediation Metrics
- Mean time to remediate (MTTR)
- Remediation rate
- Open vulnerabilities
- Resolved vulnerabilities
- Vulnerability backlog

# Compliance Metrics
- Compliance score
- Policy violations
- Audit findings
- License violations
```

### Security Dashboard Metrics

```yaml
# View in GitLab:
# Project > Security & Compliance > Vulnerability Report

# Metrics displayed:
- Vulnerability count by severity
- Vulnerability trends over time
- Top vulnerable projects
- Scanner coverage
- Remediation progress
```

---

## Exam Tips for Domain 1

1. **OWASP Top 10** - Know all 10 vulnerabilities and examples
2. **Severity Levels** - Understand Critical, High, Medium, Low
3. **CWE** - Know common CWE IDs (79, 89, 20, 78)
4. **DevSecOps** - Understand shift-left security
5. **SDLC** - Know security in each phase
6. **Compliance** - Understand PCI DSS, HIPAA, SOC 2
7. **Threat Modeling** - Know STRIDE model
8. **Security Scanners** - Know all GitLab scanner types
9. **Best Practices** - Input validation, output encoding, least privilege
10. **Metrics** - Understand key security metrics

---

## Quick Reference

### OWASP Top 10 (Quick List)

1. Broken Access Control
2. Cryptographic Failures
3. Injection
4. Insecure Design
5. Security Misconfiguration
6. Vulnerable Components
7. Authentication Failures
8. Software/Data Integrity Failures
9. Logging Failures
10. SSRF

### Severity Quick Guide

- **Critical (9.0-10.0):** Fix immediately
- **High (7.0-8.9):** Fix within days
- **Medium (4.0-6.9):** Fix within weeks
- **Low (0.1-3.9):** Fix when possible

### Security Scanners Quick Enable

```yaml
include:
  - template: Security/SAST.gitlab-ci.yml
  - template: Security/Dependency-Scanning.gitlab-ci.yml
  - template: Security/Container-Scanning.gitlab-ci.yml
  - template: Security/Secret-Detection.gitlab-ci.yml
  - template: Security/License-Scanning.gitlab-ci.yml
```
