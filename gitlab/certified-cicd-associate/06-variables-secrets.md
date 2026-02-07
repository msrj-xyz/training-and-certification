# Domain 6: Variables & Secrets Management

## Task 6.1: Variable Types and Scope ⭐⭐

### Variable Hierarchy (Precedence)

| Level | Scope | Precedence | Use Case |
|-------|-------|------------|----------|
| **Trigger** | Pipeline run | 1 (Highest) | API/Schedule triggers |
| **Pipeline** | Single pipeline | 2 | Manual pipeline runs |
| **Job** | Single job | 3 | Job-specific config |
| **Global (.gitlab-ci.yml)** | Pipeline | 4 | Pipeline defaults |
| **Group** | Group projects | 5 | Shared across group |
| **Project** | Single project | 6 | Project-specific |
| **Instance** | All projects | 7 (Lowest) | Instance-wide defaults |

### Variable Definition

```yaml
# Global variables (pipeline-wide)
variables:
  GLOBAL_VAR: "global value"
  DATABASE_URL: "postgres://localhost"
  NODE_ENV: "production"

# Job-level variables (override global)
test:
  variables:
    NODE_ENV: "test"
    DEBUG: "true"
  script:
    - echo $NODE_ENV  # Outputs: test
    - echo $GLOBAL_VAR  # Outputs: global value

# Stage-specific variables
.test-template:
  variables:
    TEST_ENV: "ci"
  script:
    - npm test

unit-test:
  extends: .test-template
  variables:
    TEST_TYPE: "unit"

integration-test:
  extends: .test-template
  variables:
    TEST_TYPE: "integration"
```

---

## Task 6.2: Predefined CI/CD Variables ⭐⭐

### Essential Predefined Variables

```yaml
build:
  script:
    # Commit information
    - echo "Commit SHA: $CI_COMMIT_SHA"
    - echo "Short SHA: $CI_COMMIT_SHORT_SHA"
    - echo "Branch: $CI_COMMIT_BRANCH"
    - echo "Tag: $CI_COMMIT_TAG"
    - echo "Message: $CI_COMMIT_MESSAGE"
    - echo "Author: $CI_COMMIT_AUTHOR"
    
    # Pipeline information
    - echo "Pipeline ID: $CI_PIPELINE_ID"
    - echo "Pipeline IID: $CI_PIPELINE_IID"
    - echo "Pipeline Source: $CI_PIPELINE_SOURCE"
    - echo "Pipeline URL: $CI_PIPELINE_URL"
    
    # Job information
    - echo "Job ID: $CI_JOB_ID"
    - echo "Job Name: $CI_JOB_NAME"
    - echo "Job Stage: $CI_JOB_STAGE"
    - echo "Job URL: $CI_JOB_URL"
    
    # Project information
    - echo "Project ID: $CI_PROJECT_ID"
    - echo "Project Name: $CI_PROJECT_NAME"
    - echo "Project Path: $CI_PROJECT_PATH"
    - echo "Project URL: $CI_PROJECT_URL"
    - echo "Project Dir: $CI_PROJECT_DIR"
    
    # Registry information
    - echo "Registry: $CI_REGISTRY"
    - echo "Registry Image: $CI_REGISTRY_IMAGE"
    - echo "Registry User: $CI_REGISTRY_USER"
    
    # Merge Request information
    - echo "MR IID: $CI_MERGE_REQUEST_IID"
    - echo "MR Title: $CI_MERGE_REQUEST_TITLE"
    - echo "Source Branch: $CI_MERGE_REQUEST_SOURCE_BRANCH_NAME"
    - echo "Target Branch: $CI_MERGE_REQUEST_TARGET_BRANCH_NAME"
```

### Variable Categories

| Category | Variables | Use Case |
|----------|-----------|----------|
| **Commit** | CI_COMMIT_* | Version info, tagging |
| **Pipeline** | CI_PIPELINE_* | Pipeline tracking |
| **Job** | CI_JOB_* | Job identification |
| **Project** | CI_PROJECT_* | Project context |
| **Registry** | CI_REGISTRY_* | Docker operations |
| **MR** | CI_MERGE_REQUEST_* | MR workflows |
| **User** | GITLAB_USER_* | User context |
| **Runner** | CI_RUNNER_* | Runner info |

### Boolean Variables

```yaml
# Check if running in CI
test:
  script:
    - |
      if [ "$CI" == "true" ]; then
        echo "Running in CI"
      fi

# Check pipeline source
deploy:
  script:
    - |
      if [ "$CI_PIPELINE_SOURCE" == "merge_request_event" ]; then
        echo "Deploy review app"
      elif [ "$CI_PIPELINE_SOURCE" == "push" ]; then
        echo "Deploy to staging"
      fi
```

---

## Task 6.3: Custom Variables in UI ⭐⭐

### Project Variables

```yaml
# Settings > CI/CD > Variables

# Variable types:
# - Variable (default)
# - File

# Variable options:
# - Protect variable (only protected branches)
# - Mask variable (hide in logs)
# - Expand variable reference (resolve $VAR)

# Example usage:
deploy:
  script:
    - echo $API_KEY  # Masked in logs
    - echo $DATABASE_URL
    - cat $SSH_PRIVATE_KEY  # File variable
```

### Variable Protection

```yaml
# Protected variable (only on protected branches)
deploy-production:
  script:
    - echo $PROD_API_KEY  # Only available on main branch
  only:
    - main

# Unprotected variable (all branches)
test:
  script:
    - echo $TEST_API_KEY  # Available on all branches
```

### Masked Variables

```yaml
# Masked variable (hidden in logs)
deploy:
  script:
    - echo $SECRET_TOKEN  # Shows [masked] in logs
    - curl -H "Authorization: Bearer $SECRET_TOKEN" api.example.com

# Requirements for masking:
# - At least 8 characters
# - No spaces
# - Base64 characters only (a-zA-Z0-9+/=)
# - Not a GitLab predefined variable
```

### File Variables

```yaml
# File variable: SSH_PRIVATE_KEY
deploy:
  before_script:
    - chmod 600 $SSH_PRIVATE_KEY
    - eval $(ssh-agent -s)
    - ssh-add $SSH_PRIVATE_KEY
  script:
    - ssh user@server "deploy.sh"

# File variable: KUBECONFIG
deploy-k8s:
  script:
    - kubectl --kubeconfig=$KUBECONFIG apply -f deployment.yaml

# File variable: SERVICE_ACCOUNT_JSON
deploy-gcp:
  script:
    - gcloud auth activate-service-account --key-file=$SERVICE_ACCOUNT_JSON
    - gcloud app deploy
```

---

## Task 6.4: Group and Instance Variables ⭐

### Group Variables

```yaml
# Group Settings > CI/CD > Variables

# Inherited by all projects in group
# Use for shared credentials:
# - DOCKER_REGISTRY_USER
# - DOCKER_REGISTRY_PASSWORD
# - SHARED_API_KEY
# - ORGANIZATION_TOKEN

deploy:
  script:
    - echo $DOCKER_REGISTRY_USER  # From group variables
    - docker login -u $DOCKER_REGISTRY_USER -p $DOCKER_REGISTRY_PASSWORD
```

### Instance Variables

```yaml
# Admin Area > Settings > CI/CD > Variables

# Available to all projects
# Use for instance-wide settings:
# - GITLAB_RUNNER_TOKEN
# - PROXY_URL
# - ARTIFACT_STORAGE_URL

build:
  script:
    - export http_proxy=$PROXY_URL
    - npm install
```

---

## Task 6.5: Variable Expansion ⭐

### Variable Reference

```yaml
variables:
  BASE_URL: "https://example.com"
  API_URL: "${BASE_URL}/api"
  VERSION: "1.0.0"
  IMAGE_TAG: "${CI_REGISTRY_IMAGE}:${VERSION}"

build:
  script:
    - echo $API_URL  # https://example.com/api
    - echo $IMAGE_TAG  # registry.gitlab.com/group/project:1.0.0
```

### Disable Variable Expansion

```yaml
# In UI: Uncheck "Expand variable reference"

# In .gitlab-ci.yml: Use single quotes
variables:
  LITERAL_VAR: '$CI_COMMIT_SHA'  # Literal string, not expanded

# Or escape with $$
variables:
  ESCAPED_VAR: "$$CI_COMMIT_SHA"  # Literal $CI_COMMIT_SHA
```

### Variable Substitution

```yaml
variables:
  ENVIRONMENT: "staging"
  DATABASE_URL: "postgres://${ENVIRONMENT}.db.example.com"
  REDIS_URL: "redis://${ENVIRONMENT}.cache.example.com"

deploy:
  script:
    - echo $DATABASE_URL  # postgres://staging.db.example.com
    - echo $REDIS_URL  # redis://staging.cache.example.com
```

---

## Task 6.6: Environment-Specific Variables ⭐⭐

### Environment Variables

```yaml
# Development environment
deploy-dev:
  stage: deploy
  variables:
    ENVIRONMENT: "development"
    API_URL: "https://dev.example.com"
    DEBUG: "true"
  script:
    - ./deploy.sh
  environment:
    name: development

# Staging environment
deploy-staging:
  stage: deploy
  variables:
    ENVIRONMENT: "staging"
    API_URL: "https://staging.example.com"
    DEBUG: "false"
  script:
    - ./deploy.sh
  environment:
    name: staging

# Production environment
deploy-production:
  stage: deploy
  variables:
    ENVIRONMENT: "production"
    API_URL: "https://example.com"
    DEBUG: "false"
  script:
    - ./deploy.sh
  environment:
    name: production
  when: manual
```

### Environment Scoped Variables (UI)

```yaml
# Settings > CI/CD > Variables
# Add environment scope:

# Variable: API_KEY
# Scope: production
# Value: prod-key-123

# Variable: API_KEY
# Scope: staging
# Value: staging-key-456

# Variable: API_KEY
# Scope: *
# Value: dev-key-789

deploy:
  script:
    - echo $API_KEY  # Different value per environment
  environment:
    name: $CI_COMMIT_REF_NAME
```

---

## Task 6.7: Secrets Management ⭐⭐

### HashiCorp Vault Integration

```yaml
# Project Settings > CI/CD > Variables
# Add Vault server URL and authentication

# Use Vault secrets
deploy:
  secrets:
    DATABASE_PASSWORD:
      vault: production/db/password@secrets
      file: false
    API_TOKEN:
      vault: production/api/token@secrets
      file: false
  script:
    - echo "Deploying with Vault secrets"
    - ./deploy.sh
```

### Azure Key Vault Integration

```yaml
# Use Azure Key Vault
deploy:
  secrets:
    AZURE_CLIENT_SECRET:
      azure_key_vault:
        name: my-key-vault
        key: client-secret
  script:
    - az login --service-principal -u $AZURE_CLIENT_ID -p $AZURE_CLIENT_SECRET
```

### AWS Secrets Manager

```yaml
# Fetch from AWS Secrets Manager
deploy:
  before_script:
    - apt-get update && apt-get install -y awscli jq
    - export DB_PASSWORD=$(aws secretsmanager get-secret-value --secret-id prod/db/password --query SecretString --output text | jq -r .password)
  script:
    - ./deploy.sh
```

---

## Task 6.8: Variable Security Best Practices ⭐

### Secure Variable Handling

```yaml
# ✅ Good: Use masked variables
deploy:
  script:
    - curl -H "Authorization: Bearer $API_TOKEN" api.example.com

# ❌ Bad: Expose secrets in logs
deploy:
  script:
    - echo "API Token: $API_TOKEN"  # Don't do this!

# ✅ Good: Use file variables for keys
deploy:
  script:
    - chmod 600 $SSH_PRIVATE_KEY
    - ssh -i $SSH_PRIVATE_KEY user@server

# ❌ Bad: Hardcode secrets
deploy:
  script:
    - export API_KEY="hardcoded-secret-123"  # Don't do this!

# ✅ Good: Protect production variables
deploy-prod:
  script:
    - echo $PROD_API_KEY  # Only on protected branches
  only:
    - main

# ✅ Good: Use short-lived tokens
deploy:
  before_script:
    - export TOKEN=$(curl -X POST auth.example.com/token)
  script:
    - curl -H "Authorization: Bearer $TOKEN" api.example.com
```

### Variable Validation

```yaml
# Validate required variables
deploy:
  before_script:
    - |
      if [ -z "$API_KEY" ]; then
        echo "Error: API_KEY is not set"
        exit 1
      fi
      if [ -z "$DATABASE_URL" ]; then
        echo "Error: DATABASE_URL is not set"
        exit 1
      fi
  script:
    - ./deploy.sh
```

---

## Task 6.9: Dynamic Variables ⭐

### Generate Variables in Jobs

```yaml
# Generate and export variables
build:
  script:
    - export BUILD_VERSION=$(cat version.txt)
    - export BUILD_DATE=$(date +%Y%m%d)
    - echo "BUILD_VERSION=$BUILD_VERSION" >> build.env
    - echo "BUILD_DATE=$BUILD_DATE" >> build.env
    - echo "DOCKER_IMAGE=$CI_REGISTRY_IMAGE:$BUILD_VERSION" >> build.env
  artifacts:
    reports:
      dotenv: build.env

# Use generated variables
deploy:
  needs:
    - build
  script:
    - echo "Deploying version $BUILD_VERSION"
    - echo "Built on $BUILD_DATE"
    - docker pull $DOCKER_IMAGE
```

### Conditional Variables

```yaml
# Set variables based on conditions
variables:
  ENVIRONMENT: "development"

deploy:
  variables:
    ENVIRONMENT: >
      $([[ "$CI_COMMIT_BRANCH" == "main" ]] && echo "production" || echo "staging")
  script:
    - echo "Deploying to $ENVIRONMENT"

# Using rules
deploy-staging:
  variables:
    ENVIRONMENT: "staging"
  script:
    - ./deploy.sh
  rules:
    - if: '$CI_COMMIT_BRANCH != "main"'

deploy-production:
  variables:
    ENVIRONMENT: "production"
  script:
    - ./deploy.sh
  rules:
    - if: '$CI_COMMIT_BRANCH == "main"'
```

---

## Task 6.10: Variable Troubleshooting ⭐

### Debug Variables

```yaml
# Print all variables (be careful with secrets!)
debug:
  script:
    - env | sort
    - echo "CI_COMMIT_SHA: $CI_COMMIT_SHA"
    - echo "CI_COMMIT_BRANCH: $CI_COMMIT_BRANCH"
    - echo "CI_PIPELINE_SOURCE: $CI_PIPELINE_SOURCE"
  when: manual

# Check specific variables
check-vars:
  script:
    - |
      echo "Checking variables..."
      echo "API_URL: ${API_URL:-NOT SET}"
      echo "DATABASE_URL: ${DATABASE_URL:-NOT SET}"
      echo "ENVIRONMENT: ${ENVIRONMENT:-NOT SET}"
```

### Variable Precedence Testing

```yaml
# Test variable precedence
variables:
  TEST_VAR: "global"

test-precedence:
  variables:
    TEST_VAR: "job-level"
  script:
    - echo $TEST_VAR  # Outputs: job-level
    
# Override with pipeline variable
# Run pipeline with TEST_VAR=pipeline-level
# Output: pipeline-level
```

### Common Variable Issues

| Issue | Cause | Solution |
|-------|-------|----------|
| **Variable not found** | Typo or not defined | Check spelling and scope |
| **Masked incorrectly** | Doesn't meet requirements | Use 8+ chars, no spaces |
| **Not expanded** | Expansion disabled | Enable in UI or use double quotes |
| **Wrong value** | Precedence issue | Check variable hierarchy |
| **Not available** | Wrong scope | Check environment/branch scope |
| **Exposed in logs** | Not masked | Enable masking in UI |

---

## Exam Tips for Domain 6

1. **Variable Precedence** - Know the hierarchy (trigger > pipeline > job > global)
2. **Predefined Variables** - Master CI_COMMIT_*, CI_PIPELINE_*, CI_PROJECT_*
3. **Masked Variables** - Understand masking requirements
4. **File Variables** - Know when to use file type
5. **Protected Variables** - Restrict to protected branches
6. **Variable Expansion** - Understand ${VAR} syntax
7. **Dotenv** - Pass variables between jobs
8. **Environment Scope** - Different values per environment
9. **Secrets Management** - Vault, Azure Key Vault integration
10. **Security** - Never expose secrets in logs

---

## Quick Reference

### Variable Definition

```yaml
# Global
variables:
  VAR: "value"

# Job-level
job:
  variables:
    VAR: "value"
  script:
    - echo $VAR
```

### Predefined Variables

```yaml
# Most used
$CI_COMMIT_SHA
$CI_COMMIT_BRANCH
$CI_COMMIT_TAG
$CI_PIPELINE_ID
$CI_JOB_NAME
$CI_PROJECT_NAME
$CI_REGISTRY_IMAGE
$CI_MERGE_REQUEST_IID
```

### Variable Security

```yaml
# UI Settings:
# ✅ Protect variable
# ✅ Mask variable
# ✅ Use file type for keys
# ✅ Set environment scope
```

### Dotenv Export

```yaml
build:
  script:
    - echo "VAR=value" >> build.env
  artifacts:
    reports:
      dotenv: build.env

deploy:
  needs: [build]
  script:
    - echo $VAR  # Available from build job
```
