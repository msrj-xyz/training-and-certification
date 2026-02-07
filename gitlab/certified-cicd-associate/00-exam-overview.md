# GitLab Certified CI/CD Associate

## Exam Information

| Aspect | Details |
|--------|---------|
| **Exam Code** | GitLab Certified CI/CD Associate |
| **Level** | Associate |
| **Duration** | Part 1: 60 minutes (Written) + Part 2: 120 minutes (Hands-on) |
| **Questions** | Part 1: 14 multiple-choice + Part 2: 14 hands-on tasks |
| **Passing Score** | Part 1: 80% (to proceed to Part 2) |
| **Format** | Multiple-choice + Hands-on lab |
| **Cost** | Varies by region |
| **Validity** | 2 years |
| **Retakes** | Part 1: Unlimited retakes, Part 2: Limited |

---

## Exam Structure

### Part 1: Written Assessment (60 minutes)
- 14 multiple-choice questions
- 80% accuracy required to proceed
- Can be retaken indefinitely
- Tests theoretical knowledge of GitLab CI/CD concepts

### Part 2: Hands-on Lab (120 minutes)
- 14 practical tasks
- Performed on a GitLab project you create
- Graded by GitLab Professional Services engineers
- Tests real-world implementation skills

---

## Domain Weightings

| Domain | Focus Areas |
|--------|-------------|
| **Domain 1** | GitLab Fundamentals & Version Control |
| **Domain 2** | CI/CD Pipeline Configuration |
| **Domain 3** | GitLab Runners & Executors |
| **Domain 4** | Jobs, Stages & Workflow |
| **Domain 5** | Artifacts, Cache & Dependencies |
| **Domain 6** | Variables & Secrets Management |
| **Domain 7** | Pipeline Security & Compliance |
| **Domain 8** | Deployment Strategies |
| **Domain 9** | Monitoring & Troubleshooting |

---

## Target Candidate Profile

- DevOps engineers and CI/CD practitioners
- Software developers implementing automation
- System administrators managing GitLab
- Team leads overseeing CI/CD processes
- Anyone working with GitLab for continuous integration/delivery

---

## Prerequisites

### Required Knowledge
- Basic understanding of Git version control
- Familiarity with command-line interface
- Understanding of software development lifecycle
- Basic knowledge of YAML syntax
- Containerization concepts (Docker basics)

### Recommended Experience
- 3-6 months working with GitLab
- Experience with CI/CD concepts
- Basic scripting knowledge (Bash, PowerShell)
- Understanding of DevOps principles

---

## Exam Environment

### Technical Requirements
| Aspect | Details |
|--------|---------|
| **Platform** | GitLab.com or self-managed instance |
| **Browser** | Modern web browser (Chrome, Firefox, Edge) |
| **Access** | GitLab account with project creation rights |
| **Tools** | Git CLI, text editor, terminal access |
| **Internet** | Stable connection required |

### Pre-configured Features
- GitLab project with CI/CD enabled
- Access to GitLab documentation
- GitLab Runner (for hands-on tasks)
- Sample .gitlab-ci.yml templates

---

## Resources Allowed During Exam

| Resource | URL |
|----------|-----|
| **GitLab Docs** | https://docs.gitlab.com |
| **GitLab CI/CD** | https://docs.gitlab.com/ee/ci/ |
| **GitLab Runner** | https://docs.gitlab.com/runner/ |
| **YAML Reference** | https://docs.gitlab.com/ee/ci/yaml/ |

> **Exam Tip:** Bookmark key documentation pages before the exam!

---

## Key Topics to Master

### GitLab Fundamentals
- Projects, groups, and namespaces
- Issues, merge requests, and milestones
- Branching strategies and workflows
- Repository management

### CI/CD Core Concepts
- Pipeline architecture
- .gitlab-ci.yml syntax and structure
- Job execution and dependencies
- Stage ordering and parallelization

### GitLab Runners
- Runner types (shared, group, specific)
- Executor types (shell, docker, kubernetes)
- Runner registration and configuration
- Tags and runner selection

### Pipeline Configuration
- Job definitions and scripts
- Before_script and after_script
- Rules, only, and except keywords
- Needs and dependencies
- Parallel and matrix jobs

### Artifacts & Cache
- Artifact creation and consumption
- Cache configuration and strategies
- Dependency management
- Build optimization

### Variables & Secrets
- CI/CD variables (predefined and custom)
- Variable precedence and scope
- Protected and masked variables
- File-type variables
- Integration with HashiCorp Vault

### Security & Compliance
- SAST, DAST, and dependency scanning
- Container scanning
- License compliance
- Security dashboards
- Protected branches and environments

### Deployment
- Environment management
- Manual vs automatic deployment
- Review apps
- Deployment strategies (blue-green, canary)
- Kubernetes integration

---

## Exam Day Preparation

### Before the Exam (Part 1)
1. Review GitLab CI/CD documentation
2. Practice YAML syntax
3. Understand pipeline keywords
4. Review Git commands
5. Prepare stable internet connection

### Before the Exam (Part 2)
1. Create a GitLab project for testing
2. Set up GitLab Runner (if required)
3. Test .gitlab-ci.yml configurations
4. Verify project permissions
5. Have documentation bookmarks ready

### During the Exam
1. Read each question carefully
2. Use GitLab documentation when needed
3. Test configurations before submitting
4. Check pipeline execution logs
5. Validate job outputs and artifacts
6. Use descriptive commit messages
7. Organize your project structure

---

## Practice Resources

### GitLab Learn Platform
- Official GitLab training courses
- Hands-on labs and exercises
- Video tutorials
- Practice projects

### Self-Study
- Create sample CI/CD pipelines
- Experiment with different runners
- Practice artifact and cache usage
- Test various deployment scenarios
- Troubleshoot pipeline failures

---

## Exam Tips Summary

1. **Master .gitlab-ci.yml syntax** - Core of the exam
2. **Understand job dependencies** - needs vs dependencies
3. **Know artifact vs cache** - Different use cases
4. **Practice runner configuration** - Tags and executors
5. **Learn variable precedence** - Scope and priority
6. **Security scanning** - Enable and configure
7. **Environment management** - Deployment targets
8. **Pipeline optimization** - Parallel jobs and caching
9. **Troubleshooting** - Read logs effectively
10. **Git basics** - Commands not covered in modules

---

## Common Pitfalls to Avoid

| Issue | Solution |
|-------|----------|
| **YAML indentation errors** | Use 2 spaces, validate syntax |
| **Missing runner tags** | Ensure job tags match runner tags |
| **Artifact expiration** | Set appropriate expiry times |
| **Cache key conflicts** | Use unique cache keys per branch |
| **Variable scope confusion** | Understand global vs job-level |
| **Incorrect dependencies** | Use needs for DAG pipelines |
| **Protected variable access** | Check branch protection settings |
| **Runner executor mismatch** | Choose appropriate executor type |

---

## Post-Certification

### Badge and Recognition
- Digital badge from GitLab
- LinkedIn certification display
- GitLab Certified Associate title
- Valid for 2 years

### Career Benefits
- Validates GitLab CI/CD expertise
- Demonstrates DevOps proficiency
- Enhances job opportunities
- Supports team credibility

---

## Cheatsheet Files

- [Domain 1: GitLab Fundamentals & Version Control](./01-gitlab-fundamentals.md)
- [Domain 2: CI/CD Pipeline Configuration](./02-pipeline-configuration.md)
- [Domain 3: GitLab Runners & Executors](./03-runners-executors.md)
- [Domain 4: Jobs, Stages & Workflow](./04-jobs-stages-workflow.md)
- [Domain 5: Artifacts, Cache & Dependencies](./05-artifacts-cache-dependencies.md)
- [Domain 6: Variables & Secrets Management](./06-variables-secrets.md)
- [Domain 7: Pipeline Security & Compliance](./07-security-compliance.md)
- [Domain 8: Deployment Strategies](./08-deployment-strategies.md)
- [Domain 9: Monitoring & Troubleshooting](./09-monitoring-troubleshooting.md)
- [**Quick Reference Card (1-page summary)**](./10-quick-reference-card.md)

---

## Additional Resources

- [GitLab CI/CD Documentation](https://docs.gitlab.com/ee/ci/)
- [GitLab CI/CD Examples](https://docs.gitlab.com/ee/ci/examples/)
- [GitLab Runner Documentation](https://docs.gitlab.com/runner/)
- [GitLab CI/CD YAML Reference](https://docs.gitlab.com/ee/ci/yaml/)
- [GitLab Learn Platform](https://about.gitlab.com/learn/)
