# Domain 3: DAST (Dynamic Application Security Testing)

## Task 3.1: DAST Overview ⭐⭐

### What is DAST?

**Dynamic Application Security Testing (DAST)** tests running applications by simulating attacks to identify runtime vulnerabilities.

| Aspect | Details |
|--------|---------|
| **Type** | Black-box testing |
| **When** | After deployment |
| **Input** | Running application URL |
| **Output** | Vulnerability report (JSON) |
| **Speed** | Slow (hours) |
| **Coverage** | Runtime vulnerabilities |

### DAST vs SAST

| Feature | SAST | DAST |
|---------|------|------|
| **Testing Type** | White-box | Black-box |
| **Requires** | Source code | Running app |
| **Stage** | Build/Test | Post-deploy |
| **Speed** | Fast | Slow |
| **Detects** | Code vulnerabilities | Runtime vulnerabilities |
| **False Positives** | Higher | Lower |

---

## Task 3.2: Enable DAST ⭐⭐

### Basic DAST Configuration

```yaml
include:
  - template: DAST.gitlab-ci.yml

variables:
  DAST_WEBSITE: "https://staging.example.com"
```

### Full DAST Configuration

```yaml
include:
  - template: DAST.gitlab-ci.yml

variables:
  # Target URL (required)
  DAST_WEBSITE: "https://staging.example.com"
  
  # Scan type
  DAST_FULL_SCAN_ENABLED: "true"  # Full scan (slower)
  
  # Spider settings
  DAST_SPIDER_MINS: "5"  # Spider duration
  DAST_SPIDER_START_AT_HOST: "true"
  
  # Authentication
  DAST_AUTH_URL: "https://staging.example.com/login"
  DAST_USERNAME: "testuser"
  DAST_PASSWORD: "$DAST_PASSWORD"  # From CI/CD variables
  DAST_USERNAME_FIELD: "username"
  DAST_PASSWORD_FIELD: "password"
  
  # Exclusions
  DAST_EXCLUDE_URLS: "logout,admin"
  
  # Timeout
  DAST_TARGET_AVAILABILITY_TIMEOUT: "60"

dast:
  stage: dast
  rules:
    - if: '$CI_COMMIT_BRANCH == "main"'
```

---

## Task 3.3: DAST Scan Types ⭐

### Passive Scan

```yaml
# Quick scan, no attacks
variables:
  DAST_FULL_SCAN_ENABLED: "false"  # Passive only
  DAST_SPIDER_MINS: "1"

# Detects:
- Missing security headers
- Cookie issues
- Information disclosure
- Basic misconfigurations
```

### Active Scan (Full Scan)

```yaml
# Full scan with attacks
variables:
  DAST_FULL_SCAN_ENABLED: "true"
  DAST_SPIDER_MINS: "5"

# Detects:
- SQL injection
- XSS
- Command injection
- Path traversal
- Authentication issues
- Session management flaws
```

### API Scan

```yaml
include:
  - template: DAST-API.gitlab-ci.yml

variables:
  DAST_API_TARGET_URL: "https://api.example.com"
  DAST_API_OPENAPI: "openapi.json"  # OpenAPI spec
  # Or use HAR file
  DAST_API_HAR: "api-requests.har"
```

---

## Task 3.4: DAST Authentication ⭐⭐

### Form-Based Authentication

```yaml
variables:
  # Login URL
  DAST_AUTH_URL: "https://staging.example.com/login"
  
  # Credentials
  DAST_USERNAME: "testuser"
  DAST_PASSWORD: "$DAST_PASSWORD"  # Masked variable
  
  # Form fields
  DAST_USERNAME_FIELD: "username"
  DAST_PASSWORD_FIELD: "password"
  DAST_SUBMIT_FIELD: "submit"
  
  # Verification
  DAST_AUTH_VERIFICATION_URL: "https://staging.example.com/dashboard"
  DAST_AUTH_VERIFICATION_SELECTOR: "css:.user-profile"
```

### Bearer Token Authentication

```yaml
variables:
  DAST_REQUEST_HEADERS: "Authorization: Bearer $API_TOKEN"
```

### Cookie-Based Authentication

```yaml
variables:
  DAST_AUTH_COOKIES: "session_id=$SESSION_ID"
```

### Custom Authentication Script

```yaml
# Create auth script
before_script:
  - |
    cat > auth.js << 'EOF'
    module.exports = async (page) => {
      await page.goto('https://staging.example.com/login');
      await page.type('#username', 'testuser');
      await page.type('#password', process.env.DAST_PASSWORD);
      await page.click('#submit');
      await page.waitForNavigation();
    };
    EOF

variables:
  DAST_AUTH_SCRIPT: "auth.js"
```

---

## Task 3.5: DAST Configuration Options ⭐⭐

### Spider Configuration

```yaml
variables:
  # Spider duration (minutes)
  DAST_SPIDER_MINS: "5"
  
  # Max depth
  DAST_MAX_DEPTH: "10"
  
  # Start at host
  DAST_SPIDER_START_AT_HOST: "true"
  
  # Include/exclude URLs
  DAST_PATHS: "/api,/admin"  # Only scan these
  DAST_EXCLUDE_URLS: "logout,delete"  # Skip these
```

### Scan Configuration

```yaml
variables:
  # Full scan
  DAST_FULL_SCAN_ENABLED: "true"
  
  # Target availability timeout
  DAST_TARGET_AVAILABILITY_TIMEOUT: "60"
  
  # Browser-based scan
  DAST_BROWSER_SCAN: "true"
  
  # AJAX spider
  DAST_USE_AJAX_SPIDER: "true"
  DAST_AJAX_SPIDER_MINS: "2"
```

### Advanced Configuration

```yaml
variables:
  # Custom ZAP config
  DAST_ZAP_CLI_OPTIONS: "-config api.disablekey=true"
  
  # Debug mode
  DAST_DEBUG: "true"
  
  # API specification
  DAST_API_SPECIFICATION: "openapi.json"
  
  # Custom rules
  DAST_ZAP_RULES: "custom-rules.conf"
```

---

## Task 3.6: Common DAST Vulnerabilities ⭐⭐

### SQL Injection

```yaml
# DAST Detection:
# - Sends payloads: ' OR '1'='1, '; DROP TABLE users--
# - Checks for SQL errors in response
# - Verifies time-based blind injection

# Example vulnerable endpoint:
GET /users?id=1' OR '1'='1

# DAST Report:
Severity: High
CWE: 89
Description: SQL injection vulnerability detected
```

### Cross-Site Scripting (XSS)

```yaml
# DAST Detection:
# - Injects: <script>alert(1)</script>
# - Checks if script executes
# - Tests reflected, stored, DOM-based XSS

# Example vulnerable endpoint:
GET /search?q=<script>alert(1)</script>

# DAST Report:
Severity: Medium
CWE: 79
Description: XSS vulnerability detected
```

### Authentication Bypass

```yaml
# DAST Detection:
# - Tests weak passwords
# - Checks for default credentials
# - Tests authentication logic flaws

# DAST Report:
Severity: High
CWE: 287
Description: Weak authentication detected
```

### Session Management

```yaml
# DAST Detection:
# - Checks cookie security flags
# - Tests session fixation
# - Verifies session timeout

# Issues found:
- Missing Secure flag on cookies
- Missing HttpOnly flag
- Weak session ID generation
- No session timeout
```

### Security Headers

```yaml
# DAST Detection:
# - Checks for security headers
# - Verifies CSP policy
# - Tests CORS configuration

# Missing headers:
- X-Frame-Options
- X-Content-Type-Options
- Content-Security-Policy
- Strict-Transport-Security
```

---

## Task 3.7: DAST with Review Apps ⭐

### Review App DAST

```yaml
# Deploy review app
deploy-review:
  stage: deploy
  script:
    - ./deploy-review.sh $CI_COMMIT_REF_SLUG
  environment:
    name: review/$CI_COMMIT_REF_SLUG
    url: https://$CI_COMMIT_REF_SLUG.review.example.com
  rules:
    - if: '$CI_PIPELINE_SOURCE == "merge_request_event"'

# Run DAST on review app
dast-review:
  stage: dast
  needs:
    - deploy-review
  variables:
    DAST_WEBSITE: "https://$CI_COMMIT_REF_SLUG.review.example.com"
    DAST_FULL_SCAN_ENABLED: "false"  # Quick scan for MR
  rules:
    - if: '$CI_PIPELINE_SOURCE == "merge_request_event"'
```

---

## Task 3.8: DAST API Scanning ⭐⭐

### OpenAPI Specification

```yaml
include:
  - template: DAST-API.gitlab-ci.yml

variables:
  DAST_API_TARGET_URL: "https://api.example.com"
  DAST_API_OPENAPI: "openapi.json"
  
  # Authentication
  DAST_API_HTTP_USERNAME: "apiuser"
  DAST_API_HTTP_PASSWORD: "$API_PASSWORD"
  
  # Or bearer token
  DAST_API_BEARER_TOKEN: "$API_TOKEN"
```

### HAR File

```yaml
# Generate HAR file from browser
# 1. Open DevTools > Network
# 2. Perform API requests
# 3. Right-click > Save all as HAR

variables:
  DAST_API_TARGET_URL: "https://api.example.com"
  DAST_API_HAR: "api-requests.har"
```

### Postman Collection

```yaml
variables:
  DAST_API_TARGET_URL: "https://api.example.com"
  DAST_API_POSTMAN_COLLECTION: "postman-collection.json"
  DAST_API_POSTMAN_COLLECTION_VARIABLES: "postman-variables.json"
```

---

## Task 3.9: DAST Performance Optimization ⭐

### Optimize DAST Scanning

```yaml
# 1. Use passive scan for MR
variables:
  DAST_FULL_SCAN_ENABLED: "false"  # Faster

# 2. Limit spider time
variables:
  DAST_SPIDER_MINS: "2"  # Reduce from default 5

# 3. Exclude unnecessary paths
variables:
  DAST_EXCLUDE_URLS: "logout,admin,static"

# 4. Run full scan only on main
dast-full:
  variables:
    DAST_FULL_SCAN_ENABLED: "true"
  rules:
    - if: '$CI_COMMIT_BRANCH == "main"'

dast-quick:
  variables:
    DAST_FULL_SCAN_ENABLED: "false"
  rules:
    - if: '$CI_PIPELINE_SOURCE == "merge_request_event"'
```

### Scheduled DAST Scans

```yaml
# Run full DAST on schedule
dast-scheduled:
  variables:
    DAST_WEBSITE: "https://production.example.com"
    DAST_FULL_SCAN_ENABLED: "true"
    DAST_SPIDER_MINS: "10"
  rules:
    - if: '$CI_PIPELINE_SOURCE == "schedule"'
```

---

## Task 3.10: DAST Best Practices ⭐⭐

### DAST Implementation Best Practices

```yaml
# 1. Start with passive scans
variables:
  DAST_FULL_SCAN_ENABLED: "false"

# 2. Use dedicated test environment
variables:
  DAST_WEBSITE: "https://staging.example.com"  # Not production

# 3. Configure authentication
variables:
  DAST_AUTH_URL: "https://staging.example.com/login"
  DAST_USERNAME: "testuser"
  DAST_PASSWORD: "$DAST_PASSWORD"

# 4. Exclude sensitive endpoints
variables:
  DAST_EXCLUDE_URLS: "logout,delete,admin/delete"

# 5. Run full scans on schedule
# Settings > CI/CD > Schedules

# 6. Review results regularly
# Security & Compliance > Vulnerability Report

# 7. Integrate with workflow
# Block MR on critical DAST findings

# 8. Monitor application during scan
# Check logs for errors or performance issues
```

### DAST Security Considerations

```yaml
# ⚠️ DAST performs real attacks
# - Use test environment only
# - Never scan production without approval
# - Inform security team
# - Monitor application during scan
# - Have rollback plan ready

# ✅ Safe DAST practices
- Dedicated test environment
- Test data only
- Rate limiting configured
- Monitoring enabled
- Backup available
```

---

## Exam Tips for Domain 3

1. **DAST Definition** - Dynamic testing of running applications
2. **Enable DAST** - Include DAST.gitlab-ci.yml template
3. **Requirements** - Deployed application URL required
4. **Scan Types** - Passive (quick) vs Active (full)
5. **Authentication** - Form-based, bearer token, cookies
6. **Configuration** - DAST_WEBSITE, DAST_FULL_SCAN_ENABLED
7. **Vulnerabilities** - SQL injection, XSS, auth bypass
8. **API Scanning** - OpenAPI, HAR, Postman
9. **Best Practices** - Use staging, exclude sensitive endpoints
10. **Performance** - Passive for MR, full for main/schedule

---

## Quick Reference

### Enable DAST

```yaml
include:
  - template: DAST.gitlab-ci.yml

variables:
  DAST_WEBSITE: "https://staging.example.com"
```

### DAST with Authentication

```yaml
variables:
  DAST_WEBSITE: "https://staging.example.com"
  DAST_AUTH_URL: "https://staging.example.com/login"
  DAST_USERNAME: "testuser"
  DAST_PASSWORD: "$DAST_PASSWORD"
  DAST_USERNAME_FIELD: "username"
  DAST_PASSWORD_FIELD: "password"
```

### Quick vs Full Scan

```yaml
# Quick (passive) - for MR
variables:
  DAST_FULL_SCAN_ENABLED: "false"
  DAST_SPIDER_MINS: "1"

# Full (active) - for main
variables:
  DAST_FULL_SCAN_ENABLED: "true"
  DAST_SPIDER_MINS: "5"
```

### DAST API Scanning

```yaml
include:
  - template: DAST-API.gitlab-ci.yml

variables:
  DAST_API_TARGET_URL: "https://api.example.com"
  DAST_API_OPENAPI: "openapi.json"
```
