# Domain 1: GitLab Fundamentals & Version Control

## Task 1.1: GitLab Project Structure ⭐

### GitLab Hierarchy

| Level | Description | Purpose |
|-------|-------------|---------|
| **Instance** | Top-level GitLab installation | Self-managed or GitLab.com |
| **Group** | Collection of projects | Organize related projects |
| **Subgroup** | Nested groups | Hierarchical organization |
| **Project** | Git repository + features | Code, CI/CD, issues, wiki |

### Project Components

| Component | Description |
|-----------|-------------|
| **Repository** | Git version control |
| **Issues** | Task and bug tracking |
| **Merge Requests** | Code review and collaboration |
| **CI/CD** | Pipelines and automation |
| **Wiki** | Documentation |
| **Snippets** | Code sharing |
| **Container Registry** | Docker image storage |
| **Package Registry** | Artifact storage |

---

## Task 1.2: Git Basics for GitLab ⭐⭐

### Essential Git Commands

```bash
# Initialize repository
git init
git clone <repository-url>

# Configure Git
git config --global user.name "Your Name"
git config --global user.email "your.email@example.com"

# Check status
git status
git log
git log --oneline --graph --all

# Stage changes
git add <file>
git add .
git add -A

# Commit changes
git commit -m "Commit message"
git commit -am "Add and commit"

# Push changes
git push origin <branch>
git push -u origin <branch>  # Set upstream

# Pull changes
git pull origin <branch>
git fetch origin
```

### Branching and Merging

```bash
# Create branch
git branch <branch-name>
git checkout -b <branch-name>  # Create and switch
git switch -c <branch-name>    # Modern syntax

# Switch branches
git checkout <branch-name>
git switch <branch-name>

# List branches
git branch                     # Local branches
git branch -r                  # Remote branches
git branch -a                  # All branches

# Merge branches
git merge <branch-name>
git merge --no-ff <branch-name>  # Force merge commit

# Delete branch
git branch -d <branch-name>    # Safe delete
git branch -D <branch-name>    # Force delete
git push origin --delete <branch-name>  # Delete remote
```

### Remote Operations

```bash
# Add remote
git remote add origin <url>

# View remotes
git remote -v

# Fetch from remote
git fetch origin
git fetch --all

# Pull with rebase
git pull --rebase origin main

# Push tags
git push --tags
```

---

## Task 1.3: GitLab Workflow ⭐

### GitLab Flow

| Stage | Action | Purpose |
|-------|--------|---------|
| **1. Create Issue** | Document task/bug | Track work |
| **2. Create Branch** | Feature/fix branch | Isolate changes |
| **3. Make Changes** | Code modifications | Implement solution |
| **4. Commit & Push** | Save and upload | Version control |
| **5. Create MR** | Merge request | Code review |
| **6. Review & Approve** | Team review | Quality assurance |
| **7. Merge** | Integrate changes | Deploy to main |
| **8. Close Issue** | Mark complete | Track progress |

### Branch Naming Conventions

```bash
# Feature branches
feature/<issue-number>-<description>
feature/123-add-login-page

# Bug fix branches
bugfix/<issue-number>-<description>
bugfix/456-fix-memory-leak

# Hotfix branches
hotfix/<issue-number>-<description>
hotfix/789-critical-security-patch

# Release branches
release/v1.2.0
release/2024-01-15
```

---

## Task 1.4: Merge Requests (MR) ⭐⭐

### Creating Merge Requests

```bash
# Via Git push
git push -o merge_request.create origin <branch>

# With options
git push -o merge_request.create \
  -o merge_request.target=main \
  -o merge_request.title="Add new feature" \
  origin <branch>
```

### MR Best Practices

| Practice | Description |
|----------|-------------|
| **Descriptive Title** | Clear, concise summary |
| **Detailed Description** | Context, changes, testing |
| **Link Issues** | Closes #123, Relates to #456 |
| **Assign Reviewers** | Request specific reviews |
| **Add Labels** | Categorize MR |
| **Set Milestone** | Track release |
| **Enable CI/CD** | Run pipeline automatically |
| **Resolve Discussions** | Address all comments |

### MR Merge Strategies

| Strategy | Description | Use Case |
|----------|-------------|----------|
| **Merge Commit** | Creates merge commit | Default, preserves history |
| **Squash and Merge** | Combines commits | Clean history |
| **Fast-forward** | Linear history | No merge commit |
| **Rebase and Merge** | Reapply commits | Linear, detailed history |

---

## Task 1.5: GitLab Issues ⭐

### Issue Management

```markdown
# Issue Template Example
## Description
Brief description of the issue

## Steps to Reproduce
1. Step one
2. Step two
3. Step three

## Expected Behavior
What should happen

## Actual Behavior
What actually happens

## Environment
- OS: Ubuntu 22.04
- Browser: Chrome 120
- GitLab Version: 16.7

## Additional Context
Screenshots, logs, etc.
```

### Issue Features

| Feature | Description |
|---------|-------------|
| **Labels** | Categorize issues (bug, feature, priority) |
| **Milestones** | Group issues by release/sprint |
| **Assignees** | Assign responsibility |
| **Due Dates** | Set deadlines |
| **Weight** | Estimate effort (1-10) |
| **Epic** | Group related issues |
| **Time Tracking** | /estimate, /spend commands |
| **Related Issues** | Link dependencies |

### Quick Actions (Slash Commands)

```markdown
/assign @username          # Assign to user
/label ~bug ~priority::high  # Add labels
/milestone %v1.0          # Set milestone
/due 2024-12-31           # Set due date
/estimate 2h              # Estimate time
/spend 1h 30m             # Log time spent
/close                    # Close issue
/reopen                   # Reopen issue
/duplicate #123           # Mark as duplicate
/relate #456              # Relate to another issue
```

---

## Task 1.6: GitLab Groups and Permissions ⭐

### Permission Levels

| Role | Permissions |
|------|-------------|
| **Guest** | View issues, leave comments |
| **Reporter** | Pull code, create issues |
| **Developer** | Push code, manage issues/MRs |
| **Maintainer** | Manage project settings, protected branches |
| **Owner** | Full access, delete project |

### Group Features

| Feature | Description |
|---------|-------------|
| **Shared Runners** | CI/CD runners for all projects |
| **Group Variables** | Shared CI/CD variables |
| **Group Labels** | Consistent labeling |
| **Group Milestones** | Cross-project milestones |
| **Subgroups** | Nested organization |
| **SAML SSO** | Enterprise authentication |

---

## Task 1.7: Protected Branches ⭐

### Branch Protection Rules

```yaml
# Settings > Repository > Protected Branches

Protected Branch: main
Allowed to merge: Maintainers
Allowed to push: No one
Allowed to force push: No
Code owner approval: Required
```

### Protection Levels

| Level | Description |
|-------|-------------|
| **No one** | Completely protected |
| **Developers + Maintainers** | Team access |
| **Maintainers** | Senior team only |
| **Specific users** | Named individuals |

### Wildcard Protection

```bash
# Protect all release branches
release/*

# Protect all feature branches
feature/*

# Protect version tags
v*.*.*
```

---

## Task 1.8: GitLab Tags and Releases ⭐

### Creating Tags

```bash
# Lightweight tag
git tag v1.0.0

# Annotated tag (recommended)
git tag -a v1.0.0 -m "Release version 1.0.0"

# Tag specific commit
git tag -a v1.0.0 <commit-hash> -m "Release 1.0.0"

# Push tags
git push origin v1.0.0
git push origin --tags  # Push all tags

# Delete tag
git tag -d v1.0.0
git push origin --delete v1.0.0
```

### Semantic Versioning

| Version | Format | Example |
|---------|--------|---------|
| **Major** | Breaking changes | v2.0.0 |
| **Minor** | New features | v1.5.0 |
| **Patch** | Bug fixes | v1.4.3 |
| **Pre-release** | Alpha/Beta | v1.0.0-beta.1 |

### GitLab Releases

```markdown
# Release Notes Template
## What's New
- Feature A: Description
- Feature B: Description

## Bug Fixes
- Fixed issue #123
- Resolved memory leak

## Breaking Changes
- API endpoint changed from /v1 to /v2

## Upgrade Notes
1. Backup database
2. Run migration script
3. Update configuration

## Contributors
@user1, @user2, @user3
```

---

## Task 1.9: GitLab Repository Settings ⭐

### Repository Configuration

| Setting | Purpose |
|---------|---------|
| **Default Branch** | Main development branch |
| **Repository Size** | Storage limits |
| **Push Rules** | Commit message format |
| **Mirror Repository** | Sync with external repo |
| **Deploy Keys** | Read-only SSH access |
| **Deploy Tokens** | CI/CD authentication |

### Push Rules Example

```yaml
# Commit message format
^(feat|fix|docs|style|refactor|test|chore)(\(.+\))?: .{1,50}

# Examples:
feat(auth): add OAuth2 support
fix(api): resolve timeout issue
docs: update README
```

---

## Task 1.10: GitLab Snippets ⭐

### Snippet Types

| Type | Visibility | Use Case |
|------|-----------|----------|
| **Personal** | User-owned | Personal code |
| **Project** | Project-scoped | Project utilities |
| **Public** | Everyone | Share publicly |
| **Internal** | Logged-in users | Company-wide |
| **Private** | Specific users | Confidential |

### Creating Snippets

```bash
# Via GitLab UI
1. Navigate to Snippets
2. Click "New snippet"
3. Add title and description
4. Add files
5. Set visibility
6. Create snippet

# Clone snippet
git clone <snippet-url>
```

---

## Exam Tips for Domain 1

1. **Git Commands** - Master basic Git operations (clone, commit, push, pull, merge)
2. **Branching** - Understand branch creation, switching, and deletion
3. **Merge Requests** - Know MR creation, review, and merge strategies
4. **Issues** - Understand issue management and quick actions
5. **Permissions** - Know role-based access control
6. **Protected Branches** - Understand branch protection rules
7. **Tags** - Know how to create and manage tags
8. **GitLab Flow** - Understand the complete workflow
9. **Remote Operations** - Master fetch, pull, and push
10. **Conflict Resolution** - Know how to resolve merge conflicts

---

## Common Git Scenarios

### Undo Last Commit (Not Pushed)

```bash
# Keep changes
git reset --soft HEAD~1

# Discard changes
git reset --hard HEAD~1
```

### Resolve Merge Conflicts

```bash
# 1. Pull latest changes
git pull origin main

# 2. Resolve conflicts in files
# Edit files, remove conflict markers

# 3. Stage resolved files
git add <resolved-files>

# 4. Complete merge
git commit -m "Resolve merge conflicts"

# 5. Push changes
git push origin <branch>
```

### Stash Changes

```bash
# Save changes temporarily
git stash

# List stashes
git stash list

# Apply stash
git stash apply
git stash pop  # Apply and remove

# Drop stash
git stash drop
```

### Cherry-pick Commits

```bash
# Apply specific commit to current branch
git cherry-pick <commit-hash>

# Cherry-pick multiple commits
git cherry-pick <commit1> <commit2>

# Cherry-pick without committing
git cherry-pick -n <commit-hash>
```

---

## GitLab CLI (glab)

```bash
# Install glab
# macOS: brew install glab
# Linux: snap install glab

# Authenticate
glab auth login

# Create MR
glab mr create --title "Feature X" --description "Adds feature X"

# List MRs
glab mr list

# View MR
glab mr view 123

# Approve MR
glab mr approve 123

# Merge MR
glab mr merge 123

# Create issue
glab issue create --title "Bug report"

# List issues
glab issue list
```
