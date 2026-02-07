# Domain 2: SAST (Static Application Security Testing)

## Task 2.1: SAST Overview ⭐⭐

### What is SAST?

**Static Application Security Testing (SAST)** analyzes source code, bytecode, or binaries to identify security vulnerabilities without executing the application.

| Aspect | Details |
|--------|---------|
| **Type** | White-box testing |
| **When** | During build/test stage |
| **Input** | Source code |
| **Output** | Vulnerability report (JSON) |
| **Speed** | Fast (minutes) |
| **Coverage** | Code-level vulnerabilities |

### SAST Benefits

```yaml
# Advantages
✅ Early detection (shift-left)
✅ No deployed application needed
✅ Finds code-level vulnerabilities
✅ Fast feedback to developers
✅ Integrates with CI/CD
✅ Covers entire codebase

# Limitations
❌ Cannot detect runtime issues
❌ May have false positives
❌ Language-specific analyzers needed
❌ Cannot test business logic flaws
```

---

## Task 2.2: Enable SAST ⭐⭐

### Basic SAST Configuration

```yaml
# Method 1: Include template
include:
  - template: Security/SAST.gitlab-ci.yml

# That's it! SAST auto-detects languages and runs appropriate analyzers
```

### Manual SAST Job

```yaml
# Method 2: Manual configuration
sast:
  stage: test
  image: registry.gitlab.com/security-products/sast:latest
  script:
    - /analyzer run
  artifacts:
    reports:
      sast: gl-sast-report.json
  rules:
    - if: '$CI_COMMIT_BRANCH'
```

### Enable via UI

```yaml
# GitLab UI Method:
# 1. Go to Security & Compliance > Configuration
# 2. Click "Configure SAST"
# 3. Create merge request
# 4. Review and merge

# This creates a commit with:
include:
  - template: Security/SAST.gitlab-ci.yml
```

---

## Task 2.3: SAST Analyzers by Language ⭐⭐

### Supported Languages and Analyzers

| Language | Analyzer | Detects |
|----------|----------|---------|
| **JavaScript/TypeScript** | ESLint, Semgrep | XSS, injection, insecure patterns |
| **Python** | Bandit, Semgrep | Injection, weak crypto, hardcoded secrets |
| **Java** | SpotBugs, Semgrep | Injection, XXE, insecure deserialization |
| **Go** | Gosec, Semgrep | Injection, weak crypto, race conditions |
| **Ruby** | Brakeman, Semgrep | SQL injection, XSS, mass assignment |
| **PHP** | phpcs-security-audit | SQL injection, XSS, file inclusion |
| **C/C++** | Flawfinder, Semgrep | Buffer overflow, format string, injection |
| **C#** | Security Code Scan | Injection, XSS, weak crypto |
| **Apex** | PMD | SOQL injection, XSS |
| **Scala** | SpotBugs | Similar to Java |
| **Kotlin** | SpotBugs | Similar to Java |
| **Swift** | Semgrep | iOS security issues |
| **Objective-C** | Semgrep | iOS security issues |

### Analyzer Auto-Detection

```yaml
# GitLab auto-detects based on files:
- package.json → JavaScript (ESLint, Semgrep)
- requirements.txt → Python (Bandit, Semgrep)
- pom.xml → Java (SpotBugs, Semgrep)
- go.mod → Go (Gosec, Semgrep)
- Gemfile → Ruby (Brakeman, Semgrep)
- composer.json → PHP (phpcs-security-audit)
- *.cs → C# (Security Code Scan)
```

---

## Task 2.4: SAST Configuration ⭐⭐

### SAST Variables

```yaml
include:
  - template: Security/SAST.gitlab-ci.yml

variables:
  # Exclude paths from scanning
  SAST_EXCLUDED_PATHS: "spec, test, tests, tmp, node_modules"
  
  # Exclude specific analyzers
  SAST_EXCLUDED_ANALYZERS: "eslint, gosec"
  
  # Set confidence level (1=Low, 2=Medium, 3=High)
  SAST_CONFIDENCE_LEVEL: 2
  
  # Disable SAST
  SAST_DISABLED: "false"
  
  # Custom analyzer image
  SAST_ANALYZER_IMAGE_TAG: "latest"
  
  # Semgrep rules
  SAST_SEMGREP_METRICS: "false"
```

### Exclude Paths

```yaml
# Exclude test files and dependencies
variables:
  SAST_EXCLUDED_PATHS: >
    spec,
    test,
    tests,
    tmp,
    node_modules,
    vendor,
    dist,
    build,
    __pycache__,
    .git
```

### Exclude Analyzers

```yaml
# Run only specific analyzers
variables:
  # Exclude all except Semgrep
  SAST_EXCLUDED_ANALYZERS: "bandit, brakeman, eslint, flawfinder, gosec, kubesec, nodejs-scan, phpcs-security-audit, pmd-apex, security-code-scan, sobelow, spotbugs"
  
# Or use default analyzers
variables:
  SAST_DEFAULT_ANALYZERS: "bandit, brakeman, eslint, flawfinder, gosec, kubesec, nodejs-scan, phpcs-security-audit, pmd-apex, security-code-scan, semgrep, sobelow, spotbugs"
```

---

## Task 2.5: SAST Report Format ⭐⭐

### SAST JSON Report Structure

```json
{
  "version": "15.0.0",
  "vulnerabilities": [
    {
      "id": "630f23...",
      "category": "sast",
      "name": "SQL Injection",
      "message": "Possible SQL injection",
      "description": "The application constructs SQL queries...",
      "cve": "CWE-89",
      "severity": "High",
      "confidence": "Medium",
      "scanner": {
        "id": "semgrep",
        "name": "Semgrep"
      },
      "location": {
        "file": "app/models/user.rb",
        "start_line": 42,
        "end_line": 42,
        "class": "User",
        "method": "find_by_username"
      },
      "identifiers": [
        {
          "type": "cwe",
          "name": "CWE-89",
          "value": "89",
          "url": "https://cwe.mitre.org/data/definitions/89.html"
        }
      ],
      "links": [
        {
          "url": "https://owasp.org/www-community/attacks/SQL_Injection"
        }
      ]
    }
  ],
  "remediations": [],
  "scan": {
    "scanner": {
      "id": "sast",
      "name": "SAST",
      "version": "4.0.0"
    },
    "type": "sast",
    "start_time": "2024-01-15T10:00:00",
    "end_time": "2024-01-15T10:05:00",
    "status": "success"
  }
}
```

### Vulnerability Fields

| Field | Description | Example |
|-------|-------------|---------|
| **id** | Unique identifier | `630f23...` |
| **category** | Scanner type | `sast` |
| **name** | Vulnerability name | `SQL Injection` |
| **severity** | Risk level | `Critical`, `High`, `Medium`, `Low` |
| **confidence** | Detection confidence | `High`, `Medium`, `Low` |
| **cve** | CWE identifier | `CWE-89` |
| **location** | Code location | File, line, method |
| **scanner** | Analyzer used | `semgrep`, `bandit` |

---

## Task 2.6: Common SAST Vulnerabilities ⭐⭐

### SQL Injection (CWE-89)

```python
# Vulnerable code
def get_user(username):
    query = f"SELECT * FROM users WHERE username = '{username}'"
    return db.execute(query)

# SAST Detection:
# - Severity: High
# - CWE: 89
# - Message: SQL injection vulnerability

# Fixed code
def get_user(username):
    query = "SELECT * FROM users WHERE username = ?"
    return db.execute(query, (username,))
```

### Cross-Site Scripting (CWE-79)

```javascript
// Vulnerable code
function displayMessage(message) {
    document.getElementById('output').innerHTML = message;
}

// SAST Detection:
// - Severity: Medium
// - CWE: 79
// - Message: XSS vulnerability

// Fixed code
function displayMessage(message) {
    const element = document.getElementById('output');
    element.textContent = message; // Safe
}
```

### Command Injection (CWE-78)

```python
# Vulnerable code
import os
def ping_host(host):
    os.system(f"ping -c 1 {host}")

# SAST Detection:
# - Severity: High
# - CWE: 78
# - Message: Command injection

# Fixed code
import subprocess
def ping_host(host):
    subprocess.run(["ping", "-c", "1", host], check=True)
```

### Path Traversal (CWE-22)

```python
# Vulnerable code
def read_file(filename):
    with open(f"/var/www/uploads/{filename}", 'r') as f:
        return f.read()

# SAST Detection:
# - Severity: High
# - CWE: 22
# - Message: Path traversal

# Fixed code
import os
def read_file(filename):
    # Validate filename
    if '..' in filename or filename.startswith('/'):
        raise ValueError("Invalid filename")
    safe_path = os.path.join('/var/www/uploads', filename)
    with open(safe_path, 'r') as f:
        return f.read()
```

### Hardcoded Credentials (CWE-798)

```python
# Vulnerable code
DATABASE_PASSWORD = "admin123"
API_KEY = "sk-1234567890abcdef"

# SAST Detection:
# - Severity: High
# - CWE: 798
# - Message: Hardcoded credentials

# Fixed code
import os
DATABASE_PASSWORD = os.environ.get('DATABASE_PASSWORD')
API_KEY = os.environ.get('API_KEY')
```

### Weak Cryptography (CWE-327)

```python
# Vulnerable code
import hashlib
def hash_password(password):
    return hashlib.md5(password.encode()).hexdigest()

# SAST Detection:
# - Severity: Medium
# - CWE: 327
# - Message: Weak cryptographic algorithm

# Fixed code
import bcrypt
def hash_password(password):
    return bcrypt.hashpw(password.encode(), bcrypt.gensalt())
```

---

## Task 2.7: SAST Custom Rules (Semgrep) ⭐

### Custom Semgrep Rules

```yaml
# .semgrep.yml
rules:
  - id: custom-sql-injection
    pattern: |
      db.execute(f"... {$VAR} ...")
    message: Possible SQL injection with f-string
    severity: ERROR
    languages:
      - python
    metadata:
      cwe: "CWE-89"
      owasp: "A03:2021"
      
  - id: custom-hardcoded-secret
    pattern: |
      $VAR = "sk-..."
    message: Hardcoded API key detected
    severity: WARNING
    languages:
      - python
    metadata:
      cwe: "CWE-798"
```

### Use Custom Rules

```yaml
include:
  - template: Security/SAST.gitlab-ci.yml

variables:
  SAST_SEMGREP_RULES: ".semgrep.yml"
```

---

## Task 2.8: SAST False Positives ⭐⭐

### Handling False Positives

```yaml
# 1. Dismiss in UI
# Security & Compliance > Vulnerability Report
# Click vulnerability > Dismiss > Select reason

# 2. Adjust confidence level
variables:
  SAST_CONFIDENCE_LEVEL: 3  # Only high confidence

# 3. Exclude specific paths
variables:
  SAST_EXCLUDED_PATHS: "test, spec, mock"

# 4. Use code comments (analyzer-specific)
```

### Semgrep Ignore

```python
# nosemgrep: python.lang.security.audit.dangerous-system-call
os.system("safe command")

# Or use nosem
os.system("safe command")  # nosem
```

### Bandit Ignore

```python
# nosec
password = "test123"  # nosec

# Or with comment
import subprocess
subprocess.call(shell=True)  # nosec B602
```

---

## Task 2.9: SAST Performance Optimization ⭐

### Optimize SAST Scanning

```yaml
# 1. Exclude unnecessary paths
variables:
  SAST_EXCLUDED_PATHS: "node_modules, vendor, dist, build, test"

# 2. Run only needed analyzers
variables:
  SAST_EXCLUDED_ANALYZERS: "eslint"  # If using Semgrep

# 3. Use specific analyzer versions
variables:
  SAST_ANALYZER_IMAGE_TAG: "4"

# 4. Parallel scanning (automatic)
# GitLab runs analyzers in parallel

# 5. Cache analyzer images
# Runners cache Docker images automatically
```

### SAST Job Optimization

```yaml
sast:
  stage: test
  # Run only on MR and main
  rules:
    - if: '$CI_PIPELINE_SOURCE == "merge_request_event"'
    - if: '$CI_COMMIT_BRANCH == "main"'
  # Set timeout
  timeout: 10 minutes
```

---

## Task 2.10: SAST Best Practices ⭐⭐

### SAST Implementation Best Practices

```yaml
# 1. Enable SAST early
include:
  - template: Security/SAST.gitlab-ci.yml

# 2. Configure appropriately
variables:
  SAST_EXCLUDED_PATHS: "test, spec, vendor"
  SAST_CONFIDENCE_LEVEL: 2

# 3. Review results regularly
# Check Security Dashboard daily

# 4. Fix high/critical first
# Prioritize by severity

# 5. Integrate with workflow
# Block MR on critical vulnerabilities

# 6. Train developers
# Share SAST findings and fixes

# 7. Monitor trends
# Track vulnerability metrics

# 8. Keep analyzers updated
# Use latest analyzer versions
```

### SAST in Merge Requests

```yaml
# Require SAST in MR
# Settings > Merge Requests > Merge checks
# ✅ Pipelines must succeed

# Security widget shows:
- New vulnerabilities introduced
- Existing vulnerabilities
- Resolved vulnerabilities
- Severity breakdown
```

---

## Exam Tips for Domain 2

1. **SAST Definition** - Static analysis of source code
2. **Enable SAST** - Include Security/SAST.gitlab-ci.yml template
3. **Analyzers** - Know language-specific analyzers (Semgrep, Bandit, etc.)
4. **Configuration** - SAST_EXCLUDED_PATHS, SAST_CONFIDENCE_LEVEL
5. **Vulnerabilities** - SQL injection, XSS, command injection
6. **CWE** - Know common CWE IDs (89, 79, 78, 22, 798, 327)
7. **Report Format** - JSON with vulnerabilities array
8. **False Positives** - Dismiss in UI, adjust confidence, exclude paths
9. **Custom Rules** - Semgrep custom rules
10. **Best Practices** - Enable early, review regularly, fix high/critical first

---

## Quick Reference

### Enable SAST

```yaml
include:
  - template: Security/SAST.gitlab-ci.yml
```

### SAST Variables

```yaml
variables:
  SAST_EXCLUDED_PATHS: "spec, test, tests, tmp"
  SAST_EXCLUDED_ANALYZERS: "eslint"
  SAST_CONFIDENCE_LEVEL: 2
```

### Common Vulnerabilities

- **SQL Injection (CWE-89):** Unsanitized SQL queries
- **XSS (CWE-79):** Unescaped user input in HTML
- **Command Injection (CWE-78):** Unsanitized shell commands
- **Path Traversal (CWE-22):** Unvalidated file paths
- **Hardcoded Secrets (CWE-798):** Credentials in code
- **Weak Crypto (CWE-327):** MD5, SHA1, DES

### View SAST Results

```yaml
# In GitLab UI:
# 1. Pipeline > Security tab
# 2. Merge Request > Security widget
# 3. Security & Compliance > Vulnerability Report
```
