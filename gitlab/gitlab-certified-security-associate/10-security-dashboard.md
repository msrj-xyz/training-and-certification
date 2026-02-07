# Domain 10: Security Dashboard & Reporting

## Task 10.1: Security Dashboard Overview ⭐⭐

### Dashboard Levels

| Level | Scope | Access | Use Case |
|-------|-------|--------|----------|
| **Project** | Single project | Project members | Project security status |
| **Group** | All group projects | Group members | Organization overview |
| **Instance** | All projects | Admin only | Enterprise-wide view |

---

## Task 10.2: Project Security Dashboard ⭐⭐

### Access Project Dashboard

```yaml
# Navigate to:
Project > Security & Compliance > Vulnerability Report

# Shows:
- Total vulnerabilities
- Vulnerabilities by severity
- Vulnerabilities by scanner
- Vulnerability trends
- Recent activity
- Top vulnerable files
```

### Dashboard Widgets

```yaml
# Severity Breakdown
Critical: 2
High: 15
Medium: 45
Low: 23
Total: 85

# Scanner Coverage
✅ SAST: Enabled
✅ Dependency Scanning: Enabled
✅ Container Scanning: Enabled
✅ Secret Detection: Enabled
❌ DAST: Not configured

# Trend Chart
[Graph showing vulnerabilities over last 30 days]

# Recent Vulnerabilities
1. CVE-2024-1234 (Critical) - Detected 2 hours ago
2. CVE-2024-5678 (High) - Detected 1 day ago
3. CVE-2024-9012 (Medium) - Detected 3 days ago
```

---

## Task 10.3: Group Security Dashboard ⭐⭐

### Access Group Dashboard

```yaml
# Navigate to:
Group > Security & Compliance > Vulnerability Report

# Shows:
- Vulnerabilities across all projects
- Most vulnerable projects
- Scanner coverage by project
- Group-level trends
- Compliance status
```

### Group Metrics

```yaml
# Project Comparison
Project A: 45 vulnerabilities (12 critical)
Project B: 23 vulnerabilities (3 critical)
Project C: 67 vulnerabilities (8 critical)

# Scanner Adoption
SAST: 85% of projects
Dependency: 90% of projects
Container: 70% of projects
Secret: 95% of projects
DAST: 40% of projects

# Remediation Rate
Last 30 days:
- Detected: 234
- Resolved: 189
- Rate: 81%
```

---

## Task 10.4: Vulnerability Report Filters ⭐⭐

### Filter Options

```yaml
# Filter by Severity
- Critical
- High
- Medium
- Low
- Unknown

# Filter by Status
- Detected
- Confirmed
- Dismissed
- Resolved

# Filter by Scanner
- SAST
- DAST
- Dependency Scanning
- Container Scanning
- Secret Detection
- License Compliance

# Filter by Project
- Select specific projects
- Multi-select

# Filter by Activity
- Last 7 days
- Last 30 days
- Last 90 days
- Custom date range

# Search
- By CVE ID
- By package name
- By file path
- By description
```

---

## Task 10.5: Merge Request Security Widget ⭐⭐

### MR Security Tab

```yaml
# Merge Request > Security tab

# Shows:
New Vulnerabilities: 3
  - CVE-2024-1234 (High)
  - CVE-2024-5678 (Medium)
  - CVE-2024-9012 (Low)

Existing Vulnerabilities: 12
  - No change from target branch

Resolved Vulnerabilities: 2
  - CVE-2023-1111 (High) - Fixed
  - CVE-2023-2222 (Medium) - Fixed

# Actions:
- View details
- Dismiss vulnerability
- Create issue
- Block merge (if policy)
```

### Security Approval

```yaml
# If scan result policy active:
⚠️ Security Approval Required
- 1 critical vulnerability detected
- Requires approval from: @security-team
- Policy: Block Critical Vulnerabilities

# Approver actions:
- Review vulnerability
- Approve (accept risk)
- Request changes (fix required)
```

---

## Task 10.6: Pipeline Security Tab ⭐

### Pipeline Security View

```yaml
# Pipeline > Security tab

# Security Jobs Status
✅ secret_detection: Passed (0 findings)
✅ sast: Passed (2 findings)
⚠️ dependency_scanning: Warning (5 findings)
❌ container_scanning: Failed (1 critical finding)

# Findings Summary
Critical: 1
High: 3
Medium: 2
Low: 1

# Download Reports
- SAST report (JSON)
- Dependency report (JSON)
- Container report (JSON)
- Combined report (JSON)
```

---

## Task 10.7: Security Metrics ⭐⭐

### Key Metrics

```yaml
# Vulnerability Metrics
- Total vulnerabilities
- Vulnerabilities by severity
- New vulnerabilities (last 30 days)
- Resolved vulnerabilities (last 30 days)
- Vulnerability backlog
- Mean time to detect (MTTD)
- Mean time to remediate (MTTR)

# Scanner Metrics
- Scanner coverage (% of projects)
- Scan frequency
- Scan success rate
- False positive rate
- Scanner performance (duration)

# Remediation Metrics
- Remediation rate
- SLA compliance
- Overdue vulnerabilities
- Vulnerabilities by age

# Compliance Metrics
- Policy compliance rate
- License violations
- Audit findings
- Security score
```

---

## Task 10.8: Export and Reporting ⭐

### Export Options

```yaml
# Export Vulnerability Report
# Security & Compliance > Vulnerability Report
# Click "Export"

# Formats:
- CSV (spreadsheet)
- JSON (API integration)
- PDF (executive report)

# CSV includes:
- Vulnerability ID
- Title
- Severity
- Status
- Scanner
- Location
- Detected date
- Resolved date
- Assignee
- Description
- CVE/CWE
```

### API Access

```bash
# Get vulnerabilities via API
curl --header "PRIVATE-TOKEN: <token>" \
  "https://gitlab.com/api/v4/projects/:id/vulnerabilities"

# Get vulnerability findings
curl --header "PRIVATE-TOKEN: <token>" \
  "https://gitlab.com/api/v4/projects/:id/vulnerability_findings"

# Get security dashboard
curl --header "PRIVATE-TOKEN: <token>" \
  "https://gitlab.com/api/v4/groups/:id/vulnerability_exports"
```

---

## Task 10.9: Compliance Dashboard ⭐

### Compliance View

```yaml
# Group > Security & Compliance > Compliance

# Shows:
- Compliance framework status
- Policy violations
- Audit events
- License compliance
- Merge request approvals
- Protected branches
- Security scan coverage

# Compliance Score
Overall: 85%
- Security scans: 90%
- License compliance: 95%
- Policy adherence: 80%
- Approval rules: 75%
```

---

## Task 10.10: Custom Dashboards ⭐

### Create Custom Dashboard

```yaml
# Use GitLab API + BI tools
# - Grafana
# - Tableau
# - Power BI
# - Custom web app

# Example metrics:
- Vulnerability trends
- Scanner adoption
- Remediation velocity
- Security score
- Compliance status
- Team performance
```

### Grafana Integration

```yaml
# Grafana dashboard panels:
1. Total Vulnerabilities (gauge)
2. Vulnerabilities by Severity (pie chart)
3. Vulnerability Trend (line chart)
4. MTTR by Severity (bar chart)
5. Scanner Coverage (gauge)
6. Top Vulnerable Projects (table)
7. Remediation Rate (gauge)
8. SLA Compliance (gauge)
```

---

## Exam Tips for Domain 10

1. **Dashboard Levels** - Project, Group, Instance
2. **Project Dashboard** - Single project view
3. **Group Dashboard** - Multi-project view
4. **Filters** - Severity, status, scanner, project
5. **MR Widget** - New, existing, resolved vulnerabilities
6. **Pipeline Tab** - Security job status and findings
7. **Metrics** - MTTD, MTTR, remediation rate
8. **Export** - CSV, JSON, PDF formats
9. **Compliance** - Framework status, violations
10. **API** - Programmatic access to security data

---

## Quick Reference

### Access Dashboards

```yaml
# Project Dashboard
Project > Security & Compliance > Vulnerability Report

# Group Dashboard
Group > Security & Compliance > Vulnerability Report

# MR Security
Merge Request > Security tab

# Pipeline Security
Pipeline > Security tab
```

### Key Metrics

```yaml
# Vulnerability Metrics
- Total: Count of all vulnerabilities
- MTTD: Mean time to detect
- MTTR: Mean time to remediate
- Backlog: Unresolved vulnerabilities

# Scanner Metrics
- Coverage: % of projects with scanners
- Success rate: % of successful scans
- False positive rate: % of false positives
```

### Export Report

```yaml
# Via UI
Security & Compliance > Vulnerability Report > Export

# Via API
curl --header "PRIVATE-TOKEN: <token>" \
  "https://gitlab.com/api/v4/projects/:id/vulnerabilities"
```
