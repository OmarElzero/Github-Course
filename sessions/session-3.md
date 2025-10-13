# Session 3 — GitHub Collaboration & Professional Workflows

**Duration:** 3 hours  
**Venue:** Creativa Lab C  
**Instructors:** Omar Betawy, Amr Khaled

---

## 📖 Story Introduction

**Sara's** skills with Git branching are impressive. Professor Hassan notices and assigns her team a major project: build a **Student Management System** with **four developers** working simultaneously.

The team is excited but quickly faces chaos:
- **Ahmed** pushes code that breaks everyone's work
- **Mona** can't push because she doesn't have permissions
- **Youssef** overwrites Sara's changes by accident
- Code reviews? What are those?

Their mentor explains: "You know Git, but now you need to learn **GitHub workflows**. This is how professional teams collaborate without destroying each other's work!"

Today, you'll master:
- GitHub collaboration models and permissions
- Professional Pull Request workflows
- Authentication (SSH vs Personal Access Tokens)
- Code review best practices
- Git Flow and GitHub Flow
- Tags and releases
- Cherry-picking commits

By the end, you'll collaborate like senior developers at tech companies!

---

## 🎯 Session Objectives

By the end of this session, you will be able to:
- ✅ Set up team repositories with proper permissions
- ✅ Use SSH keys for secure authentication
- ✅ Create and review Pull Requests professionally
- ✅ Conduct effective code reviews
- ✅ Implement Git Flow and GitHub Flow
- ✅ Create tags and releases
- ✅ Cherry-pick commits between branches
- ✅ Handle team collaboration scenarios
- ✅ Manage repository settings and protection rules

---

## Part 1: GitHub Collaboration Models

### Model 1: Shared Repository (Centralized)

**Best for:** Small teams, trusted members, private projects

**Structure:**
```
Organization/Company Repository
    ↓
Team Members (Direct Push Access)
    ↓
Everyone pushes to same repo
```

**Pros:**
- ✅ Simple setup
- ✅ Single source of truth
- ✅ Easy to manage

**Cons:**
- ❌ Everyone can push to main
- ❌ Less control over quality
- ❌ Requires trust

**[IMAGE: Centralized model diagram]**  
*Description: One repository with multiple contributors having direct access*

### Model 2: Fork & Pull Request (Distributed)

**Best for:** Open source, large teams, external contributors

**Structure:**
```
Original Repository (Upstream)
    ↓
Fork → Contributor's Copy
    ↓
Pull Request → Original
```

**Pros:**
- ✅ Code review required
- ✅ Safe from bad changes
- ✅ Anyone can contribute

**Cons:**
- ❌ More complex workflow
- ❌ Need to sync forks
- ❌ Extra steps

**[IMAGE: Fork & PR model diagram]**  
*Description: Original repo with multiple forks and PR arrows back*

---

## Part 2: Repository Permissions

### Permission Levels

| Level | Read | Clone | Create Issues | Push | Delete | Settings |
|-------|------|-------|---------------|------|--------|----------|
| **Read** | ✅ | ✅ | ✅ | ❌ | ❌ | ❌ |
| **Triage** | ✅ | ✅ | ✅ + Manage | ❌ | ❌ | ❌ |
| **Write** | ✅ | ✅ | ✅ + Manage | ✅ | ❌ | ❌ |
| **Maintain** | ✅ | ✅ | ✅ + Manage | ✅ | ❌ | Some |
| **Admin** | ✅ | ✅ | ✅ + Manage | ✅ | ✅ | ✅ |

**[IMAGE: Permission pyramid]**  
*Description: Hierarchy from Read at bottom to Admin at top*

### Real-World Usage

**For Sara's team project:**
- **Sara**: Admin (project lead)
- **Ahmed, Mona, Youssef**: Write (team members)
- **Professor Hassan**: Read (observer)
- **External testers**: Read (view only)

---

## PRACTICE 1: Set Up Team Repository

**Task:** Create a team project with proper permissions.

```bash
# On GitHub:
# 1. Create repository "team-project"
# 2. Make it private (for team work)
# 3. Add README.md and .gitignore (Python)

# 4. Add collaborators:
#    Settings → Collaborators → Add people
#    - Add 2-3 teammates with "Write" permission
#    - Add 1 person with "Read" permission (observer)

# 5. Each team member:
#    - Check email for invitation
#    - Accept invitation
#    - Clone repository
git clone https://github.com/TEAM_LEAD/team-project.git

# 6. Verify access:
#    Each member makes a branch and pushes
git switch -c test/YOUR_NAME
echo "Test by YOUR_NAME" > test.txt
git add test.txt
git commit -m "Test commit by YOUR_NAME"
git push origin test/YOUR_NAME
```

**Success:** Everyone can push their test branches! ✅

---

## 🔑 Part 3: Authentication — SSH vs Personal Access Tokens

### The Problem with Passwords

GitHub **disabled password authentication** in 2021. You must use:
1. **Personal Access Tokens (HTTPS)**
2. **SSH Keys (SSH protocol)**

**[IMAGE: Comparison of HTTPS vs SSH authentication]**  
*Description: Two authentication methods side by side*

---

### Method 1: Personal Access Token (PAT)

**When to use:**
- ✅ Quick setup
- ✅ Works everywhere
- ✅ Can be revoked easily

**Setup:**

1. **Generate token on GitHub:**
   - Settings → Developer settings → Personal access tokens → Tokens (classic)
   - Click "Generate new token (classic)"
   - Name: "Git Access"
   - Expiration: 90 days (your choice)
   - Scopes: Check **repo** (full control)
   - Generate token
   - **COPY IT IMMEDIATELY** (can't see again!)

**[IMAGE: PAT generation page with scopes]**  
*Description: Token creation form with repo checkbox selected*

2. **Use token when pushing:**
```bash
git push origin main
Username: your-username
Password: ghp_xxxxxxxxxxxxxxxxxxxx  # Paste token here
```

3. **Cache credentials (optional):**
```bash
# Cache for 1 hour
git config --global credential.helper 'cache --timeout=3600'

# OR store permanently (less secure)
git config --global credential.helper store
```

**[IMAGE: Terminal showing credential prompt]**  
*Description: Username and password prompt with token*

---

### Method 2: SSH Keys (Recommended!)

**When to use:**
- ✅ More secure
- ✅ No password prompts
- ✅ Professional standard

**Setup:**

#### Step 1: Check for Existing Keys

```bash
ls -la ~/.ssh
```

Look for:
- `id_rsa` and `id_rsa.pub` (RSA)
- `id_ed25519` and `id_ed25519.pub` (Ed25519 - recommended)

**[IMAGE: SSH directory listing]**  
*Description: Terminal showing .ssh directory contents*

#### Step 2: Generate New SSH Key

```bash
# Generate Ed25519 key (modern, secure)
ssh-keygen -t ed25519 -C "your-email@example.com"

# OR RSA if Ed25519 not supported
ssh-keygen -t rsa -b 4096 -C "your-email@example.com"
```

**Prompts:**
```
Enter file in which to save the key: (Press Enter for default)
Enter passphrase: (Optional but recommended)
Enter same passphrase again:
```

**Output:**
```
Your identification has been saved in /home/user/.ssh/id_ed25519
Your public key has been saved in /home/user/.ssh/id_ed25519.pub
```

**[IMAGE: SSH key generation output]**  
*Description: Terminal showing key generation success*

#### Step 3: Add Key to SSH Agent

```bash
# Start ssh-agent
eval "$(ssh-agent -s)"

# Add private key
ssh-add ~/.ssh/id_ed25519
```

**[IMAGE: SSH agent started]**  
*Description: Agent pid shown in terminal*

#### Step 4: Copy Public Key

```bash
# Linux/Mac
cat ~/.ssh/id_ed25519.pub

# Windows (Git Bash)
clip < ~/.ssh/id_ed25519.pub

# Manually copy the output (starts with ssh-ed25519)
```

**Output looks like:**
```
ssh-ed25519 AAAAC3NzaC1lZDI1NTE5AAAAIJl3dIeudNqd0DPMRD6OIh65tjkxFNOtwGcWB2gCgPhk your-email@example.com
```

**[IMAGE: Public key displayed]**  
*Description: Terminal showing public key (partially blurred)*

#### Step 5: Add to GitHub

1. Go to GitHub: Settings → SSH and GPG keys
2. Click **New SSH key**
3. Title: "My Laptop" (descriptive name)
4. Key type: Authentication Key
5. Paste the public key
6. Click **Add SSH key**

**[IMAGE: GitHub SSH key addition page]**  
*Description: Form with title and key fields*

#### Step 6: Test Connection

```bash
ssh -T git@github.com
```

**First time:**
```
The authenticity of host 'github.com' can't be established.
ED25519 key fingerprint is SHA256:+DiY3wvvV6TuJJhbpZisF/zLDA0zPMSvHdkr4UvCOqU.
Are you sure you want to continue connecting (yes/no)? yes
```

**Success:**
```
Hi your-username! You've successfully authenticated, but GitHub does not provide shell access.
```

✅ **SSH is working!**

**[IMAGE: Successful SSH test]**  
*Description: Terminal showing authentication success*

#### Step 7: Use SSH URLs

**Change remote to SSH:**
```bash
# Check current remote
git remote -v

# Change to SSH
git remote set-url origin git@github.com:username/repo.git

# Verify
git remote -v
```

**Now pushing doesn't ask for password!**

**[IMAGE: HTTPS vs SSH URL comparison]**  
*Description: Two URLs side by side showing difference*

---

## PRACTICE 2: Set Up SSH Authentication

**Full workflow:**

```bash
# 1. Generate SSH key
ssh-keygen -t ed25519 -C "student@fcai.edu"
# Press Enter for default location
# Enter passphrase (recommended)

# 2. Start agent and add key
eval "$(ssh-agent -s)"
ssh-add ~/.ssh/id_ed25519

# 3. Copy public key
cat ~/.ssh/id_ed25519.pub
# Copy the output

# 4. Add to GitHub
# Go to: github.com → Settings → SSH keys
# Add the key

# 5. Test
ssh -T git@github.com

# 6. Update your repository
cd ~/path/to/your/repo
git remote set-url origin git@github.com:USERNAME/REPO.git

# 7. Test push
echo "SSH test" >> README.md
git add README.md
git commit -m "Test SSH push"
git push origin main
# No password prompt!
```

---

## Part 4: Pull Request Workflow

### What is a Pull Request?

A **Pull Request (PR)** is:
- A request to merge your branch into another branch
- A place for code review and discussion
- GitHub's killer feature for collaboration

**[IMAGE: Pull Request concept diagram]**  
*Description: Feature branch requesting to merge into main*

### Professional PR Workflow

```
1. Create feature branch
   ↓
2. Make changes and commit
   ↓
3. Push branch to GitHub
   ↓
4. Create Pull Request
   ↓
5. Code Review (discussion, changes)
   ↓
6. Tests pass (CI/CD)
   ↓
7. Approval from reviewers
   ↓
8. Merge to main
   ↓
9. Delete feature branch
```

**[IMAGE: Complete PR workflow diagram]**  
*Description: Nine steps visualized with arrows*

---

## Part 5: Creating a Pull Request

### Step-by-Step: Sara's First PR

#### Step 1: Create Feature Branch

```bash
git switch main
git pull origin main
git switch -c feature/add-student-class
```

#### Step 2: Develop Feature

Create `student.py`:

```python
# student.py
class Student:
    """Represents a student in the system"""
    
    def __init__(self, student_id, name, email, major):
        self.student_id = student_id
        self.name = name
        self.email = email
        self.major = major
        self.enrolled_courses = []
    
    def enroll(self, course):
        """Enroll in a course"""
        if course not in self.enrolled_courses:
            self.enrolled_courses.append(course)
            return True
        return False
    
    def drop(self, course):
        """Drop a course"""
        if course in self.enrolled_courses:
            self.enrolled_courses.remove(course)
            return True
        return False
    
    def get_info(self):
        """Get student information"""
        return {
            'id': self.student_id,
            'name': self.name,
            'email': self.email,
            'major': self.major,
            'courses': self.enrolled_courses
        }
```

**[IMAGE: Student class code in editor]**  
*Description: Complete class implementation*

#### Step 3: Write Tests

Create `test_student.py`:

```python
# test_student.py
import unittest
from student import Student

class TestStudent(unittest.TestCase):
    
    def setUp(self):
        self.student = Student(
            student_id="12345",
            name="Sara Ahmed",
            email="sara@fcai.edu",
            major="Computer Science"
        )
    
    def test_enroll(self):
        result = self.student.enroll("Data Structures")
        self.assertTrue(result)
        self.assertIn("Data Structures", self.student.enrolled_courses)
    
    def test_enroll_duplicate(self):
        self.student.enroll("Algorithms")
        result = self.student.enroll("Algorithms")
        self.assertFalse(result)
    
    def test_drop(self):
        self.student.enroll("Databases")
        result = self.student.drop("Databases")
        self.assertTrue(result)
        self.assertNotIn("Databases", self.student.enrolled_courses)
    
    def test_get_info(self):
        info = self.student.get_info()
        self.assertEqual(info['name'], "Sara Ahmed")
        self.assertEqual(info['major'], "Computer Science")

if __name__ == '__main__':
    unittest.main()
```

#### Step 4: Commit Changes

```bash
git add student.py test_student.py
git commit -m "Add Student class with enrollment features

- Implement Student class with basic attributes
- Add enroll() and drop() methods
- Include comprehensive unit tests
- All tests passing"
```

**[IMAGE: Terminal showing commit]**  
*Description: Multi-line commit message*

#### Step 5: Push Branch

```bash
git push origin feature/add-student-class
```

**Output:**
```
remote: Create a pull request for 'feature/add-student-class' on GitHub by visiting:
remote:      https://github.com/username/repo/pull/new/feature/add-student-class
```

**[IMAGE: Terminal with PR creation link]**  
*Description: Helpful link to create PR*

#### Step 6: Create PR on GitHub

1. Go to repository on GitHub
2. See banner: "feature/add-student-class had recent pushes"
3. Click **"Compare & pull request"**

**[IMAGE: GitHub PR creation banner]**  
*Description: Yellow banner with button*

4. Fill PR form:

**Title:**
```
Add Student class with enrollment features
```

**Description:**
```markdown
## Description
Implements the Student class for the student management system.

## Changes
- ✅ Student class with basic attributes (id, name, email, major)
- ✅ Enroll/drop course functionality
- ✅ Get info method for student data
- ✅ Comprehensive unit tests

## Testing
- All unit tests pass
- Tested with Python 3.9
```

Run:
```bash
python -m pytest test_student.py -v
```

## Type of Change
- [x] New feature
- [ ] Bug fix
- [ ] Documentation update

## Checklist
- [x] Code follows project style
- [x] Tests added and passing
- [x] Documentation updated
- [x] No breaking changes

## Related Issues
Implements #12
```

5. Select reviewers (Ahmed, Mona)
6. Add labels: `enhancement`, `priority: high`
7. Link to project board
8. Click **"Create pull request"**

**[IMAGE: Completed PR form]**  
*Description: All fields filled professionally*

**[IMAGE: Created PR page]**  
*Description: PR showing all details, reviewers, labels*

---

## PRACTICE 3: Create Your First Pull Request

**Complete workflow:**

```bash
# 1. Create feature
git switch -c feature/add-calculator-power

# 2. Implement feature
echo "def power(a, b): return a ** b" >> calculator.py
git add calculator.py
git commit -m "Add power function to calculator"

# 3. Push
git push origin feature/add-calculator-power

# 4. Go to GitHub and create PR with:
#    - Clear title
#    - Detailed description
#    - Select 1-2 reviewers
#    - Add label "enhancement"

# 5. Wait for review...
```

---

## Part 6: Code Review Best Practices

### For Reviewers

**What to look for:**

1. **Functionality**: Does it work correctly?
2. **Tests**: Are there tests? Do they pass?
3. **Code Quality**: Is it readable and maintainable?
4. **Performance**: Any obvious inefficiencies?
5. **Security**: Any vulnerabilities?
6. **Documentation**: Is it documented?

**[IMAGE: Code review checklist]**  
*Description: Checklist with all points*

### Reviewing Sara's PR

Ahmed reviews Sara's Student class:

**Good comment:**
```
Great work on the Student class! I have a few suggestions:

1. Line 15: Consider adding email validation. We should check 
   if it's a valid email format before accepting.
   
   Suggestion:
   ```python
   import re
   EMAIL_REGEX = r'^[a-zA-Z0-9._%+-]+@[a-zA-Z0-9.-]+\.[a-zA-Z]{2,}$'
   
   if not re.match(EMAIL_REGEX, email):
       raise ValueError("Invalid email format")
   ```

2. Line 25: The `enroll` method doesn't check if the course 
   exists in the system. Should we validate against a course list?

3. Great job on the tests! Could you add a test for invalid email?

Overall: ✅ Approve with minor suggestions
```

✅ Constructive, specific, helpful

**[IMAGE: Good code review comment]**  
*Description: Inline comment on specific lines*

**Bad comment:**
```
This is wrong. Fix it.
```

❌ Not helpful, no explanation, rude

### Responding to Reviews

Sara responds professionally:

```
Thanks for the thorough review, @ahmed! Great catches!

1. ✅ Email validation: Added in commit abc123
2. ✅ Course validation: Added `validate_course()` method
3. ✅ Invalid email test: Added to test suite

All tests still passing. Ready for another review!
```

**[IMAGE: PR conversation thread]**  
*Description: Review comments and author responses*

### Requesting Changes

Ahmed clicks **"Request changes"** with:

```
Please address the email validation before merging.
Also add documentation for the new validate_course method.
```

PR status: **🔴 Changes requested**

**[IMAGE: PR with changes requested status]**  
*Description: Red status badge on PR*

### Approving PR

After Sara makes changes:

Ahmed clicks **"Approve"** with:

```
Perfect! All concerns addressed. LGTM!
```

PR status: **✅ Approved**

Mona also approves: **✅ 2/2 approvals**

**[IMAGE: PR with approvals]**  
*Description: Green checkmarks from reviewers*

---

## PRACTICE 4: Code Review Exercise

**Pair up with a partner:**

**Person A:**
```bash
# 1. Create feature with intentional issues
git switch -c feature/with-issues

# 2. Add code with problems:
#    - No error handling
#    - No documentation
#    - Magic numbers
#    - No tests

def calculate(x, y):
    return x * 2.5 + y / 3.7

git add .
git commit -m "Add calculation"
git push origin feature/with-issues

# 3. Create PR
```

**Person B:**
```bash
# 1. Review the PR on GitHub
# 2. Leave at least 3 constructive comments
# 3. Request changes
```

**Person A:**
```bash
# 1. Address all comments
# 2. Push fixes
# 3. Respond to reviewer
```

**Person B:**
```bash
# 1. Review fixes
# 2. Approve PR
```

---

## Part 7: Merging Pull Requests

### Merge Strategies on GitHub

#### 1. Create a Merge Commit (Default)

```
Before:
       ┌─ C ─ D (feature)
A ─ B ─┤
       └─ E (main)

After:
A ─ B ─ E ─ F (merge commit)
     └─ C ─ D ─┘
```

**When to use:**
- ✅ Want to preserve feature branch history
- ✅ Want to see when features were integrated

**[IMAGE: Merge commit diagram]**  
*Description: Branch merging with merge commit*

#### 2. Squash and Merge

```
Before:
Feature branch: C ─ D ─ E ─ F (4 commits)

After:
Main: A ─ B ─ G (1 combined commit)
```

**When to use:**
- ✅ Want clean, linear history
- ✅ Feature has messy commits
- ✅ Want one commit per feature

**[IMAGE: Squash merge diagram]**  
*Description: Multiple commits compressed to one*

#### 3. Rebase and Merge

```
Before:
       ┌─ C ─ D (feature)
A ─ B ─┤
       └─ E (main)

After:
A ─ B ─ E ─ C' ─ D' (linear)
```

**When to use:**
- ✅ Want perfectly linear history
- ✅ Feature branch is clean
- ✅ No merge commits wanted

**[IMAGE: Rebase merge diagram]**  
*Description: Linear history without merge commit*

### Merging Sara's PR

1. All checks pass ✅
2. 2 approvals ✅
3. No conflicts ✅
4. Branch is up to date ✅

Sara clicks **"Squash and merge"**:

**Commit message:**
```
Add Student class with enrollment features (#15)

* Add Student class implementation
* Add enroll/drop methods
* Add email validation
* Add comprehensive tests
* Address code review feedback
```

5. Click **"Confirm squash and merge"**
6. Click **"Delete branch"** (cleanup)

**[IMAGE: Merge button options]**  
*Description: Dropdown with three merge strategies*

**[IMAGE: Delete branch option]**  
*Description: Delete branch button after merge*

---

## PRACTICE 5: Complete PR Workflow

**Full cycle:**

```bash
# 1. Feature development
git switch -c feature/add-course-class
# Write Course class
# Write tests
git add .
git commit -m "Add Course class"
git push origin feature/add-course-class

# 2. Create PR
# Do this on GitHub

# 3. Get review
# Wait for partner's review

# 4. Address feedback
# Make requested changes
git add .
git commit -m "Address review feedback"
git push origin feature/add-course-class

# 5. Get approval and merge
# Do this on GitHub

# 6. Update local main
git switch main
git pull origin main
git branch -d feature/add-course-class

# 7. Verify
git log --oneline
# Your feature should be in main!
```

---

## 🏢 Part 8: Git Flow — Enterprise Workflow

### What is Git Flow?

A branching model for projects with **scheduled releases**.

**Branch Types:**

| Branch | Purpose | Base | Merge Into | Lifetime |
|--------|---------|------|------------|----------|
| `main` | Production code | - | - | Permanent |
| `develop` | Integration | main | main | Permanent |
| `feature/*` | New features | develop | develop | Temporary |
| `release/*` | Release prep | develop | main & develop | Temporary |
| `hotfix/*` | Urgent fixes | main | main & develop | Temporary |

**[IMAGE: Complete Git Flow diagram]**  
*Description: All branch types and their relationships*

### Git Flow in Action

#### Feature Development

```bash
# Start feature
git checkout develop
git checkout -b feature/user-profile

# Work on feature
git add .
git commit -m "Add user profile page"

# Finish feature
git checkout develop
git merge feature/user-profile
git branch -d feature/user-profile
git push origin develop
```

**[IMAGE: Feature branch lifecycle]**  
*Description: Feature branching from develop and merging back*

#### Release Preparation

```bash
# Create release branch
git checkout develop
git checkout -b release/v1.5.0

# Prepare release (version bumps, changelog, bug fixes)
echo "1.5.0" > VERSION
git add VERSION
git commit -m "Bump version to 1.5.0"

# Update changelog
git add CHANGELOG.md
git commit -m "Update changelog for v1.5.0"

# Merge to main (production)
git checkout main
git merge release/v1.5.0
git tag -a v1.5.0 -m "Release version 1.5.0"

# Also merge back to develop
git checkout develop
git merge release/v1.5.0

# Cleanup
git branch -d release/v1.5.0
git push origin main develop --tags
```

**[IMAGE: Release branch workflow]**  
*Description: Release merging to both main and develop*

#### Hotfix (Emergency)

```bash
# Critical bug in production!
git checkout main
git checkout -b hotfix/security-fix

# Fix the bug
git add .
git commit -m "Fix critical security vulnerability"

# Merge to main
git checkout main
git merge hotfix/security-fix
git tag -a v1.5.1 -m "Hotfix: Security patch"

# Also merge to develop
git checkout develop
git merge hotfix/security-fix

# Cleanup
git branch -d hotfix/security-fix
git push origin main develop --tags
```

**[IMAGE: Hotfix workflow]**  
*Description: Hotfix from main to main and develop*

---

## Part 9: GitHub Flow — Simplified

### What is GitHub Flow?

A simpler workflow for **continuous deployment**.

**Rules:**
1. `main` branch is **always deployable**
2. Create **descriptive branches** from main
3. **Commit often** to your branch
4. **Open PR early** for feedback
5. **Deploy from branch** for testing
6. **Merge after approval** and tests pass

**[IMAGE: GitHub Flow diagram]**  
*Description: Simple flow with main and feature branches*

### Comparison

| Aspect | Git Flow | GitHub Flow |
|--------|----------|-------------|
| Complexity | High | Low |
| Branches | 5 types | 2 types |
| Best For | Scheduled releases | Continuous deployment |
| Release Cycle | Weeks/months | Daily/hourly |
| Learning Curve | Steep | Gentle |
| Team Size | Large | Any size |

**[IMAGE: Side-by-side comparison]**  
*Description: Visual comparison of both workflows*

---

## PRACTICE 6: Git Flow Simulation

**Simulate a complete release cycle:**

```bash
# Setup
git init git-flow-demo
cd git-flow-demo

# Create develop branch
git checkout -b develop
echo "# App" > README.md
git add README.md
git commit -m "Initial commit"

# Feature 1
git checkout -b feature/login develop
echo "login" > login.py
git add login.py
git commit -m "Add login"
git checkout develop
git merge feature/login
git branch -d feature/login

# Feature 2
git checkout -b feature/dashboard develop
echo "dashboard" > dashboard.py
git add dashboard.py
git commit -m "Add dashboard"
git checkout develop
git merge feature/dashboard
git branch -d feature/dashboard

# Release
git checkout -b release/v1.0.0 develop
echo "1.0.0" > VERSION
git add VERSION
git commit -m "Version 1.0.0"

# Merge to main
git checkout -b main develop
git merge release/v1.0.0
git tag -a v1.0.0 -m "Release 1.0.0"

# Merge back to develop
git checkout develop
git merge release/v1.0.0
git branch -d release/v1.0.0

# Hotfix
git checkout -b hotfix/critical main
echo "fix" > fix.txt
git add fix.txt
git commit -m "Critical fix"
git checkout main
git merge hotfix/critical
git tag -a v1.0.1 -m "Hotfix 1.0.1"
git checkout develop
git merge hotfix/critical
git branch -d hotfix/critical

# Visualize
git log --oneline --graph --all
```

---

## Part 10: Tags and Releases

### What are Tags?

**Tags** mark specific points in history, typically releases.

**Types:**

**Lightweight tag** (simple pointer):
```bash
git tag v1.0.0
```

**Annotated tag** (recommended - includes metadata):
```bash
git tag -a v1.0.0 -m "Release version 1.0.0"
```

**[IMAGE: Tag on commit timeline]**  
*Description: Commit history with tags marking releases*

### Semantic Versioning

Format: `MAJOR.MINOR.PATCH`

```
v2.3.1
│ │ └─ PATCH: Bug fixes (backward compatible)
│ └─── MINOR: New features (backward compatible)
└───── MAJOR: Breaking changes
```

**Examples:**
- `v1.0.0` → `v1.0.1`: Bug fix
- `v1.0.1` → `v1.1.0`: New feature
- `v1.1.0` → `v2.0.0`: Breaking change

**[IMAGE: SemVer breakdown visual]**  
*Description: Version number explained with examples*

### Creating Tags

```bash
# Create annotated tag
git tag -a v1.2.0 -m "Release 1.2.0: Add user authentication"

# List tags
git tag

# Show tag details
git show v1.2.0

# Push single tag
git push origin v1.2.0

# Push all tags
git push origin --tags
```

**[IMAGE: Tag creation and push]**  
*Description: Terminal showing tag commands*

### Creating GitHub Release

1. Go to repository → **Releases**
2. Click **"Create a new release"**
3. Choose tag: `v1.2.0` (or create new)
4. Release title: "Version 1.2.0 - Authentication Update"
5. Description:

```markdown
## What's New

### Features
- User registration and login
- Password hashing with bcrypt
- 🎫 Session management
- Email validation

### Improvements
- Faster database queries
- Better error messages
- Updated UI design

### Bug Fixes
- Fix login redirect issue (#42)
- Fix session timeout (#38)

### Documentation
- API documentation updated
- Installation guide improved

## 📦 Installation

```bash
pip install -r requirements.txt
python setup.py install
```

## 🔄 Upgrade

```bash
git pull origin main
pip install -r requirements.txt --upgrade
```

## Breaking Changes

None! This is a backward-compatible release.

## 👥 Contributors

Thanks to @ahmed, @mona, @youssef for their contributions!

## Stats

- 45 commits
- 12 files changed
- 850 additions, 320 deletions
```

6. Attach files (optional): Binary downloads, documentation
7. **Prerelease** checkbox if beta/RC
8. Click **"Publish release"**

**[IMAGE: GitHub release creation form]**  
*Description: Complete form with all sections filled*

**[IMAGE: Published release page]**  
*Description: Release showing download counts and assets*

---

## PRACTICE 7: Create a Release

```bash
# 1. Prepare for release
git checkout main
git pull origin main

# 2. Update version
echo "2.0.0" > VERSION
git add VERSION
git commit -m "Bump version to 2.0.0"

# 3. Update changelog
cat >> CHANGELOG.md << EOF
## [2.0.0] - 2025-02-20

### Added
- New feature X
- New feature Y

### Changed
- Improved performance

### Fixed
- Bug #123
EOF

git add CHANGELOG.md
git commit -m "Update changelog for v2.0.0"

# 4. Create tag
git tag -a v2.0.0 -m "Release 2.0.0: Major update"

# 5. Push
git push origin main --tags

# 6. Create release on GitHub
# Follow the steps above
```

---

## Part 11: Cherry-Picking

### What is Cherry-Picking?

**Cherry-pick** copies a specific commit from one branch to another.

```
Branch A: A ─ B ─ C ─ D
                  ↓ (cherry-pick)
Branch B: E ─ F ─ C' (copy of C)
```

**[IMAGE: Cherry-pick visualization]**  
*Description: Single commit being copied between branches*

### When to Use

✅ **Good uses:**
- Apply hotfix to multiple branches
- Port feature to older version
- Extract specific fix

❌ **Avoid when:**
- Should merge entire branch instead
- Creates duplicate commits

### Cherry-Pick Example

```bash
# Scenario: Bug fix on develop, need on release branch

# 1. Find commit hash
git log develop --oneline
# a1b2c3d Fix critical bug

# 2. Switch to release branch
git checkout release/v2.0.0

# 3. Cherry-pick the fix
git cherry-pick a1b2c3d

# Done! Commit copied to release branch
```

**[IMAGE: Cherry-pick terminal output]**  
*Description: Successful cherry-pick with commit info*

### Cherry-Pick with Conflicts

```bash
git cherry-pick a1b2c3d

# If conflict:
# 1. Fix conflicts manually
# 2. Stage files
git add fixed-file.py

# 3. Continue cherry-pick
git cherry-pick --continue

# OR abort
git cherry-pick --abort
```

### Cherry-Pick Multiple Commits

```bash
# Cherry-pick range of commits
git cherry-pick a1b2c3d..e5f6g7h

# Cherry-pick specific commits
git cherry-pick a1b2c3d c3d4e5f e5f6g7h
```

---

## PRACTICE 8: Cherry-Pick Exercise

```bash
# Setup scenario
git init cherry-demo
cd cherry-demo

# Main branch
git checkout -b main
echo "main" > main.txt
git add main.txt
git commit -m "Main commit"

# Feature branch with 3 commits
git checkout -b feature
echo "feature 1" > feature1.txt
git add feature1.txt
git commit -m "Feature 1"

echo "feature 2" > feature2.txt
git add feature2.txt
git commit -m "Feature 2 - the one we want"

echo "feature 3" > feature3.txt
git add feature3.txt
git commit -m "Feature 3"

# Now cherry-pick just Feature 2 to main
git log feature --oneline
# Note the hash of "Feature 2" commit

git checkout main
git cherry-pick <HASH_OF_FEATURE_2>

# Verify
git log --oneline
ls  # Should have main.txt and feature2.txt only
```

---

## Part 12: Branch Protection Rules

### Why Protect Branches?

Prevent:
- ❌ Direct pushes to main
- ❌ Merging without review
- ❌ Merging with failing tests
- ❌ Force pushes
- ❌ Branch deletion

**[IMAGE: Unprotected vs protected branch]**  
*Description: Comparison showing protection benefits*

### Setting Up Protection

**On GitHub:**
1. Settings → Branches
2. Click **"Add rule"**
3. Branch name pattern: `main`
4. Enable:
   - ✅ Require pull request before merging
     - ✅ Require approvals (2)
     - ✅ Dismiss stale reviews
   - ✅ Require status checks to pass
     - ✅ Require branches to be up to date
   - ✅ Require conversation resolution
   - ✅ Require signed commits (optional)
   - ✅ Require linear history (optional)
   - ✅ Include administrators
5. Click **"Create"**

**[IMAGE: Branch protection settings]**  
*Description: All checkboxes and options*

### Result

Now if you try:
```bash
git push origin main
```

**Error:**
```
! [remote rejected] main -> main (protected branch hook declined)
error: failed to push some refs
```

**Must use Pull Request!** ✅

**[IMAGE: Protected branch push rejection]**  
*Description: Error message from protected branch*

---

## PRACTICE 9: Set Up Branch Protection

```bash
# 1. On GitHub, protect main branch:
#    - Require PR
#    - Require 1 approval
#    - Require status checks

# 2. Try to push directly (should fail)
git checkout main
echo "test" >> README.md
git add README.md
git commit -m "Test direct push"
git push origin main
# ❌ Should fail!

# 3. Do it the right way
git checkout -b fix/update-readme
git push origin fix/update-readme
# Create PR on GitHub
# Get approval
# Merge via PR
```

---

## MEGA CHALLENGE: Complete Team Collaboration

**Scenario:** Build a Library Management System as a team of 4.

### Setup Phase (10 min)

```bash
# Team Lead:
# 1. Create repository "library-system"
# 2. Add 3 teammates as collaborators
# 3. Set up branch protection on main
# 4. Create initial structure

mkdir library-system
cd library-system
git init
echo "# Library Management System" > README.md
echo "python-library" > .gitignore
git add .
git commit -m "Initial commit"
# Push to GitHub
```

### Development Phase (40 min)

**Person 1: Book Management**
```bash
git checkout -b feature/books
# Create books.py with Book class
# Create tests
git add .
git commit -m "Add Book management"
git push origin feature/books
# Create PR → Get review → Merge
```

**Person 2: Member Management**
```bash
git checkout -b feature/members
# Create members.py with Member class
# Create tests
git add .
git commit -m "Add Member management"
git push origin feature/members
# Create PR → Get review → Merge
```

**Person 3: Borrowing System**
```bash
git checkout -b feature/borrowing
# Create borrowing.py
# Handle book checkout/return
# Create tests
git add .
git commit -m "Add borrowing system"
git push origin feature/borrowing
# Create PR → Get review → Merge
```

**Person 4: Search & Filters**
```bash
git checkout -b feature/search
# Create search.py
# Add search functionality
# Create tests
git add .
git commit -m "Add search features"
git push origin feature/search
# Create PR → Get review → Merge
```

### Integration Phase (20 min)

**Everyone:**
```bash
# Pull latest main
git checkout main
git pull origin main

# Create integration branch together
git checkout -b feature/integrate-all

# Integrate all features
# Create main.py that uses all modules
# Fix any integration issues
# Test everything works together

git add .
git commit -m "Integrate all features"
git push origin feature/integrate-all
# Create PR → Team reviews → Merge
```

### Release Phase (15 min)

**Team Lead:**
```bash
# Create release
git checkout main
git pull origin main

# Use Git Flow for release
git checkout -b release/v1.0.0

# Update version
echo "1.0.0" > VERSION

# Create changelog
cat > CHANGELOG.md << EOF
# Changelog

## [1.0.0] - 2025-02-20

### Features
- Book management system
- Member management
- Borrowing and returns
- Search and filtering

### Contributors
- Person 1: Book management
- Person 2: Member management
- Person 3: Borrowing system
- Person 4: Search features
EOF

git add VERSION CHANGELOG.md
git commit -m "Prepare release 1.0.0"

# Merge to main
git checkout main
git merge release/v1.0.0
git tag -a v1.0.0 -m "Release 1.0.0"

# Push with tags
git push origin main --tags

# Create GitHub Release (on website)
```

### Hotfix Phase (10 min)

**Simulate bug in production:**

```bash
# Bug discovered in production!
git checkout main
git checkout -b hotfix/fix-borrow-bug

# Fix the bug
# (edit borrowing.py)
git add borrowing.py
git commit -m "Fix critical borrowing bug"

# Create PR with "hotfix" label
git push origin hotfix/fix-borrow-bug

# Fast-track review and merge
# Create tag v1.0.1
```

### Success Criteria

- ✅ 4 feature branches created and merged
- ✅ All merges via Pull Request
- ✅ At least 2 code reviews per PR
- ✅ Branch protection enforced
- ✅ Release created with tag
- ✅ Hotfix applied successfully
- ✅ Clean commit history
- ✅ All features working together

**[IMAGE: Final git network graph]**  
*Description: Complex but organized branching structure*

---

## 📚 Session Summary

### What You Mastered

✅ **GitHub Collaboration**
- Team repository setup
- Permission management
- Collaboration models

✅ **Authentication**
- SSH keys (recommended!)
- Personal Access Tokens
- Secure workflows

✅ **Pull Requests**
- Creating professional PRs
- Code review process
- Merging strategies

✅ **Workflows**
- Git Flow (enterprise)
- GitHub Flow (agile)
- When to use each

✅ **Release Management**
- Tags and versioning
- Creating releases
- Semantic versioning

✅ **Advanced Techniques**
- Cherry-picking commits
- Branch protection
- Team workflows

### Command Reference

```bash
# AUTHENTICATION
ssh-keygen -t ed25519 -C "email@example.com"
ssh-add ~/.ssh/id_ed25519
ssh -T git@github.com
git remote set-url origin git@github.com:user/repo.git

# COLLABORATION
git clone URL
git remote add upstream URL
git fetch origin
git pull origin main
git push origin branch

# TAGGING
git tag                           # List tags
git tag v1.0.0                    # Lightweight tag
git tag -a v1.0.0 -m "message"   # Annotated tag
git push origin v1.0.0            # Push tag
git push origin --tags            # Push all tags
git tag -d v1.0.0                 # Delete local tag
git push origin :refs/tags/v1.0.0 # Delete remote tag

# CHERRY-PICK
git cherry-pick <commit-hash>     # Copy commit
git cherry-pick <hash1> <hash2>   # Multiple commits
git cherry-pick --continue        # After fixing conflicts
git cherry-pick --abort           # Cancel cherry-pick

# GIT FLOW
git checkout -b feature/name develop
git checkout develop
git merge feature/name
git checkout -b release/v1.0.0 develop
git checkout main
git merge release/v1.0.0
git tag -a v1.0.0
```

---

## You're a Collaboration Pro!

You can now:
- ✅ Set up team repositories securely
- ✅ Authenticate with SSH
- ✅ Create and review Pull Requests
- ✅ Follow professional workflows
- ✅ Manage releases and tags
- ✅ Use advanced Git techniques
- ✅ Work effectively in teams

**Next session:** Open source contribution, forking, GitHub Actions, and building your portfolio!

---

**End of Session 3**
