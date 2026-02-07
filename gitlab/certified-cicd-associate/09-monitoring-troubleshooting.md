# Domain 9: Monitoring & Troubleshooting

## Task 9.1: Pipeline Monitoring ⭐⭐

### Pipeline Status

```yaml
# View pipeline status
# Project > CI/CD > Pipelines

# Pipeline states:
# - running: Pipeline is executing
# - pending: Waiting for runner
# - passed: All jobs succeeded
# - failed: At least one job failed
# - canceled: Pipeline was canceled
# - skipped: Pipeline was skipped
# - manual: Waiting for manual action
```

### Pipeline Badges

```markdown
# Add to README.md
[![pipeline status](https://gitlab.com/group/project/badges/main/pipeline.svg)](https://gitlab.com/group/project/-/commits/main)

[![coverage report](https://gitlab.com/group/project/badges/main/coverage.svg)](https://gitlab.com/group/project/-/commits/main)
```

### Pipeline Notifications

```yaml
# .gitlab-ci.yml
notify-success:
  stage: .post
  script:
    - |
      curl -X POST $SLACK_WEBHOOK_URL \
        -H 'Content-Type: application/json' \
        -d "{
          \"text\": \"✅ Pipeline #$CI_PIPELINE_ID passed\",
          \"attachments\": [{
            \"color\": \"good\",
            \"fields\": [
              {\"title\": \"Project\", \"value\": \"$CI_PROJECT_NAME\", \"short\": true},
              {\"title\": \"Branch\", \"value\": \"$CI_COMMIT_BRANCH\", \"short\": true},
              {\"title\": \"Commit\", \"value\": \"$CI_COMMIT_SHORT_SHA\", \"short\": true},
              {\"title\": \"Author\", \"value\": \"$CI_COMMIT_AUTHOR\", \"short\": true}
            ]
          }]
        }"
  when: on_success

notify-failure:
  stage: .post
  script:
    - |
      curl -X POST $SLACK_WEBHOOK_URL \
        -H 'Content-Type: application/json' \
        -d "{
          \"text\": \"❌ Pipeline #$CI_PIPELINE_ID failed\",
          \"attachments\": [{
            \"color\": \"danger\",
            \"fields\": [
              {\"title\": \"Project\", \"value\": \"$CI_PROJECT_NAME\", \"short\": true},
              {\"title\": \"Branch\", \"value\": \"$CI_COMMIT_BRANCH\", \"short\": true},
              {\"title\": \"Failed Job\", \"value\": \"$CI_JOB_NAME\", \"short\": true},
              {\"title\": \"Pipeline URL\", \"value\": \"$CI_PIPELINE_URL\", \"short\": false}
            ]
          }]
        }"
  when: on_failure
```

---

## Task 9.2: Job Logs and Debugging ⭐⭐

### View Job Logs

```bash
# In GitLab UI:
# Project > CI/CD > Pipelines > [Pipeline] > [Job]

# Job log sections:
# 1. Runner information
# 2. Preparing environment
# 3. Getting source code
# 4. Downloading artifacts
# 5. Restoring cache
# 6. Running before_script
# 7. Running script
# 8. Running after_script
# 9. Saving cache
# 10. Uploading artifacts
# 11. Cleaning up
```

### Debug Mode

```yaml
# Enable debug logging
variables:
  CI_DEBUG_TRACE: "true"  # Shows all commands

# Enable debug for specific job
debug-job:
  variables:
    CI_DEBUG_TRACE: "true"
  script:
    - echo "Debug mode enabled"
    - env | sort
```

### Verbose Output

```yaml
# Verbose script execution
test:
  script:
    - set -x  # Enable verbose mode
    - npm install
    - npm test
    - set +x  # Disable verbose mode
```

### Log Sections

```yaml
# Collapsible log sections
build:
  script:
    - echo -e "\e[0Ksection_start:$(date +%s):install[collapsed=true]\r\e[0KInstalling dependencies"
    - npm install
    - echo -e "\e[0Ksection_end:$(date +%s):install\r\e[0K"
    
    - echo -e "\e[0Ksection_start:$(date +%s):build[collapsed=false]\r\e[0KBuilding application"
    - npm run build
    - echo -e "\e[0Ksection_end:$(date +%s):build\r\e[0K"
```

---

## Task 9.3: Common Pipeline Issues ⭐⭐

### Issue: Job Stuck in Pending

```yaml
# Cause: No available runners with matching tags
# Solution 1: Check runner tags
test:
  tags:
    - docker  # Ensure runner has this tag
  script:
    - npm test

# Solution 2: Use shared runners
test:
  script:
    - npm test
  # No tags = use any available runner

# Solution 3: Check runner status
# Settings > CI/CD > Runners
# Ensure runners are active and not paused
```

### Issue: Image Pull Failure

```yaml
# Cause: Invalid image name or authentication
# Solution 1: Use correct image
test:
  image: node:18-alpine  # Correct image name
  script:
    - npm test

# Solution 2: Authenticate to private registry
test:
  image: registry.example.com/private/image:latest
  before_script:
    - docker login -u $CI_REGISTRY_USER -p $CI_REGISTRY_PASSWORD $CI_REGISTRY
  script:
    - npm test

# Solution 3: Use public images
test:
  image: node:18  # Public image from Docker Hub
  script:
    - npm test
```

### Issue: Script Failure

```yaml
# Cause: Command not found or script error
# Solution 1: Install dependencies
test:
  before_script:
    - apt-get update && apt-get install -y curl
  script:
    - curl https://example.com

# Solution 2: Check command availability
test:
  script:
    - which npm || (echo "npm not found" && exit 1)
    - npm test

# Solution 3: Use correct image
test:
  image: node:18  # Image with npm pre-installed
  script:
    - npm test
```

### Issue: Timeout

```yaml
# Cause: Job exceeds timeout limit
# Solution 1: Increase job timeout
long-job:
  script:
    - ./long-running-script.sh
  timeout: 3 hours

# Solution 2: Optimize script
test:
  script:
    - npm ci --prefer-offline  # Faster install
    - npm test -- --maxWorkers=4  # Parallel tests

# Solution 3: Split into multiple jobs
test-unit:
  script:
    - npm run test:unit

test-integration:
  script:
    - npm run test:integration
```

---

## Task 9.4: Artifact and Cache Issues ⭐

### Issue: Artifacts Not Found

```yaml
# Cause: Artifacts not uploaded or expired
# Solution 1: Check artifact paths
build:
  script:
    - npm run build
    - ls -la dist/  # Verify files exist
  artifacts:
    paths:
      - dist/
    expire_in: 1 week

# Solution 2: Use needs with artifacts
deploy:
  needs:
    - job: build
      artifacts: true  # Explicitly download artifacts
  script:
    - ls -la dist/
    - ./deploy.sh

# Solution 3: Check expiration
build:
  artifacts:
    paths:
      - dist/
    expire_in: never  # Don't expire
```

### Issue: Cache Not Working

```yaml
# Cause: Cache key mismatch or cache cleared
# Solution 1: Use consistent cache key
test:
  cache:
    key: ${CI_COMMIT_REF_SLUG}  # Per-branch cache
    paths:
      - node_modules/
  script:
    - npm ci
    - npm test

# Solution 2: Use file-based cache key
test:
  cache:
    key:
      files:
        - package-lock.json  # Auto-invalidate on changes
    paths:
      - node_modules/
  script:
    - npm ci
    - npm test

# Solution 3: Use fallback keys
test:
  cache:
    key: ${CI_COMMIT_REF_SLUG}
    paths:
      - node_modules/
    fallback_keys:
      - main  # Fallback to main branch cache
  script:
    - npm ci
    - npm test
```

---

## Task 9.5: Runner Issues ⭐

### Issue: Runner Not Picking Jobs

```bash
# Cause: Runner offline, tags mismatch, or concurrent limit
# Solution 1: Check runner status
sudo gitlab-runner status

# Solution 2: Verify runner registration
sudo gitlab-runner verify

# Solution 3: Check runner logs
sudo journalctl -u gitlab-runner -f

# Solution 4: Restart runner
sudo gitlab-runner restart

# Solution 5: Check concurrent limit
# /etc/gitlab-runner/config.toml
concurrent = 4  # Increase if needed
```

### Issue: Docker Permission Denied

```bash
# Cause: gitlab-runner user not in docker group
# Solution: Add user to docker group
sudo usermod -aG docker gitlab-runner
sudo systemctl restart gitlab-runner

# Verify
sudo -u gitlab-runner docker ps
```

### Issue: Out of Disk Space

```bash
# Cause: Docker images and cache buildup
# Solution 1: Clean Docker
docker system prune -a -f

# Solution 2: Clean GitLab Runner cache
sudo gitlab-runner cache-clear

# Solution 3: Automate cleanup
# Add to cron
0 2 * * * docker system prune -a -f
```

---

## Task 9.6: Performance Optimization ⭐⭐

### Pipeline Optimization

```yaml
# Use DAG pipelines (needs)
build-frontend:
  stage: build
  script:
    - npm run build:frontend

build-backend:
  stage: build
  script:
    - go build

test-frontend:
  stage: test
  needs: [build-frontend]  # Don't wait for backend
  script:
    - npm test

test-backend:
  stage: test
  needs: [build-backend]  # Don't wait for frontend
  script:
    - go test
```

### Cache Optimization

```yaml
# Use cache policy
setup:
  stage: .pre
  cache:
    key:
      files:
        - package-lock.json
    paths:
      - node_modules/
    policy: push  # Only upload
  script:
    - npm ci

test:
  stage: test
  cache:
    key:
      files:
        - package-lock.json
    paths:
      - node_modules/
    policy: pull  # Only download
  script:
    - npm test
```

### Parallel Jobs

```yaml
# Run tests in parallel
test:
  script:
    - npm test -- --shard=$CI_NODE_INDEX/$CI_NODE_TOTAL
  parallel: 5  # Split into 5 jobs
```

### Image Optimization

```yaml
# Use smaller images
build:
  image: node:18-alpine  # Smaller than node:18
  script:
    - npm run build

# Use specific versions
build:
  image: node:18.17.0-alpine  # Specific version, better caching
  script:
    - npm run build
```

---

## Task 9.7: CI/CD Analytics ⭐

### Pipeline Analytics

```yaml
# View in GitLab UI:
# Project > Analytics > CI/CD Analytics

# Metrics:
# - Pipeline success rate
# - Pipeline duration
# - Job duration
# - Most failed jobs
# - Pipeline trends
```

### Custom Metrics

```yaml
# Export metrics
metrics:
  stage: .post
  script:
    - |
      echo "pipeline_duration_seconds $CI_PIPELINE_DURATION" >> metrics.txt
      echo "pipeline_id $CI_PIPELINE_ID" >> metrics.txt
      echo "commit_sha $CI_COMMIT_SHA" >> metrics.txt
  artifacts:
    reports:
      metrics: metrics.txt
  when: always
```

### DORA Metrics

```yaml
# Deployment Frequency
# Lead Time for Changes
# Mean Time to Recovery
# Change Failure Rate

# View in GitLab UI:
# Project > Analytics > Value Stream Analytics
```

---

## Task 9.8: Troubleshooting Workflows ⭐

### Debug Workflow

```yaml
# 1. Check pipeline status
# Project > CI/CD > Pipelines

# 2. View failed job logs
# Click on failed job

# 3. Enable debug mode
debug:
  variables:
    CI_DEBUG_TRACE: "true"
  script:
    - echo "Debugging..."
    - env | sort
  when: manual

# 4. Reproduce locally
# docker run -it node:18-alpine sh
# npm install
# npm test

# 5. Fix and retry
# Update .gitlab-ci.yml
# git commit && git push
```

### Common Debugging Commands

```yaml
debug:
  script:
    # Check environment
    - env | sort
    - pwd
    - ls -la
    
    # Check system
    - uname -a
    - df -h
    - free -m
    
    # Check tools
    - which node npm docker kubectl
    - node --version
    - npm --version
    
    # Check network
    - ping -c 3 gitlab.com
    - curl -I https://gitlab.com
    
    # Check files
    - cat .gitlab-ci.yml
    - cat package.json
```

---

## Task 9.9: Error Messages and Solutions ⭐⭐

### Common Error Messages

| Error | Cause | Solution |
|-------|-------|----------|
| **No runner available** | No runners with matching tags | Check runner tags or remove tags |
| **Image pull failed** | Invalid image or auth | Use correct image name and credentials |
| **Command not found** | Missing dependency | Install in before_script or use different image |
| **Permission denied** | File permissions | Use chmod or run as correct user |
| **Timeout** | Job too long | Increase timeout or optimize script |
| **Out of memory** | Insufficient memory | Use smaller image or increase runner memory |
| **Artifacts not found** | Expired or not uploaded | Check artifact paths and expiration |
| **Cache not found** | Cache key mismatch | Use consistent cache key |
| **YAML syntax error** | Invalid YAML | Use CI Lint to validate |
| **Variable not set** | Missing variable | Define in UI or .gitlab-ci.yml |

### YAML Validation

```bash
# Use CI Lint
# Project > CI/CD > Editor > Validate

# Or use API
curl --header "PRIVATE-TOKEN: <token>" \
  "https://gitlab.com/api/v4/projects/:id/ci/lint" \
  --data "content=$(cat .gitlab-ci.yml)"
```

---

## Task 9.10: Best Practices for Reliability ⭐

### Reliable Pipelines

```yaml
# Use specific image versions
build:
  image: node:18.17.0-alpine  # Not node:latest
  script:
    - npm run build

# Add retry for flaky tests
test:
  script:
    - npm test
  retry:
    max: 2
    when:
      - runner_system_failure
      - stuck_or_timeout_failure

# Use health checks
deploy:
  script:
    - ./deploy.sh
    - ./health-check.sh || (./rollback.sh && exit 1)

# Add timeouts
long-job:
  script:
    - ./long-script.sh
  timeout: 1 hour

# Use allow_failure for optional jobs
experimental:
  script:
    - npm run experimental-tests
  allow_failure: true
```

### Monitoring and Alerting

```yaml
# Monitor pipeline health
monitor:
  stage: .post
  script:
    - |
      if [ "$CI_PIPELINE_STATUS" == "failed" ]; then
        curl -X POST $ALERT_WEBHOOK \
          -d "Pipeline $CI_PIPELINE_ID failed"
      fi
  when: always

# Track metrics
metrics:
  stage: .post
  script:
    - |
      curl -X POST $METRICS_ENDPOINT \
        -d "pipeline_duration=$CI_PIPELINE_DURATION"
  when: always
```

---

## Exam Tips for Domain 9

1. **Job Logs** - Know how to read and interpret logs
2. **Debug Mode** - Use CI_DEBUG_TRACE for troubleshooting
3. **Common Issues** - Recognize pending, timeout, permission errors
4. **Runner Issues** - Know how to check runner status
5. **Cache Issues** - Understand cache key and fallback
6. **Artifact Issues** - Know expiration and download
7. **Performance** - Use DAG, parallel, cache optimization
8. **Error Messages** - Recognize common errors and solutions
9. **YAML Validation** - Use CI Lint
10. **Monitoring** - Track pipeline metrics and status

---

## Quick Reference

### Debug Job

```yaml
debug:
  variables:
    CI_DEBUG_TRACE: "true"
  script:
    - env | sort
    - pwd
    - ls -la
    - which node npm docker
  when: manual
```

### Common Fixes

```yaml
# Fix: No runner available
job:
  tags: []  # Remove tags or use correct tags

# Fix: Image pull failed
job:
  image: node:18-alpine  # Use correct image

# Fix: Timeout
job:
  timeout: 2 hours  # Increase timeout

# Fix: Cache not working
job:
  cache:
    key:
      files:
        - package-lock.json  # File-based key
    paths:
      - node_modules/
```

### Runner Commands

```bash
# Check status
sudo gitlab-runner status

# View logs
sudo journalctl -u gitlab-runner -f

# Restart
sudo gitlab-runner restart

# Verify
sudo gitlab-runner verify

# Clean cache
docker system prune -a -f
```
