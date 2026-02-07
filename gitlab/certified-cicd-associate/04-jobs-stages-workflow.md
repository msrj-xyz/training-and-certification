# Domain 4: Jobs, Stages & Workflow

## Task 4.1: Job Dependencies (needs) ⭐⭐

### DAG (Directed Acyclic Graph) Pipelines

```yaml
# Traditional sequential pipeline
stages:
  - build
  - test
  - deploy

build:
  stage: build
  script: make build

test:
  stage: test
  script: make test

deploy:
  stage: deploy
  script: make deploy

# DAG pipeline with needs
build-frontend:
  stage: build
  script: npm run build:frontend

build-backend:
  stage: build
  script: go build

test-frontend:
  stage: test
  needs: [build-frontend]  # Only wait for frontend build
  script: npm test

test-backend:
  stage: test
  needs: [build-backend]  # Only wait for backend build
  script: go test

deploy:
  stage: deploy
  needs: [test-frontend, test-backend]  # Wait for both tests
  script: ./deploy.sh
```

### Needs vs Dependencies

| Feature | needs | dependencies |
|---------|-------|--------------|
| **Purpose** | Job execution order | Artifact download |
| **Stage** | Can skip stages | Same stage or earlier |
| **Performance** | Faster (parallel) | No impact on order |
| **Artifacts** | Downloads by default | Explicit control |

```yaml
# needs: Controls execution order + downloads artifacts
test:
  needs: [build]
  script: npm test

# needs with artifacts control
test:
  needs:
    - job: build
      artifacts: true  # Download artifacts (default)
  script: npm test

test-no-artifacts:
  needs:
    - job: build
      artifacts: false  # Don't download artifacts
  script: npm test

# dependencies: Only controls artifacts
test:
  dependencies: [build]  # Download artifacts from build
  script: npm test

test-no-deps:
  dependencies: []  # Don't download any artifacts
  script: npm test
```

---

## Task 4.2: Job Execution Control ⭐⭐

### When Keyword

```yaml
# on_success: Run if previous jobs succeeded (default)
deploy:
  stage: deploy
  when: on_success
  script: ./deploy.sh

# on_failure: Run if previous job failed
notify-failure:
  stage: .post
  when: on_failure
  script: ./notify-slack.sh "Build failed"

# always: Always run regardless of status
cleanup:
  stage: .post
  when: always
  script: ./cleanup.sh

# manual: Require manual trigger
deploy-production:
  stage: deploy
  when: manual
  script: ./deploy-prod.sh

# delayed: Run after delay
deploy-canary:
  stage: deploy
  when: delayed
  start_in: 30 minutes
  script: ./deploy-canary.sh

# never: Never run (used with rules)
skip-job:
  when: never
  script: echo "This won't run"
```

### Allow Failure

```yaml
# Job failure doesn't fail pipeline
experimental-test:
  script: npm run experimental-tests
  allow_failure: true

# Conditional allow_failure
test:
  script: npm test
  allow_failure:
    exit_codes: [137, 255]  # Allow specific exit codes

# With rules
test:
  script: npm test
  rules:
    - if: '$CI_COMMIT_BRANCH == "main"'
      allow_failure: false
    - allow_failure: true
```

### Retry

```yaml
# Retry up to 2 times
test:
  script: npm test
  retry: 2

# Retry with conditions
test:
  script: npm test
  retry:
    max: 2
    when:
      - runner_system_failure
      - stuck_or_timeout_failure
      - script_failure

# Retry options
test:
  script: npm test
  retry:
    max: 3
    when:
      - always  # Retry on any failure
      - unknown_failure
      - script_failure
      - api_failure
      - stuck_or_timeout_failure
      - runner_system_failure
      - runner_unsupported
      - stale_schedule
      - job_execution_timeout
      - archived_failure
      - unmet_prerequisites
      - scheduler_failure
      - data_integrity_failure
```

### Timeout

```yaml
# Job-level timeout
long-running-job:
  script: ./long-process.sh
  timeout: 3 hours

# Different timeout formats
job1:
  timeout: 1h 30m
  script: ./script.sh

job2:
  timeout: 90 minutes
  script: ./script.sh

job3:
  timeout: 5400 seconds
  script: ./script.sh
```

---

## Task 4.3: Job Interruptibility ⭐

### Interruptible Jobs

```yaml
# Allow job to be interrupted by newer pipeline
build:
  script: make build
  interruptible: true  # Can be cancelled

# Prevent interruption
deploy:
  script: ./deploy.sh
  interruptible: false  # Cannot be cancelled

# Use case: Cancel old MR pipelines
test:
  script: npm test
  interruptible: true
  rules:
    - if: '$CI_PIPELINE_SOURCE == "merge_request_event"'
```

### Auto-cancel Redundant Pipelines

```yaml
# Project Settings > CI/CD > General pipelines
# Enable: "Auto-cancel redundant pipelines"

# Behavior:
# - New pipeline cancels older running pipelines
# - Only for same ref (branch/tag)
# - Respects interruptible setting
```

---

## Task 4.4: Job Artifacts ⭐⭐

### Basic Artifacts

```yaml
build:
  script:
    - npm run build
  artifacts:
    paths:
      - dist/
      - build/
    expire_in: 1 week

# Multiple artifact configurations
test:
  script:
    - npm test
  artifacts:
    paths:
      - coverage/
    reports:
      junit: test-results.xml
      coverage_report:
        coverage_format: cobertura
        path: coverage/cobertura.xml
    expire_in: 30 days
    when: always  # Save even on failure
```

### Artifact Options

| Option | Description | Example |
|--------|-------------|---------|
| **paths** | Files/directories to save | `paths: - dist/` |
| **exclude** | Files to exclude | `exclude: - "*.log"` |
| **expire_in** | Retention period | `expire_in: 1 week` |
| **when** | When to save | `when: always` |
| **name** | Archive name | `name: "$CI_JOB_NAME-$CI_COMMIT_REF_NAME"` |
| **expose_as** | UI display name | `expose_as: "Build artifacts"` |
| **reports** | Special reports | `reports: junit: test.xml` |
| **untracked** | Include untracked files | `untracked: true` |
| **public** | Public access | `public: false` |

### Artifact Expiration

```yaml
# Time units
artifacts:
  expire_in: 30 seconds
  expire_in: 5 minutes
  expire_in: 2 hours
  expire_in: 3 days
  expire_in: 4 weeks
  expire_in: 6 months
  expire_in: 1 year
  expire_in: never  # Keep forever

# Combined units
artifacts:
  expire_in: 1 week 2 days 3 hours
```

### Artifact Reports

```yaml
# JUnit test reports
test:
  script: npm test
  artifacts:
    reports:
      junit: test-results.xml

# Code coverage
test:
  script: npm test
  artifacts:
    reports:
      coverage_report:
        coverage_format: cobertura
        path: coverage/cobertura.xml

# SAST security scanning
sast:
  script: ./security-scan.sh
  artifacts:
    reports:
      sast: gl-sast-report.json

# Multiple reports
test:
  script: npm test
  artifacts:
    reports:
      junit: test-results.xml
      coverage_report:
        coverage_format: cobertura
        path: coverage/cobertura.xml
      dotenv: build.env
```

### Downloading Artifacts

```yaml
# Automatic download with needs
deploy:
  needs:
    - job: build
      artifacts: true  # Download artifacts (default)
  script:
    - ls dist/  # Artifacts available

# Explicit dependencies
deploy:
  dependencies:
    - build
  script:
    - ls dist/

# No artifacts
deploy:
  needs:
    - job: build
      artifacts: false  # Don't download
  script:
    - ./deploy.sh
```

---

## Task 4.5: Job Cache ⭐⭐

### Cache vs Artifacts

| Feature | Cache | Artifacts |
|---------|-------|-----------|
| **Purpose** | Speed up builds | Pass data between jobs |
| **Scope** | Runner-specific | Pipeline-wide |
| **Reliability** | Best effort | Guaranteed |
| **Use Case** | Dependencies | Build outputs |
| **Storage** | Runner or S3 | GitLab storage |

### Basic Cache

```yaml
# Global cache
cache:
  paths:
    - node_modules/
  key: ${CI_COMMIT_REF_SLUG}

# Job-level cache
build:
  script:
    - npm install
    - npm run build
  cache:
    paths:
      - node_modules/
    key: ${CI_COMMIT_REF_SLUG}
```

### Cache Keys

```yaml
# Static key
cache:
  key: my-cache
  paths:
    - vendor/

# Dynamic key with branch
cache:
  key: ${CI_COMMIT_REF_SLUG}
  paths:
    - node_modules/

# Key with files (cache invalidation)
cache:
  key:
    files:
      - package-lock.json
  paths:
    - node_modules/

# Prefix with files
cache:
  key:
    prefix: ${CI_COMMIT_REF_SLUG}
    files:
      - Gemfile.lock
  paths:
    - vendor/ruby

# Multiple caches
cache:
  - key:
      files:
        - package-lock.json
    paths:
      - node_modules/
  - key:
      files:
        - Gemfile.lock
    paths:
      - vendor/ruby
```

### Cache Policy

```yaml
# pull-push: Download and upload (default)
build:
  cache:
    key: ${CI_COMMIT_REF_SLUG}
    paths:
      - node_modules/
    policy: pull-push
  script:
    - npm install
    - npm run build

# pull: Only download
test:
  cache:
    key: ${CI_COMMIT_REF_SLUG}
    paths:
      - node_modules/
    policy: pull
  script:
    - npm test

# push: Only upload
setup:
  cache:
    key: ${CI_COMMIT_REF_SLUG}
    paths:
      - node_modules/
    policy: push
  script:
    - npm install
```

### Cache Fallback

```yaml
# Fallback to main branch cache
cache:
  key: ${CI_COMMIT_REF_SLUG}
  paths:
    - node_modules/
  fallback_keys:
    - main
    - develop
```

### When to Use Cache

```yaml
# Good: Dependencies
build:
  cache:
    paths:
      - node_modules/
      - .npm/
      - vendor/
  script:
    - npm ci
    - npm run build

# Bad: Build outputs (use artifacts instead)
build:
  cache:
    paths:
      - dist/  # Wrong! Use artifacts
  script:
    - npm run build
```

---

## Task 4.6: Job Services ⭐

### Service Containers

```yaml
# Single service
test:
  image: node:18
  services:
    - postgres:14
  variables:
    POSTGRES_DB: testdb
    POSTGRES_USER: user
    POSTGRES_PASSWORD: password
  script:
    - npm test

# Multiple services
integration-test:
  image: node:18
  services:
    - postgres:14
    - redis:7
    - mongo:6
  variables:
    POSTGRES_DB: testdb
    REDIS_HOST: redis
    MONGO_HOST: mongo
  script:
    - npm run test:integration
```

### Service Configuration

```yaml
# Service with alias
test:
  image: node:18
  services:
    - name: postgres:14
      alias: database
  variables:
    DATABASE_HOST: database  # Use alias
  script:
    - npm test

# Service with command
test:
  services:
    - name: postgres:14
      command: ["postgres", "-c", "log_statement=all"]
  script:
    - npm test

# Service with entrypoint
test:
  services:
    - name: custom-service:latest
      entrypoint: ["/bin/sh", "-c"]
      command: ["custom-command"]
  script:
    - npm test
```

### Common Services

```yaml
# PostgreSQL
services:
  - postgres:14
variables:
  POSTGRES_DB: testdb
  POSTGRES_USER: user
  POSTGRES_PASSWORD: password
  POSTGRES_HOST_AUTH_METHOD: trust

# MySQL
services:
  - mysql:8
variables:
  MYSQL_DATABASE: testdb
  MYSQL_ROOT_PASSWORD: password

# Redis
services:
  - redis:7

# MongoDB
services:
  - mongo:6
variables:
  MONGO_INITDB_DATABASE: testdb

# Docker-in-Docker
services:
  - docker:dind
variables:
  DOCKER_TLS_CERTDIR: "/certs"
```

---

## Task 4.7: Job Environment ⭐

### Environment Deployment

```yaml
# Basic environment
deploy-staging:
  stage: deploy
  script:
    - ./deploy.sh staging
  environment:
    name: staging
    url: https://staging.example.com

# Production with manual trigger
deploy-production:
  stage: deploy
  script:
    - ./deploy.sh production
  environment:
    name: production
    url: https://example.com
  when: manual

# Dynamic environment
deploy-review:
  stage: deploy
  script:
    - ./deploy.sh review-$CI_COMMIT_REF_SLUG
  environment:
    name: review/$CI_COMMIT_REF_SLUG
    url: https://$CI_COMMIT_REF_SLUG.example.com
    on_stop: stop-review
  rules:
    - if: '$CI_PIPELINE_SOURCE == "merge_request_event"'

stop-review:
  stage: deploy
  script:
    - ./cleanup.sh review-$CI_COMMIT_REF_SLUG
  environment:
    name: review/$CI_COMMIT_REF_SLUG
    action: stop
  when: manual
  rules:
    - if: '$CI_PIPELINE_SOURCE == "merge_request_event"'
```

### Environment Options

| Option | Description | Example |
|--------|-------------|---------|
| **name** | Environment name | `name: production` |
| **url** | Deployment URL | `url: https://example.com` |
| **on_stop** | Cleanup job | `on_stop: stop-review` |
| **action** | Environment action | `action: stop` |
| **auto_stop_in** | Auto cleanup | `auto_stop_in: 1 day` |
| **kubernetes** | K8s namespace | `kubernetes: namespace: prod` |
| **deployment_tier** | Tier classification | `deployment_tier: production` |

### Deployment Tiers

```yaml
deploy-dev:
  environment:
    name: development
    deployment_tier: development

deploy-staging:
  environment:
    name: staging
    deployment_tier: staging

deploy-prod:
  environment:
    name: production
    deployment_tier: production
```

---

## Task 4.8: Job Resource Management ⭐

### Resource Group

```yaml
# Ensure only one deployment at a time
deploy:
  stage: deploy
  script:
    - ./deploy.sh
  resource_group: production
  environment:
    name: production

# Multiple jobs sharing resource
deploy-app:
  script: ./deploy-app.sh
  resource_group: production

deploy-db:
  script: ./deploy-db.sh
  resource_group: production
```

### Job Concurrency

```yaml
# Limit concurrent jobs
deploy:
  script: ./deploy.sh
  resource_group: ${CI_ENVIRONMENT_NAME}
  environment:
    name: production

# Per-environment concurrency
deploy-staging:
  script: ./deploy.sh
  resource_group: staging
  environment:
    name: staging

deploy-production:
  script: ./deploy.sh
  resource_group: production
  environment:
    name: production
```

---

## Task 4.9: Job Inheritance ⭐

### Default Keyword

```yaml
# Global defaults
default:
  image: node:18
  before_script:
    - npm ci
  cache:
    paths:
      - node_modules/
  retry: 2
  timeout: 1h

# Jobs inherit defaults
test:
  script:
    - npm test

build:
  script:
    - npm run build

# Override defaults
deploy:
  image: alpine:latest  # Override image
  before_script: []  # Clear before_script
  script:
    - ./deploy.sh
```

### Extends Keyword

```yaml
# Template job
.deploy-template:
  stage: deploy
  script:
    - ./deploy.sh
  only:
    - main

# Extend template
deploy-staging:
  extends: .deploy-template
  environment:
    name: staging

deploy-production:
  extends: .deploy-template
  environment:
    name: production
  when: manual
```

---

## Task 4.10: Job Workflow ⭐

### Workflow Rules

```yaml
# Control when pipeline runs
workflow:
  rules:
    - if: '$CI_PIPELINE_SOURCE == "merge_request_event"'
    - if: '$CI_COMMIT_BRANCH == "main"'
    - if: '$CI_COMMIT_TAG'

# Prevent duplicate pipelines
workflow:
  rules:
    - if: '$CI_PIPELINE_SOURCE == "merge_request_event"'
    - if: '$CI_COMMIT_BRANCH && $CI_OPEN_MERGE_REQUESTS'
      when: never
    - if: '$CI_COMMIT_BRANCH'

# Auto-cancel redundant pipelines
workflow:
  auto_cancel:
    on_new_commit: interruptible
```

---

## Exam Tips for Domain 4

1. **needs vs dependencies** - Understand the difference
2. **when keyword** - Know all options (on_success, manual, delayed)
3. **Artifacts** - Know paths, expire_in, reports
4. **Cache** - Understand keys, policies, fallback
5. **Services** - Know how to configure database services
6. **Environment** - Understand deployment environments
7. **Resource Group** - Prevent concurrent deployments
8. **Workflow** - Control pipeline execution
9. **Retry** - Know retry conditions
10. **Interruptible** - Cancel redundant pipelines
