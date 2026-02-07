# Domain 2: CI/CD Pipeline Configuration

## Task 2.1: .gitlab-ci.yml Basics ⭐⭐

### Pipeline Structure

```yaml
# Basic .gitlab-ci.yml structure
stages:
  - build
  - test
  - deploy

variables:
  GLOBAL_VAR: "value"

before_script:
  - echo "Runs before every job"

after_script:
  - echo "Runs after every job"

build-job:
  stage: build
  script:
    - echo "Building application"
    - make build

test-job:
  stage: test
  script:
    - echo "Running tests"
    - make test

deploy-job:
  stage: deploy
  script:
    - echo "Deploying application"
    - make deploy
```

### YAML Syntax Rules

| Rule | Description | Example |
|------|-------------|---------|
| **Indentation** | 2 spaces (no tabs) | `  key: value` |
| **Lists** | Dash with space | `- item` |
| **Strings** | Quotes optional | `"value"` or `value` |
| **Multi-line** | Pipe or greater-than | `\|` or `>` |
| **Comments** | Hash symbol | `# comment` |
| **Anchors** | Reuse config | `&anchor` and `*anchor` |

---

## Task 2.2: Job Configuration ⭐⭐

### Job Anatomy

```yaml
job-name:
  stage: test                    # Stage assignment
  image: node:18-alpine          # Docker image
  services:                      # Additional containers
    - postgres:14
  variables:                     # Job-specific variables
    DATABASE_URL: "postgres://localhost"
  before_script:                 # Pre-job commands
    - npm install
  script:                        # Main commands (required)
    - npm test
    - npm run coverage
  after_script:                  # Post-job commands
    - echo "Cleanup"
  artifacts:                     # Save outputs
    paths:
      - coverage/
    expire_in: 1 week
  cache:                         # Speed up builds
    paths:
      - node_modules/
  tags:                          # Runner selection
    - docker
    - linux
  only:                          # When to run (legacy)
    - main
  except:                        # When not to run (legacy)
    - tags
  rules:                         # Modern conditional logic
    - if: '$CI_COMMIT_BRANCH == "main"'
  allow_failure: false           # Pipeline continues on failure
  when: on_success               # Execution condition
  retry: 2                       # Retry on failure
  timeout: 1h                    # Job timeout
  dependencies: []               # Artifact dependencies
  needs: []                      # DAG dependencies
```

### Job Keywords Reference

| Keyword | Purpose | Example |
|---------|---------|---------|
| **script** | Commands to execute (required) | `script: - npm test` |
| **stage** | Pipeline stage | `stage: test` |
| **image** | Docker image | `image: node:18` |
| **services** | Additional containers | `services: - redis:7` |
| **before_script** | Pre-execution commands | `before_script: - npm install` |
| **after_script** | Post-execution commands | `after_script: - cleanup.sh` |
| **variables** | Environment variables | `variables: ENV: prod` |
| **artifacts** | Files to save | `artifacts: paths: - dist/` |
| **cache** | Files to cache | `cache: paths: - node_modules/` |
| **tags** | Runner tags | `tags: - docker` |
| **rules** | Conditional execution | `rules: - if: $CI_COMMIT_BRANCH` |
| **needs** | Job dependencies (DAG) | `needs: - build-job` |
| **dependencies** | Artifact dependencies | `dependencies: - build-job` |
| **allow_failure** | Continue on failure | `allow_failure: true` |
| **when** | Execution timing | `when: manual` |
| **retry** | Retry attempts | `retry: 2` |
| **timeout** | Maximum duration | `timeout: 30m` |
| **parallel** | Parallel instances | `parallel: 5` |
| **extends** | Inherit configuration | `extends: .template` |

---

## Task 2.3: Stages and Pipeline Flow ⭐

### Default Stages

```yaml
# GitLab default stages (if not specified)
stages:
  - .pre        # Special stage, runs first
  - build
  - test
  - deploy
  - .post       # Special stage, runs last
```

### Custom Stages

```yaml
stages:
  - prepare
  - compile
  - unit-test
  - integration-test
  - security-scan
  - package
  - staging-deploy
  - production-deploy
  - cleanup

prepare-env:
  stage: prepare
  script:
    - echo "Setting up environment"

compile-code:
  stage: compile
  script:
    - make compile

run-unit-tests:
  stage: unit-test
  script:
    - npm run test:unit

run-integration-tests:
  stage: integration-test
  script:
    - npm run test:integration
```

### Stage Execution Rules

| Rule | Description |
|------|-------------|
| **Sequential** | Stages run in order |
| **Parallel** | Jobs in same stage run in parallel |
| **Blocking** | Stage waits for all jobs to complete |
| **Failure** | Failed job stops pipeline (unless allow_failure) |
| **Manual** | Manual jobs don't block by default |

---

## Task 2.4: Rules and Conditional Execution ⭐⭐

### Rules Syntax

```yaml
# Basic rules
job:
  script: echo "Hello"
  rules:
    - if: '$CI_COMMIT_BRANCH == "main"'
      when: always
    - if: '$CI_COMMIT_BRANCH =~ /^feature/'
      when: manual
    - when: never

# Multiple conditions
job:
  script: echo "Deploy"
  rules:
    - if: '$CI_COMMIT_BRANCH == "main" && $CI_PIPELINE_SOURCE == "push"'
    - if: '$CI_COMMIT_TAG'
      when: manual

# Changes-based rules
test-frontend:
  script: npm test
  rules:
    - changes:
        - "src/**/*.js"
        - "src/**/*.vue"
      when: always
    - when: never

# Exists-based rules
deploy:
  script: ./deploy.sh
  rules:
    - exists:
        - Dockerfile
      when: always
```

### Rules Clauses

| Clause | Purpose | Example |
|--------|---------|---------|
| **if** | Conditional expression | `if: '$CI_COMMIT_BRANCH == "main"'` |
| **changes** | File changes detection | `changes: - "*.js"` |
| **exists** | File existence check | `exists: - Dockerfile` |
| **when** | Execution timing | `when: manual` |
| **allow_failure** | Failure handling | `allow_failure: true` |
| **variables** | Set variables | `variables: DEPLOY: "true"` |

### When Options

| Option | Description |
|--------|-------------|
| **on_success** | Run if previous jobs succeeded (default) |
| **on_failure** | Run if previous job failed |
| **always** | Always run regardless of status |
| **manual** | Require manual trigger |
| **delayed** | Run after delay |
| **never** | Never run |

### Common Rule Patterns

```yaml
# Run only on main branch
rules:
  - if: '$CI_COMMIT_BRANCH == "main"'

# Run on merge requests
rules:
  - if: '$CI_PIPELINE_SOURCE == "merge_request_event"'

# Run on tags
rules:
  - if: '$CI_COMMIT_TAG'

# Run on schedules
rules:
  - if: '$CI_PIPELINE_SOURCE == "schedule"'

# Skip for draft MRs
rules:
  - if: '$CI_MERGE_REQUEST_TITLE =~ /^Draft:/'
    when: never
  - if: '$CI_PIPELINE_SOURCE == "merge_request_event"'

# Run on file changes
rules:
  - changes:
      - "src/**/*"
      - "tests/**/*"
    when: always
  - when: never
```

---

## Task 2.5: Predefined CI/CD Variables ⭐⭐

### Essential Variables

| Variable | Description | Example |
|----------|-------------|---------|
| **CI_COMMIT_SHA** | Commit hash | `a1b2c3d4...` |
| **CI_COMMIT_SHORT_SHA** | Short commit hash | `a1b2c3d4` |
| **CI_COMMIT_BRANCH** | Branch name | `main` |
| **CI_COMMIT_TAG** | Tag name | `v1.0.0` |
| **CI_COMMIT_MESSAGE** | Commit message | `Fix bug #123` |
| **CI_COMMIT_AUTHOR** | Commit author | `John Doe` |
| **CI_PIPELINE_ID** | Pipeline ID | `12345` |
| **CI_PIPELINE_IID** | Pipeline IID | `42` |
| **CI_PIPELINE_SOURCE** | Pipeline trigger | `push`, `merge_request_event` |
| **CI_JOB_ID** | Job ID | `67890` |
| **CI_JOB_NAME** | Job name | `test-job` |
| **CI_JOB_STAGE** | Stage name | `test` |
| **CI_PROJECT_ID** | Project ID | `123` |
| **CI_PROJECT_NAME** | Project name | `my-app` |
| **CI_PROJECT_PATH** | Project path | `group/my-app` |
| **CI_PROJECT_URL** | Project URL | `https://gitlab.com/group/my-app` |
| **CI_REGISTRY** | Container registry | `registry.gitlab.com` |
| **CI_REGISTRY_IMAGE** | Image path | `registry.gitlab.com/group/my-app` |
| **CI_MERGE_REQUEST_IID** | MR number | `42` |
| **CI_MERGE_REQUEST_TITLE** | MR title | `Add feature X` |
| **CI_MERGE_REQUEST_SOURCE_BRANCH** | Source branch | `feature-x` |
| **CI_MERGE_REQUEST_TARGET_BRANCH** | Target branch | `main` |

### Using Variables in Jobs

```yaml
build:
  script:
    - echo "Building commit $CI_COMMIT_SHORT_SHA"
    - echo "Branch: $CI_COMMIT_BRANCH"
    - echo "Pipeline ID: $CI_PIPELINE_ID"
    - docker build -t $CI_REGISTRY_IMAGE:$CI_COMMIT_SHORT_SHA .
    - docker tag $CI_REGISTRY_IMAGE:$CI_COMMIT_SHORT_SHA $CI_REGISTRY_IMAGE:latest

deploy:
  script:
    - echo "Deploying to production"
    - echo "Project: $CI_PROJECT_NAME"
    - echo "URL: $CI_PROJECT_URL"
    - ./deploy.sh --version=$CI_COMMIT_TAG
```

---

## Task 2.6: Custom Variables ⭐

### Variable Definition Levels

```yaml
# Global variables (entire pipeline)
variables:
  GLOBAL_VAR: "global value"
  DATABASE_URL: "postgres://localhost"

# Job-level variables (override global)
test-job:
  variables:
    TEST_ENV: "staging"
    DEBUG: "true"
  script:
    - echo $GLOBAL_VAR
    - echo $TEST_ENV
```

### Variable Precedence (Highest to Lowest)

1. Trigger variables (API, schedule)
2. Pipeline variables (manual run)
3. Job variables
4. Global variables (.gitlab-ci.yml)
5. Group variables (Settings)
6. Project variables (Settings)
7. Instance variables (Admin)
8. Predefined variables

### Variable Types

```yaml
# String variable
variables:
  APP_NAME: "my-application"

# Multi-line variable
variables:
  CERTIFICATE: |
    -----BEGIN CERTIFICATE-----
    MIICertificateData...
    -----END CERTIFICATE-----

# File variable (in UI)
# Type: File
# Key: SSH_PRIVATE_KEY
# Value: <file content>

# Using file variable
deploy:
  script:
    - chmod 600 $SSH_PRIVATE_KEY
    - ssh -i $SSH_PRIVATE_KEY user@server
```

---

## Task 2.7: Anchors and Templates ⭐

### YAML Anchors

```yaml
# Define anchor
.default-job: &default-config
  image: node:18
  before_script:
    - npm install
  cache:
    paths:
      - node_modules/

# Use anchor
test-unit:
  <<: *default-config
  script:
    - npm run test:unit

test-integration:
  <<: *default-config
  script:
    - npm run test:integration
```

### Extends Keyword

```yaml
# Define template
.deploy-template:
  stage: deploy
  image: alpine:latest
  before_script:
    - apk add --no-cache curl
  only:
    - main

# Extend template
deploy-staging:
  extends: .deploy-template
  script:
    - ./deploy.sh staging
  environment:
    name: staging

deploy-production:
  extends: .deploy-template
  script:
    - ./deploy.sh production
  environment:
    name: production
  when: manual
```

### Hidden Jobs (Templates)

```yaml
# Hidden job (starts with .)
.test-template:
  image: node:18
  before_script:
    - npm ci
  cache:
    key: ${CI_COMMIT_REF_SLUG}
    paths:
      - node_modules/

# Use template
unit-tests:
  extends: .test-template
  script:
    - npm run test:unit

e2e-tests:
  extends: .test-template
  script:
    - npm run test:e2e
```

---

## Task 2.8: Include and External Configuration ⭐

### Include Types

```yaml
# Include local file
include:
  - local: '/templates/build.yml'

# Include from another project
include:
  - project: 'group/ci-templates'
    ref: main
    file: '/templates/deploy.yml'

# Include from URL
include:
  - remote: 'https://example.com/ci/template.yml'

# Include template
include:
  - template: Security/SAST.gitlab-ci.yml

# Multiple includes
include:
  - local: '/templates/build.yml'
  - local: '/templates/test.yml'
  - template: Security/Dependency-Scanning.gitlab-ci.yml
  - project: 'group/ci-templates'
    file: '/templates/deploy.yml'
```

### Include with Rules

```yaml
include:
  - local: '/templates/production.yml'
    rules:
      - if: '$CI_COMMIT_BRANCH == "main"'
  - local: '/templates/development.yml'
    rules:
      - if: '$CI_COMMIT_BRANCH != "main"'
```

---

## Task 2.9: Parallel and Matrix Jobs ⭐

### Parallel Jobs

```yaml
# Run job 5 times in parallel
test:
  script:
    - npm test
  parallel: 5

# Access parallel index
test:
  script:
    - echo "Running test $CI_NODE_INDEX of $CI_NODE_TOTAL"
    - npm test -- --shard=$CI_NODE_INDEX/$CI_NODE_TOTAL
  parallel: 5
```

### Matrix Jobs

```yaml
# Test multiple versions
test:
  image: node:${NODE_VERSION}
  script:
    - npm test
  parallel:
    matrix:
      - NODE_VERSION: ['16', '18', '20']

# Multiple dimensions
test:
  image: ${OS}:${VERSION}
  script:
    - run-tests.sh
  parallel:
    matrix:
      - OS: ['ubuntu', 'alpine']
        VERSION: ['20.04', '22.04']
      - OS: 'debian'
        VERSION: ['11', '12']

# Complex matrix
build:
  script:
    - echo "Building for $PLATFORM with $COMPILER"
  parallel:
    matrix:
      - PLATFORM: ['linux', 'macos', 'windows']
        COMPILER: ['gcc', 'clang']
        ARCH: ['x64', 'arm64']
```

---

## Task 2.10: Trigger and Multi-Project Pipelines ⭐

### Trigger Downstream Pipeline

```yaml
# Trigger another project's pipeline
trigger-downstream:
  stage: deploy
  trigger:
    project: group/downstream-project
    branch: main
    strategy: depend  # Wait for completion

# Trigger with variables
trigger-deploy:
  trigger:
    project: group/deploy-project
    branch: main
  variables:
    ENVIRONMENT: "production"
    VERSION: $CI_COMMIT_TAG
```

### Parent-Child Pipelines

```yaml
# Generate child pipeline
generate-config:
  stage: build
  script:
    - ./generate-pipeline.sh > pipeline.yml
  artifacts:
    paths:
      - pipeline.yml

# Trigger child pipeline
trigger-child:
  stage: deploy
  trigger:
    include:
      - artifact: pipeline.yml
        job: generate-config
    strategy: depend
```

---

## Exam Tips for Domain 2

1. **YAML Syntax** - Master indentation (2 spaces) and structure
2. **Job Keywords** - Know script, stage, rules, artifacts, cache
3. **Rules vs Only/Except** - Rules are modern, more powerful
4. **Variables** - Understand predefined and custom variables
5. **Stages** - Know default stages and execution order
6. **Extends** - Use for DRY (Don't Repeat Yourself) configuration
7. **Include** - Know local, remote, project, template includes
8. **Parallel** - Understand parallel and matrix jobs
9. **Triggers** - Know downstream and parent-child pipelines
10. **Validation** - Use CI Lint to validate .gitlab-ci.yml

---

## Common Pipeline Patterns

### Basic CI/CD Pipeline

```yaml
stages:
  - build
  - test
  - deploy

build:
  stage: build
  script:
    - npm install
    - npm run build
  artifacts:
    paths:
      - dist/

test:
  stage: test
  script:
    - npm test
  coverage: '/Coverage: \d+\.\d+%/'

deploy:
  stage: deploy
  script:
    - ./deploy.sh
  only:
    - main
  when: manual
```

### Docker Build Pipeline

```yaml
build-image:
  stage: build
  image: docker:latest
  services:
    - docker:dind
  script:
    - docker login -u $CI_REGISTRY_USER -p $CI_REGISTRY_PASSWORD $CI_REGISTRY
    - docker build -t $CI_REGISTRY_IMAGE:$CI_COMMIT_SHORT_SHA .
    - docker push $CI_REGISTRY_IMAGE:$CI_COMMIT_SHORT_SHA
```

### Monorepo Pipeline

```yaml
test-frontend:
  script: npm test
  rules:
    - changes:
        - "frontend/**/*"

test-backend:
  script: go test ./...
  rules:
    - changes:
        - "backend/**/*"
```
