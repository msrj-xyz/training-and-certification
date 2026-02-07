# Domain 6: Secret Detection

## Task 6.1: Secret Detection Overview ⭐⭐

### What is Secret Detection?

**Secret Detection** scans repositories for exposed secrets like API keys, passwords, and tokens.

| Aspect | Details |
|--------|---------|
| **Type** | Pattern matching |
| **When** | Pre-commit, commit, push |
| **Input** | Git repository |
| **Output** | Secret findings (JSON) |
| **Speed** | Fast (seconds) |
| **Coverage** | Entire git history |

---

## Task 6.2: Enable Secret Detection ⭐⭐

### Basic Configuration

```yaml
include:
  - template: Security/Secret-Detection.gitlab-ci.yml

# Scans:
# - Current commit
# - Git history (if enabled)
# - All branches
```

### Custom Configuration

```yaml
include:
  - template: Security/Secret-Detection.gitlab-ci.yml

variables:
  # Scan git history
  SECRET_DETECTION_HISTORIC_SCAN: "true"
  
  # Exclude paths
  SECRET_DETECTION_EXCLUDED_PATHS: "tests/, docs/"
  
  # Disable secret detection
  SECRET_DETECTION_DISABLED: "false"
  
  # Log level
  SECRET_DETECTION_LOG_LEVEL: "info"
```

---

## Task 6.3: Types of Secrets Detected ⭐⭐

### Common Secrets

| Type | Pattern Example | Risk |
|------|----------------|------|
| **AWS Access Key** | AKIA[0-9A-Z]{16} | Critical |
| **GitHub Token** | ghp_[a-zA-Z0-9]{36} | High |
| **GitLab Token** | glpat-[a-zA-Z0-9_-]{20} | High |
| **Slack Webhook** | https://hooks.slack.com/services/... | Medium |
| **Private SSH Key** | -----BEGIN RSA PRIVATE KEY----- | Critical |
| **API Keys** | api_key=... | High |
| **Passwords** | password=... | High |
| **JWT Tokens** | eyJ[a-zA-Z0-9_-]*\.[a-zA-Z0-9_-]* | High |
| **Database URLs** | postgres://user:pass@host | High |
| **OAuth Tokens** | oauth_token=... | High |

### Examples

```python
# AWS Access Key
AWS_ACCESS_KEY_ID = "AKIAIOSFODNN7EXAMPLE"
AWS_SECRET_ACCESS_KEY = "wJalrXUtnFEMI/K7MDENG/bPxRfiCYEXAMPLEKEY"

# GitHub Token
GITHUB_TOKEN = "ghp_1234567890abcdefghijklmnopqrstuvwxyz"

# Database URL
DATABASE_URL = "postgresql://user:password@localhost:5432/db"

# API Key
API_KEY = "sk-1234567890abcdefghijklmnopqrstuvwxyz"

# Private Key
PRIVATE_KEY = """-----BEGIN RSA PRIVATE KEY-----
MIIEpAIBAAKCAQEA...
-----END RSA PRIVATE KEY-----"""
```

---

## Task 6.4: Secret Detection Rules ⭐

### Built-in Rules

```yaml
# GitLab Secret Detection uses Gitleaks
# 100+ built-in rules for:
- Cloud providers (AWS, GCP, Azure)
- Version control (GitHub, GitLab, Bitbucket)
- Communication (Slack, Discord, Telegram)
- Databases (PostgreSQL, MySQL, MongoDB)
- Payment (Stripe, PayPal)
- And many more...
```

### Custom Rules

```yaml
# .gitleaks.toml
[[rules]]
id = "custom-api-key"
description = "Custom API Key"
regex = '''api[_-]?key[_-]?=["']?([a-zA-Z0-9]{32})["']?'''
tags = ["api", "key"]

[[rules]]
id = "custom-token"
description = "Custom Token"
regex = '''token[_-]?=["']?([a-zA-Z0-9]{40})["']?'''
tags = ["token"]
```

---

## Task 6.5: Secret Detection Allowlist ⭐⭐

### Allowlist Configuration

```yaml
# .gitleaks.toml
[allowlist]
description = "Allowlist for test secrets"

# Allowlist specific values
regexes = [
  '''test[_-]?api[_-]?key''',
  '''dummy[_-]?token''',
  '''example[_-]?secret'''
]

# Allowlist specific files
paths = [
  '''tests/.*''',
  '''docs/.*''',
  '''examples/.*'''
]

# Allowlist specific commits
commits = [
  "abc123def456"
]
```

### Inline Allowlist

```python
# gitleaks:allow
API_KEY = "test-api-key-for-development"

# Or use nosec
SECRET = "dummy-secret"  # nosec
```

---

## Task 6.6: Preventing Secret Commits ⭐⭐

### Pre-commit Hooks

```bash
# Install pre-commit
pip install pre-commit

# .pre-commit-config.yaml
repos:
  - repo: https://github.com/gitleaks/gitleaks
    rev: v8.18.0
    hooks:
      - id: gitleaks

# Install hooks
pre-commit install

# Now secrets are detected before commit
```

### Git Hooks

```bash
# .git/hooks/pre-commit
#!/bin/bash
gitleaks protect --staged --verbose

if [ $? -ne 0 ]; then
  echo "❌ Secret detected! Commit blocked."
  exit 1
fi
```

---

## Task 6.7: Secret Remediation ⭐⭐

### Remove Secrets from History

```bash
# Method 1: BFG Repo-Cleaner
java -jar bfg.jar --replace-text passwords.txt repo.git
cd repo.git
git reflog expire --expire=now --all
git gc --prune=now --aggressive

# Method 2: git filter-branch
git filter-branch --force --index-filter \
  "git rm --cached --ignore-unmatch path/to/secret" \
  --prune-empty --tag-name-filter cat -- --all

# Method 3: git filter-repo
git filter-repo --path path/to/secret --invert-paths

# Force push (⚠️ Dangerous)
git push origin --force --all
```

### Rotate Compromised Secrets

```yaml
# 1. Identify exposed secret
Secret: AWS_ACCESS_KEY_ID
Value: AKIAIOSFODNN7EXAMPLE
Exposed: 2024-01-15

# 2. Rotate immediately
- Revoke old key in AWS Console
- Generate new key
- Update in GitLab CI/CD variables
- Update in all environments

# 3. Monitor for misuse
- Check CloudTrail logs
- Review access patterns
- Set up alerts

# 4. Document incident
- What was exposed
- When it was exposed
- Actions taken
- Lessons learned
```

---

## Task 6.8: Secret Management Best Practices ⭐⭐

### Secure Secret Storage

```yaml
# ✅ Use GitLab CI/CD Variables
# Settings > CI/CD > Variables
# - Masked: Yes
# - Protected: Yes
# - Environment scope: production

# ✅ Use HashiCorp Vault
deploy:
  secrets:
    DATABASE_PASSWORD:
      vault: production/db/password@secrets

# ✅ Use Cloud Secret Managers
# - AWS Secrets Manager
# - GCP Secret Manager
# - Azure Key Vault

# ❌ Never commit secrets
# - No hardcoded credentials
# - No .env files in git
# - No secrets in code
```

### Environment Variables

```yaml
# ✅ Good: Use environment variables
import os
API_KEY = os.environ.get('API_KEY')
DATABASE_URL = os.environ.get('DATABASE_URL')

# ❌ Bad: Hardcoded secrets
API_KEY = "sk-1234567890"
DATABASE_URL = "postgres://user:pass@host/db"
```

---

## Task 6.9: Secret Detection in CI/CD ⭐

### Pipeline Integration

```yaml
stages:
  - .pre
  - build
  - test
  - deploy

# Secret detection runs first
secret_detection:
  stage: .pre
  include:
    - template: Security/Secret-Detection.gitlab-ci.yml
  rules:
    - if: '$CI_COMMIT_BRANCH'

# Block pipeline if secrets found
build:
  stage: build
  needs:
    - secret_detection
  script:
    - npm run build
```

---

## Task 6.10: Common Mistakes ⭐

### Anti-patterns

```python
# ❌ Secrets in code
API_KEY = "sk-1234567890"

# ❌ Secrets in comments
# API_KEY: sk-1234567890

# ❌ Secrets in config files
# config.json
{
  "api_key": "sk-1234567890"
}

# ❌ Secrets in environment files committed
# .env (in git)
API_KEY=sk-1234567890

# ❌ Secrets in logs
print(f"API Key: {API_KEY}")

# ❌ Secrets in error messages
raise Exception(f"Auth failed with key: {API_KEY}")

# ❌ Secrets in URLs
url = f"https://api.example.com?key={API_KEY}"
```

---

## Exam Tips for Domain 6

1. **Definition** - Scans for exposed secrets in git
2. **Enable** - Include Security/Secret-Detection.gitlab-ci.yml
3. **Scope** - Current commit + git history (if enabled)
4. **Types** - AWS keys, GitHub tokens, API keys, passwords
5. **Allowlist** - .gitleaks.toml for test secrets
6. **Prevention** - Pre-commit hooks, git hooks
7. **Remediation** - Remove from history, rotate secrets
8. **Best Practices** - Use CI/CD variables, never commit
9. **Storage** - Vault, Secret Manager, masked variables
10. **Historic Scan** - SECRET_DETECTION_HISTORIC_SCAN=true

---

## Quick Reference

### Enable Secret Detection

```yaml
include:
  - template: Security/Secret-Detection.gitlab-ci.yml

variables:
  SECRET_DETECTION_HISTORIC_SCAN: "true"
```

### Allowlist Test Secrets

```yaml
# .gitleaks.toml
[allowlist]
regexes = [
  '''test[_-]?api[_-]?key''',
  '''dummy[_-]?token'''
]
paths = [
  '''tests/.*''',
  '''examples/.*'''
]
```

### Secure Secret Usage

```python
# ✅ Good
import os
API_KEY = os.environ.get('API_KEY')

# ❌ Bad
API_KEY = "sk-1234567890"
```
