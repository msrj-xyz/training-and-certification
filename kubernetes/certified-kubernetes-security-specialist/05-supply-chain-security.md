# Domain 5: Supply Chain Security (20%)

## Task 5.1: Minimize Base Image Footprint ⭐

### Image Best Practices

| Practice | Implementation |
|----------|----------------|
| **Minimal base** | distroless, alpine, scratch |
| **Multi-stage builds** | Separate build and runtime |
| **No root** | USER directive |
| **Specific tags** | Avoid :latest |
| **Read-only** | No writable layers needed |

### Distroless Images

```dockerfile
# Build stage
FROM golang:1.21 AS builder
WORKDIR /app
COPY . .
RUN CGO_ENABLED=0 go build -o myapp

# Runtime stage - distroless
FROM gcr.io/distroless/static:nonroot
COPY --from=builder /app/myapp /
USER nonroot:nonroot
ENTRYPOINT ["/myapp"]
```

### Alpine-based Image

```dockerfile
FROM alpine:3.19
RUN apk add --no-cache ca-certificates
RUN adduser -D -u 1000 appuser
USER appuser
COPY --chown=appuser:appuser app /app
ENTRYPOINT ["/app"]
```

### Multi-stage Build

```dockerfile
# Stage 1: Build
FROM node:20-alpine AS build
WORKDIR /app
COPY package*.json ./
RUN npm ci --only=production

# Stage 2: Runtime
FROM node:20-alpine
RUN addgroup -S app && adduser -S app -G app
WORKDIR /app
COPY --from=build /app/node_modules ./node_modules
COPY --chown=app:app . .
USER app
CMD ["node", "server.js"]
```

---

## Task 5.2: Secure Supply Chain - Image Registries ⭐⭐

### Whitelist Allowed Registries (OPA Gatekeeper)

```yaml
apiVersion: templates.gatekeeper.sh/v1
kind: ConstraintTemplate
metadata:
  name: k8sallowedrepos
spec:
  crd:
    spec:
      names:
        kind: K8sAllowedRepos
      validation:
        openAPIV3Schema:
          type: object
          properties:
            repos:
              type: array
              items:
                type: string
  targets:
    - target: admission.k8s.gatekeeper.sh
      rego: |
        package k8sallowedrepos
        violation[{"msg": msg}] {
          container := input.review.object.spec.containers[_]
          satisfied := [good | repo = input.parameters.repos[_]; good = startswith(container.image, repo)]
          not any(satisfied)
          msg := sprintf("Container %v uses image %v from untrusted registry", [container.name, container.image])
        }
```

### Constraint for Allowed Registries

```yaml
apiVersion: constraints.gatekeeper.sh/v1beta1
kind: K8sAllowedRepos
metadata:
  name: allowed-repos
spec:
  match:
    kinds:
      - apiGroups: [""]
        kinds: ["Pod"]
  parameters:
    repos:
      - "gcr.io/my-project/"
      - "docker.io/mycompany/"
      - "registry.internal.com/"
```

### ImagePolicyWebhook

```yaml
# /etc/kubernetes/admission/admission-config.yaml
apiVersion: apiserver.config.k8s.io/v1
kind: AdmissionConfiguration
plugins:
  - name: ImagePolicyWebhook
    configuration:
      imagePolicy:
        kubeConfigFile: /etc/kubernetes/admission/imagepolicy-kubeconfig.yaml
        allowTTL: 50
        denyTTL: 50
        retryBackoff: 500
        defaultAllow: false
```

---

## Task 5.3: Image Signing and Validation ⭐

### Cosign (Sigstore)

```bash
# Generate key pair
cosign generate-key-pair

# Sign image
cosign sign --key cosign.key docker.io/myimage:v1.0

# Verify signature
cosign verify --key cosign.pub docker.io/myimage:v1.0
```

### Validate Images with Policy Controller

```yaml
apiVersion: policy.sigstore.dev/v1beta1
kind: ClusterImagePolicy
metadata:
  name: require-signature
spec:
  images:
    - glob: "docker.io/mycompany/*"
  authorities:
    - key:
        data: |
          -----BEGIN PUBLIC KEY-----
          MFkwEwYHKoZIzj0CAQYIKoZIzj0DAQcDQgAE...
          -----END PUBLIC KEY-----
```

---

## Task 5.4: Static Analysis of Workloads ⭐⭐

### Kubesec - Security Risk Analysis

```bash
# Scan YAML manifest
kubesec scan pod.yaml

# Scan from stdin
cat pod.yaml | kubesec scan -

# CI/CD integration
kubesec scan deployment.yaml --exit-code 2
```

### Kubesec Scoring

| Score | Meaning |
|-------|---------|
| **> 0** | Secure configurations found |
| **0** | Neutral |
| **< 0** | Security concerns |

### Dockerfile Linting (Hadolint)

```bash
# Scan Dockerfile
hadolint Dockerfile

# Ignore specific rules
hadolint --ignore DL3008 Dockerfile
```

### Common Dockerfile Issues

| Issue | Fix |
|-------|-----|
| **DL3007** | Using :latest tag | Use specific version |
| **DL3008** | apt-get without version | Pin versions |
| **DL3002** | Running as root | Add USER directive |

---

## Task 5.5: Scan Images for Vulnerabilities ⭐⭐

### Trivy - Image Scanner

```bash
# Scan image
trivy image nginx:1.24

# Scan with severity filter
trivy image --severity HIGH,CRITICAL nginx:1.24

# Scan local image
trivy image --input image.tar

# Output as JSON
trivy image -f json -o results.json nginx:1.24

# Ignore unfixed vulnerabilities
trivy image --ignore-unfixed nginx:1.24

# Scan filesystem
trivy fs /path/to/project

# Scan Kubernetes resources
trivy k8s --report summary cluster
```

### Trivy Severity Levels

| Severity | Action |
|----------|--------|
| **CRITICAL** | Fix immediately |
| **HIGH** | Fix soon |
| **MEDIUM** | Plan to fix |
| **LOW** | Monitor |
| **UNKNOWN** | Investigate |

### Trivy in CI/CD

```yaml
# GitHub Actions example
- name: Scan image
  uses: aquasecurity/trivy-action@master
  with:
    image-ref: 'myimage:${{ github.sha }}'
    format: 'table'
    exit-code: '1'
    severity: 'CRITICAL,HIGH'
```

### Grype - Alternative Scanner

```bash
# Scan image
grype nginx:1.24

# Scan directory
grype dir:/path/to/project
```

---

## Task 5.6: Admission Controllers for Image Security ⭐

### Enable Admission Controllers

```yaml
# kube-apiserver.yaml
spec:
  containers:
  - command:
    - kube-apiserver
    - --enable-admission-plugins=NodeRestriction,PodSecurityAdmission,ImagePolicyWebhook
```

### Deny Privileged Pods

```yaml
apiVersion: templates.gatekeeper.sh/v1
kind: ConstraintTemplate
metadata:
  name: k8sdenyprivileged
spec:
  crd:
    spec:
      names:
        kind: K8sDenyPrivileged
  targets:
    - target: admission.k8s.gatekeeper.sh
      rego: |
        package k8sdenyprivileged
        violation[{"msg": msg}] {
          container := input.review.object.spec.containers[_]
          container.securityContext.privileged == true
          msg := sprintf("Privileged container not allowed: %v", [container.name])
        }
```

---

## Exam Tips for Domain 5

1. **Distroless** - Minimal attack surface images
2. **Multi-stage builds** - Separate build and runtime
3. **Trivy** - Primary image scanning tool
4. **Kubesec** - YAML manifest security analysis
5. **OPA/Gatekeeper** - Policy enforcement
6. **Allowed registries** - Whitelist trusted registries
7. **Image signing** - Cosign for verification
8. **No :latest** - Use specific image tags
9. **Non-root user** - USER directive in Dockerfile
10. **Admission controllers** - Enforce policies at admission
