# Domain 8: Deployment Strategies

## Task 8.1: Environment Management ⭐⭐

### Basic Environment Configuration

```yaml
# Development environment
deploy-dev:
  stage: deploy
  script:
    - ./deploy.sh development
  environment:
    name: development
    url: https://dev.example.com
  rules:
    - if: '$CI_COMMIT_BRANCH == "develop"'

# Staging environment
deploy-staging:
  stage: deploy
  script:
    - ./deploy.sh staging
  environment:
    name: staging
    url: https://staging.example.com
  rules:
    - if: '$CI_COMMIT_BRANCH == "main"'

# Production environment
deploy-production:
  stage: deploy
  script:
    - ./deploy.sh production
  environment:
    name: production
    url: https://example.com
  rules:
    - if: '$CI_COMMIT_BRANCH == "main"'
  when: manual
```

### Dynamic Environments

```yaml
# Review apps for merge requests
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

# Stop review app
stop-review:
  stage: deploy
  script:
    - ./cleanup-review.sh $CI_COMMIT_REF_SLUG
  environment:
    name: review/$CI_COMMIT_REF_SLUG
    action: stop
  rules:
    - if: '$CI_PIPELINE_SOURCE == "merge_request_event"'
  when: manual
```

### Environment Tiers

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

## Task 8.2: Manual Deployments ⭐

### Manual Approval

```yaml
# Require manual trigger
deploy-production:
  stage: deploy
  script:
    - ./deploy.sh production
  environment:
    name: production
  when: manual
  only:
    - main

# Manual with allow_failure
deploy-optional:
  stage: deploy
  script:
    - ./deploy.sh optional
  when: manual
  allow_failure: true  # Pipeline succeeds even if not run
```

### Delayed Deployments

```yaml
# Deploy after 30 minutes
deploy-canary:
  stage: deploy
  script:
    - ./deploy-canary.sh
  environment:
    name: production-canary
  when: delayed
  start_in: 30 minutes
  only:
    - main

# Manual override of delayed job
# Click "Play" button to deploy immediately
```

---

## Task 8.3: Blue-Green Deployment ⭐⭐

### Blue-Green Strategy

```yaml
variables:
  BLUE_ENV: "production-blue"
  GREEN_ENV: "production-green"

# Deploy to green (inactive) environment
deploy-green:
  stage: deploy
  script:
    - ./deploy.sh $GREEN_ENV
    - ./health-check.sh $GREEN_ENV
  environment:
    name: $GREEN_ENV
    url: https://green.example.com
  only:
    - main

# Switch traffic to green
switch-to-green:
  stage: deploy
  needs:
    - deploy-green
  script:
    - ./switch-traffic.sh $GREEN_ENV
    - echo "Traffic switched to green"
  environment:
    name: production
    url: https://example.com
  when: manual
  only:
    - main

# Rollback to blue if needed
rollback-to-blue:
  stage: deploy
  script:
    - ./switch-traffic.sh $BLUE_ENV
    - echo "Rolled back to blue"
  environment:
    name: production
  when: manual
  only:
    - main
```

### Blue-Green with Kubernetes

```yaml
deploy-green:
  stage: deploy
  script:
    # Deploy new version with green label
    - kubectl apply -f k8s/deployment-green.yaml
    - kubectl wait --for=condition=available deployment/app-green
    - kubectl apply -f k8s/service-green.yaml
  environment:
    name: production-green

switch-traffic:
  stage: deploy
  needs:
    - deploy-green
  script:
    # Update service selector to green
    - kubectl patch service app -p '{"spec":{"selector":{"version":"green"}}}'
  environment:
    name: production
  when: manual
```

---

## Task 8.4: Canary Deployment ⭐⭐

### Canary Strategy

```yaml
# Deploy canary (10% traffic)
deploy-canary:
  stage: deploy
  script:
    - ./deploy-canary.sh 10
  environment:
    name: production-canary
    url: https://canary.example.com
  only:
    - main

# Monitor canary
monitor-canary:
  stage: deploy
  needs:
    - deploy-canary
  script:
    - ./monitor-metrics.sh
    - ./check-error-rate.sh
  when: delayed
  start_in: 15 minutes
  only:
    - main

# Promote canary to 50%
promote-canary-50:
  stage: deploy
  needs:
    - monitor-canary
  script:
    - ./deploy-canary.sh 50
  when: manual
  only:
    - main

# Promote canary to 100%
promote-canary-100:
  stage: deploy
  needs:
    - promote-canary-50
  script:
    - ./deploy-canary.sh 100
  environment:
    name: production
  when: manual
  only:
    - main

# Rollback canary
rollback-canary:
  stage: deploy
  script:
    - ./rollback-canary.sh
  when: manual
  only:
    - main
```

### Canary with Kubernetes

```yaml
deploy-canary:
  stage: deploy
  script:
    # Deploy canary deployment
    - kubectl apply -f k8s/deployment-canary.yaml
    - kubectl scale deployment app-canary --replicas=1
    - kubectl scale deployment app-stable --replicas=9
    # 10% canary, 90% stable
  environment:
    name: production-canary

promote-canary:
  stage: deploy
  script:
    # Increase canary replicas
    - kubectl scale deployment app-canary --replicas=5
    - kubectl scale deployment app-stable --replicas=5
    # 50% canary, 50% stable
  when: manual
```

---

## Task 8.5: Rolling Deployment ⭐

### Rolling Update

```yaml
deploy-rolling:
  stage: deploy
  script:
    # Deploy with rolling update
    - kubectl set image deployment/app app=$CI_REGISTRY_IMAGE:$CI_COMMIT_SHA
    - kubectl rollout status deployment/app
  environment:
    name: production
  only:
    - main

# Rollback if needed
rollback-deployment:
  stage: deploy
  script:
    - kubectl rollout undo deployment/app
  environment:
    name: production
  when: manual
  only:
    - main
```

### Rolling with Health Checks

```yaml
deploy-rolling:
  stage: deploy
  script:
    - ./deploy-rolling.sh
    - |
      for i in {1..10}; do
        if ./health-check.sh; then
          echo "Deployment successful"
          exit 0
        fi
        echo "Waiting for deployment... ($i/10)"
        sleep 30
      done
      echo "Deployment failed"
      exit 1
  environment:
    name: production
  only:
    - main
```

---

## Task 8.6: Feature Flags ⭐

### Feature Flag Deployment

```yaml
# Deploy with feature flag disabled
deploy-with-flag:
  stage: deploy
  script:
    - ./deploy.sh
    - ./set-feature-flag.sh new-feature false
  environment:
    name: production
  only:
    - main

# Enable feature flag gradually
enable-feature-10:
  stage: deploy
  script:
    - ./set-feature-flag.sh new-feature 10  # 10% users
  when: manual
  only:
    - main

enable-feature-50:
  stage: deploy
  script:
    - ./set-feature-flag.sh new-feature 50  # 50% users
  when: manual
  only:
    - main

enable-feature-100:
  stage: deploy
  script:
    - ./set-feature-flag.sh new-feature 100  # All users
  when: manual
  only:
    - main
```

### Feature Flag with LaunchDarkly

```yaml
deploy:
  stage: deploy
  script:
    - ./deploy.sh
    - |
      curl -X PATCH https://app.launchdarkly.com/api/v2/flags/default/new-feature \
        -H "Authorization: $LAUNCHDARKLY_API_KEY" \
        -H "Content-Type: application/json" \
        -d '{"variations": [{"value": false}]}'
  environment:
    name: production
```

---

## Task 8.7: Rollback Strategies ⭐⭐

### Automatic Rollback

```yaml
deploy:
  stage: deploy
  script:
    - ./deploy.sh
    - ./health-check.sh || (./rollback.sh && exit 1)
  environment:
    name: production
  only:
    - main

# Rollback on failure
.post-deploy:
  stage: .post
  script:
    - |
      if [ "$CI_JOB_STATUS" == "failed" ]; then
        ./rollback.sh
      fi
  when: on_failure
```

### Manual Rollback

```yaml
# Deploy new version
deploy:
  stage: deploy
  script:
    - ./deploy.sh $CI_COMMIT_SHA
  environment:
    name: production
  only:
    - main

# Rollback to previous version
rollback:
  stage: deploy
  script:
    - ./rollback.sh
  environment:
    name: production
  when: manual
  only:
    - main
```

### Kubernetes Rollback

```yaml
# Deploy
deploy:
  stage: deploy
  script:
    - kubectl set image deployment/app app=$CI_REGISTRY_IMAGE:$CI_COMMIT_SHA
    - kubectl rollout status deployment/app
  environment:
    name: production

# Rollback
rollback:
  stage: deploy
  script:
    - kubectl rollout undo deployment/app
    - kubectl rollout status deployment/app
  environment:
    name: production
  when: manual
```

---

## Task 8.8: Multi-Cloud Deployment ⭐

### AWS Deployment

```yaml
deploy-aws:
  stage: deploy
  image: amazon/aws-cli
  script:
    - aws configure set aws_access_key_id $AWS_ACCESS_KEY_ID
    - aws configure set aws_secret_access_key $AWS_SECRET_ACCESS_KEY
    - aws configure set region us-east-1
    - aws ecs update-service --cluster prod --service app --force-new-deployment
  environment:
    name: production-aws
  only:
    - main
```

### GCP Deployment

```yaml
deploy-gcp:
  stage: deploy
  image: google/cloud-sdk:alpine
  script:
    - echo $GCP_SERVICE_ACCOUNT_KEY | base64 -d > key.json
    - gcloud auth activate-service-account --key-file=key.json
    - gcloud config set project $GCP_PROJECT_ID
    - gcloud run deploy app --image $CI_REGISTRY_IMAGE:$CI_COMMIT_SHA --region us-central1
  environment:
    name: production-gcp
  only:
    - main
```

### Azure Deployment

```yaml
deploy-azure:
  stage: deploy
  image: mcr.microsoft.com/azure-cli
  script:
    - az login --service-principal -u $AZURE_CLIENT_ID -p $AZURE_CLIENT_SECRET --tenant $AZURE_TENANT_ID
    - az webapp deployment container config --name app --resource-group prod --docker-custom-image-name $CI_REGISTRY_IMAGE:$CI_COMMIT_SHA
  environment:
    name: production-azure
  only:
    - main
```

---

## Task 8.9: Kubernetes Deployment ⭐⭐

### Basic Kubernetes Deployment

```yaml
deploy-k8s:
  stage: deploy
  image: bitnami/kubectl:latest
  script:
    # Configure kubectl
    - kubectl config set-cluster k8s --server="$KUBE_URL" --insecure-skip-tls-verify=true
    - kubectl config set-credentials admin --token="$KUBE_TOKEN"
    - kubectl config set-context default --cluster=k8s --user=admin
    - kubectl config use-context default
    
    # Deploy
    - kubectl set image deployment/app app=$CI_REGISTRY_IMAGE:$CI_COMMIT_SHA -n production
    - kubectl rollout status deployment/app -n production
  environment:
    name: production
    kubernetes:
      namespace: production
  only:
    - main
```

### Helm Deployment

```yaml
deploy-helm:
  stage: deploy
  image: alpine/helm:latest
  script:
    # Configure kubectl
    - kubectl config set-cluster k8s --server="$KUBE_URL" --insecure-skip-tls-verify=true
    - kubectl config set-credentials admin --token="$KUBE_TOKEN"
    - kubectl config set-context default --cluster=k8s --user=admin
    - kubectl config use-context default
    
    # Deploy with Helm
    - helm upgrade --install app ./helm-chart \
        --set image.tag=$CI_COMMIT_SHA \
        --namespace production \
        --create-namespace \
        --wait
  environment:
    name: production
  only:
    - main
```

### Kustomize Deployment

```yaml
deploy-kustomize:
  stage: deploy
  image: bitnami/kubectl:latest
  script:
    - kubectl config set-cluster k8s --server="$KUBE_URL" --insecure-skip-tls-verify=true
    - kubectl config set-credentials admin --token="$KUBE_TOKEN"
    - kubectl config set-context default --cluster=k8s --user=admin
    - kubectl config use-context default
    
    # Deploy with Kustomize
    - cd k8s/overlays/production
    - kustomize edit set image app=$CI_REGISTRY_IMAGE:$CI_COMMIT_SHA
    - kubectl apply -k .
  environment:
    name: production
  only:
    - main
```

---

## Task 8.10: Deployment Verification ⭐

### Health Checks

```yaml
deploy:
  stage: deploy
  script:
    - ./deploy.sh
    
verify-deployment:
  stage: deploy
  needs:
    - deploy
  script:
    # Health check
    - |
      for i in {1..30}; do
        if curl -f https://example.com/health; then
          echo "Health check passed"
          exit 0
        fi
        echo "Waiting for service... ($i/30)"
        sleep 10
      done
      echo "Health check failed"
      exit 1
  environment:
    name: production
```

### Smoke Tests

```yaml
deploy:
  stage: deploy
  script:
    - ./deploy.sh

smoke-tests:
  stage: deploy
  needs:
    - deploy
  script:
    # Run smoke tests
    - npm run test:smoke
    - ./check-critical-paths.sh
    - ./verify-integrations.sh
  environment:
    name: production
  allow_failure: false
```

### Monitoring Integration

```yaml
deploy:
  stage: deploy
  script:
    - ./deploy.sh
    - |
      # Send deployment event to monitoring
      curl -X POST https://monitoring.example.com/api/events \
        -H "Authorization: Bearer $MONITORING_API_KEY" \
        -d "{
          \"event\": \"deployment\",
          \"version\": \"$CI_COMMIT_SHA\",
          \"environment\": \"production\",
          \"timestamp\": \"$(date -u +%Y-%m-%dT%H:%M:%SZ)\"
        }"
  environment:
    name: production
```

---

## Exam Tips for Domain 8

1. **Environments** - Know name, url, on_stop, auto_stop_in
2. **Manual Deployments** - Understand when: manual
3. **Blue-Green** - Know traffic switching strategy
4. **Canary** - Understand gradual rollout
5. **Rolling** - Know kubectl rollout commands
6. **Rollback** - Understand rollback strategies
7. **Review Apps** - Dynamic environments for MRs
8. **Kubernetes** - Know kubectl and helm deployment
9. **Health Checks** - Verify deployment success
10. **Environment Tiers** - development, staging, production

---

## Quick Reference

### Basic Deployment

```yaml
deploy:
  stage: deploy
  script:
    - ./deploy.sh
  environment:
    name: production
    url: https://example.com
  only:
    - main
  when: manual
```

### Review App

```yaml
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
```

### Kubernetes Deployment

```yaml
deploy-k8s:
  stage: deploy
  image: bitnami/kubectl:latest
  script:
    - kubectl set image deployment/app app=$CI_REGISTRY_IMAGE:$CI_COMMIT_SHA
    - kubectl rollout status deployment/app
  environment:
    name: production
```
