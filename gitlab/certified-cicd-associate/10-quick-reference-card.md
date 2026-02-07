# GitLab Certified CI/CD Associate - Quick Reference Card

## Exam Format
| Aspect | Details |
|--------|---------|
| **Part 1** | 14 multiple-choice (60 min, 80% to pass) |
| **Part 2** | 14 hands-on tasks (120 min) |
| **Retakes** | Part 1: Unlimited, Part 2: Limited |

---

## Essential .gitlab-ci.yml Structure

```yaml
stages:
  - build
  - test
  - deploy

variables:
  GLOBAL_VAR: "value"

before_script:
  - echo "Runs before every job"

job-name:
  stage: build
  image: node:18-alpine
  services:
    - postgres:14
  variables:
    JOB_VAR: "value"
  before_script:
    - npm ci
  script:
    - npm run build
  after_script:
    - echo "Cleanup"
  artifacts:
    paths:
      - dist/
    expire_in: 1 week
  cache:
    key:
      files:
        - package-lock.json
    paths:
      - node_modules/
  tags:
    - docker
  rules:
    - if: '$CI_COMMIT_BRANCH == "main"'
  needs: []
  dependencies: []
  allow_failure: false
  when: on_success
  retry: 2
  timeout: 1h
  environment:
    name: production
    url: https://example.com
```

---

## Critical Keywords

| Keyword | Purpose | Example |
|---------|---------|---------|
| **script** | Commands (required) | `script: - npm test` |
| **stage** | Pipeline stage | `stage: test` |
| **image** | Docker image | `image: node:18` |
| **rules** | Conditional execution | `rules: - if: $CI_COMMIT_BRANCH` |
| **needs** | Job dependencies (DAG) | `needs: [build]` |
| **artifacts** | Save outputs | `artifacts: paths: - dist/` |
| **cache** | Speed up builds | `cache: paths: - node_modules/` |
| **environment** | Deployment target | `environment: name: production` |
| **when** | Execution timing | `when: manual` |
| **tags** | Runner selection | `tags: - docker` |

---

## Rules vs Only/Except

```yaml
# Modern: rules (recommended)
job:
  rules:
    - if: '$CI_COMMIT_BRANCH == "main"'
    - if: '$CI_PIPELINE_SOURCE == "merge_request_event"'
    - changes:
        - "src/**/*"

# Legacy: only/except (deprecated)
job:
  only:
    - main
    - merge_requests
  except:
    - tags
```

---

## Predefined Variables (Most Used)

```yaml
# Commit
$CI_COMMIT_SHA              # Full commit hash
$CI_COMMIT_SHORT_SHA        # Short commit hash
$CI_COMMIT_BRANCH           # Branch name
$CI_COMMIT_TAG              # Tag name
$CI_COMMIT_MESSAGE          # Commit message

# Pipeline
$CI_PIPELINE_ID             # Pipeline ID
$CI_PIPELINE_SOURCE         # push, merge_request_event, schedule

# Job
$CI_JOB_ID                  # Job ID
$CI_JOB_NAME                # Job name
$CI_JOB_STAGE               # Stage name

# Project
$CI_PROJECT_ID              # Project ID
$CI_PROJECT_NAME            # Project name
$CI_PROJECT_PATH            # group/project
$CI_PROJECT_DIR             # /builds/group/project

# Registry
$CI_REGISTRY                # registry.gitlab.com
$CI_REGISTRY_IMAGE          # registry.gitlab.com/group/project
$CI_REGISTRY_USER           # gitlab-ci-token
$CI_REGISTRY_PASSWORD       # Token

# Merge Request
$CI_MERGE_REQUEST_IID       # MR number
$CI_MERGE_REQUEST_TITLE     # MR title
$CI_MERGE_REQUEST_SOURCE_BRANCH_NAME
$CI_MERGE_REQUEST_TARGET_BRANCH_NAME
```

---

## Artifacts vs Cache

| Feature | Artifacts | Cache |
|---------|-----------|-------|
| **Purpose** | Pass data between jobs | Speed up builds |
| **Reliability** | Guaranteed | Best effort |
| **Scope** | Pipeline-wide | Runner-specific |
| **Use For** | Build outputs, reports | Dependencies |
| **Storage** | GitLab | Runner or S3 |

```yaml
# Artifacts: Build outputs
build:
  script:
    - npm run build
  artifacts:
    paths:
      - dist/
    expire_in: 1 week

# Cache: Dependencies
build:
  cache:
    key:
      files:
        - package-lock.json
    paths:
      - node_modules/
  script:
    - npm ci
    - npm run build
```

---

## needs vs dependencies

```yaml
# needs: Controls execution order + downloads artifacts
test:
  needs: [build]  # Wait for build, download artifacts
  script:
    - npm test

# needs without artifacts
test:
  needs:
    - job: build
      artifacts: false  # Don't download artifacts
  script:
    - npm test

# dependencies: Only controls artifacts
test:
  dependencies: [build]  # Download artifacts only
  script:
    - npm test
```

---

## Runner Types

| Type | Scope | Use Case |
|------|-------|----------|
| **Shared** | Instance-wide | All projects |
| **Group** | Group-level | Group projects |
| **Specific** | Project-level | Single project |

### Runner Registration

```bash
sudo gitlab-runner register \
  --non-interactive \
  --url "https://gitlab.com/" \
  --registration-token "TOKEN" \
  --executor "docker" \
  --docker-image "alpine:latest" \
  --description "my-runner" \
  --tag-list "docker,linux"
```

### Executor Types

| Executor | Isolation | Use Case |
|----------|-----------|----------|
| **Shell** | None | Simple scripts |
| **Docker** | Container | Most common |
| **Kubernetes** | Pod | Cloud-native |

---

## Security Scanning

```yaml
# Enable all security scans
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

# Dependency Scanning
DS_REMEDIATE: "true"

# Container Scanning
CS_SEVERITY_THRESHOLD: "MEDIUM"
CS_IMAGE: $CI_REGISTRY_IMAGE:$CI_COMMIT_SHA

# Secret Detection
SECRET_DETECTION_HISTORIC_SCAN: "true"
```

---

## Environments

```yaml
# Basic environment
deploy:
  stage: deploy
  script:
    - ./deploy.sh
  environment:
    name: production
    url: https://example.com
  when: manual
  only:
    - main

# Review app (dynamic environment)
deploy-review:
  stage: deploy
  script:
    - ./deploy-review.sh $CI_COMMIT_REF_SLUG
  environment:
    name: review/$CI_COMMIT_REF_SLUG
    url: https://$CI_COMMIT_REF_SLUG.review.example.com
    on_stop: stop-review
    auto_stop_in: 1 week
  rules:
    - if: '$CI_PIPELINE_SOURCE == "merge_request_event"'

stop-review:
  stage: deploy
  script:
    - ./cleanup-review.sh $CI_COMMIT_REF_SLUG
  environment:
    name: review/$CI_COMMIT_REF_SLUG
    action: stop
  when: manual
  rules:
    - if: '$CI_PIPELINE_SOURCE == "merge_request_event"'
```

---

## Deployment Strategies

### Blue-Green

```yaml
deploy-green:
  script:
    - ./deploy.sh green
  environment:
    name: production-green

switch-traffic:
  needs: [deploy-green]
  script:
    - ./switch-traffic.sh green
  environment:
    name: production
  when: manual
```

### Canary

```yaml
deploy-canary:
  script:
    - ./deploy-canary.sh 10  # 10% traffic
  environment:
    name: production-canary

promote-canary:
  script:
    - ./deploy-canary.sh 100  # 100% traffic
  when: manual
```

### Rolling

```yaml
deploy-rolling:
  script:
    - kubectl set image deployment/app app=$CI_REGISTRY_IMAGE:$CI_COMMIT_SHA
    - kubectl rollout status deployment/app
  environment:
    name: production
```

---

## Kubernetes Deployment

```yaml
deploy-k8s:
  stage: deploy
  image: bitnami/kubectl:latest
  script:
    # Configure kubectl
    - kubectl config set-cluster k8s --server="$KUBE_URL"
    - kubectl config set-credentials admin --token="$KUBE_TOKEN"
    - kubectl config set-context default --cluster=k8s --user=admin
    - kubectl config use-context default
    
    # Deploy
    - kubectl set image deployment/app app=$CI_REGISTRY_IMAGE:$CI_COMMIT_SHA
    - kubectl rollout status deployment/app
  environment:
    name: production
```

---

## Common Patterns

### Docker Build & Push

```yaml
build:
  stage: build
  image: docker:latest
  services:
    - docker:dind
  before_script:
    - docker login -u $CI_REGISTRY_USER -p $CI_REGISTRY_PASSWORD $CI_REGISTRY
  script:
    - docker build -t $CI_REGISTRY_IMAGE:$CI_COMMIT_SHA .
    - docker tag $CI_REGISTRY_IMAGE:$CI_COMMIT_SHA $CI_REGISTRY_IMAGE:latest
    - docker push $CI_REGISTRY_IMAGE:$CI_COMMIT_SHA
    - docker push $CI_REGISTRY_IMAGE:latest
```

### Node.js Pipeline

```yaml
stages:
  - build
  - test
  - deploy

variables:
  NODE_ENV: "production"

cache:
  key:
    files:
      - package-lock.json
  paths:
    - node_modules/

build:
  stage: build
  image: node:18-alpine
  script:
    - npm ci
    - npm run build
  artifacts:
    paths:
      - dist/
    expire_in: 1 week

test:
  stage: test
  image: node:18-alpine
  script:
    - npm ci
    - npm test
  coverage: '/Coverage: \d+\.\d+%/'

deploy:
  stage: deploy
  script:
    - ./deploy.sh
  environment:
    name: production
  only:
    - main
  when: manual
```

---

## Troubleshooting

### Debug Mode

```yaml
debug:
  variables:
    CI_DEBUG_TRACE: "true"
  script:
    - env | sort
    - pwd
    - ls -la
  when: manual
```

### Common Issues

| Issue | Solution |
|-------|----------|
| **Job pending** | Check runner tags |
| **Image pull failed** | Use correct image name |
| **Command not found** | Install in before_script |
| **Timeout** | Increase timeout or optimize |
| **Artifacts not found** | Check paths and expiration |
| **Cache not working** | Use file-based cache key |

### Runner Commands

```bash
# Status
sudo gitlab-runner status

# Logs
sudo journalctl -u gitlab-runner -f

# Restart
sudo gitlab-runner restart

# Verify
sudo gitlab-runner verify

# Clean
docker system prune -a -f
```

---

## Variable Security

```yaml
# In UI: Settings > CI/CD > Variables
# ✅ Protect variable (only protected branches)
# ✅ Mask variable (hide in logs)
# ✅ Use file type for keys

# Variable precedence (highest to lowest):
# 1. Trigger variables
# 2. Pipeline variables
# 3. Job variables
# 4. Global variables (.gitlab-ci.yml)
# 5. Group variables
# 6. Project variables
# 7. Instance variables
# 8. Predefined variables
```

---

## Git Commands (Exam Essentials)

```bash
# Clone
git clone <url>

# Branch
git checkout -b feature-branch
git branch -d feature-branch

# Commit
git add .
git commit -m "Message"
git push origin feature-branch

# Merge
git checkout main
git merge feature-branch

# Rebase
git rebase main

# Stash
git stash
git stash pop

# Cherry-pick
git cherry-pick <commit-hash>

# Reset
git reset --soft HEAD~1  # Keep changes
git reset --hard HEAD~1  # Discard changes
```

---

## Exam Day Checklist

### Part 1 (Written)
- ✅ Review .gitlab-ci.yml syntax
- ✅ Know predefined variables
- ✅ Understand rules and conditions
- ✅ Know artifacts vs cache
- ✅ Understand runner types
- ✅ Know security scanning templates
- ✅ Understand environments
- ✅ Know deployment strategies

### Part 2 (Hands-on)
- ✅ Create GitLab project
- ✅ Test .gitlab-ci.yml locally if possible
- ✅ Use CI Lint to validate YAML
- ✅ Check pipeline execution
- ✅ Verify job logs
- ✅ Test artifact downloads
- ✅ Verify environment deployments
- ✅ Use GitLab documentation

---

## Critical Reminders

| Topic | Remember |
|-------|----------|
| **YAML** | 2 spaces indentation, no tabs |
| **script** | Required in every job |
| **rules** | Modern, replaces only/except |
| **needs** | DAG pipelines, faster execution |
| **cache key** | Use files for auto-invalidation |
| **artifacts** | Set expire_in to save storage |
| **masked variables** | 8+ chars, no spaces, base64 only |
| **protected variables** | Only on protected branches |
| **image versions** | Use specific versions, not latest |
| **runner tags** | Must match job tags |
| **environment** | Required for deployments |
| **when: manual** | Requires manual trigger |
| **allow_failure** | Pipeline continues on failure |
| **retry** | Auto-retry on failure |
| **timeout** | Prevent stuck jobs |

---

## Quick Validation

```bash
# Validate .gitlab-ci.yml
# Project > CI/CD > Editor > Validate

# Or use CI Lint API
curl --header "PRIVATE-TOKEN: <token>" \
  "https://gitlab.com/api/v4/projects/:id/ci/lint" \
  --data "content=$(cat .gitlab-ci.yml)"
```

---

## Resources During Exam

- ✅ https://docs.gitlab.com
- ✅ https://docs.gitlab.com/ee/ci/
- ✅ https://docs.gitlab.com/runner/
- ✅ https://docs.gitlab.com/ee/ci/yaml/

> **Tip:** Bookmark key pages before exam!

---

## Final Tips

1. **Read questions carefully** - Understand requirements
2. **Use CI Lint** - Validate YAML syntax
3. **Check logs** - Debug failed jobs
4. **Test incrementally** - Commit and test often
5. **Use documentation** - Reference allowed during exam
6. **Manage time** - Don't spend too long on one task
7. **Verify results** - Check pipeline execution
8. **Use specific versions** - Avoid latest tags
9. **Follow best practices** - Security, performance
10. **Stay calm** - You've got this! 🚀

---

## Good Luck! 🎉

Remember:
- Part 1: 80% to pass (can retake)
- Part 2: Hands-on tasks (graded by GitLab)
- Use documentation during exam
- Practice makes perfect!
