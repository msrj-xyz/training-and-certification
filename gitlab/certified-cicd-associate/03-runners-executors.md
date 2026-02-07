# Domain 3: GitLab Runners & Executors

## Task 3.1: Runner Types ⭐⭐

### Runner Scope

| Type | Scope | Use Case |
|------|-------|----------|
| **Shared Runners** | Instance-wide | Available to all projects |
| **Group Runners** | Group-level | Available to group and subgroups |
| **Specific Runners** | Project-level | Dedicated to specific project |

### Runner Characteristics

```yaml
# Runner configuration comparison
Shared Runner:
  - Managed by GitLab admin
  - Shared across all projects
  - Fair usage queue
  - Limited customization
  - Cost-effective for small projects

Group Runner:
  - Managed by group owners
  - Shared within group
  - Group-specific configuration
  - Better resource control
  - Ideal for organization teams

Specific Runner:
  - Managed by project maintainers
  - Dedicated resources
  - Full customization
  - Higher cost
  - Best for sensitive/high-performance workloads
```

---

## Task 3.2: Runner Installation ⭐⭐

### Linux Installation

```bash
# Download GitLab Runner
curl -L "https://packages.gitlab.com/install/repositories/runner/gitlab-runner/script.deb.sh" | sudo bash

# Install GitLab Runner
sudo apt-get install gitlab-runner

# Check version
gitlab-runner --version

# Check status
sudo gitlab-runner status
```

### macOS Installation

```bash
# Using Homebrew
brew install gitlab-runner

# Start service
brew services start gitlab-runner
```

### Windows Installation

```powershell
# Download binary
Invoke-WebRequest -Uri "https://gitlab-runner-downloads.s3.amazonaws.com/latest/binaries/gitlab-runner-windows-amd64.exe" -OutFile "gitlab-runner.exe"

# Install service
.\gitlab-runner.exe install

# Start service
.\gitlab-runner.exe start
```

### Docker Installation

```bash
# Run GitLab Runner in Docker
docker run -d --name gitlab-runner --restart always \
  -v /srv/gitlab-runner/config:/etc/gitlab-runner \
  -v /var/run/docker.sock:/var/run/docker.sock \
  gitlab/gitlab-runner:latest
```

---

## Task 3.3: Runner Registration ⭐⭐

### Registration Process

```bash
# Interactive registration
sudo gitlab-runner register

# Non-interactive registration
sudo gitlab-runner register \
  --non-interactive \
  --url "https://gitlab.com/" \
  --registration-token "PROJECT_REGISTRATION_TOKEN" \
  --executor "docker" \
  --docker-image "alpine:latest" \
  --description "docker-runner" \
  --tag-list "docker,linux" \
  --run-untagged="true" \
  --locked="false" \
  --access-level="not_protected"
```

### Registration Parameters

| Parameter | Description | Example |
|-----------|-------------|---------|
| **--url** | GitLab instance URL | `https://gitlab.com/` |
| **--registration-token** | Project/group token | `GR1348941...` |
| **--executor** | Executor type | `docker`, `shell`, `kubernetes` |
| **--docker-image** | Default Docker image | `alpine:latest` |
| **--description** | Runner description | `my-docker-runner` |
| **--tag-list** | Runner tags | `docker,linux,production` |
| **--run-untagged** | Run untagged jobs | `true` or `false` |
| **--locked** | Lock to project | `true` or `false` |
| **--access-level** | Protected branches | `not_protected` or `ref_protected` |

### Finding Registration Token

```bash
# Project-specific runner
Settings > CI/CD > Runners > Expand > Registration token

# Group runner
Group > Settings > CI/CD > Runners > Registration token

# Instance runner (Admin only)
Admin Area > Overview > Runners > Register an instance runner
```

---

## Task 3.4: Executor Types ⭐⭐

### Executor Comparison

| Executor | Isolation | Performance | Complexity | Use Case |
|----------|-----------|-------------|------------|----------|
| **Shell** | None | Fast | Low | Simple scripts, local dev |
| **Docker** | Container | Medium | Medium | Most common, isolated builds |
| **Docker+Machine** | Container | Medium | High | Auto-scaling cloud |
| **Kubernetes** | Pod | Medium | High | Cloud-native, scalable |
| **VirtualBox** | VM | Slow | Medium | Full OS isolation |
| **Parallels** | VM | Slow | Medium | macOS builds |
| **SSH** | Remote | Varies | Medium | Remote execution |
| **Custom** | Varies | Varies | High | Custom implementation |

### Shell Executor

```bash
# Register shell executor
sudo gitlab-runner register \
  --executor "shell" \
  --description "shell-runner"

# Characteristics
- Runs directly on host
- No isolation
- Fast execution
- Requires dependencies pre-installed
- Security concerns
```

### Docker Executor

```bash
# Register Docker executor
sudo gitlab-runner register \
  --executor "docker" \
  --docker-image "alpine:latest" \
  --docker-volumes "/var/run/docker.sock:/var/run/docker.sock"

# Characteristics
- Container isolation
- Clean environment per job
- Docker-in-Docker support
- Image caching
- Most popular choice
```

### Kubernetes Executor

```bash
# Register Kubernetes executor
sudo gitlab-runner register \
  --executor "kubernetes" \
  --kubernetes-host "https://k8s.example.com" \
  --kubernetes-namespace "gitlab-runner"

# Characteristics
- Pod-based execution
- Auto-scaling
- Resource limits
- Cloud-native
- Complex setup
```

---

## Task 3.5: Runner Configuration ⭐⭐

### Config File Location

```bash
# Linux
/etc/gitlab-runner/config.toml

# macOS
/usr/local/etc/gitlab-runner/config.toml

# Windows
C:\GitLab-Runner\config.toml
```

### Basic Configuration

```toml
concurrent = 4  # Max concurrent jobs
check_interval = 0  # Check for new jobs interval

[[runners]]
  name = "docker-runner"
  url = "https://gitlab.com/"
  token = "RUNNER_TOKEN"
  executor = "docker"
  
  [runners.docker]
    tls_verify = false
    image = "alpine:latest"
    privileged = false
    disable_cache = false
    volumes = ["/cache"]
    shm_size = 0
  
  [runners.cache]
    Type = "s3"
    Shared = true
    [runners.cache.s3]
      ServerAddress = "s3.amazonaws.com"
      BucketName = "runner-cache"
      BucketLocation = "us-east-1"
```

### Docker Executor Configuration

```toml
[[runners]]
  name = "docker-runner"
  executor = "docker"
  
  [runners.docker]
    image = "alpine:latest"
    privileged = false
    disable_entrypoint_overwrite = false
    oom_kill_disable = false
    disable_cache = false
    volumes = ["/cache", "/var/run/docker.sock:/var/run/docker.sock"]
    shm_size = 0
    pull_policy = ["if-not-present"]
    allowed_images = ["alpine:*", "node:*", "python:*"]
    allowed_services = ["postgres:*", "redis:*"]
```

### Kubernetes Executor Configuration

```toml
[[runners]]
  name = "kubernetes-runner"
  executor = "kubernetes"
  
  [runners.kubernetes]
    host = "https://k8s.example.com"
    namespace = "gitlab-runner"
    privileged = false
    cpu_limit = "1"
    memory_limit = "2Gi"
    service_cpu_limit = "500m"
    service_memory_limit = "1Gi"
    helper_cpu_limit = "200m"
    helper_memory_limit = "256Mi"
    poll_interval = 3
    poll_timeout = 180
    
    [runners.kubernetes.pod_labels]
      "app" = "gitlab-runner"
      "environment" = "production"
```

---

## Task 3.6: Runner Tags ⭐⭐

### Using Tags

```yaml
# Job with specific tags
build-docker:
  tags:
    - docker
    - linux
  script:
    - docker build -t myapp .

build-macos:
  tags:
    - macos
    - xcode
  script:
    - xcodebuild

deploy-production:
  tags:
    - production
    - deploy
  script:
    - ./deploy.sh
```

### Tag Strategies

| Strategy | Description | Example |
|----------|-------------|---------|
| **Platform** | OS/architecture | `linux`, `windows`, `macos`, `arm64` |
| **Executor** | Executor type | `docker`, `shell`, `kubernetes` |
| **Environment** | Deployment target | `dev`, `staging`, `production` |
| **Capability** | Special features | `gpu`, `ssd`, `high-memory` |
| **Location** | Geographic | `us-east`, `eu-west`, `asia` |
| **Team** | Ownership | `frontend`, `backend`, `devops` |

### Tag Best Practices

```yaml
# Good: Specific and descriptive
deploy-prod:
  tags:
    - docker
    - production
    - us-east-1
  script:
    - ./deploy.sh

# Bad: Too generic
deploy:
  tags:
    - runner
  script:
    - ./deploy.sh

# Good: Multiple tags for flexibility
test:
  tags:
    - docker
    - linux
    - test-suite
  script:
    - npm test
```

---

## Task 3.7: Runner Management ⭐

### Runner Commands

```bash
# List runners
sudo gitlab-runner list

# Verify runner
sudo gitlab-runner verify

# Start runner
sudo gitlab-runner start

# Stop runner
sudo gitlab-runner stop

# Restart runner
sudo gitlab-runner restart

# Unregister runner
sudo gitlab-runner unregister --name "runner-name"
sudo gitlab-runner unregister --url "https://gitlab.com/" --token "TOKEN"

# Unregister all runners
sudo gitlab-runner unregister --all-runners

# Run single job
sudo gitlab-runner run-single \
  --url "https://gitlab.com/" \
  --token "TOKEN" \
  --executor "docker" \
  --docker-image "alpine:latest"
```

### Runner Status

```bash
# Check runner status
sudo gitlab-runner status

# View runner logs
sudo journalctl -u gitlab-runner -f

# Debug mode
sudo gitlab-runner --debug run
```

### Runner Maintenance

```bash
# Update runner
sudo apt-get update
sudo apt-get install gitlab-runner

# Backup configuration
sudo cp /etc/gitlab-runner/config.toml /backup/config.toml.backup

# Restore configuration
sudo cp /backup/config.toml.backup /etc/gitlab-runner/config.toml
sudo gitlab-runner restart
```

---

## Task 3.8: Runner Autoscaling ⭐

### Docker Machine Autoscaling

```toml
[[runners]]
  name = "autoscale-runner"
  executor = "docker+machine"
  limit = 10  # Max concurrent jobs
  
  [runners.machine]
    IdleCount = 2  # Idle machines
    IdleTime = 600  # Idle timeout (seconds)
    MaxBuilds = 100  # Builds per machine
    MachineDriver = "amazonec2"
    MachineName = "gitlab-runner-%s"
    MachineOptions = [
      "amazonec2-access-key=XXXX",
      "amazonec2-secret-key=XXXX",
      "amazonec2-region=us-east-1",
      "amazonec2-vpc-id=vpc-xxxxx",
      "amazonec2-subnet-id=subnet-xxxxx",
      "amazonec2-zone=a",
      "amazonec2-instance-type=t3.medium",
      "amazonec2-security-group=gitlab-runner"
    ]
  
  [runners.cache]
    Type = "s3"
    [runners.cache.s3]
      ServerAddress = "s3.amazonaws.com"
      BucketName = "runner-cache"
```

### Kubernetes Autoscaling

```yaml
# Kubernetes HPA for GitLab Runner
apiVersion: autoscaling/v2
kind: HorizontalPodAutoscaler
metadata:
  name: gitlab-runner
spec:
  scaleTargetRef:
    apiVersion: apps/v1
    kind: Deployment
    name: gitlab-runner
  minReplicas: 2
  maxReplicas: 10
  metrics:
  - type: Resource
    resource:
      name: cpu
      target:
        type: Utilization
        averageUtilization: 70
```

---

## Task 3.9: Runner Security ⭐

### Security Best Practices

| Practice | Description |
|----------|-------------|
| **Isolated Runners** | Use specific runners for sensitive projects |
| **Protected Runners** | Restrict to protected branches |
| **Privileged Mode** | Disable unless required |
| **Image Restrictions** | Whitelist allowed images |
| **Network Isolation** | Use private networks |
| **Secrets Management** | Use masked/protected variables |
| **Regular Updates** | Keep runner version current |
| **Audit Logs** | Monitor runner activity |

### Protected Runner Configuration

```toml
[[runners]]
  name = "protected-runner"
  executor = "docker"
  
  # Only run protected branches
  access_level = "ref_protected"
  
  [runners.docker]
    image = "alpine:latest"
    privileged = false  # Disable privileged mode
    allowed_images = ["alpine:*", "node:18"]  # Whitelist images
    allowed_services = ["postgres:14"]  # Whitelist services
```

### Disable Privileged Mode

```yaml
# In .gitlab-ci.yml
build:
  image: docker:latest
  services:
    - docker:dind
  variables:
    DOCKER_TLS_CERTDIR: "/certs"
  script:
    - docker build -t myapp .
  # Note: Requires privileged: true in runner config
  # Use kaniko for unprivileged builds instead
```

### Kaniko for Unprivileged Builds

```yaml
build:
  image:
    name: gcr.io/kaniko-project/executor:debug
    entrypoint: [""]
  script:
    - mkdir -p /kaniko/.docker
    - echo "{\"auths\":{\"$CI_REGISTRY\":{\"username\":\"$CI_REGISTRY_USER\",\"password\":\"$CI_REGISTRY_PASSWORD\"}}}" > /kaniko/.docker/config.json
    - /kaniko/executor --context $CI_PROJECT_DIR --dockerfile $CI_PROJECT_DIR/Dockerfile --destination $CI_REGISTRY_IMAGE:$CI_COMMIT_TAG
```

---

## Task 3.10: Runner Troubleshooting ⭐

### Common Issues

| Issue | Cause | Solution |
|-------|-------|----------|
| **Runner not picking jobs** | Tags mismatch | Check job tags match runner tags |
| **Docker permission denied** | User not in docker group | `sudo usermod -aG docker gitlab-runner` |
| **Out of disk space** | Cache/images buildup | Clean Docker: `docker system prune -a` |
| **Connection timeout** | Network/firewall | Check connectivity to GitLab |
| **Token invalid** | Expired/revoked token | Re-register runner |
| **Concurrent limit** | Too many jobs | Increase `concurrent` in config |
| **Image pull failure** | Registry auth | Check credentials |

### Debug Commands

```bash
# Run in debug mode
sudo gitlab-runner --debug run

# Check runner connectivity
curl -I https://gitlab.com/

# Test Docker
docker run hello-world

# Check disk space
df -h

# Check Docker disk usage
docker system df

# View runner logs
sudo journalctl -u gitlab-runner -f --no-pager

# Check runner process
ps aux | grep gitlab-runner

# Verify runner registration
sudo gitlab-runner verify
```

### Log Locations

```bash
# System logs
/var/log/gitlab-runner/

# Systemd logs
journalctl -u gitlab-runner

# Docker logs
docker logs gitlab-runner
```

---

## Exam Tips for Domain 3

1. **Runner Types** - Know shared, group, and specific runners
2. **Executors** - Understand shell, docker, kubernetes differences
3. **Registration** - Master runner registration process
4. **Tags** - Know how to use and match tags
5. **Configuration** - Understand config.toml structure
6. **Security** - Know protected runners and privileged mode
7. **Autoscaling** - Understand docker+machine and kubernetes scaling
8. **Troubleshooting** - Know common issues and solutions
9. **Commands** - Master gitlab-runner CLI commands
10. **Best Practices** - Isolation, security, resource management

---

## Quick Reference

### Runner Registration (One-liner)

```bash
sudo gitlab-runner register --non-interactive --url "https://gitlab.com/" --registration-token "TOKEN" --executor "docker" --docker-image "alpine:latest" --description "my-runner" --tag-list "docker,linux"
```

### Config.toml Template

```toml
concurrent = 4
check_interval = 0

[[runners]]
  name = "my-runner"
  url = "https://gitlab.com/"
  token = "TOKEN"
  executor = "docker"
  [runners.docker]
    image = "alpine:latest"
    privileged = false
    volumes = ["/cache"]
```

### Job with Runner Selection

```yaml
job:
  tags:
    - docker
    - linux
  image: node:18
  script:
    - npm test
```
