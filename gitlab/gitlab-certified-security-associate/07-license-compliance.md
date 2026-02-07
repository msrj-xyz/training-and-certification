# Domain 7: License Compliance

## Task 7.1: License Compliance Overview ⭐⭐

### What is License Compliance?

**License Compliance** scans project dependencies to detect license violations and ensure compliance with organizational policies.

| Aspect | Details |
|--------|---------|
| **Type** | License analysis |
| **When** | During build stage |
| **Input** | Dependency manifests |
| **Output** | License report (JSON) |
| **Speed** | Fast (seconds) |
| **Coverage** | All dependencies |

---

## Task 7.2: Enable License Compliance ⭐⭐

### Basic Configuration

```yaml
include:
  - template: Security/License-Scanning.gitlab-ci.yml

# Auto-detects package managers:
# - npm, yarn (JavaScript)
# - pip, pipenv (Python)
# - bundler (Ruby)
# - maven, gradle (Java)
# - composer (PHP)
# - go modules (Go)
```

### Custom Configuration

```yaml
include:
  - template: Security/License-Scanning.gitlab-ci.yml

variables:
  # Exclude paths
  LICENSE_FINDER_CLI_OPTS: "--decisions-file=license-decisions.yml"
  
  # Disable license scanning
  LICENSE_MANAGEMENT_DISABLED: "false"
```

---

## Task 7.3: Common Software Licenses ⭐⭐

### License Categories

| Category | Examples | Commercial Use | Modifications | Distribution |
|----------|----------|----------------|---------------|--------------|
| **Permissive** | MIT, Apache-2.0, BSD | ✅ Yes | ✅ Yes | ✅ Yes |
| **Copyleft** | GPL-3.0, AGPL-3.0 | ⚠️ Conditions | ✅ Yes | ⚠️ Must share source |
| **Weak Copyleft** | LGPL, MPL | ✅ Yes | ✅ Yes | ⚠️ Partial |
| **Proprietary** | Commercial | ❌ License required | ❌ No | ❌ No |
| **Public Domain** | Unlicense, CC0 | ✅ Yes | ✅ Yes | ✅ Yes |

### Popular Licenses

```yaml
# Permissive (Business-friendly)
MIT License:
  - Very permissive
  - Commercial use allowed
  - Minimal restrictions
  - Most popular

Apache License 2.0:
  - Patent grant included
  - Commercial use allowed
  - Trademark protection

BSD Licenses (2-Clause, 3-Clause):
  - Very permissive
  - Commercial use allowed
  - Simple terms

# Copyleft (Restrictive)
GPL-3.0:
  - Strong copyleft
  - Derivative works must be GPL
  - Source code must be shared

AGPL-3.0:
  - Strongest copyleft
  - Network use = distribution
  - SaaS must share source

LGPL-3.0:
  - Weak copyleft
  - Can link with proprietary
  - Library-focused

# Other
MPL-2.0:
  - File-level copyleft
  - Can mix with proprietary
  - Mozilla Public License
```

---

## Task 7.4: License Policies ⭐⭐

### Configure License Policies

```yaml
# Settings > Security & Compliance > License Compliance

# Approved Licenses (Allow)
✅ MIT
✅ Apache-2.0
✅ BSD-2-Clause
✅ BSD-3-Clause
✅ ISC

# Denied Licenses (Block)
❌ GPL-3.0
❌ AGPL-3.0
❌ SSPL
❌ Commons Clause

# Requires Review
⚠️ LGPL-3.0
⚠️ MPL-2.0
⚠️ EPL-2.0
```

### License Decision File

```yaml
# license-decisions.yml
---
- - :who: "Security Team"
    :why: "Approved for use"
    :versions: []
    :when: 2024-01-15
  - :license: "MIT"
    :approval: "approved"

- - :who: "Legal Team"
    :why: "Copyleft restrictions"
    :versions: []
    :when: 2024-01-15
  - :license: "GPL-3.0"
    :approval: "denied"

- - :who: "Security Team"
    :why: "Requires legal review"
    :versions: []
    :when: 2024-01-15
  - :license: "LGPL-3.0"
    :approval: "review"
```

---

## Task 7.5: License Compliance Reports ⭐

### Report Format

```json
{
  "version": "2.1",
  "licenses": [
    {
      "id": "MIT",
      "name": "MIT License",
      "url": "https://opensource.org/licenses/MIT"
    }
  ],
  "dependencies": [
    {
      "name": "express",
      "version": "4.18.2",
      "package_manager": "npm",
      "path": "package.json",
      "licenses": [
        {
          "id": "MIT",
          "name": "MIT License",
          "url": "https://opensource.org/licenses/MIT"
        }
      ]
    },
    {
      "name": "some-gpl-package",
      "version": "1.0.0",
      "package_manager": "npm",
      "path": "package.json",
      "licenses": [
        {
          "id": "GPL-3.0",
          "name": "GNU General Public License v3.0",
          "url": "https://www.gnu.org/licenses/gpl-3.0.html"
        }
      ]
    }
  ]
}
```

---

## Task 7.6: License Compliance in MR ⭐

### Merge Request Widget

```yaml
# License compliance widget shows:
- New licenses introduced
- Denied licenses detected
- License policy violations
- Requires approval if violations

# Example MR widget:
⚠️ License Compliance
- 1 new license detected: GPL-3.0
- Policy violation: GPL-3.0 is denied
- Action required: Remove dependency or get approval
```

---

## Task 7.7: Handling License Violations ⭐⭐

### Resolution Strategies

```yaml
# Strategy 1: Replace with alternative
# GPL-3.0 package → MIT alternative
npm uninstall gpl-package
npm install mit-alternative

# Strategy 2: Get legal approval
# Document exception
# Update license policy
# Add to approved list

# Strategy 3: Dual licensing
# Some packages offer multiple licenses
# Choose compatible license

# Strategy 4: Remove dependency
# If not critical, remove it
npm uninstall problematic-package

# Strategy 5: Vendor the code
# Copy code into project
# Comply with license terms
# Maintain separately
```

---

## Task 7.8: License Compliance Best Practices ⭐

### Best Practices

```yaml
# 1. Define license policy early
# Document approved/denied licenses

# 2. Review licenses before adding dependencies
npm info package license
pip show package | grep License

# 3. Enable license scanning in CI/CD
include:
  - template: Security/License-Scanning.gitlab-ci.yml

# 4. Block MR with violations
# Settings > Merge Requests > Merge checks

# 5. Regular license audits
# Review license compliance dashboard

# 6. Educate developers
# Share license policy
# Explain implications

# 7. Use SBOM (Software Bill of Materials)
# Track all dependencies
# Document licenses

# 8. Monitor license changes
# Dependencies can change licenses
# Stay updated
```

---

## Task 7.9: SBOM (Software Bill of Materials) ⭐

### Generate SBOM

```yaml
# CycloneDX SBOM
sbom:
  stage: build
  image: cyclonedx/cyclonedx-cli
  script:
    - cyclonedx-cli generate -o sbom.json
  artifacts:
    paths:
      - sbom.json
    reports:
      cyclonedx: sbom.json
```

### SBOM Benefits

```yaml
# SBOM provides:
- Complete dependency list
- License information
- Vulnerability tracking
- Supply chain transparency
- Compliance evidence

# Use cases:
- Security audits
- License compliance
- Vendor requirements
- Regulatory compliance
```

---

## Task 7.10: Common License Issues ⭐

### License Conflicts

```yaml
# Scenario: Mixing incompatible licenses
Project License: Apache-2.0
Dependency: GPL-3.0

# Problem:
# - GPL-3.0 requires derivative works to be GPL
# - Apache-2.0 is incompatible with GPL-3.0
# - Cannot distribute combined work

# Solution:
# - Replace GPL dependency
# - Change project license to GPL
# - Get legal advice
```

### Dual Licensing

```yaml
# Some packages offer multiple licenses
Package: some-library
Licenses:
  - GPL-3.0 (free, copyleft)
  - Commercial (paid, proprietary)

# Choose based on needs:
# - GPL-3.0: If project is open source
# - Commercial: If project is proprietary
```

---

## Exam Tips for Domain 7

1. **Definition** - Scans dependencies for license violations
2. **Enable** - Include Security/License-Scanning.gitlab-ci.yml
3. **Categories** - Permissive, copyleft, weak copyleft
4. **Common Licenses** - MIT, Apache-2.0, GPL-3.0, AGPL-3.0
5. **Policies** - Approved, denied, requires review
6. **MR Widget** - Shows new licenses and violations
7. **Resolution** - Replace, approve, remove, vendor
8. **Best Practices** - Define policy, review before adding
9. **SBOM** - Software Bill of Materials
10. **Conflicts** - GPL incompatible with many licenses

---

## Quick Reference

### Enable License Compliance

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

### Check License

```bash
# npm
npm info package license

# pip
pip show package | grep License

# bundler
bundle info gem-name

# maven
mvn dependency:tree
```
