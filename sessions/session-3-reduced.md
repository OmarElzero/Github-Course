# Session 3 — GitHub Collaboration (Quick Reference)

**Duration:** 30-40 minutes  
**Format:** Summary + Commands + Challenge

---

## Quick Summary

**GitHub collaboration** enables teams to work together safely through Pull Requests, code reviews, and proper permissions. Professional workflows prevent code conflicts and maintain quality.

**Key Concepts:**
- SSH vs PAT for authentication
- Pull Requests for code review
- Git Flow for structured releases
- Tags for version marking
- Cherry-pick for selective commits

---

## Part 1: Authentication Methods

### SSH Keys (Recommended)

**Generate SSH key:**
```bash
# Generate key
ssh-keygen -t ed25519 -C "your.email@example.com"

# Start SSH agent
eval "$(ssh-agent -s)"

# Add key to agent
ssh-add ~/.ssh/id_ed25519

# Copy public key
cat ~/.ssh/id_ed25519.pub
```

**Add to GitHub:**
1. GitHub → Settings → SSH and GPG keys
2. Click "New SSH key"
3. Paste public key
4. Save

**Use SSH URL:**
```bash
git remote add origin git@github.com:username/repo.git
```

### Personal Access Token

**Create token:**
1. GitHub → Settings → Developer settings → Tokens
2. Generate new token (classic)
3. Select "repo" scope
4. Copy token (save securely!)

**Use with HTTPS:**
```bash
git remote add origin https://github.com/username/repo.git
# Use token as password when prompted
```

---

## Part 2: Pull Requests (PRs)

### Creating a PR

```bash
# 1. Create feature branch
git switch -c feature-name

# 2. Make changes and commit
git add .
git commit -m "Add feature"

# 3. Push branch to GitHub
git push -u origin feature-name

# 4. On GitHub:
# - Click "Compare & pull request"
# - Add title and description
# - Request reviewers
# - Create pull request
```

### PR Best Practices

**Good PR:**
- ✅ Small, focused changes
- ✅ Clear title and description
- ✅ Links to issue/ticket
- ✅ Tests included
- ✅ One purpose per PR

**Bad PR:**
- ❌ Multiple unrelated changes
- ❌ Vague description
- ❌ Hundreds of files changed
- ❌ No tests

### Reviewing PRs

```bash
# Fetch PR to test locally
git fetch origin pull/123/head:pr-123
git switch pr-123

# Test the code
# Run tests
# Check functionality

# Leave review on GitHub:
# - Comment on specific lines
# - Request changes or approve
# - Suggest improvements
```

### Merging PRs

**Three merge strategies:**

1. **Merge commit** (default):
   ```
   Keeps all commits + creates merge commit
   ```

2. **Squash and merge**:
   ```
   Combines all commits into one
   Clean history, loses detail
   ```

3. **Rebase and merge**:
   ```
   Replays commits on base
   Linear history
   ```

---

## Part 3: Git Flow Workflow

### Branch Strategy

```
main         (production-ready)
  ↓
develop      (integration branch)
  ↓
feature/     (new features)
hotfix/      (urgent fixes)
release/     (release preparation)
```

### Git Flow Commands

```bash
# Start new feature
git switch -c feature/user-auth develop

# Work on feature
git add .
git commit -m "Add login functionality"

# Finish feature (merge to develop)
git switch develop
git merge feature/user-auth
git branch -d feature/user-auth

# Create release branch
git switch -c release/1.0.0 develop

# Merge release to main and develop
git switch main
git merge release/1.0.0
git tag -a v1.0.0 -m "Version 1.0.0"

git switch develop
git merge release/1.0.0
git branch -d release/1.0.0

# Hotfix for production
git switch -c hotfix/critical-bug main
# Fix bug
git switch main
git merge hotfix/critical-bug
git tag -a v1.0.1 -m "Hotfix 1.0.1"

git switch develop
git merge hotfix/critical-bug
```

---

## Part 4: GitHub Flow (Simpler Alternative)

### Workflow

```
main (always deployable)
  ↓
feature branch
  ↓
Pull Request
  ↓
Code Review
  ↓
Merge to main
  ↓
Deploy
```

### Commands

```bash
# 1. Create feature branch from main
git switch -c feature-name

# 2. Make commits
git commit -am "Implement feature"

# 3. Push and create PR
git push -u origin feature-name

# 4. After review, merge on GitHub

# 5. Update local main
git switch main
git pull
git branch -d feature-name
```

---

## Part 5: Tags & Releases

### Creating Tags

```bash
# Lightweight tag
git tag v1.0.0

# Annotated tag (recommended)
git tag -a v1.0.0 -m "Release version 1.0.0"

# Tag specific commit
git tag -a v1.0.0 9fceb02 -m "Version 1.0.0"

# Push tags
git push --tags

# List tags
git tag
git tag -l "v1.*"
```

### Semantic Versioning

```
v1.2.3
│ │ │
│ │ └─ PATCH (bug fixes)
│ └─── MINOR (new features, backwards compatible)
└───── MAJOR (breaking changes)
```

### Creating Releases on GitHub

1. Go to repository → Releases
2. Click "Create a new release"
3. Choose tag or create new
4. Add release notes
5. Attach binaries (optional)
6. Publish

---

## Part 6: Cherry-Pick

### What is Cherry-Pick?

Copy specific commits from one branch to another.

```
main:     A ─ B ─ C
                    ↓ cherry-pick commit X
feature:  A ─ X ─ Y
```

### Commands

```bash
# Cherry-pick single commit
git switch main
git cherry-pick abc123f

# Cherry-pick multiple commits
git cherry-pick abc123f def456a

# Cherry-pick range
git cherry-pick abc123f..def456a

# Cherry-pick without committing
git cherry-pick -n abc123f

# Abort cherry-pick
git cherry-pick --abort

# Continue after conflict
git cherry-pick --continue
```

### Use Cases

- Port bug fix to release branch
- Apply specific feature to different version
- Recover commits from deleted branch

---

## Complete Command Reference

```bash
# SSH SETUP
ssh-keygen -t ed25519 -C "email"        # Generate SSH key
ssh-add ~/.ssh/id_ed25519               # Add to agent
cat ~/.ssh/id_ed25519.pub               # Display public key

# PULL REQUESTS
git push -u origin branch-name          # Push branch for PR
git fetch origin pull/ID/head:pr-ID     # Fetch PR locally

# GIT FLOW
git switch -c feature/name develop      # New feature branch
git switch -c hotfix/name main          # New hotfix branch
git switch -c release/1.0.0 develop     # New release branch

# TAGS
git tag v1.0.0                          # Lightweight tag
git tag -a v1.0.0 -m "message"          # Annotated tag
git push --tags                          # Push all tags
git tag -d v1.0.0                        # Delete local tag
git push origin --delete v1.0.0          # Delete remote tag
git tag -l                               # List tags

# CHERRY-PICK
git cherry-pick commit-hash             # Apply commit to current branch
git cherry-pick -n commit-hash          # Apply without committing
git cherry-pick --abort                 # Cancel cherry-pick
git cherry-pick --continue              # Continue after conflict

# COLLABORATION
git pull --rebase                       # Pull with rebase
git push --force-with-lease             # Safe force push
git fetch --all                         # Fetch all remotes
```

---

## Comprehensive Challenge (40 minutes)

### Task: Team Collaboration Simulation

**Scenario:** You're working on a team blog platform.

**Setup:**
1. Create repository "team-blog"
2. Add collaborators (or simulate with branches)
3. Set up SSH authentication

**Tasks:**

**Part 1: Feature Development (15 min)**
```bash
# Create develop branch
git switch -c develop

# Create feature branch
git switch -c feature/add-comments develop

# Create comment functionality
# File: comments.py
"""
def add_comment(post_id, user, text):
    return {
        'post': post_id,
        'user': user,
        'text': text,
        'timestamp': 'now'
    }
"""

# Commit
git add comments.py
git commit -m "Add comment functionality"

# Push and create PR
git push -u origin feature/add-comments
```

**Part 2: Hotfix (10 min)**
```bash
# Switch to main
git switch main

# Create hotfix
git switch -c hotfix/fix-login-bug

# Fix the bug (simulate)
echo "Fixed login validation" > fix.txt
git add fix.txt
git commit -m "Fix critical login validation bug"

# Merge to main
git switch main
git merge hotfix/fix-login-bug

# Tag the hotfix
git tag -a v1.0.1 -m "Hotfix: Login validation"

# Merge to develop
git switch develop
git merge hotfix/fix-login-bug
```

**Part 3: Cherry-Pick (10 min)**
```bash
# Feature branch needs the hotfix
git switch feature/add-comments

# Find the hotfix commit hash
git log main --oneline -5

# Cherry-pick the fix
git cherry-pick <hotfix-commit-hash>
```

**Part 4: Release (5 min)**
```bash
# Create release branch
git switch -c release/2.0.0 develop

# Merge to main
git switch main
git merge release/2.0.0

# Create release tag
git tag -a v2.0.0 -m "Release 2.0.0: Comments feature"

# Push everything
git push --all
git push --tags
```

**Success Criteria:**
- ✅ SSH authentication working
- ✅ Feature branch created and pushed
- ✅ Hotfix applied to multiple branches
- ✅ Cherry-pick successful
- ✅ Tags created for versions
- ✅ Clean Git history

---

## Common Collaboration Patterns

**Daily Workflow:**
```bash
# Morning: Update local branches
git switch main
git pull
git switch develop
git pull

# Start new feature
git switch -c feature/my-feature develop

# Regular commits during day
git add .
git commit -m "Descriptive message"

# End of day: Push work
git push -u origin feature/my-feature
```

**Before Creating PR:**
```bash
# Update with latest develop
git switch develop
git pull
git switch feature/my-feature
git rebase develop

# Run tests
# Fix any conflicts
# Push (may need force)
git push --force-with-lease
```

**After PR Merged:**
```bash
# Update main/develop
git switch main
git pull

# Delete feature branch
git branch -d feature/my-feature
git push origin --delete feature/my-feature
```

---

## Tips for Team Collaboration

**Communication:**
- Write clear PR descriptions
- Link to issues/tickets
- Tag reviewers
- Respond to feedback promptly

**Code Quality:**
- Keep PRs small (< 400 lines)
- One feature per PR
- Include tests
- Update documentation

**Branch Management:**
- Delete merged branches
- Keep branch names descriptive
- Follow team naming conventions
- Don't push to main directly

**Conflict Prevention:**
- Pull frequently
- Communicate with team
- Avoid working on same files
- Use feature toggles for WIP

---

## What You've Learned

✅ SSH vs PAT authentication  
✅ Creating and reviewing Pull Requests  
✅ Git Flow and GitHub Flow workflows  
✅ Creating tags and releases  
✅ Cherry-picking commits  
✅ Professional collaboration patterns  

**Next:** Session 4 covers open source contribution and advanced GitHub features!

---

**End of Session 3 Quick Reference**
