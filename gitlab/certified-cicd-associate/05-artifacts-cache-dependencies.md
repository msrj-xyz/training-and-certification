# Domain 5: Artifacts, Cache & Dependencies

## Task 5.1: Artifact Management ⭐⭐

### Artifact Configuration

```yaml
build:
  script:
    - npm run build
    - npm run test
  artifacts:
    paths:
      - dist/
      - build/
      - public/
    exclude:
      - "**/*.log"
      - "**/*.tmp"
      - dist/test/
    expire_in: 1 week
    when: on_success
    name: "$CI_JOB_NAME-$CI_COMMIT_REF_SLUG"
    expose_as: "Build Artifacts"
```

### Artifact Paths

```yaml
# Single path
artifacts:
  paths:
    - dist/

# Multiple paths
artifacts:
  paths:
    - dist/
    - build/
    - coverage/
    - docs/

# Wildcard patterns
artifacts:
  paths:
    - "*.jar"
    - "target/**/*.war"
    - "build/**/output"

# Exclude patterns
artifacts:
  paths:
    - dist/
  exclude:
    - "dist/**/*.map"
    - "dist/**/*.log"
    - "dist/test/**/*"
```

### Artifact Expiration

```yaml
# Short-term artifacts
test:
  artifacts:
    paths:
      - test-results/
    expire_in: 1 day

# Medium-term artifacts
build:
  artifacts:
    paths:
      - dist/
    expire_in: 1 week

# Long-term artifacts
release:
  artifacts:
    paths:
      - release/
    expire_in: 1 year

# Permanent artifacts
archive:
  artifacts:
    paths:
      - archive/
    expire_in: never
```

### Conditional Artifact Saving

```yaml
# Save on success (default)
build:
  script:
    - npm run build
  artifacts:
    paths:
      - dist/
    when: on_success

# Save on failure
test:
  script:
    - npm test
  artifacts:
    paths:
      - test-results/
      - screenshots/
    when: on_failure

# Always save
test-with-coverage:
  script:
    - npm test
  artifacts:
    paths:
      - coverage/
      - test-results/
    when: always
```

---

## Task 5.2: Artifact Reports ⭐⭐

### JUnit Test Reports

```yaml
test:
  script:
    - npm test
  artifacts:
    reports:
      junit: test-results.xml

# Multiple test result files
test:
  script:
    - npm test
  artifacts:
    reports:
      junit:
        - test-results/unit.xml
        - test-results/integration.xml
        - test-results/e2e.xml
```

### Coverage Reports

```yaml
test:
  script:
    - npm test
  artifacts:
    reports:
      coverage_report:
        coverage_format: cobertura
        path: coverage/cobertura-coverage.xml

# Multiple coverage formats
test:
  artifacts:
    reports:
      coverage_report:
        coverage_format: cobertura
        path: coverage/cobertura.xml
    paths:
      - coverage/  # HTML coverage report
```

### Security Reports

```yaml
# SAST (Static Application Security Testing)
sast:
  script:
    - ./security-scan.sh
  artifacts:
    reports:
      sast: gl-sast-report.json

# Dependency Scanning
dependency_scanning:
  script:
    - ./dependency-scan.sh
  artifacts:
    reports:
      dependency_scanning: gl-dependency-scanning-report.json

# Container Scanning
container_scanning:
  script:
    - ./container-scan.sh
  artifacts:
    reports:
      container_scanning: gl-container-scanning-report.json

# DAST (Dynamic Application Security Testing)
dast:
  script:
    - ./dast-scan.sh
  artifacts:
    reports:
      dast: gl-dast-report.json
```

### Performance Reports

```yaml
# Load performance
load_performance:
  script:
    - ./load-test.sh
  artifacts:
    reports:
      load_performance: load-performance.json

# Browser performance
browser_performance:
  script:
    - ./browser-test.sh
  artifacts:
    reports:
      browser_performance: browser-performance.json

# Metrics
metrics:
  script:
    - ./collect-metrics.sh
  artifacts:
    reports:
      metrics: metrics.txt
```

### Dotenv Reports

```yaml
# Export variables to subsequent jobs
build:
  script:
    - echo "BUILD_VERSION=1.2.3" >> build.env
    - echo "BUILD_DATE=$(date)" >> build.env
    - echo "DOCKER_IMAGE=$CI_REGISTRY_IMAGE:$CI_COMMIT_SHORT_SHA" >> build.env
  artifacts:
    reports:
      dotenv: build.env

# Use exported variables
deploy:
  needs:
    - build
  script:
    - echo "Deploying version $BUILD_VERSION"
    - echo "Built on $BUILD_DATE"
    - docker pull $DOCKER_IMAGE
```

---

## Task 5.3: Cache Strategies ⭐⭐

### Cache Key Strategies

```yaml
# Per-branch cache
cache:
  key: ${CI_COMMIT_REF_SLUG}
  paths:
    - node_modules/

# Per-job cache
cache:
  key: ${CI_JOB_NAME}
  paths:
    - .cache/

# File-based cache (auto-invalidation)
cache:
  key:
    files:
      - package-lock.json
  paths:
    - node_modules/

# Prefix + files
cache:
  key:
    prefix: ${CI_COMMIT_REF_SLUG}
    files:
      - package-lock.json
      - yarn.lock
  paths:
    - node_modules/

# Multiple lock files
cache:
  key:
    files:
      - Gemfile.lock
      - package-lock.json
      - composer.lock
  paths:
    - vendor/
    - node_modules/
```

### Cache Policies

```yaml
# Setup job: Only push cache
setup-dependencies:
  stage: .pre
  script:
    - npm ci
  cache:
    key:
      files:
        - package-lock.json
    paths:
      - node_modules/
    policy: push

# Build/test jobs: Only pull cache
build:
  stage: build
  script:
    - npm run build
  cache:
    key:
      files:
        - package-lock.json
    paths:
      - node_modules/
    policy: pull

test:
  stage: test
  script:
    - npm test
  cache:
    key:
      files:
        - package-lock.json
    paths:
      - node_modules/
    policy: pull
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
    - master

# Multiple fallbacks
cache:
  key: feature-${CI_COMMIT_REF_SLUG}
  paths:
    - .cache/
  fallback_keys:
    - feature-main
    - develop
    - main
```

### Multiple Caches

```yaml
# Cache different dependencies separately
cache:
  - key:
      files:
        - package-lock.json
    paths:
      - node_modules/
      - .npm/
  - key:
      files:
        - Gemfile.lock
    paths:
      - vendor/ruby
  - key:
      files:
        - requirements.txt
    paths:
      - venv/
```

---

## Task 5.4: Cache vs Artifacts Decision Matrix ⭐⭐

### When to Use Cache

```yaml
# ✅ Good: Dependencies
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

# ✅ Good: Compiler cache
build-cpp:
  cache:
    key: ${CI_COMMIT_REF_SLUG}
    paths:
      - .ccache/
  script:
    - make build

# ✅ Good: Package manager cache
build-python:
  cache:
    key:
      files:
        - requirements.txt
    paths:
      - .pip-cache/
  script:
    - pip install -r requirements.txt
```

### When to Use Artifacts

```yaml
# ✅ Good: Build outputs
build:
  script:
    - npm run build
  artifacts:
    paths:
      - dist/

# ✅ Good: Test results
test:
  script:
    - npm test
  artifacts:
    paths:
      - test-results/
    reports:
      junit: test-results.xml

# ✅ Good: Compiled binaries
compile:
  script:
    - make
  artifacts:
    paths:
      - bin/
      - lib/
```

### Anti-patterns

```yaml
# ❌ Bad: Build outputs in cache
build:
  cache:
    paths:
      - dist/  # Wrong! Use artifacts
  script:
    - npm run build

# ❌ Bad: Dependencies in artifacts
build:
  artifacts:
    paths:
      - node_modules/  # Wrong! Use cache
  script:
    - npm install
```

---

## Task 5.5: Dependency Management ⭐

### Node.js Dependencies

```yaml
# npm with cache
build:
  image: node:18
  cache:
    key:
      files:
        - package-lock.json
    paths:
      - node_modules/
      - .npm/
  before_script:
    - npm ci --cache .npm --prefer-offline
  script:
    - npm run build

# Yarn with cache
build:
  image: node:18
  cache:
    key:
      files:
        - yarn.lock
    paths:
      - node_modules/
      - .yarn-cache/
  before_script:
    - yarn install --frozen-lockfile --cache-folder .yarn-cache
  script:
    - yarn build

# pnpm with cache
build:
  image: node:18
  cache:
    key:
      files:
        - pnpm-lock.yaml
    paths:
      - .pnpm-store/
  before_script:
    - corepack enable
    - pnpm config set store-dir .pnpm-store
    - pnpm install --frozen-lockfile
  script:
    - pnpm build
```

### Python Dependencies

```yaml
# pip with cache
test:
  image: python:3.11
  cache:
    key:
      files:
        - requirements.txt
    paths:
      - .pip-cache/
  before_script:
    - pip install --cache-dir .pip-cache -r requirements.txt
  script:
    - pytest

# Poetry with cache
test:
  image: python:3.11
  cache:
    key:
      files:
        - poetry.lock
    paths:
      - .venv/
  before_script:
    - pip install poetry
    - poetry config virtualenvs.in-project true
    - poetry install
  script:
    - poetry run pytest
```

### Ruby Dependencies

```yaml
# Bundler with cache
test:
  image: ruby:3.2
  cache:
    key:
      files:
        - Gemfile.lock
    paths:
      - vendor/ruby
  before_script:
    - bundle config set --local path 'vendor/ruby'
    - bundle install
  script:
    - bundle exec rspec
```

### Go Dependencies

```yaml
# Go modules with cache
build:
  image: golang:1.21
  cache:
    key:
      files:
        - go.sum
    paths:
      - .go-cache/
  variables:
    GOCACHE: ${CI_PROJECT_DIR}/.go-cache
  before_script:
    - go mod download
  script:
    - go build
```

### Java/Maven Dependencies

```yaml
# Maven with cache
build:
  image: maven:3.9-eclipse-temurin-17
  cache:
    key:
      files:
        - pom.xml
    paths:
      - .m2/repository
  variables:
    MAVEN_OPTS: "-Dmaven.repo.local=${CI_PROJECT_DIR}/.m2/repository"
  script:
    - mvn clean package
```

### PHP/Composer Dependencies

```yaml
# Composer with cache
build:
  image: php:8.2
  cache:
    key:
      files:
        - composer.lock
    paths:
      - vendor/
      - .composer-cache/
  before_script:
    - composer install --prefer-dist --no-progress --no-interaction --optimize-autoloader
  script:
    - php artisan test
```

---

## Task 5.6: Artifact Download Control ⭐

### Selective Artifact Download

```yaml
build-frontend:
  stage: build
  script:
    - npm run build:frontend
  artifacts:
    paths:
      - dist/frontend/

build-backend:
  stage: build
  script:
    - go build
  artifacts:
    paths:
      - dist/backend/

# Download only frontend artifacts
test-frontend:
  stage: test
  needs:
    - job: build-frontend
      artifacts: true
  script:
    - npm test

# Download only backend artifacts
test-backend:
  stage: test
  needs:
    - job: build-backend
      artifacts: true
  script:
    - go test

# Download both artifacts
deploy:
  stage: deploy
  needs:
    - job: build-frontend
      artifacts: true
    - job: build-backend
      artifacts: true
  script:
    - ./deploy.sh

# Download no artifacts
notify:
  stage: .post
  needs:
    - job: build-frontend
      artifacts: false
    - job: build-backend
      artifacts: false
  script:
    - ./notify.sh
```

### Dependencies Keyword

```yaml
# Download specific artifacts
deploy:
  stage: deploy
  dependencies:
    - build-frontend
    - build-backend
  script:
    - ./deploy.sh

# Download no artifacts
cleanup:
  stage: .post
  dependencies: []
  script:
    - ./cleanup.sh
```

---

## Task 5.7: Artifact Storage and Limits ⭐

### Artifact Size Optimization

```yaml
# Compress artifacts
build:
  script:
    - npm run build
    - tar -czf dist.tar.gz dist/
  artifacts:
    paths:
      - dist.tar.gz
    expire_in: 1 week

# Exclude unnecessary files
build:
  script:
    - npm run build
  artifacts:
    paths:
      - dist/
    exclude:
      - "dist/**/*.map"
      - "dist/**/*.log"
      - "dist/test/**/*"
      - "dist/**/*.md"
```

### Artifact Cleanup

```yaml
# Short expiration for temporary artifacts
test:
  artifacts:
    paths:
      - test-results/
    expire_in: 1 day

# Longer expiration for important artifacts
build:
  artifacts:
    paths:
      - dist/
    expire_in: 1 month

# Keep release artifacts
release:
  artifacts:
    paths:
      - release/
    expire_in: never
  only:
    - tags
```

---

## Task 5.8: Cache Clearing and Troubleshooting ⭐

### Clear Cache

```yaml
# Clear cache job
clear-cache:
  stage: .pre
  script:
    - echo "Cache cleared"
  cache:
    key: ${CI_COMMIT_REF_SLUG}
    paths:
      - node_modules/
    policy: push
  when: manual

# Force cache rebuild
rebuild-cache:
  stage: .pre
  script:
    - rm -rf node_modules/
    - npm ci
  cache:
    key: ${CI_COMMIT_REF_SLUG}-rebuild
    paths:
      - node_modules/
    policy: push
  when: manual
```

### Cache Debugging

```yaml
# Debug cache
test:
  script:
    - echo "Cache key: ${CI_COMMIT_REF_SLUG}"
    - ls -la node_modules/ || echo "No cache found"
    - npm ci
    - npm test
  cache:
    key: ${CI_COMMIT_REF_SLUG}
    paths:
      - node_modules/
```

---

## Task 5.9: Advanced Artifact Patterns ⭐

### Artifact Naming

```yaml
# Dynamic artifact names
build:
  script:
    - npm run build
  artifacts:
    name: "$CI_JOB_NAME-$CI_COMMIT_REF_SLUG-$CI_COMMIT_SHORT_SHA"
    paths:
      - dist/

# Date-based naming
build:
  script:
    - npm run build
  artifacts:
    name: "build-$(date +%Y%m%d-%H%M%S)"
    paths:
      - dist/
```

### Artifact Exposure

```yaml
# Expose artifacts in MR
build:
  script:
    - npm run build
  artifacts:
    expose_as: "Build Output"
    paths:
      - dist/

# Multiple exposed artifacts
test:
  script:
    - npm test
  artifacts:
    expose_as: "Test Results"
    paths:
      - test-results/
    reports:
      junit: test-results.xml
```

### Untracked Files

```yaml
# Include untracked files
build:
  script:
    - ./generate-files.sh
  artifacts:
    untracked: true
    paths:
      - generated/

# Exclude specific untracked files
build:
  script:
    - ./generate-files.sh
  artifacts:
    untracked: true
    exclude:
      - "*.log"
      - "*.tmp"
```

---

## Task 5.10: Monorepo Artifact Strategies ⭐

### Monorepo Cache

```yaml
# Per-package cache
build-frontend:
  script:
    - cd packages/frontend
    - npm ci
    - npm run build
  cache:
    key:
      files:
        - packages/frontend/package-lock.json
    paths:
      - packages/frontend/node_modules/

build-backend:
  script:
    - cd packages/backend
    - npm ci
    - npm run build
  cache:
    key:
      files:
        - packages/backend/package-lock.json
    paths:
      - packages/backend/node_modules/

# Shared cache
build-all:
  script:
    - npm ci
    - npm run build
  cache:
    key:
      files:
        - package-lock.json
    paths:
      - node_modules/
      - packages/*/node_modules/
```

### Monorepo Artifacts

```yaml
# Per-package artifacts
build-frontend:
  script:
    - npm run build:frontend
  artifacts:
    paths:
      - packages/frontend/dist/

build-backend:
  script:
    - npm run build:backend
  artifacts:
    paths:
      - packages/backend/dist/

# Combined artifacts
build-all:
  script:
    - npm run build
  artifacts:
    paths:
      - packages/*/dist/
```

---

## Exam Tips for Domain 5

1. **Artifacts vs Cache** - Know when to use each
2. **Cache Keys** - Understand file-based keys
3. **Cache Policies** - Know pull, push, pull-push
4. **Artifact Reports** - JUnit, coverage, security
5. **Expiration** - Set appropriate expire_in
6. **Dependencies** - Control artifact downloads
7. **Dotenv** - Pass variables between jobs
8. **Cache Fallback** - Use fallback_keys
9. **Exclude Patterns** - Optimize artifact size
10. **Multiple Caches** - Cache different dependencies

---

## Quick Reference

### Cache Template

```yaml
cache:
  key:
    files:
      - package-lock.json
  paths:
    - node_modules/
  policy: pull-push
  fallback_keys:
    - main
```

### Artifact Template

```yaml
artifacts:
  paths:
    - dist/
  exclude:
    - "**/*.log"
  expire_in: 1 week
  when: on_success
  reports:
    junit: test-results.xml
```
