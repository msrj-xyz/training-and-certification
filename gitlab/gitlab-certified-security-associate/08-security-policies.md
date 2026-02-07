# Domain 8: Security Policies & Governance

## Task 8.1: Security Policies Overview ⭐⭐

### Policy Types

| Policy Type | Purpose | Scope |
|-------------|---------|-------|
| **Scan Execution Policy** | Enforce security scans | Project/Group |
| **Scan Result Policy** | Block based on findings | Project/Group |
| **Approval Rules** | Require approvals | Merge Request |
| **Protected Branches** | Branch protection | Repository |
| **Protected Environments** | Deployment protection | Environment |

---

## Task 8.2: Scan Execution Policies ⭐⭐

### Create Scan Execution Policy

```yaml
# .gitlab/security-policies/scan-execution-policy.yml
---
scan_execution_policy:
- name: Enforce SAST and Dependency Scanning
  description: Run SAST and dependency scanning on all branches
  enabled: true
  rules:
  - type: pipeline
    branches:
    - main
    - develop
    - release/*
  actions:
  - scan: sast
  - scan: dependency_scanning
  - scan: secret_detection
```

### Policy Actions

```yaml
# Available scans:
actions:
  - scan: sast
  - scan: dast
  - scan: dependency_scanning
  - scan: container_scanning
  - scan: secret_detection
  - scan: coverage_fuzzing
  - scan: api_fuzzing
```

---

## Task 8.3: Scan Result Policies ⭐⭐

### Create Scan Result Policy

```yaml
# .gitlab/security-policies/scan-result-policy.yml
---
scan_result_policy:
- name: Block Critical Vulnerabilities
  description: Block MR if critical vulnerabilities found
  enabled: true
  rules:
  - type: scan_finding
    branches:
    - main
    scanners:
    - sast
    - dependency_scanning
    - container_scanning
    vulnerabilities_allowed: 0
    severity_levels:
    - critical
    vulnerability_states:
    - newly_detected
  actions:
  - type: require_approval
    approvals_required: 2
    user_approvers:
    - security-team
```

### Policy Rules

```yaml
# Severity levels:
severity_levels:
  - critical
  - high
  - medium
  - low
  - unknown

# Vulnerability states:
vulnerability_states:
  - newly_detected
  - detected
  - confirmed
  - resolved
  - dismissed

# Scanners:
scanners:
  - sast
  - dast
  - dependency_scanning
  - container_scanning
  - secret_detection
```

---

## Task 8.4: Approval Rules ⭐⭐

### Security Approval Rules

```yaml
# Settings > Merge Requests > Approval Rules

# Rule: Security Team Approval
Name: Security Review
Approvals required: 2
Approvers: @security-team
Conditions:
  - Security scan findings
  - License violations
  - New dependencies

# Rule: Critical Vulnerabilities
Name: Critical Vulnerability Review
Approvals required: 1
Approvers: @security-lead
Conditions:
  - Critical severity findings
  - High severity findings (>5)
```

### Code Owner Approval

```yaml
# CODEOWNERS file
# Security team owns security configs
/.gitlab/security-policies/ @security-team
/Dockerfile @security-team
/.gitlab-ci.yml @security-team

# Requires approval from code owners
# Settings > Merge Requests > Merge checks
# ✅ Require approval from code owners
```

---

## Task 8.5: Protected Branches ⭐

### Branch Protection

```yaml
# Settings > Repository > Protected Branches

# main branch:
Allowed to merge: Maintainers
Allowed to push: No one
Allowed to force push: No
Code owner approval: Required

# release/* branches:
Allowed to merge: Maintainers
Allowed to push: No one
Code owner approval: Required

# develop branch:
Allowed to merge: Developers + Maintainers
Allowed to push: Developers + Maintainers
```

---

## Task 8.6: Protected Environments ⭐

### Environment Protection

```yaml
# Settings > CI/CD > Protected Environments

# production:
Protected: Yes
Allowed to deploy: Maintainers
Approval required: Yes
Approvers: @ops-team
Deployment frequency: Manual only

# staging:
Protected: Yes
Allowed to deploy: Developers + Maintainers
Approval required: No
```

---

## Task 8.7: Compliance Frameworks ⭐

### Apply Compliance Framework

```yaml
# Group > Settings > General > Compliance frameworks

# Create framework:
Name: PCI DSS
Description: Payment Card Industry compliance
Color: #FF0000
Compliance pipeline: compliance/pci-dss-pipeline.yml

# Apply to projects:
# Project > Settings > General > Compliance framework
# Select: PCI DSS
```

### Compliance Pipeline

```yaml
# compliance/pci-dss-pipeline.yml
include:
  - template: Security/SAST.gitlab-ci.yml
  - template: Security/Dependency-Scanning.gitlab-ci.yml
  - template: Security/Container-Scanning.gitlab-ci.yml
  - template: Security/Secret-Detection.gitlab-ci.yml

# Additional compliance checks
pci-compliance-check:
  stage: .pre
  script:
    - ./check-pci-compliance.sh
  rules:
    - if: '$CI_COMMIT_BRANCH == "main"'
```

---

## Task 8.8: Audit Events ⭐

### Security Audit Events

```yaml
# Group > Security & Compliance > Audit Events

# Tracked events:
- Security policy changes
- Approval rule changes
- Protected branch changes
- User permission changes
- Security scan configuration changes
- Vulnerability status changes
- License policy changes

# Audit log entry:
Event: Security policy updated
User: @admin
Date: 2024-01-15 10:30:00
Details: Updated scan execution policy
IP Address: 192.168.1.100
```

---

## Task 8.9: Policy Management Best Practices ⭐⭐

### Best Practices

```yaml
# 1. Start with scan execution policies
# Enforce security scans on all branches

# 2. Gradually add scan result policies
# Start with critical only, expand to high

# 3. Use group-level policies
# Apply to all projects in group

# 4. Document policy decisions
# Why certain policies are in place

# 5. Regular policy reviews
# Update as threats evolve

# 6. Test policies in staging
# Before applying to production

# 7. Communicate policies
# Ensure team understands requirements

# 8. Monitor policy compliance
# Use compliance dashboard

# 9. Handle exceptions properly
# Document and approve exceptions

# 10. Integrate with workflow
# Don't block unnecessarily
```

---

## Task 8.10: Policy Enforcement ⭐

### Enforcement Levels

```yaml
# Level 1: Advisory (Soft)
# - Show warnings
# - Don't block MR
# - Track compliance

# Level 2: Blocking (Hard)
# - Block MR merge
# - Require approval
# - Enforce compliance

# Level 3: Preventive
# - Block pipeline
# - Fail early
# - Stop before deployment
```

### Example Enforcement

```yaml
# Soft enforcement (warning)
scan_result_policy:
  actions:
  - type: send_notification
    users:
    - @security-team

# Hard enforcement (blocking)
scan_result_policy:
  actions:
  - type: require_approval
    approvals_required: 1
    user_approvers:
    - security-team
```

---

## Exam Tips for Domain 8

1. **Policy Types** - Scan execution, scan result, approval
2. **Scan Execution** - Enforce which scans run
3. **Scan Result** - Block based on findings
4. **Approval Rules** - Require security team approval
5. **Protected Branches** - main, release branches
6. **Protected Environments** - production, staging
7. **Compliance Frameworks** - PCI DSS, HIPAA, SOC 2
8. **Audit Events** - Track security changes
9. **Best Practices** - Start simple, expand gradually
10. **Enforcement** - Advisory vs blocking

---

## Quick Reference

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
```

### Scan Result Policy

```yaml
scan_result_policy:
- name: Block Critical
  enabled: true
  rules:
  - type: scan_finding
    severity_levels: [critical]
    vulnerabilities_allowed: 0
  actions:
  - type: require_approval
    approvals_required: 1
```

### Protected Branch

```yaml
# Settings > Repository > Protected Branches
Branch: main
Merge: Maintainers only
Push: No one
Force push: No
Code owners: Required
```
