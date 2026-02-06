# Domain 1: SDLC Automation (22%)

## Task 1.1: CI/CD Pipeline Automation

### AWS CodePipeline ⭐

| Component | Description |
|-----------|-------------|
| **Stages** | Sequential pipeline phases |
| **Actions** | Tasks within stages |
| **Artifacts** | Build outputs passed between stages |
| **Transitions** | Enable/disable stage progression |
| **Approval Actions** | Manual approval gates |
| **Cross-region** | Deploy to multiple regions |
| **Cross-account** | Deploy across accounts |

#### Pipeline Action Types
| Category | Actions |
|----------|---------|
| **Source** | S3, CodeCommit, GitHub, ECR |
| **Build** | CodeBuild, Jenkins |
| **Test** | CodeBuild, Third-party |
| **Deploy** | CodeDeploy, ECS, Lambda, CloudFormation |
| **Approval** | Manual approval |
| **Invoke** | Lambda, Step Functions |

> **Exam Tip:** Understand cross-account pipelines with IAM roles and KMS key sharing!

---

### AWS CodeBuild ⭐

| Feature | Description |
|---------|-------------|
| **buildspec.yml** | Build specification file |
| **Phases** | install, pre_build, build, post_build |
| **Environment** | Managed or custom images |
| **Compute Types** | Small, Medium, Large, 2XLarge |
| **Caching** | S3, Local |
| **Batch Builds** | Parallel builds |
| **Reports** | Test and coverage reports |

#### buildspec.yml Structure
```yaml
version: 0.2
phases:
  install:
    runtime-versions:
      python: 3.9
  pre_build:
    commands:
      - pip install -r requirements.txt
  build:
    commands:
      - python -m pytest
      - python setup.py build
  post_build:
    commands:
      - echo "Build completed"
artifacts:
  files:
    - '**/*'
  name: build-output
cache:
  paths:
    - '/root/.cache/pip/**/*'
```

---

### AWS CodeDeploy ⭐

| Deployment Type | Description | Use Case |
|-----------------|-------------|----------|
| **In-Place** | Update existing instances | EC2, On-premises |
| **Blue/Green** | New instances, switch traffic | EC2, ECS, Lambda |

#### Deployment Configurations
| Type | Configuration |
|------|---------------|
| **EC2** | AllAtOnce, HalfAtATime, OneAtATime |
| **Lambda** | Canary, Linear, AllAtOnce |
| **ECS** | Canary, Linear, AllAtOnce |

#### AppSpec File (appspec.yml)
| Hook | When |
|------|------|
| **BeforeInstall** | Before installing new version |
| **AfterInstall** | After installing |
| **ApplicationStart** | Start application |
| **ValidateService** | Verify deployment |
| **BeforeAllowTraffic** | Before shifting traffic |
| **AfterAllowTraffic** | After traffic shifted |

> **Exam Tip:** Know the difference between In-Place (faster, downtime) vs Blue/Green (safer, more resources)!

---

### Deployment Strategies

| Strategy | Description | Risk | Rollback |
|----------|-------------|------|----------|
| **All-at-once** | Deploy to all targets | High | Manual |
| **Rolling** | Gradual updates | Medium | Manual |
| **Rolling with Batch** | Maintain capacity | Medium | Manual |
| **Immutable** | New instances first | Low | Fast |
| **Blue/Green** | Parallel environment | Low | Instant |
| **Canary** | Small % first | Low | Fast |
| **Linear** | Gradual % increase | Low | Fast |

---

### Lambda Deployment with SAM/CDK

| Method | Description |
|--------|-------------|
| **SAM** | Serverless Application Model |
| **CDK** | Cloud Development Kit |
| **Versions** | Immutable function versions |
| **Aliases** | Pointer to versions |
| **Traffic Shifting** | Weighted aliases |

#### Lambda Alias Traffic Shifting
```yaml
# SAM template
DeploymentPreference:
  Type: Canary10Percent5Minutes
  Alarms:
    - !Ref AliasErrorMetricAlarm
  Hooks:
    PreTraffic: !Ref PreTrafficHook
    PostTraffic: !Ref PostTrafficHook
```

---

## Task 1.2: Source Control and Branching

### AWS CodeCommit

| Feature | Description |
|---------|-------------|
| **Git Repository** | Fully managed Git |
| **Triggers** | SNS, Lambda on events |
| **Notifications** | CloudWatch Events/EventBridge |
| **Approval Rules** | PR templates |
| **Cross-account** | Repository sharing |

---

### Branching Strategies

| Strategy | Description | Best For |
|----------|-------------|----------|
| **GitFlow** | Feature, develop, release, hotfix, main | Complex releases |
| **GitHub Flow** | Main + feature branches | Continuous deployment |
| **Trunk-based** | Main with feature flags | High-velocity teams |

---

## Task 1.3: Testing Strategies

### Testing in CI/CD

| Test Type | When | Tool |
|-----------|------|------|
| **Unit Tests** | Build phase | CodeBuild |
| **Integration Tests** | Post-build | CodeBuild |
| **End-to-End** | Pre-deploy | CodeBuild + Selenium |
| **Load Tests** | Pre-production | CodeBuild + Custom |
| **Security Scan** | Build/Deploy | Inspector, CodeGuru |

---

### AWS CodeGuru

| Feature | Description |
|---------|-------------|
| **Reviewer** | Automated code reviews |
| **Profiler** | Application performance insights |
| **Secrets Detection** | Find hardcoded secrets |

---

## Task 1.4: Artifact Management

### AWS CodeArtifact

| Feature | Description |
|---------|-------------|
| **Package Types** | npm, PyPI, Maven, NuGet |
| **Upstream Repos** | Connect to public registries |
| **Domains** | Cross-account sharing |
| **Encryption** | KMS encryption |

---

### Amazon ECR

| Feature | Description |
|---------|-------------|
| **Image Scanning** | Vulnerability detection |
| **Lifecycle Policies** | Auto-cleanup old images |
| **Cross-region Replication** | Multi-region distribution |
| **Immutable Tags** | Prevent tag overwrite |

---

## Exam Tips for Domain 1

1. **CodePipeline architecture:**
   - Cross-account = IAM roles + KMS key policy
   - Cross-region = S3 artifact buckets per region
   - Manual approval = SNS notification
2. **CodeBuild optimization:**
   - Caching = faster builds (S3 or local)
   - Batch builds = parallel execution
   - Reports = test and coverage
3. **CodeDeploy strategies:**
   - In-Place = faster, has downtime
   - Blue/Green = safer, instant rollback
   - Hooks = lifecycle event scripts
4. **Lambda deployments:**
   - Versions = immutable snapshots
   - Aliases = pointers with traffic shifting
   - Canary/Linear = gradual rollout
5. **Testing integration:**
   - Unit tests in build phase
   - Integration tests post-build
   - CodeGuru for code quality
6. **AppSpec file:**
   - YAML or JSON format
   - Lifecycle hooks order matters
   - ValidateService = verify deployment
7. **Rollback strategies:**
   - Automatic on CloudWatch alarms
   - Manual through console/CLI
   - Blue/Green = instant switch
8. **Artifact management:**
   - CodeArtifact = packages
   - ECR = container images
   - S3 = general artifacts
9. **Multi-account CI/CD:**
   - Central tools account
   - AssumeRole for deployments
   - Shared artifact buckets
10. **EventBridge for CI/CD:**
    - Pipeline state changes
    - CodeBuild notifications
    - Cross-account event routing
