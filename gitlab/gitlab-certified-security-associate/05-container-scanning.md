# Domain 5: Container Scanning

## Task 5.1: Container Scanning Overview ⭐⭐

### What is Container Scanning?

**Container Scanning** analyzes Docker images for vulnerabilities in OS packages and application dependencies.

| Aspect | Details |
|--------|---------|
| **Type** | Image analysis |
| **When** | After image build |
| **Input** | Docker image |
| **Output** | Vulnerability report (JSON) |
| **Speed** | Fast (1-5 minutes) |
| **Coverage** | OS packages, app dependencies |

### Why Container Scanning?

```yaml
# Container images contain:
- Base OS (Ubuntu, Alpine, etc.)
- System packages (apt, apk packages)
- Application dependencies
- Configuration files

# Vulnerabilities can exist in:
- Outdated base images
- Vulnerable OS packages
- Outdated application libraries
- Misconfigurations

# Example: Alpine 3.12 image
- 50+ OS packages
- Each package can have vulnerabilities
- Need continuous scanning
```

---

## Task 5.2: Enable Container Scanning ⭐⭐

### Basic Configuration

```yaml
include:
  - template: Security/Container-Scanning.gitlab-ci.yml

# Build and push image first
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

### Custom Configuration

```yaml
include:
  - template: Security/Container-Scanning.gitlab-ci.yml

variables:
  # Image to scan
  CS_IMAGE: $CI_REGISTRY_IMAGE:$CI_COMMIT_SHA
  
  # Severity threshold
  CS_SEVERITY_THRESHOLD: "MEDIUM"  # UNKNOWN, LOW, MEDIUM, HIGH, CRITICAL
  
  # Analyzer image
  CS_ANALYZER_IMAGE: "registry.gitlab.com/security-products/container-scanning:latest"
  
  # Disable dependency list
  CS_DISABLE_DEPENDENCY_LIST: "false"
  
  # Disable container scanning
  CS_DISABLED: "false"
```

---

## Task 5.3: Container Scanning with Trivy ⭐⭐

### Trivy Scanner

```yaml
# GitLab uses Trivy by default
# Trivy scans:
- OS packages (Alpine, Debian, Ubuntu, RHEL, etc.)
- Application dependencies (npm, pip, gem, etc.)
- IaC misconfigurations
- Secrets in images

# Trivy databases:
- CVE database
- OS-specific advisories
- Language-specific advisories
```

### Manual Trivy Scan

```yaml
container-scan:
  stage: test
  image:
    name: aquasec/trivy:latest
    entrypoint: [""]
  script:
    # Scan image
    - trivy image --format json --output gl-container-scanning-report.json $CI_REGISTRY_IMAGE:$CI_COMMIT_SHA
    
    # Or scan with severity filter
    - trivy image --severity HIGH,CRITICAL $CI_REGISTRY_IMAGE:$CI_COMMIT_SHA
  artifacts:
    reports:
      container_scanning: gl-container-scanning-report.json
```

### Trivy Configuration

```yaml
container-scan:
  image: aquasec/trivy:latest
  script:
    - trivy image \
        --format json \
        --output gl-container-scanning-report.json \
        --severity HIGH,CRITICAL \
        --ignore-unfixed \
        --no-progress \
        $CI_REGISTRY_IMAGE:$CI_COMMIT_SHA
  artifacts:
    reports:
      container_scanning: gl-container-scanning-report.json
```

---

## Task 5.4: Common Container Vulnerabilities ⭐⭐

### OS Package Vulnerabilities

```yaml
# Example: Ubuntu base image
FROM ubuntu:20.04

# Vulnerable packages:
- openssl 1.1.1f (CVE-2021-3711)
- curl 7.68.0 (CVE-2021-22876)
- libssh 0.9.3 (CVE-2020-16135)

# Container scanning detects:
Severity: High
Package: openssl
Version: 1.1.1f
Fixed in: 1.1.1k
CVE: CVE-2021-3711
```

### Application Dependency Vulnerabilities

```yaml
# Example: Node.js app
FROM node:14
COPY package.json .
RUN npm install

# Vulnerable dependencies in image:
- lodash 4.17.20 (CVE-2021-23337)
- axios 0.21.0 (CVE-2021-3749)

# Container scanning detects both:
# 1. OS vulnerabilities (from node:14 base)
# 2. npm package vulnerabilities
```

### Base Image Vulnerabilities

```yaml
# Outdated base images
FROM ubuntu:18.04  # EOL, many vulnerabilities
FROM node:10       # EOL, unsupported
FROM python:3.6    # EOL, unsupported

# Container scanning detects:
- Hundreds of vulnerabilities
- No security updates available
- Recommendation: Update base image
```

---

## Task 5.5: Container Security Best Practices ⭐⭐

### Secure Dockerfile

```dockerfile
# ✅ Use specific versions
FROM node:18.17.0-alpine3.18  # Not node:latest

# ✅ Use minimal base images
FROM alpine:3.18  # Smaller attack surface

# ✅ Run as non-root user
RUN addgroup -g 1000 appuser && \
    adduser -D -u 1000 -G appuser appuser
USER appuser

# ✅ Multi-stage builds
FROM node:18-alpine AS builder
WORKDIR /app
COPY package*.json ./
RUN npm ci --only=production

FROM node:18-alpine
COPY --from=builder /app/node_modules ./node_modules
COPY . .
USER node
CMD ["node", "server.js"]

# ✅ Update packages
RUN apk update && apk upgrade

# ✅ Remove unnecessary packages
RUN apk del build-dependencies

# ✅ Use .dockerignore
# .dockerignore:
node_modules
.git
.env
*.md
```

### Insecure Dockerfile (Anti-patterns)

```dockerfile
# ❌ Using latest tag
FROM ubuntu:latest

# ❌ Running as root
USER root

# ❌ Installing unnecessary packages
RUN apt-get install -y vim curl wget netcat

# ❌ Exposing secrets
ENV API_KEY="sk-1234567890"

# ❌ Not updating packages
# No apt-get update && apt-get upgrade

# ❌ Large image size
# Including build tools in final image
```

---

## Task 5.6: Image Optimization ⭐

### Reduce Image Size

```dockerfile
# Before: 1.2 GB
FROM ubuntu:20.04
RUN apt-get update && apt-get install -y python3 python3-pip
COPY requirements.txt .
RUN pip3 install -r requirements.txt
COPY . .
CMD ["python3", "app.py"]

# After: 150 MB
FROM python:3.11-alpine
WORKDIR /app
COPY requirements.txt .
RUN pip install --no-cache-dir -r requirements.txt
COPY . .
USER nobody
CMD ["python", "app.py"]

# Improvements:
# - Alpine base (5 MB vs 72 MB)
# - No cache (--no-cache-dir)
# - Minimal packages
# - Non-root user
```

### Multi-Stage Builds

```dockerfile
# Stage 1: Build
FROM node:18 AS builder
WORKDIR /app
COPY package*.json ./
RUN npm ci
COPY . .
RUN npm run build

# Stage 2: Production
FROM node:18-alpine
WORKDIR /app
COPY --from=builder /app/dist ./dist
COPY --from=builder /app/node_modules ./node_modules
USER node
EXPOSE 3000
CMD ["node", "dist/server.js"]

# Benefits:
# - Build tools not in final image
# - Smaller image size
# - Fewer vulnerabilities
```

---

## Task 5.7: Container Scanning in CI/CD ⭐⭐

### Complete Pipeline

```yaml
stages:
  - build
  - test
  - scan
  - deploy

# Build image
build:
  stage: build
  image: docker:latest
  services:
    - docker:dind
  before_script:
    - docker login -u $CI_REGISTRY_USER -p $CI_REGISTRY_PASSWORD $CI_REGISTRY
  script:
    - docker build -t $CI_REGISTRY_IMAGE:$CI_COMMIT_SHA .
    - docker push $CI_REGISTRY_IMAGE:$CI_COMMIT_SHA

# Scan image
container_scanning:
  stage: scan
  needs:
    - build
  include:
    - template: Security/Container-Scanning.gitlab-ci.yml
  variables:
    CS_IMAGE: $CI_REGISTRY_IMAGE:$CI_COMMIT_SHA
    CS_SEVERITY_THRESHOLD: "HIGH"

# Deploy only if scan passes
deploy:
  stage: deploy
  needs:
    - container_scanning
  script:
    - kubectl set image deployment/app app=$CI_REGISTRY_IMAGE:$CI_COMMIT_SHA
  only:
    - main
```

---

## Task 5.8: Vulnerability Remediation ⭐⭐

### Update Base Image

```dockerfile
# Before
FROM node:14  # Has vulnerabilities

# After
FROM node:18-alpine  # Latest LTS, fewer vulnerabilities

# Rebuild and rescan
docker build -t myapp:latest .
```

### Update OS Packages

```dockerfile
# Add to Dockerfile
RUN apk update && \
    apk upgrade && \
    apk add --no-cache \
        ca-certificates \
        && rm -rf /var/cache/apk/*
```

### Update Application Dependencies

```dockerfile
# Update package.json
{
  "dependencies": {
    "express": "4.18.2",  # Updated
    "lodash": "4.17.21"   # Updated
  }
}

# Rebuild image
docker build -t myapp:latest .
```

---

## Task 5.9: Container Scanning Reports ⭐

### Report Format

```json
{
  "version": "15.0.0",
  "vulnerabilities": [
    {
      "id": "CVE-2021-3711",
      "category": "container_scanning",
      "name": "OpenSSL vulnerability",
      "severity": "High",
      "description": "Buffer overflow in OpenSSL",
      "location": {
        "dependency": {
          "package": {
            "name": "openssl"
          },
          "version": "1.1.1f"
        },
        "operating_system": "ubuntu",
        "image": "myapp:latest"
      },
      "identifiers": [
        {
          "type": "cve",
          "name": "CVE-2021-3711",
          "value": "CVE-2021-3711"
        }
      ],
      "solution": "Upgrade to openssl 1.1.1k"
    }
  ]
}
```

---

## Task 5.10: Advanced Container Security ⭐

### Image Signing

```yaml
# Sign images with Cosign
sign-image:
  stage: sign
  image: gcr.io/projectsigstore/cosign:latest
  script:
    - cosign sign --key cosign.key $CI_REGISTRY_IMAGE:$CI_COMMIT_SHA

# Verify signature
verify-image:
  stage: verify
  image: gcr.io/projectsigstore/cosign:latest
  script:
    - cosign verify --key cosign.pub $CI_REGISTRY_IMAGE:$CI_COMMIT_SHA
```

### Runtime Security

```yaml
# Use Falco for runtime security
# Detects:
- Unexpected process execution
- File system modifications
- Network connections
- Privilege escalation
```

---

## Exam Tips for Domain 5

1. **Definition** - Scans Docker images for vulnerabilities
2. **Enable** - Include Security/Container-Scanning.gitlab-ci.yml
3. **Requirements** - Built and pushed Docker image
4. **Scanner** - Trivy (default)
5. **Detects** - OS packages, app dependencies
6. **Configuration** - CS_IMAGE, CS_SEVERITY_THRESHOLD
7. **Best Practices** - Use Alpine, non-root, multi-stage
8. **Remediation** - Update base image, update packages
9. **Optimization** - Minimal images, multi-stage builds
10. **Security** - Specific versions, no secrets, no root

---

## Quick Reference

### Enable Container Scanning

```yaml
include:
  - template: Security/Container-Scanning.gitlab-ci.yml

variables:
  CS_IMAGE: $CI_REGISTRY_IMAGE:$CI_COMMIT_SHA
```

### Secure Dockerfile Template

```dockerfile
FROM node:18-alpine
WORKDIR /app
RUN apk update && apk upgrade
COPY package*.json ./
RUN npm ci --only=production
COPY . .
RUN addgroup -g 1000 appuser && \
    adduser -D -u 1000 -G appuser appuser
USER appuser
EXPOSE 3000
CMD ["node", "server.js"]
```

### Trivy Manual Scan

```bash
trivy image --severity HIGH,CRITICAL myapp:latest
```
