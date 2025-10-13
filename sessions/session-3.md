# Session 3 — The Internship: Branching & Professional Workflow

**Duration:** 2 hours 30 minutes  
**Venue:** Creativa Lab C  
**Instructors:** Omar Betawy, Amr Khaled

---

## 📖 Story Introduction

**Sara** is thrilled! After showing her GitHub portfolio with the Student Management System project, she landed a summer internship at **TechWave Solutions**, a fast-growing software company. On her first day, her mentor **Karim** shows her the company's main product codebase.

Sara is overwhelmed. The repository has:
- **15 active branches** for different features
- Multiple developers working simultaneously
- Automated testing and deployment
- Strict code review processes

"Welcome to professional software development," Karim smiles. "Here's how we work: we never push directly to `main`. Every feature gets its own branch. Every bug fix is isolated. We use Git Flow to manage releases."

Sara realizes college projects were just the beginning. Today, you'll learn the advanced branching strategies used by real companies to manage complex development workflows.

---

## 🎯 Session Objectives

By the end of this session, you will be able to:
- Understand why branches are essential for team development
- Create, switch, and manage multiple branches
- Merge branches using different strategies
- Use rebase to maintain clean history
- Implement Git Flow and GitHub Flow
- Handle complex merge conflicts
- Undo mistakes safely with reset, revert, and stash
- Use tags for versioning and releases
- Conduct professional code reviews

---

## 🌳 Part 1: Understanding Branches

### What is a Branch?

A **branch** is an independent line of development. Think of it as a parallel universe where you can make changes without affecting the main timeline.

**[IMAGE: Tree diagram showing main branch with feature branches]**  
*Description: Main trunk with multiple branches growing from it*

### Why Use Branches?

**Scenario without branches:**
```
main: feature1 → bug → feature2 → broken → fix → feature3
```
Everything is mixed! Hard to track what belongs where.

**Scenario with branches:**
```
main:     v1.0 ────────────────→ v1.1 ──────→ v2.0
            ↓                      ↑            ↑
feature-1:  └─→ work ─→ work ─→─┘            │
bug-fix:         └─→ fix ──────────────────→─┘
feature-2:              └─→ work ─→ work ─→─┘
```
Clean, organized, trackable!

**[IMAGE: Comparison diagram of linear vs branched development]**  
*Description: Messy single line vs organized branches*

### Real-World Branch Uses

1. **Feature Development**: Each new feature gets its own branch
2. **Bug Fixes**: Isolate fixes to test separately
3. **Experiments**: Try ideas without risk
4. **Releases**: Maintain stable release branches
5. **Hotfixes**: Quick fixes for production issues

---

## 🔄 Part 2: Branch Basics

### Checking Current Branch

```bash
git branch
```

**Expected Output:**
```
* main
```

The `*` indicates your current branch.

**[IMAGE: Terminal showing git branch output]**  
*Description: Branch list with asterisk*

### Creating a New Branch

**Method 1: Create but don't switch**
```bash
git branch feature/add-login
```

**Method 2: Create and switch immediately**
```bash
git checkout -b feature/add-login
```

**Method 3: Modern syntax (Git 2.23+)**
```bash
git switch -c feature/add-login
```

**[IMAGE: Terminal showing branch creation commands]**  
*Description: All three methods with outputs*

### Branch Naming Conventions

**Good practices:**
```
feature/user-authentication
bugfix/fix-login-error
hotfix/security-patch
release/v1.2.0
```

**Common prefixes:**
- `feature/` - New features
- `bugfix/` or `fix/` - Bug fixes
- `hotfix/` - Urgent production fixes
- `release/` - Release preparation
- `experimental/` - Experiments
- `docs/` - Documentation updates

**[IMAGE: Branch naming convention chart]**  
*Description: Prefix meanings and examples*

### Switching Between Branches

```bash
# Old way
git checkout main

# New way (Git 2.23+)
git switch main
```

**Important:** Uncommitted changes travel with you when switching!

---

## 💼 Part 3: TechWave's First Feature

### The Task

Karim assigns Sara her first task: Add a login system to the company's web application.

**[IMAGE: Trello/Jira-style task card]**  
*Description: Task card with description and acceptance criteria*

### Step 1: Create Feature Branch

```bash
# Make sure you're on main
git checkout main

# Pull latest changes
git pull origin main

# Create feature branch
git checkout -b feature/user-login
```

**[IMAGE: Terminal showing workflow]**  
*Description: Commands executed in sequence*

### Step 2: Develop the Feature

Create `auth.py`:

```python
# auth.py
# TechWave Solutions - User Authentication Module
# Author: Sara Ahmed
# Date: June 2025

import hashlib
import secrets
from datetime import datetime, timedelta

class User:
    """Represents a user in the system"""
    
    def __init__(self, username, email, password_hash):
        self.username = username
        self.email = email
        self.password_hash = password_hash
        self.created_at = datetime.now()
        self.last_login = None
        self.is_active = True
    
    def __repr__(self):
        return f"User({self.username}, {self.email})"

class AuthManager:
    """Handles user authentication operations"""
    
    def __init__(self):
        self.users = {}
        self.sessions = {}
    
    def hash_password(self, password):
        """Hash password using SHA-256"""
        salt = secrets.token_hex(16)
        pwd_hash = hashlib.sha256((password + salt).encode()).hexdigest()
        return f"{salt}${pwd_hash}"
    
    def verify_password(self, stored_hash, provided_password):
        """Verify password against stored hash"""
        salt, pwd_hash = stored_hash.split('$')
        test_hash = hashlib.sha256((provided_password + salt).encode()).hexdigest()
        return pwd_hash == test_hash
    
    def register_user(self, username, email, password):
        """Register a new user"""
        if username in self.users:
            return False, "Username already exists"
        
        if len(password) < 8:
            return False, "Password must be at least 8 characters"
        
        password_hash = self.hash_password(password)
        user = User(username, email, password_hash)
        self.users[username] = user
        
        return True, "User registered successfully"
    
    def login(self, username, password):
        """Authenticate user and create session"""
        if username not in self.users:
            return False, None, "Invalid credentials"
        
        user = self.users[username]
        
        if not user.is_active:
            return False, None, "Account is deactivated"
        
        if not self.verify_password(user.password_hash, password):
            return False, None, "Invalid credentials"
        
        # Create session token
        session_token = secrets.token_urlsafe(32)
        self.sessions[session_token] = {
            'username': username,
            'created_at': datetime.now(),
            'expires_at': datetime.now() + timedelta(hours=24)
        }
        
        user.last_login = datetime.now()
        return True, session_token, "Login successful"
    
    def verify_session(self, session_token):
        """Check if session is valid"""
        if session_token not in self.sessions:
            return False, "Invalid session"
        
        session = self.sessions[session_token]
        
        if datetime.now() > session['expires_at']:
            del self.sessions[session_token]
            return False, "Session expired"
        
        return True, session['username']
    
    def logout(self, session_token):
        """Destroy session"""
        if session_token in self.sessions:
            del self.sessions[session_token]
            return True
        return False

# Example usage
if __name__ == "__main__":
    auth = AuthManager()
    
    # Register user
    success, message = auth.register_user("sara", "sara@techwave.com", "SecurePass123")
    print(f"Registration: {message}")
    
    # Login
    success, token, message = auth.login("sara", "SecurePass123")
    print(f"Login: {message}")
    
    if success:
        # Verify session
        valid, username = auth.verify_session(token)
        print(f"Session valid for: {username}")
        
        # Logout
        auth.logout(token)
        print("Logged out successfully")
```

**[IMAGE: Code editor showing auth.py]**  
*Description: Complete authentication module*

### Step 3: Commit Your Work

```bash
git add auth.py
git commit -m "Add user authentication module with login/logout"
```

### Step 4: Add Tests

Create `test_auth.py`:

```python
# test_auth.py
# Unit tests for authentication module

import unittest
from auth import AuthManager, User

class TestAuthManager(unittest.TestCase):
    
    def setUp(self):
        """Set up test fixtures"""
        self.auth = AuthManager()
    
    def test_user_registration(self):
        """Test successful user registration"""
        success, message = self.auth.register_user(
            "testuser", 
            "test@example.com", 
            "password123"
        )
        self.assertTrue(success)
        self.assertIn("testuser", self.auth.users)
    
    def test_duplicate_username(self):
        """Test registration with duplicate username"""
        self.auth.register_user("testuser", "test1@example.com", "password123")
        success, message = self.auth.register_user("testuser", "test2@example.com", "password123")
        self.assertFalse(success)
        self.assertEqual(message, "Username already exists")
    
    def test_weak_password(self):
        """Test registration with weak password"""
        success, message = self.auth.register_user("testuser", "test@example.com", "123")
        self.assertFalse(success)
        self.assertIn("at least 8 characters", message)
    
    def test_successful_login(self):
        """Test login with correct credentials"""
        self.auth.register_user("testuser", "test@example.com", "password123")
        success, token, message = self.auth.login("testuser", "password123")
        self.assertTrue(success)
        self.assertIsNotNone(token)
    
    def test_failed_login(self):
        """Test login with incorrect password"""
        self.auth.register_user("testuser", "test@example.com", "password123")
        success, token, message = self.auth.login("testuser", "wrongpassword")
        self.assertFalse(success)
        self.assertIsNone(token)
    
    def test_session_verification(self):
        """Test session token verification"""
        self.auth.register_user("testuser", "test@example.com", "password123")
        success, token, _ = self.auth.login("testuser", "password123")
        
        valid, username = self.auth.verify_session(token)
        self.assertTrue(valid)
        self.assertEqual(username, "testuser")
    
    def test_logout(self):
        """Test logout functionality"""
        self.auth.register_user("testuser", "test@example.com", "password123")
        success, token, _ = self.auth.login("testuser", "password123")
        
        self.auth.logout(token)
        valid, _ = self.auth.verify_session(token)
        self.assertFalse(valid)

if __name__ == '__main__':
    unittest.main()
```

Commit the tests:

```bash
git add test_auth.py
git commit -m "Add comprehensive unit tests for authentication"
```

**[IMAGE: Terminal showing test file commit]**  
*Description: Git commit output for tests*

### Step 5: Check Your Branch History

```bash
git log --oneline
```

**Expected Output:**
```
b3c4d5e Add comprehensive unit tests for authentication
a2b3c4d Add user authentication module with login/logout
```

**[IMAGE: Terminal showing commit log]**  
*Description: Two commits on feature branch*

---

## 🔀 Part 4: Merging Strategies

### The Scenario

Sara's feature is ready. Meanwhile, her colleague **Hassan** has merged changes to `main`. Now Sara needs to integrate her work.

**[IMAGE: Diagram showing diverged branches]**  
*Description: main and feature branches with different commits*

### Strategy 1: Merge Commit (Default)

Creates a new "merge commit" that ties two branches together.

```bash
# Switch to main
git checkout main

# Pull latest changes
git pull origin main

# Merge feature branch
git merge feature/user-login
```

**Result:**
```
*   Merge branch 'feature/user-login' into main
|\
| * Add comprehensive unit tests
| * Add user authentication module
* | Other team's commit
* | Another commit
|/
* Previous commit
```

**[IMAGE: Git graph showing merge commit]**  
*Description: Two lines converging into one merge commit*

**Pros:**
- ✅ Preserves complete history
- ✅ Shows when features were integrated
- ✅ Non-destructive

**Cons:**
- ❌ Creates extra merge commits
- ❌ History can become cluttered

### Strategy 2: Fast-Forward Merge

If no changes occurred on main, Git simply moves the pointer forward.

```bash
git merge feature/user-login
```

**Result:**
```
Before:
main:    A - B
            \
feature:     C - D

After:
main:    A - B - C - D
```

**[IMAGE: Fast-forward merge diagram]**  
*Description: Linear progression without merge commit*

**Automatic** if possible, otherwise falls back to merge commit.

### Strategy 3: Squash Merge

Combines all feature commits into one commit on main.

```bash
git checkout main
git merge --squash feature/user-login
git commit -m "Add user authentication feature"
```

**Result:**
```
feature: A - B - C - D (multiple commits)
main:    E (single combined commit)
```

**[IMAGE: Squash merge visualization]**  
*Description: Multiple commits compressed into one*

**Pros:**
- ✅ Clean, linear history
- ✅ Each feature = one commit

**Cons:**
- ❌ Loses detailed commit history
- ❌ Harder to revert individual changes

### Strategy 4: Rebase (We'll cover this next!)

---

## ⚡ Part 5: Rebasing - The Secret Weapon

### What is Rebasing?

**Rebasing** replays your commits on top of another branch, creating a linear history.

**[IMAGE: Rebase vs Merge comparison diagram]**  
*Description: Left side showing merge commit tree, right side showing linear rebased history*

### Why Rebase?

**Benefits:**
- ✅ Clean, linear history
- ✅ Easier to understand commit progression
- ✅ No merge commit clutter
- ✅ Makes git log beautiful

**When to use:**
- ✅ On your local feature branch before merging
- ✅ To catch up with main branch
- ✅ To clean up commits before PR

**When NOT to use:**
- ❌ On public/shared branches
- ❌ After pushing to shared repository
- ❌ On commits others depend on

### Interactive Rebase Demo

Sara wants to clean up her commits before merging.

```bash
# View commit history
git log --oneline

# Output:
# b3c4d5e Add comprehensive unit tests for authentication
# a2b3c4d Add user authentication module with login/logout
# f1e2d3c Fix typo in comment
# a1b2c3d Update function name
```

Start interactive rebase:

```bash
git rebase -i HEAD~4
```

An editor opens:

```
pick a1b2c3d Update function name
pick f1e2d3c Fix typo in comment
pick a2b3c4d Add user authentication module with login/logout
pick b3c4d5e Add comprehensive unit tests for authentication

# Commands:
# p, pick = use commit
# r, reword = use commit, but edit the commit message
# e, edit = use commit, but stop for amending
# s, squash = use commit, but meld into previous commit
# f, fixup = like "squash", but discard this commit's log message
# d, drop = remove commit
```

**[IMAGE: Interactive rebase editor view]**  
*Description: Vim/nano editor showing rebase commands*

Sara decides to combine the small fixes:

```
pick a1b2c3d Update function name
fixup f1e2d3c Fix typo in comment
pick a2b3c4d Add user authentication module with login/logout
pick b3c4d5e Add comprehensive unit tests for authentication
```

Save and close. Result:

```
# New clean history:
b3c4d5e Add comprehensive unit tests for authentication
a2b3c4d Add user authentication module with login/logout
a1b2c3d Update function name
```

**[IMAGE: Before and after rebase comparison]**  
*Description: Messy history transformed into clean history*

### Rebasing onto Main

To update feature branch with latest main:

```bash
# On feature branch
git checkout feature/user-login

# Rebase onto main
git rebase main
```

**What happens:**
1. Git finds common ancestor
2. Saves your commits temporarily
3. Fast-forwards to main's latest commit
4. Replays your commits on top

**[IMAGE: Rebase process step-by-step diagram]**  
*Description: Three stages showing commit replay process*

### Handling Rebase Conflicts

If conflicts occur during rebase:

```bash
# Git pauses and shows:
# CONFLICT (content): Merge conflict in auth.py
# error: could not apply a2b3c4d... Add user authentication
```

**Resolve the conflict:**
1. Edit conflicting files
2. Remove conflict markers
3. Stage resolved files: `git add auth.py`
4. Continue rebase: `git rebase --continue`

**If you want to abort:**
```bash
git rebase --abort
```

**[IMAGE: Rebase conflict resolution workflow]**  
*Description: Flowchart showing resolve, continue, or abort options*

---

## 🏢 Part 6: Git Flow - Enterprise Workflow

### What is Git Flow?

**Git Flow** is a branching model designed for projects with scheduled releases.

**[IMAGE: Complete Git Flow diagram with all branch types]**  
*Description: Main, develop, feature, release, and hotfix branches*

### Branch Types in Git Flow

| Branch | Purpose | Lifespan | Base | Merge Into |
|--------|---------|----------|------|------------|
| `main` | Production code | Permanent | - | - |
| `develop` | Integration branch | Permanent | main | main |
| `feature/*` | New features | Temporary | develop | develop |
| `release/*` | Release prep | Temporary | develop | main & develop |
| `hotfix/*` | Urgent fixes | Temporary | main | main & develop |

### Git Flow Workflow

#### 1. Feature Development

```bash
# Start from develop
git checkout develop
git pull origin develop

# Create feature branch
git checkout -b feature/payment-integration

# Work on feature...
git add .
git commit -m "Add payment integration"

# Finish feature
git checkout develop
git merge feature/payment-integration
git push origin develop
git branch -d feature/payment-integration
```

**[IMAGE: Feature branch lifecycle in Git Flow]**  
*Description: Branch creation from develop, work, merge back*

#### 2. Release Preparation

```bash
# Create release branch from develop
git checkout develop
git checkout -b release/v1.5.0

# Bug fixes, version bumps, documentation updates
git commit -m "Bump version to 1.5.0"
git commit -m "Update CHANGELOG"

# Merge to main (production)
git checkout main
git merge release/v1.5.0
git tag -a v1.5.0 -m "Release version 1.5.0"
git push origin main --tags

# Also merge back to develop
git checkout develop
git merge release/v1.5.0

# Delete release branch
git branch -d release/v1.5.0
```

**[IMAGE: Release branch workflow]**  
*Description: Release branch merging to both main and develop*

#### 3. Hotfix (Emergency Fix)

```bash
# Branch from main (production)
git checkout main
git checkout -b hotfix/critical-security-fix

# Make the fix
git commit -m "Fix SQL injection vulnerability"

# Merge to main
git checkout main
git merge hotfix/critical-security-fix
git tag -a v1.5.1 -m "Hotfix: Security patch"
git push origin main --tags

# Also merge to develop
git checkout develop
git merge hotfix/critical-security-fix

# Delete hotfix branch
git branch -d hotfix/critical-security-fix
```

**[IMAGE: Hotfix branch workflow]**  
*Description: Emergency fix branching from and merging to main and develop*

### TechWave's Git Flow Implementation

Karim explains TechWave's setup:

```
main          [v1.0.0] ─────────────── [v1.1.0] ─────────
                           ↑               ↑
develop       ────────────────────────────────────────────
                 ↓        ↑       ↓      ↑
feature/login    └─ work ─┘       │      │
feature/payment            └─ work ──────┘
```

**Protected Branches:**
- `main` - Only release and hotfix merges allowed
- `develop` - Requires PR approval

---

## 🔄 Part 7: GitHub Flow - Simplified Workflow

### What is GitHub Flow?

A simpler alternative to Git Flow, used by GitHub itself and many companies.

**[IMAGE: GitHub Flow diagram]**  
*Description: Simple flow showing main branch with feature branches*

### Core Principles

1. **Main branch is always deployable**
2. **Create descriptive branches** from main
3. **Commit often** with clear messages
4. **Open Pull Request** early for feedback
5. **Deploy from branch** for testing
6. **Merge after approval** and automated checks pass

### GitHub Flow in Practice

```bash
# 1. Create branch from main
git checkout main
git pull origin main
git checkout -b add-notifications

# 2. Make commits
git commit -m "Add notification service"
git commit -m "Add email templates"
git commit -m "Add push notification support"

# 3. Push and create PR
git push origin add-notifications
# Go to GitHub and create Pull Request

# 4. Address review feedback
git commit -m "Address review comments"
git push origin add-notifications

# 5. After approval, merge via GitHub interface

# 6. Delete branch
git branch -d add-notifications
```

**[IMAGE: GitHub Flow step-by-step with screenshots]**  
*Description: Each step visualized with terminal and GitHub UI*

### Git Flow vs GitHub Flow

| Aspect | Git Flow | GitHub Flow |
|--------|----------|-------------|
| Complexity | High | Low |
| Branches | 5 types | 2 types (main + features) |
| Best For | Scheduled releases | Continuous deployment |
| Release Cycle | Weeks/months | Daily/hourly |
| Learning Curve | Steep | Gentle |

**[IMAGE: Comparison infographic]**  
*Description: Side-by-side visual comparison*

---

## 🎯 Part 8: Advanced Branch Operations

### Stashing - Save Work for Later

You're working on a feature but need to switch branches urgently:

```bash
# You have uncommitted changes
git status
# Output: modified: auth.py

# Stash your work
git stash save "Work in progress on auth module"

# Your working directory is now clean
git status
# Output: nothing to commit, working tree clean

# Switch to another branch
git checkout hotfix/urgent-bug

# After fixing the bug, come back
git checkout feature/user-login

# Restore your stashed work
git stash pop
```

**[IMAGE: Stash workflow diagram]**  
*Description: Changes being saved, branch switch, changes restored*

**Stash commands:**

```bash
git stash list              # View all stashes
git stash apply             # Apply stash but keep it
git stash pop               # Apply and delete stash
git stash drop stash@{0}    # Delete specific stash
git stash clear             # Delete all stashes
```

**[IMAGE: Terminal showing stash commands]**  
*Description: List of stashes with descriptions*

### Cherry-Picking - Copy Specific Commits

You want ONE commit from another branch:

```bash
# On feature-a, there's a useful commit
git log feature-a --oneline
# a1b2c3d Fix validation bug
# e4f5g6h Add feature A

# You want just the bug fix on your branch
git checkout feature-b
git cherry-pick a1b2c3d
```

**[IMAGE: Cherry-pick visualization]**  
*Description: Single commit being copied between branches*

**Use cases:**
- Apply hotfix to multiple branches
- Backport features to older versions
- Extract specific fixes

### Viewing Branch Differences

```bash
# See what's in feature that's not in main
git log main..feature/user-login

# See what's in main that's not in feature
git log feature/user-login..main

# See all commits in both but not in both
git log main...feature/user-login

# See actual file differences
git diff main..feature/user-login
```

**[IMAGE: Terminal showing log differences]**  
*Description: Commit logs showing branch divergence*

### Renaming Branches

```bash
# Rename current branch
git branch -m new-name

# Rename another branch
git branch -m old-name new-name

# Update remote
git push origin -u new-name
git push origin --delete old-name
```

---

## 🏷️ Part 9: Tags and Releases

### What are Tags?

**Tags** mark specific points in history, typically for releases.

**[IMAGE: Timeline with tags marking release versions]**  
*Description: Commit history with v1.0, v1.1, v2.0 tags*

### Types of Tags

**Lightweight tags** (simple pointers):
```bash
git tag v1.0.0
```

**Annotated tags** (with metadata - recommended):
```bash
git tag -a v1.0.0 -m "Release version 1.0.0"
```

### Creating a Release

Sara's team is releasing TechWave App v1.0:

```bash
# Make sure main is ready
git checkout main
git pull origin main

# Create annotated tag
git tag -a v1.0.0 -m "Initial release - User authentication system"

# Push tag to GitHub
git push origin v1.0.0

# Or push all tags
git push origin --tags
```

**[IMAGE: Terminal showing tag creation and push]**  
*Description: Tag commands with output*

### Viewing Tags

```bash
# List all tags
git tag

# Output:
# v0.1.0
# v0.2.0
# v1.0.0

# Show tag details
git show v1.0.0
```

**[IMAGE: Tag details output]**  
*Description: Tag information with commit details*

### Semantic Versioning

TechWave follows **SemVer**: `MAJOR.MINOR.PATCH`

```
v1.2.3
│ │ └── PATCH: Bug fixes
│ └──── MINOR: New features (backward compatible)
└────── MAJOR: Breaking changes
```

**Examples:**
- `v1.0.0` → `v1.0.1`: Bug fix
- `v1.0.1` → `v1.1.0`: New feature
- `v1.1.0` → `v2.0.0`: Breaking change

**[IMAGE: Semantic versioning breakdown]**  
*Description: Visual explanation of version numbers*

### Creating GitHub Release

1. Go to repository on GitHub
2. Click **Releases** → **Create a new release**
3. Choose tag: `v1.0.0`
4. Release title: "TechWave App v1.0.0"
5. Description:
   ```markdown
   ## Features
   - User registration and login
   - Session management
   - Password hashing
   - Comprehensive test suite
   
   ## Installation
   Download the source code and run:
   `pip install -r requirements.txt`
   
   ## Documentation
   See the [Wiki](link) for full documentation.
   ```
6. Attach binary files (optional)
7. Click **Publish release**

**[IMAGE: GitHub release creation form]**  
*Description: Form filled with release details*

**[IMAGE: Published release page]**  
*Description: Release showing download statistics and assets*

### Checking Out Tags

```bash
# View code at specific tag
git checkout v1.0.0

# Warning: You're in "detached HEAD" state
# To make changes, create a branch:
git checkout -b hotfix-from-v1.0.0
```

---

## ↩️ Part 10: Undoing Mistakes

### The Three Ways to Undo

| Command | Changes History? | Use When |
|---------|------------------|----------|
| `git revert` | No | Public branches |
| `git reset` | Yes | Local, not pushed |
| `git checkout` | No | Discard unstaged changes |

**[IMAGE: Undo commands comparison flowchart]**  
*Description: Decision tree for choosing undo method*

### Scenario 1: Undo Uncommitted Changes

**Discard changes in one file:**
```bash
git restore auth.py
# Or old syntax: git checkout -- auth.py
```

**Discard all changes:**
```bash
git restore .
```

**[IMAGE: Terminal showing restore command]**  
*Description: File changes being discarded*

### Scenario 2: Undo Last Commit (Not Pushed)

**Keep the changes, undo the commit:**
```bash
git reset --soft HEAD~1
```

**Discard the changes entirely:**
```bash
git reset --hard HEAD~1
```

**Undo multiple commits:**
```bash
git reset --soft HEAD~3  # Undo last 3 commits
```

**[IMAGE: Reset options diagram]**  
*Description: Soft, mixed, and hard reset effects*

### Scenario 3: Undo Pushed Commit (Safely)

Sara accidentally pushed broken code to main:

```bash
# Create a reverse commit (safe for public branches)
git revert HEAD

# This creates a new commit that undoes the changes
git push origin main
```

**Result:**
```
* d4e5f6g Revert "Add broken feature"
* c3d4e5f Add broken feature  ← This is undone
* b2c3d4e Previous commit
```

**[IMAGE: Revert commit in history]**  
*Description: New commit undoing previous commit*

### Scenario 4: Edit Last Commit Message

```bash
# Change the last commit message
git commit --amend -m "Corrected message"

# Add forgotten files to last commit
git add forgotten_file.py
git commit --amend --no-edit
```

**⚠️ Warning:** Only amend if you haven't pushed!

### Scenario 5: Emergency - Find Lost Commits

```bash
# View all actions (including deleted commits)
git reflog

# Output:
# a1b2c3d HEAD@{0}: reset: moving to HEAD~1
# b2c3d4e HEAD@{1}: commit: Important feature
# c3d4e5f HEAD@{2}: commit: Another feature

# Recover the lost commit
git checkout b2c3d4e
git checkout -b recovered-feature
```

**[IMAGE: reflog output showing commit history]**  
*Description: Complete history including resets and rebases*

---

## 👥 Part 11: Code Review Best Practices

### What Makes a Good Code Review?

**For Reviewers:**
- ✅ Be constructive and respectful
- ✅ Explain *why* changes are needed
- ✅ Ask questions to understand intent
- ✅ Praise good code
- ✅ Look for bugs, style issues, and architecture

**For Authors:**
- ✅ Keep PRs small and focused
- ✅ Write descriptive PR descriptions
- ✅ Respond to all comments
- ✅ Don't take criticism personally
- ✅ Update PR based on feedback

**[IMAGE: Good vs bad code review comments comparison]**  
*Description: Helpful vs unhelpful review comments*

### PR Review Checklist

Create `.github/PULL_REQUEST_TEMPLATE.md`:

```markdown
## Description
Brief description of changes

## Type of Change
- [ ] Bug fix
- [ ] New feature
- [ ] Breaking change
- [ ] Documentation update

## Testing
- [ ] Unit tests pass
- [ ] Manual testing completed
- [ ] Edge cases considered

## Checklist
- [ ] Code follows project style guide
- [ ] Comments added for complex logic
- [ ] Documentation updated
- [ ] No console.log or debug code
- [ ] All variables have meaningful names

## Screenshots (if applicable)
Add screenshots here

## Related Issues
Closes #123
```

**[IMAGE: PR template in action on GitHub]**  
*Description: PR with all sections filled*

### Sara's Code Review Experience

Hassan reviews Sara's authentication PR:

**Good comment:**
```
I like the session management approach! One suggestion: 
consider using bcrypt instead of SHA-256 for password 
hashing. Bcrypt is specifically designed for passwords 
and includes automatic salting. 

Example: https://pypi.org/project/bcrypt/
```

✅ Constructive, explains why, provides resources

**Bad comment:**
```
This is wrong. Use bcrypt.
```

❌ Not helpful, no explanation

**[IMAGE: GitHub PR conversation showing constructive feedback]**  
*Description: Comments with suggestions and discussions*

---

## 🎯 Hands-On Exercise: Git Flow Simulation

### Team Project: Build a Blog Application

**Teams of 3-4 people**

#### Roles:
- **Product Owner**: Manages releases
- **Developer 1**: User authentication
- **Developer 2**: Blog post CRUD
- **Developer 3**: Comment system

#### Setup:

```bash
# Product Owner creates repository and main/develop branches
git init blog-application
cd blog-application

# Create initial commit
echo "# Blog Application" > README.md
git add README.md
git commit -m "Initial commit"

# Create develop branch
git branch develop
git push -u origin main
git push -u origin develop
```

#### Tasks:

**1. Feature Development (30 minutes)**
- Each developer creates feature branch from `develop`
- Implements their feature
- Commits with good messages
- Pushes to GitHub

**2. Code Review (15 minutes)**
- Create Pull Requests to `develop`
- Review each other's code
- Address feedback

**3. Release Preparation (15 minutes)**
- Product Owner creates `release/v1.0.0` from `develop`
- Update version numbers
- Update CHANGELOG
- Merge to `main` with tag

**4. Hotfix Simulation (10 minutes)**
- Introduce a "bug" in main
- Create hotfix branch
- Fix and merge to both main and develop

**[IMAGE: Exercise workflow diagram]**  
*Description: Complete Git Flow cycle for exercise*

---

## 🔧 Part 12: Troubleshooting Advanced Issues

### Issue 1: Merge Conflict in Binary Files

**Problem:** Can't merge image/PDF files

**Solution:**
```bash
# Choose your version
git checkout --ours file.png

# Or choose their version
git checkout --theirs file.png

git add file.png
git commit
```

### Issue 2: Accidentally Committed to Wrong Branch

**Solution:**
```bash
# Move commit to correct branch
git branch correct-branch      # Create branch with commit
git reset --hard HEAD~1        # Remove from current branch
git checkout correct-branch    # Switch to correct branch
```

### Issue 3: Need to Merge Specific Files Only

**Solution:**
```bash
# Checkout specific files from another branch
git checkout feature-branch -- path/to/file.py
git commit -m "Cherry-pick specific file from feature"
```

### Issue 4: Branches Diverged Too Much

**Solution:**
```bash
# Rebase interactively to clean up
git rebase -i main

# Or create a fresh branch with desired changes
git checkout -b clean-feature
git cherry-pick commit1 commit2 commit3
```

**[IMAGE: Troubleshooting flowchart]**  
*Description: Common issues and their solutions*

---

## 📊 Part 13: Visualizing Your Repository

### Using Git Log Effectively

**Beautiful graph view:**
```bash
git log --oneline --graph --all --decorate
```

**[IMAGE: Terminal showing colorful git log graph]**  
*Description: ASCII art representation of branches*

**See who changed what:**
```bash
git log --author="Sara" --since="2 weeks ago"
```

**Find when a bug was introduced:**
```bash
git bisect start
git bisect bad                 # Current version has bug
git bisect good v1.0.0         # v1.0.0 was good
# Git checks out middle commit, test it
git bisect good  # or bad
# Repeat until bug commit is found
```

### GitKraken / Git Graph Extensions

**For visual learners, use GUI tools:**
- GitKraken
- SourceTree
- VS Code Git Graph extension

**[IMAGE: GitKraken interface showing branch visualization]**  
*Description: Modern GUI showing repository history*

---

## 📚 Session Summary

### What You Mastered

✅ **Branch Management**: Create, switch, merge, delete  
✅ **Merge Strategies**: Merge commit, fast-forward, squash  
✅ **Rebasing**: Interactive rebase, rebasing onto main  
✅ **Git Flow**: Enterprise workflow with multiple branch types  
✅ **GitHub Flow**: Simplified continuous deployment workflow  
✅ **Advanced Operations**: Stash, cherry-pick, reflog  
✅ **Tags & Releases**: Semantic versioning, creating releases  
✅ **Undoing Mistakes**: Revert, reset, restore  
✅ **Code Review**: Best practices for reviews  

### Essential Commands

```bash
# Branch operations
git branch feature-name           # Create branch
git checkout -b feature-name      # Create and switch
git switch feature-name           # Switch branch (modern)
git branch -d feature-name        # Delete branch
git push origin feature-name      # Push branch

# Merging
git merge feature-name            # Merge branch
git merge --squash feature-name   # Squash merge
git rebase main                   # Rebase onto main
git rebase -i HEAD~3              # Interactive rebase

# Stashing
git stash                         # Save work temporarily
git stash pop                     # Restore and remove stash
git stash list                    # View all stashes

# Tags
git tag v1.0.0                    # Create tag
git tag -a v1.0.0 -m "Release"    # Annotated tag
git push origin v1.0.0            # Push tag

# Undoing
git restore file.py               # Discard unstaged changes
git reset --soft HEAD~1           # Undo commit, keep changes
git revert HEAD                   # Create reverse commit

# Advanced
git cherry-pick commit-hash       # Copy specific commit
git reflog                        # View all actions
```

---

## 🎉 Sara's Internship Success

Sara successfully implemented the authentication feature using professional Git workflows. Her achievements:

- ✅ Used feature branches for clean development
- ✅ Rebased to maintain linear history
- ✅ Created comprehensive tests
- ✅ Submitted well-documented PR
- ✅ Addressed code review feedback promptly
- ✅ Tagged and released v1.0.0

Karim was impressed: "You've mastered what takes some developers years to learn. You're thinking like a senior engineer already!"

At the end of summer, Sara received a job offer from TechWave Solutions! 🎊

---

## 📖 Additional Resources

### Documentation
- Git Branching Guide: https://git-scm.com/book/en/v2/Git-Branching-Basic-Branching-and-Merging
- Git Flow Guide: https://nvie.com/posts/a-successful-git-branching-model/
- GitHub Flow: https://guides.github.com/introduction/flow/
- Interactive Rebase: https://git-scm.com/book/en/v2/Git-Tools-Rewriting-History

### Tools
- GitKraken: https://www.gitkraken.com
- SourceTree: https://www.sourcetreeapp.com
- Git Graph (VS Code): Search in extensions marketplace

### Practice
- Learn Git Branching: https://learngitbranching.js.org
- Visualizing Git: http://git-school.github.io/visualizing-git/

---

## 🔮 Next Session Preview

**Session 4 — The Job: Open Source, Forking & Advanced GitHub**

Sara's friend **Ahmed** discovers open source! He finds a bug in a popular Python library and wants to contribute. He also wants to create a portfolio website to showcase his projects.

You'll learn:
- Contributing to open source projects
- Forking workflow and upstream remotes
- GitHub Actions for CI/CD
- GitHub Pages for hosting websites
- Advanced GitHub features
- Security best practices

Get ready to become part of the global developer community! 🌍

---

**End of Session 3**
