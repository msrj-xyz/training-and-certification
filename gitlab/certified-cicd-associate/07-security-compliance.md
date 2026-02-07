# Domain 7: Pipeline Security & Compliance

## Task 7.1: Security Scanning Overview ⭐⭐

### GitLab Security Scanners

| Scanner | Purpose | Stage | Report |
|---------|---------|-------|--------|
| **SAST** | Static code analysis | test | gl-sast-report.json |
| **DAST** | Dynamic app testing | dast | gl-dast-report.json |
| **Dependency Scanning** | Vulnerable dependencies | test | gl-dependency-scanning-report.json |
| **Container Scanning** | Container vulnerabilities | test | gl-container-scanning-report.json |
| **Secret Detection** | Exposed secrets | test | gl-secret-detection-report.json |
| **License Compliance** | License violations | test | gl-license-scanning-report.json |
| **Coverage Fuzzing** | Fuzz testing | fuzz | gl-coverage-fuzzing-report.json |
| **API Fuzzing** | API fuzz testing | fuzz | gl-api-fuzzing-report.json |

---

## Task 7.2: SAST (Static Application Security Testing) ⭐⭐

### Enable SAST

```yaml
# Include SAST template
include:
  - template: Security/SAST.gitlab-ci.yml

# Auto-detects languages and runs appropriate scanners
# Supports: Java, JavaScript, Python, Go, Ruby, PHP, C/C++, etc.
```

### Custom SAST Configuration

```yaml
include:
  - template: Security/SAST.gitlab-ci.yml

# Customize SAST
variables:
  SAST_EXCLUDED_PATHS: "spec, test, tests, tmp"
  SAST_EXCLUDED_ANALYZERS: "eslint, gosec"
  SAST_DEFAULT_ANALYZERS: "bandit, brakeman, flawfinder, kubesec, nodejs-scan, phpcs-security-audit, pmd-apex, security-code-scan, semgrep, sobelow, spotbugs"

# Override SAST job
sast:
  variables:
    SAST_CONFIDENCE_LEVEL: 2  # 1=Low, 2=Medium, 3=High
  rules:
    - if: '$CI_COMMIT_BRANCH == "main"'
    - if: '$CI_PIPELINE_SOURCE == "merge_request_event"'
```

### SAST Analyzers by Language

| Language | Analyzer |
|----------|----------|
| **JavaScript/TypeScript** | ESLint, Semgrep |
| **Python** | Bandit, Semgrep |
| **Java** | SpotBugs, Semgrep |
| **Go** | Gosec, Semgrep |
| **Ruby** | Brakeman, Semgrep |
| **PHP** | phpcs-security-audit |
| **C/C++** | Flawfinder, Semgrep |
| **C#** | Security Code Scan |
| **Apex** | PMD |

---

## Task 7.3: Dependency Scanning ⭐⭐

### Enable Dependency Scanning

```yaml
include:
  - template: Security/Dependency-Scanning.gitlab-ci.yml

# Auto-detects package managers:
# npm, yarn, pip, bundler, maven, gradle, composer, go modules, etc.
```

### Custom Dependency Scanning

```yaml
include:
  - template: Security/Dependency-Scanning.gitlab-ci.yml

variables:
  DS_EXCLUDED_PATHS: "spec, test, tests, tmp"
  DS_EXCLUDED_ANALYZERS: "gemnasium, gemnasium-maven, gemnasium-python"
  DS_DEFAULT_ANALYZERS: "gemnasium, gemnasium-maven, gemnasium-python, bundler-audit, retire.js"

# Override dependency scanning job
gemnasium-dependency_scanning:
  variables:
    DS_REMEDIATE: "true"  # Auto-create MR for fixes
  rules:
    - if: '$CI_COMMIT_BRANCH == "main"'
    - if: '$CI_PIPELINE_SOURCE == "merge_request_event"'
```

### Dependency Scanning Analyzers

| Package Manager | Analyzer |
|----------------|----------|
| **npm/yarn** | Gemnasium, Retire.js |
| **pip** | Gemnasium-Python |
| **bundler** | Bundler-Audit, Gemnasium |
| **maven** | Gemnasium-Maven |
| **gradle** | Gemnasium-Maven |
| **composer** | Gemnasium |
| **go modules** | Gemnasium |
| **NuGet** | Gemnasium |

---

## Task 7.4: Container Scanning ⭐⭐

### Enable Container Scanning

```yaml
include:
  - template: Security/Container-Scanning.gitlab-ci.yml

# Scans Docker images for vulnerabilities
# Requires image to be built and pushed to registry

build:
  stage: build
  image: docker:latest
  services:
    - docker:dind
  script:
    - docker build -t $CI_REGISTRY_IMAGE:$CI_COMMIT_SHA .
    - docker push $CI_REGISTRY_IMAGE:$CI_COMMIT_SHA

# Container scanning runs automatically
```

### Custom Container Scanning

```yaml
include:
  - template: Security/Container-Scanning.gitlab-ci.yml

variables:
  CS_IMAGE: $CI_REGISTRY_IMAGE:$CI_COMMIT_SHA
  CS_SEVERITY_THRESHOLD: "MEDIUM"  # UNKNOWN, LOW, MEDIUM, HIGH, CRITICAL
  CS_ANALYZER_IMAGE: "registry.gitlab.com/security-products/container-scanning:latest"

container_scanning:
  variables:
    CS_DISABLE_DEPENDENCY_LIST: "false"
  rules:
    - if: '$CI_COMMIT_BRANCH == "main"'
```

### Trivy Scanner

```yaml
# Use Trivy for container scanning
container-scan:
  stage: test
  image:
    name: aquasec/trivy:latest
    entrypoint: [""]
  script:
    - trivy image --format json --output gl-container-scanning-report.json $CI_REGISTRY_IMAGE:$CI_COMMIT_SHA
  artifacts:
    reports:
      container_scanning: gl-container-scanning-report.json
```

---

## Task 7.5: Secret Detection ⭐⭐

### Enable Secret Detection

```yaml
include:
  - template: Security/Secret-Detection.gitlab-ci.yml

# Scans for exposed secrets in:
# - Source code
# - Commit history
# - Configuration files
```

### Custom Secret Detection

```yaml
include:
  - template: Security/Secret-Detection.gitlab-ci.yml

variables:
  SECRET_DETECTION_EXCLUDED_PATHS: "tests/, docs/"
  SECRET_DETECTION_HISTORIC_SCAN: "true"  # Scan git history

secret_detection:
  rules:
    - if: '$CI_COMMIT_BRANCH'
```

### Common Detected Secrets

- AWS Access Keys
- Azure Service Principal
- GCP Service Account
- Private SSH Keys
- API Tokens
- Database Passwords
- OAuth Tokens
- Slack Webhooks
- GitHub Tokens

---

## Task 7.6: DAST (Dynamic Application Security Testing) ⭐

### Enable DAST

```yaml
include:
  - template: DAST.gitlab-ci.yml

variables:
  DAST_WEBSITE: "https://staging.example.com"
  DAST_FULL_SCAN_ENABLED: "true"

# DAST requires running application
# Usually runs after deployment
```

### DAST Configuration

```yaml
include:
  - template: DAST.gitlab-ci.yml

variables:
  DAST_WEBSITE: "https://staging.example.com"
  DAST_AUTH_URL: "https://staging.example.com/login"
  DAST_USERNAME: "testuser"
  DAST_PASSWORD: "$DAST_PASSWORD"  # From CI/CD variables
  DAST_USERNAME_FIELD: "username"
  DAST_PASSWORD_FIELD: "password"
  DAST_FULL_SCAN_ENABLED: "true"
  DAST_SPIDER_MINS: "5"
  DAST_TARGET_AVAILABILITY_TIMEOUT: "60"

dast:
  rules:
    - if: '$CI_COMMIT_BRANCH == "main"'
      when: always
```

### DAST API Scanning

```yaml
include:
  - template: DAST-API.gitlab-ci.yml

variables:
  DAST_API_TARGET_URL: "https://api.example.com"
  DAST_API_OPENAPI: "openapi.json"  # OpenAPI spec
  DAST_API_HAR: "api-requests.har"  # HAR file

dast_api:
  rules:
    - if: '$CI_COMMIT_BRANCH == "main"'
```

---

## Task 7.7: License Compliance ⭐

### Enable License Scanning

```yaml
include:
  - template: Security/License-Scanning.gitlab-ci.yml

# Detects licenses in dependencies
# Supports: npm, pip, bundler, maven, gradle, composer, go modules
```

### License Policies

```yaml
include:
  - template: Security/License-Scanning.gitlab-ci.yml

# Configure in UI:
# Settings > Security & Compliance > License Compliance

# Approve licenses:
# - MIT
# - Apache-2.0
# - BSD-3-Clause

# Deny licenses:
# - GPL-3.0
# - AGPL-3.0
```

---

## Task 7.8: Security Dashboard and Reports ⭐

### Security Reports

```yaml
# All security scans
include:
  - template: Security/SAST.gitlab-ci.yml
  - template: Security/Dependency-Scanning.gitlab-ci.yml
  - template: Security/Container-Scanning.gitlab-ci.yml
  - template: Security/Secret-Detection.gitlab-ci.yml
  - template: Security/License-Scanning.gitlab-ci.yml

# View results:
# - Security & Compliance > Vulnerability Report
# - Merge Request > Security tab
# - Pipeline > Security tab
```

### Custom Security Report

```yaml
security-scan:
  stage: test
  script:
    - ./custom-security-scan.sh
  artifacts:
    reports:
      sast: gl-sast-report.json
      dependency_scanning: gl-dependency-scanning-report.json
      container_scanning: gl-container-scanning-report.json
```

---

## Task 7.9: Protected Branches and Environments ⭐⭐

### Protected Branches

```yaml
# Settings > Repository > Protected Branches

# Configuration:
# Branch: main
# Allowed to merge: Maintainers
# Allowed to push: No one
# Allowed to force push: No
# Code owner approval: Required

# Use protected variables
deploy-production:
  script:
    - echo $PROD_API_KEY  # Only available on protected branches
  only:
    - main
```

### Protected Environments

```yaml
# Settings > CI/CD > Environments

# Configuration:
# Environment: production
# Protected: Yes
# Allowed to deploy: Maintainers

deploy-production:
  stage: deploy
  script:
    - ./deploy.sh
  environment:
    name: production
  only:
    - main
  when: manual
```

### Deployment Approvals

```yaml
# Settings > CI/CD > Protected Environments
# Enable: Required approvals before deployment

deploy-production:
  stage: deploy
  script:
    - ./deploy.sh
  environment:
    name: production
  rules:
    - if: '$CI_COMMIT_BRANCH == "main"'
      when: manual
  # Requires approval before running
```

---

## Task 7.10: Compliance Frameworks ⭐

### Compliance Pipeline

```yaml
# Include compliance template
include:
  - project: 'compliance/pipeline-templates'
    ref: main
    file: '/compliance-template.yml'

# Compliance checks
compliance-check:
  stage: .pre
  script:
    - ./check-compliance.sh
  rules:
    - if: '$CI_COMMIT_BRANCH == "main"'

# Audit logging
audit-log:
  stage: .post
  script:
    - ./log-audit.sh
  when: always
```

### Separation of Duties

```yaml
# Different roles for different stages
build:
  stage: build
  script:
    - npm run build
  # Developers can run

approve-deployment:
  stage: deploy
  script:
    - echo "Deployment approved"
  when: manual
  only:
    - main
  # Only maintainers can approve

deploy-production:
  stage: deploy
  needs:
    - approve-deployment
  script:
    - ./deploy.sh
  environment:
    name: production
```

---

## Task 7.11: Security Best Practices ⭐⭐

### Secure Pipeline Configuration

```yaml
# ✅ Good practices
variables:
  # Use masked variables
  API_KEY: $API_KEY  # Defined in UI as masked

build:
  # Use specific image versions
  image: node:18.17.0-alpine
  
  # Disable privileged mode
  services:
    - name: docker:dind
      command: ["--tls=false"]
  
  # Use protected variables on protected branches
  script:
    - echo $PROD_API_KEY  # Only on main
  only:
    - main

# ❌ Bad practices
bad-example:
  # Don't use latest tag
  image: node:latest
  
  # Don't expose secrets
  script:
    - echo "API Key: $API_KEY"
  
  # Don't hardcode secrets
  variables:
    SECRET: "hardcoded-secret-123"
```

### Image Security

```yaml
# Use trusted registries
build:
  image: registry.gitlab.com/gitlab-org/gitlab-runner:latest
  
# Scan images before use
verify-image:
  stage: .pre
  script:
    - trivy image node:18-alpine
  allow_failure: false

# Use minimal images
build:
  image: node:18-alpine  # Smaller attack surface
  script:
    - npm run build
```

### Secrets Management

```yaml
# ✅ Use CI/CD variables (masked)
deploy:
  script:
    - curl -H "Authorization: Bearer $API_TOKEN" api.example.com

# ✅ Use file variables for keys
deploy:
  script:
    - chmod 600 $SSH_PRIVATE_KEY
    - ssh -i $SSH_PRIVATE_KEY user@server

# ✅ Use Vault integration
deploy:
  secrets:
    DATABASE_PASSWORD:
      vault: production/db/password@secrets
  script:
    - ./deploy.sh

# ❌ Don't commit secrets
# ❌ Don't echo secrets
# ❌ Don't use secrets in URLs
```

---

## Task 7.12: Vulnerability Management ⭐

### Vulnerability Workflow

```yaml
# 1. Detect vulnerabilities
include:
  - template: Security/SAST.gitlab-ci.yml
  - template: Security/Dependency-Scanning.gitlab-ci.yml

# 2. Review in Security Dashboard
# Security & Compliance > Vulnerability Report

# 3. Create issues for vulnerabilities
# Click "Create issue" in vulnerability details

# 4. Track remediation
# Link MR to vulnerability issue

# 5. Verify fix
# Re-run security scans
```

### Auto-remediation

```yaml
include:
  - template: Security/Dependency-Scanning.gitlab-ci.yml

variables:
  DS_REMEDIATE: "true"  # Auto-create MR for dependency updates

# GitLab creates MR with dependency updates
# Review and merge to fix vulnerabilities
```

---

## Exam Tips for Domain 7

1. **Security Templates** - Know how to include security scanning templates
2. **SAST** - Understand static code analysis
3. **Dependency Scanning** - Know package manager support
4. **Container Scanning** - Understand image vulnerability scanning
5. **Secret Detection** - Know what secrets are detected
6. **Protected Branches** - Understand branch protection
7. **Protected Environments** - Know deployment protection
8. **Masked Variables** - Understand masking requirements
9. **Security Reports** - Know report formats and locations
10. **Best Practices** - Never expose secrets, use specific image versions

---

## Quick Reference

### Enable All Security Scans

```yaml
include:
  - template: Security/SAST.gitlab-ci.yml
  - template: Security/Dependency-Scanning.gitlab-ci.yml
  - template: Security/Container-Scanning.gitlab-ci.yml
  - template: Security/Secret-Detection.gitlab-ci.yml
  - template: Security/License-Scanning.gitlab-ci.yml
```

### Security Variables

```yaml
# SAST
SAST_EXCLUDED_PATHS: "spec, test, tests, tmp"
SAST_CONFIDENCE_LEVEL: 2

# Dependency Scanning
DS_EXCLUDED_PATHS: "spec, test, tests, tmp"
DS_REMEDIATE: "true"

# Container Scanning
CS_SEVERITY_THRESHOLD: "MEDIUM"
CS_IMAGE: $CI_REGISTRY_IMAGE:$CI_COMMIT_SHA

# Secret Detection
SECRET_DETECTION_HISTORIC_SCAN: "true"
```

### Protected Deployment

```yaml
deploy-production:
  stage: deploy
  script:
    - ./deploy.sh
  environment:
    name: production
  only:
    - main
  when: manual
  # Uses protected variables
  # Requires approval (if configured)
```
