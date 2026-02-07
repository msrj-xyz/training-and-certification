# Domain 4: Dependency Scanning

## Task 4.1: Dependency Scanning Overview ⭐⭐

### What is Dependency Scanning?

**Dependency Scanning** analyzes project dependencies for known security vulnerabilities using CVE databases.

| Aspect | Details |
|--------|---------|
| **Type** | Composition analysis (SCA) |
| **When** | During build stage |
| **Input** | Dependency manifests |
| **Output** | Vulnerability report (JSON) |
| **Speed** | Fast (seconds to minutes) |
| **Coverage** | Third-party dependencies |

### Why Dependency Scanning?

```yaml
# Modern applications use many dependencies
- Average app: 200+ dependencies
- Transitive dependencies: 1000+
- Known vulnerabilities: Growing daily
- Supply chain attacks: Increasing

# Examples of vulnerable dependencies:
- Log4Shell (CVE-2021-44228) - Critical
- Spring4Shell (CVE-2022-22965) - Critical
- Heartbleed (CVE-2014-0160) - Critical
```

---

## Task 4.2: Enable Dependency Scanning ⭐⭐

### Basic Configuration

```yaml
include:
  - template: Security/Dependency-Scanning.gitlab-ci.yml

# Auto-detects package managers:
# - npm (package.json, package-lock.json)
# - pip (requirements.txt, Pipfile)
# - bundler (Gemfile, Gemfile.lock)
# - maven (pom.xml)
# - gradle (build.gradle)
# - composer (composer.json, composer.lock)
# - go modules (go.mod, go.sum)
# - NuGet (.csproj, packages.config)
```

### Custom Configuration

```yaml
include:
  - template: Security/Dependency-Scanning.gitlab-ci.yml

variables:
  # Exclude paths
  DS_EXCLUDED_PATHS: "spec, test, tests, tmp"
  
  # Exclude analyzers
  DS_EXCLUDED_ANALYZERS: "gemnasium, gemnasium-maven"
  
  # Default analyzers
  DS_DEFAULT_ANALYZERS: "gemnasium, gemnasium-maven, gemnasium-python, bundler-audit, retire.js"
  
  # Remediation
  DS_REMEDIATE: "true"  # Auto-create MR for fixes
  
  # Disable dependency scanning
  DS_DISABLED: "false"
```

---

## Task 4.3: Supported Package Managers ⭐⭐

### Package Manager Support

| Language | Package Manager | Files | Analyzer |
|----------|----------------|-------|----------|
| **JavaScript** | npm, yarn | package.json, package-lock.json, yarn.lock | Gemnasium, Retire.js |
| **Python** | pip, pipenv, poetry | requirements.txt, Pipfile, poetry.lock | Gemnasium-Python |
| **Ruby** | bundler | Gemfile, Gemfile.lock | Bundler-Audit, Gemnasium |
| **Java** | maven, gradle | pom.xml, build.gradle | Gemnasium-Maven |
| **PHP** | composer | composer.json, composer.lock | Gemnasium |
| **Go** | go modules | go.mod, go.sum | Gemnasium |
| **C#** | NuGet | .csproj, packages.config | Gemnasium |
| **Scala** | sbt | build.sbt | Gemnasium-Maven |

### JavaScript/Node.js

```yaml
# package.json
{
  "dependencies": {
    "express": "4.17.1",
    "lodash": "4.17.20"  # Vulnerable version
  }
}

# Dependency scanning detects:
- CVE-2021-23337 in lodash
- Severity: High
- Fixed in: 4.17.21
```

### Python

```yaml
# requirements.txt
Django==2.2.0  # Vulnerable version
requests==2.25.1

# Dependency scanning detects:
- CVE-2021-33203 in Django
- Severity: Medium
- Fixed in: 2.2.24
```

### Ruby

```yaml
# Gemfile
gem 'rails', '5.2.0'  # Vulnerable version
gem 'nokogiri', '1.10.0'

# Dependency scanning detects:
- CVE-2020-8164 in Rails
- Severity: High
- Fixed in: 5.2.4.3
```

### Java

```yaml
# pom.xml
<dependency>
  <groupId>org.apache.logging.log4j</groupId>
  <artifactId>log4j-core</artifactId>
  <version>2.14.0</version>  <!-- Vulnerable -->
</dependency>

# Dependency scanning detects:
- CVE-2021-44228 (Log4Shell)
- Severity: Critical
- Fixed in: 2.17.1
```

---

## Task 4.4: Dependency Scanning Analyzers ⭐

### Gemnasium

```yaml
# Universal dependency scanner
# Supports: npm, pip, bundler, maven, gradle, composer, go, NuGet

# Uses GitLab Advisory Database
# - 100,000+ vulnerabilities
# - Updated daily
# - Covers all major ecosystems
```

### Bundler-Audit

```yaml
# Ruby-specific scanner
# Uses Ruby Advisory Database
# Checks Gemfile.lock

# Enable:
include:
  - template: Security/Dependency-Scanning.gitlab-ci.yml

# Automatically runs for Ruby projects
```

### Retire.js

```yaml
# JavaScript-specific scanner
# Focuses on frontend libraries
# Checks for outdated JS libraries

# Detects vulnerabilities in:
- jQuery
- Angular
- React
- Vue
- Bootstrap
```

---

## Task 4.5: Vulnerability Remediation ⭐⭐

### Manual Remediation

```yaml
# 1. View vulnerability in Security Dashboard
# Security & Compliance > Vulnerability Report

# 2. Check fix version
Vulnerability: CVE-2021-23337
Package: lodash
Current: 4.17.20
Fixed in: 4.17.21

# 3. Update dependency
# package.json
{
  "dependencies": {
    "lodash": "4.17.21"  # Updated
  }
}

# 4. Test and commit
npm install
npm test
git commit -am "fix: update lodash to 4.17.21"
```

### Auto-Remediation

```yaml
include:
  - template: Security/Dependency-Scanning.gitlab-ci.yml

variables:
  DS_REMEDIATE: "true"

# GitLab automatically:
# 1. Detects vulnerable dependencies
# 2. Checks for fixed versions
# 3. Creates merge request with updates
# 4. Runs tests
# 5. Assigns to maintainer
```

### Remediation Strategies

```yaml
# Strategy 1: Update to fixed version
lodash: 4.17.20 → 4.17.21

# Strategy 2: Update to latest
lodash: 4.17.20 → 4.17.21 (latest)

# Strategy 3: Replace with alternative
moment → date-fns (more secure)

# Strategy 4: Remove if unused
# Check if dependency is actually used

# Strategy 5: Accept risk (temporary)
# Document reason and plan
```

---

## Task 4.6: Transitive Dependencies ⭐

### Understanding Transitive Dependencies

```yaml
# Direct dependency
package.json:
  express: 4.17.1

# Transitive dependencies (express depends on):
  body-parser: 1.19.0
  cookie: 0.4.0
  debug: 2.6.9
  ...and 30+ more

# Vulnerability in transitive dependency:
  debug: 2.6.9 (vulnerable)
  
# Solution:
# Update express to version that uses fixed debug
```

### Dependency Tree

```bash
# View dependency tree
npm list
yarn list
pip show <package>
bundle show <gem>
mvn dependency:tree

# Example output:
myapp@1.0.0
├── express@4.17.1
│   ├── body-parser@1.19.0
│   ├── debug@2.6.9  ← Vulnerable
│   └── ...
```

---

## Task 4.7: License Compliance Integration ⭐

### Dependency Scanning + License Compliance

```yaml
# Enable both
include:
  - template: Security/Dependency-Scanning.gitlab-ci.yml
  - template: Security/License-Scanning.gitlab-ci.yml

# Detects:
# 1. Security vulnerabilities
# 2. License violations

# Example:
Package: some-library
Vulnerability: CVE-2021-12345 (High)
License: GPL-3.0 (Denied)

# Action: Find alternative package
```

---

## Task 4.8: False Positives and Exceptions ⭐

### Handling False Positives

```yaml
# 1. Dismiss in UI
# Security & Compliance > Vulnerability Report
# Click vulnerability > Dismiss > Select reason:
# - False positive
# - Used in tests only
# - Protected by other controls
# - Risk accepted

# 2. Exclude specific advisories
# Create .gitlab/dependency-scanning-exceptions.yml
exceptions:
  - advisory_id: "CVE-2021-12345"
    reason: "False positive - not exploitable in our context"
    expiry: "2024-12-31"
```

### Vulnerability Allowlist

```yaml
# .gitlab/dependency-scanning-allowlist.yml
allowlist:
  - name: "lodash"
    version: "4.17.20"
    cve: "CVE-2021-23337"
    reason: "Mitigated by input validation"
    expires: "2024-06-30"
```

---

## Task 4.9: Dependency Scanning Best Practices ⭐⭐

### Best Practices

```yaml
# 1. Enable dependency scanning early
include:
  - template: Security/Dependency-Scanning.gitlab-ci.yml

# 2. Keep dependencies updated
# Use Dependabot or Renovate

# 3. Use lock files
# - package-lock.json (npm)
# - yarn.lock (yarn)
# - Gemfile.lock (bundler)
# - poetry.lock (poetry)

# 4. Review dependencies before adding
# Check:
# - Maintenance status
# - Security history
# - License
# - Alternatives

# 5. Minimize dependencies
# Remove unused dependencies

# 6. Pin versions
# Avoid wildcards: "^1.0.0" or "*"
# Use exact: "1.0.0"

# 7. Monitor continuously
# Run on every commit

# 8. Automate updates
variables:
  DS_REMEDIATE: "true"

# 9. Review transitive dependencies
npm audit
yarn audit
pip-audit

# 10. Use private registries
# For internal packages
```

### Dependency Management Tools

```yaml
# Dependabot (GitHub)
# - Automated dependency updates
# - Security alerts
# - Version updates

# Renovate (Multi-platform)
# - Automated updates
# - Customizable rules
# - Supports many package managers

# Snyk
# - Vulnerability scanning
# - Fix recommendations
# - License compliance

# WhiteSource
# - Open source management
# - License compliance
# - Security scanning
```

---

## Task 4.10: Common Vulnerable Dependencies ⭐⭐

### Critical Vulnerabilities

```yaml
# Log4Shell (CVE-2021-44228)
Package: log4j-core
Versions: 2.0-beta9 to 2.14.1
Severity: Critical (10.0)
Impact: Remote code execution
Fix: Update to 2.17.1+

# Spring4Shell (CVE-2022-22965)
Package: spring-beans
Versions: 5.3.0 to 5.3.17, 5.2.0 to 5.2.19
Severity: Critical (9.8)
Impact: Remote code execution
Fix: Update to 5.3.18+ or 5.2.20+

# Prototype Pollution (lodash)
Package: lodash
Versions: < 4.17.21
Severity: High
Impact: Prototype pollution
Fix: Update to 4.17.21+

# SQL Injection (Sequelize)
Package: sequelize
Versions: < 6.3.5
Severity: High
Impact: SQL injection
Fix: Update to 6.3.5+

# Path Traversal (express)
Package: express
Versions: < 4.17.3
Severity: Medium
Impact: Path traversal
Fix: Update to 4.17.3+
```

---

## Exam Tips for Domain 4

1. **Definition** - Scans dependencies for known vulnerabilities
2. **Enable** - Include Security/Dependency-Scanning.gitlab-ci.yml
3. **Package Managers** - npm, pip, bundler, maven, gradle, composer, go
4. **Analyzers** - Gemnasium, Bundler-Audit, Retire.js
5. **Remediation** - Manual update or DS_REMEDIATE=true
6. **Transitive** - Understand indirect dependencies
7. **Lock Files** - package-lock.json, Gemfile.lock, etc.
8. **False Positives** - Dismiss in UI or use allowlist
9. **Best Practices** - Keep updated, use lock files, minimize deps
10. **Critical CVEs** - Log4Shell, Spring4Shell, prototype pollution

---

## Quick Reference

### Enable Dependency Scanning

```yaml
include:
  - template: Security/Dependency-Scanning.gitlab-ci.yml
```

### Auto-Remediation

```yaml
variables:
  DS_REMEDIATE: "true"
```

### Exclude Paths

```yaml
variables:
  DS_EXCLUDED_PATHS: "spec, test, tests, tmp"
```

### Check Dependencies

```bash
# npm
npm audit
npm audit fix

# yarn
yarn audit
yarn audit fix

# pip
pip-audit

# bundler
bundle audit
bundle audit update

# maven
mvn dependency-check:check
```

### Update Dependencies

```bash
# npm
npm update
npm install lodash@latest

# pip
pip install --upgrade package

# bundler
bundle update gem-name

# maven
mvn versions:use-latest-versions
```
