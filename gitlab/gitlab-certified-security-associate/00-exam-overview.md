# GitLab Certified Security Associate

## Exam Information

| Aspect | Details |
|--------|---------|
| **Exam Code** | GitLab Certified Security Associate |
| **Level** | Associate |
| **Duration** | Part 1: 60 minutes (Written) + Part 2: 120 minutes (Hands-on) |
| **Questions** | Part 1: 14 multiple-choice + Part 2: 14 hands-on tasks |
| **Passing Score** | Part 1: 80% (to proceed to Part 2) |
| **Format** | Multiple-choice + Hands-on lab |
| **Cost** | Varies by region |
| **Validity** | 2 years |
| **Retakes** | Part 1: Unlimited retakes, Part 2: Limited |

---

## Exam Structure

### Part 1: Written Assessment (60 minutes)
- 14 multiple-choice questions
- 80% accuracy required to proceed
- Can be retaken indefinitely
- Tests theoretical knowledge of GitLab security concepts

### Part 2: Hands-on Lab (120 minutes)
- 14 practical security tasks
- Performed on a GitLab project you create
- Graded by GitLab Professional Services engineers
- Tests real-world security implementation skills

---

## Domain Weightings

| Domain | Focus Areas |
|--------|-------------|
| **Domain 1** | Security Fundamentals & DevSecOps |
| **Domain 2** | Static Application Security Testing (SAST) |
| **Domain 3** | Dynamic Application Security Testing (DAST) |
| **Domain 4** | Dependency Scanning |
| **Domain 5** | Container Scanning |
| **Domain 6** | Secret Detection |
| **Domain 7** | License Compliance |
| **Domain 8** | Security Policies & Governance |
| **Domain 9** | Vulnerability Management |
| **Domain 10** | Security Dashboard & Reporting |

---

## Target Candidate Profile

- Security engineers and DevSecOps practitioners
- Application security specialists
- DevOps engineers with security focus
- Security analysts working with CI/CD
- Compliance and governance professionals
- Anyone responsible for application security in GitLab

---

## Prerequisites

### Required Knowledge
- Basic understanding of application security concepts
- Familiarity with GitLab CI/CD pipelines
- Understanding of software development lifecycle
- Basic knowledge of security vulnerabilities (OWASP Top 10)
- Understanding of containerization and Docker

### Recommended Experience
- 6+ months working with GitLab
- Experience with security scanning tools
- Understanding of DevSecOps principles
- Familiarity with security compliance requirements
- Basic knowledge of security policies

---

## Exam Environment

### Technical Requirements
| Aspect | Details |
|--------|---------|
| **Platform** | GitLab.com or self-managed instance |
| **Browser** | Modern web browser (Chrome, Firefox, Edge) |
| **Access** | GitLab account with project creation rights |
| **Tools** | Git CLI, security scanning tools |
| **Internet** | Stable connection required |

### Pre-configured Features
- GitLab project with CI/CD enabled
- Access to GitLab security features
- Security scanning templates
- Sample vulnerable applications

---

## Resources Allowed During Exam

| Resource | URL |
|----------|-----|
| **GitLab Docs** | https://docs.gitlab.com |
| **Security Docs** | https://docs.gitlab.com/ee/user/application_security/ |
| **SAST** | https://docs.gitlab.com/ee/user/application_security/sast/ |
| **DAST** | https://docs.gitlab.com/ee/user/application_security/dast/ |
| **Dependency Scanning** | https://docs.gitlab.com/ee/user/application_security/dependency_scanning/ |
| **Container Scanning** | https://docs.gitlab.com/ee/user/application_security/container_scanning/ |
| **Secret Detection** | https://docs.gitlab.com/ee/user/application_security/secret_detection/ |

> **Exam Tip:** Bookmark key security documentation pages before the exam!

---

## Key Topics to Master

### Security Scanning
- SAST (Static Application Security Testing)
- DAST (Dynamic Application Security Testing)
- Dependency Scanning
- Container Scanning
- Secret Detection
- License Compliance
- IaC Scanning
- API Security Testing
- Coverage Fuzzing

### Security Configuration
- Security templates inclusion
- Scanner configuration and customization
- Security variables
- Analyzer selection
- Scan result interpretation
- False positive management

### Vulnerability Management
- Security Dashboard
- Vulnerability Report
- Merge Request Security Widget
- Vulnerability status management
- Issue creation from vulnerabilities
- Remediation workflows

### Security Policies
- Scan Execution Policies
- Scan Result Policies
- Approval rules for security
- Protected branches and environments
- Compliance frameworks
- Policy management

### Security Reporting
- Security reports in pipelines
- Merge request security tab
- Project security dashboard
- Group security dashboard
- Vulnerability trends
- Compliance reports

---

## Exam Day Preparation

### Before the Exam (Part 1)
1. Review GitLab security documentation
2. Understand all security scanner types
3. Know security template inclusion
4. Review OWASP Top 10 vulnerabilities
5. Understand security policies
6. Prepare stable internet connection

### Before the Exam (Part 2)
1. Create a GitLab project for testing
2. Practice enabling security scanners
3. Test vulnerable applications
4. Verify security dashboard access
5. Practice vulnerability management
6. Have documentation bookmarks ready

### During the Exam
1. Read each question carefully
2. Use GitLab documentation when needed
3. Test security configurations
4. Check pipeline security reports
5. Validate vulnerability detection
6. Review security dashboard
7. Verify policy enforcement

---

## Practice Resources

### GitLab Learn Platform
- Official GitLab Security Essentials training
- Hands-on security labs
- Video tutorials on security features
- Practice vulnerable applications

### Self-Study
- Enable all security scanners
- Practice with OWASP WebGoat
- Test different vulnerability types
- Configure security policies
- Practice vulnerability remediation
- Test compliance frameworks

### Vulnerable Applications for Practice
- OWASP WebGoat
- DVWA (Damn Vulnerable Web Application)
- Juice Shop
- NodeGoat
- RailsGoat

---

## Exam Tips Summary

1. **Know all scanner types** - SAST, DAST, Dependency, Container, Secret
2. **Master template inclusion** - Security/SAST.gitlab-ci.yml, etc.
3. **Understand scanner configuration** - Variables, analyzers, exclusions
4. **Know vulnerability severities** - Critical, High, Medium, Low, Unknown
5. **Practice vulnerability management** - Status, assignee, issue creation
6. **Understand security policies** - Scan execution, scan result policies
7. **Know security dashboard** - Project, group, vulnerability report
8. **Master false positive handling** - Dismiss, confirm, resolve
9. **Understand compliance** - Frameworks, audit events, compliance dashboard
10. **Practice hands-on** - Enable scanners, fix vulnerabilities, configure policies

---

## Common Pitfalls to Avoid

| Issue | Solution |
|-------|----------|
| **Scanner not running** | Check template inclusion and runner availability |
| **No vulnerabilities detected** | Verify scanner configuration and test with known vulnerable code |
| **DAST requires deployed app** | Ensure application is deployed and accessible |
| **Container scanning needs image** | Build and push Docker image first |
| **Secret detection false positives** | Use allowlist to exclude test secrets |
| **License compliance issues** | Configure approved/denied licenses |
| **Policy not enforcing** | Check policy scope and conditions |
| **Security report not showing** | Verify artifact report format |

---

## Security Scanner Overview

### SAST (Static Application Security Testing)
- **Purpose:** Analyze source code for vulnerabilities
- **When:** During build/test stage
- **Languages:** Java, JavaScript, Python, Go, Ruby, PHP, C/C++, C#, etc.
- **Detects:** SQL injection, XSS, code injection, insecure configurations

### DAST (Dynamic Application Security Testing)
- **Purpose:** Test running application for vulnerabilities
- **When:** After deployment
- **Requirements:** Deployed application URL
- **Detects:** Runtime vulnerabilities, authentication issues, API vulnerabilities

### Dependency Scanning
- **Purpose:** Scan dependencies for known vulnerabilities
- **When:** During build stage
- **Package Managers:** npm, pip, bundler, maven, gradle, composer, go modules
- **Detects:** Vulnerable dependencies, outdated packages

### Container Scanning
- **Purpose:** Scan Docker images for vulnerabilities
- **When:** After image build
- **Requirements:** Built Docker image
- **Detects:** OS vulnerabilities, package vulnerabilities

### Secret Detection
- **Purpose:** Detect exposed secrets in code
- **When:** During commit/push
- **Scope:** Source code, commit history
- **Detects:** API keys, passwords, tokens, certificates

### License Compliance
- **Purpose:** Detect license violations
- **When:** During build stage
- **Scope:** Project dependencies
- **Detects:** Unapproved licenses, license conflicts

---

## Post-Certification

### Badge and Recognition
- Digital badge from GitLab
- LinkedIn certification display
- GitLab Certified Security Associate title
- Valid for 2 years

### Career Benefits
- Validates GitLab security expertise
- Demonstrates DevSecOps proficiency
- Enhances security career opportunities
- Supports team security posture

---

## Cheatsheet Files

- [Domain 1: Security Fundamentals & DevSecOps](./01-security-fundamentals.md)
- [Domain 2: SAST (Static Application Security Testing)](./02-sast.md)
- [Domain 3: DAST (Dynamic Application Security Testing)](./03-dast.md)
- [Domain 4: Dependency Scanning](./04-dependency-scanning.md)
- [Domain 5: Container Scanning](./05-container-scanning.md)
- [Domain 6: Secret Detection](./06-secret-detection.md)
- [Domain 7: License Compliance](./07-license-compliance.md)
- [Domain 8: Security Policies & Governance](./08-security-policies.md)
- [Domain 9: Vulnerability Management](./09-vulnerability-management.md)
- [Domain 10: Security Dashboard & Reporting](./10-security-dashboard.md)
- [**Quick Reference Card (1-page summary)**](./11-quick-reference-card.md)

---

## Additional Resources

- [GitLab Security Documentation](https://docs.gitlab.com/ee/user/application_security/)
- [GitLab Security Essentials Training](https://about.gitlab.com/services/education/gitlab-security-essentials/)
- [OWASP Top 10](https://owasp.org/www-project-top-ten/)
- [CWE (Common Weakness Enumeration)](https://cwe.mitre.org/)
- [CVE (Common Vulnerabilities and Exposures)](https://cve.mitre.org/)
- [GitLab Security Blog](https://about.gitlab.com/blog/categories/security/)

---

**Good luck with your certification! 🔒**

*Remember: Security is everyone's responsibility. The more you practice with GitLab security features, the more confident you'll become in securing your applications.*

---

**Last Updated:** February 2026  
**Version:** 1.0  
**Maintainer:** Senior GitLab Security Instructor (10+ years experience)
