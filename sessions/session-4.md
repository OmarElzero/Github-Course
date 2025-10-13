# Session 4 — Open Source Contribution & Advanced GitHub Features

**Duration:** 3 hours  
**Venue:** Creativa Lab C  
**Instructors:** Omar Betawy, Amr Khaled

---

## 📖 Story Introduction

**Sara** has become a Git master. Her team's project is successful and she's confident with collaboration workflows.

One day, she finds a bug in a popular Python library she uses daily. She mentions it to Professor Hassan:

**Sara:** "Professor, I found a bug in the `data-tools` library. The sorting function crashes with empty lists."

**Professor:** "Excellent observation! Did you report it?"

**Sara:** "I posted on Twitter... no response yet."

**Professor:** *smiles* "Sara, that library is open source. You don't just report bugs—you can **FIX** them! You can contribute to the actual codebase that millions use."

**Sara:** "Me? Contribute to a real project? How?"

**Professor:** "Welcome to **open source contribution**. It's how software advances. You'll also learn GitHub Actions for automation, GitHub Pages for documentation, and advanced features. These skills will make you stand out in the industry!"

Today's journey:
- 🍴 Forking and contributing to open source projects
- 🤖 GitHub Actions for CI/CD automation
- 📄 GitHub Pages for project websites
- 🔒 Repository security and best practices
- 📦 Git Submodules for managing dependencies
- 🏆 Building your developer portfolio

By the end, you'll be contributing to real projects!

---

## 🎯 Session Objectives

By the end of this session, you will be able to:
- ✅ Fork repositories and contribute to open source projects
- ✅ Follow open source contribution guidelines
- ✅ Set up GitHub Actions workflows
- ✅ Create automated tests and deployments
- ✅ Build project websites with GitHub Pages
- ✅ Manage dependencies with Git Submodules
- ✅ Implement security best practices
- ✅ Build a professional developer portfolio
- ✅ Navigate large codebases effectively

---

## Part 1: Forking — Your Copy of Any Project

### What is Forking?

A **fork** is your personal copy of someone else's repository.

```
Original Repository (Upstream)
    👥 Owner: facebook/react
    ↓ FORK
Your Copy (Origin)
    👤 Owner: your-username/react
```

**[IMAGE: Fork concept diagram]**  
*Description: Original repo and forked copy relationship*

### Why Fork?

✅ **Contribute to open source** without direct access  
✅ **Experiment safely** without affecting original  
✅ **Customize** projects for your needs  
✅ **Learn** from real codebases  
✅ **Build portfolio** with contributions  

### Fork vs Clone

| Action | Fork | Clone |
|--------|------|-------|
| What | Create copy on GitHub | Download to computer |
| Where | GitHub → GitHub | GitHub → Local |
| Purpose | Contribute/customize | Work locally |
| Connection | Linked to original | Independent |

**[IMAGE: Fork vs Clone visual]**  
*Description: Fork staying on GitHub, clone going to laptop*

---

## Part 2: The Complete Fork Workflow

### Step-by-Step: Contributing to Open Source

#### Step 1: Fork the Repository

1. Go to repository (e.g., `github.com/original-owner/project`)
2. Click **"Fork"** button (top right)
3. Choose your account
4. Wait for fork to complete

**Result:** You now have `github.com/your-username/project`

**[IMAGE: Fork button on GitHub]**  
*Description: Location of fork button with counter*

#### Step 2: Clone YOUR Fork

```bash
# Clone YOUR fork (not the original!)
git clone git@github.com:your-username/project.git
cd project

# Verify remote
git remote -v
# origin	git@github.com:your-username/project.git (fetch)
# origin	git@github.com:your-username/project.git (push)
```

**[IMAGE: Cloning fork terminal]**  
*Description: Clone command with output*

#### Step 3: Add Upstream Remote

```bash
# Add original repo as "upstream"
git remote add upstream git@github.com:original-owner/project.git

# Verify
git remote -v
# origin	  git@github.com:your-username/project.git
# upstream	git@github.com:original-owner/project.git
```

**Now you have TWO remotes:**
- `origin`: Your fork (you can push here)
- `upstream`: Original repo (read-only for you)

**[IMAGE: Remote setup diagram]**  
*Description: Origin (your fork) and upstream (original) relationship*

#### Step 4: Create Feature Branch

```bash
# Update main from upstream
git checkout main
git pull upstream main

# Create feature branch
git checkout -b fix/empty-list-bug
```

#### Step 5: Make Changes

Find and fix the bug:

```python
# Before (buggy code in data_tools/sorting.py)
def sort_items(items):
    return items.sort()  # Returns None! Bug!

# After (your fix)
def sort_items(items):
    if not items:  # Handle empty list
        return []
    return sorted(items)  # Return new sorted list
```

Add tests:

```python
# tests/test_sorting.py
def test_sort_empty_list():
    """Test that empty list doesn't crash"""
    result = sort_items([])
    assert result == []

def test_sort_normal_list():
    """Test normal sorting"""
    result = sort_items([3, 1, 2])
    assert result == [1, 2, 3]
```

**[IMAGE: Code diff showing fix]**  
*Description: Before and after code with highlighting*

#### Step 6: Commit Changes

```bash
# Run tests first!
python -m pytest tests/

# If tests pass, commit
git add data_tools/sorting.py tests/test_sorting.py
git commit -m "Fix: Handle empty list in sort_items()

- Add check for empty list
- Return empty list instead of None
- Add tests for empty list case
- Fixes issue #42"
```

**[IMAGE: Commit with descriptive message]**  
*Description: Multi-line commit message*

#### Step 7: Push to YOUR Fork

```bash
git push origin fix/empty-list-bug
```

**[IMAGE: Push to fork]**  
*Description: Terminal showing successful push*

#### Step 8: Create Pull Request

1. Go to **YOUR fork** on GitHub
2. See banner: "Compare & pull request"
3. Click button
4. **IMPORTANT:** 
   - Base repository: `original-owner/project`
   - Base branch: `main`
   - Head repository: `your-username/project`
   - Compare branch: `fix/empty-list-bug`

**[IMAGE: PR creation showing base and head]**  
*Description: Dropdown showing cross-repository PR*

5. Fill PR template:

```markdown
## Description
Fixes a bug where `sort_items()` crashes with empty lists.

## Issue
Closes #42

## Changes
- Added check for empty list in `sort_items()`
- Return empty list instead of None
- Added test cases for empty list scenario

## Testing
- All existing tests pass
- New test `test_sort_empty_list` added and passes

```bash
python -m pytest tests/ -v
# All tests passing ✅
```

## Checklist
- [x] Tests added and passing
- [x] Documentation updated (if needed)
- [x] Code follows project style guide
- [x] Signed contributor agreement
```

6. Click **"Create pull request"**

**[IMAGE: Completed PR from fork]**  
*Description: PR showing cross-repo contribution*

#### Step 9: Respond to Feedback

Maintainers review your PR:

```
Maintainer: "Great fix! Could you also add a docstring 
to explain the function behavior?"
```

Make requested changes:

```bash
# Make changes
git add .
git commit -m "Add docstring to sort_items()"
git push origin fix/empty-list-bug
# PR updates automatically!
```

**[IMAGE: PR conversation with updates]**  
*Description: Review comments and pushed updates*

#### Step 10: Celebration!

Maintainer approves and merges:

```
✅ Merged! Thanks for contributing!
```

**Your contribution is now part of the project!**

**Check your GitHub profile → Contributions graph** 📈

**[IMAGE: Contribution merged]**  
*Description: Merged PR with celebration*

**[IMAGE: GitHub contribution graph]**  
*Description: Green squares showing contributions*

---

## PRACTICE 1: Fork and Fix

**Find a beginner-friendly project:**

```bash
# Good projects for first contribution:
# - first-contributions/first-contributions
# - github/training-kit
# - microsoft/vscode-docs

# 1. Fork the repository
# (Do on GitHub)

# 2. Clone your fork
git clone git@github.com:YOUR_USERNAME/REPO.git
cd REPO

# 3. Add upstream
git remote add upstream git@github.com:ORIGINAL_OWNER/REPO.git

# 4. Create branch
git checkout -b add-my-name

# 5. Make a small change (add your name to contributors)
echo "- Your Name (@your-username)" >> CONTRIBUTORS.md

# 6. Commit
git add CONTRIBUTORS.md
git commit -m "Add Your Name to contributors"

# 7. Push
git push origin add-my-name

# 8. Create PR on GitHub
```

---

## Part 3: Keeping Your Fork Updated

### The Problem

```
Time passes...
↓
Upstream (original) gets 50 new commits
Your fork is OUTDATED
↓
Your PR has conflicts!
```

**[IMAGE: Outdated fork diagram]**  
*Description: Fork falling behind upstream*

### Solution: Sync Regularly

```bash
# 1. Fetch upstream changes
git fetch upstream

# 2. Checkout your main
git checkout main

# 3. Merge upstream changes
git merge upstream/main

# 4. Push to YOUR fork
git push origin main
```

**[IMAGE: Sync workflow]**  
*Description: Fetching from upstream, merging, pushing to origin*

### Update Feature Branch

```bash
# Your feature branch needs updates too!
git checkout fix/empty-list-bug
git merge main
# OR
git rebase main

# Push updated branch
git push origin fix/empty-list-bug --force-with-lease
```

### Quick Sync (GitHub UI)

1. Go to your fork on GitHub
2. See "This branch is 15 commits behind original:main"
3. Click **"Sync fork"**
4. Click **"Update branch"**

**[IMAGE: GitHub sync fork button]**  
*Description: Sync fork UI on GitHub*

---

## PRACTICE 2: Sync Your Fork

```bash
# Setup: Fork is behind upstream

# 1. Check status
git fetch upstream
git log main..upstream/main --oneline
# Shows commits you're missing

# 2. Update main
git checkout main
git merge upstream/main
git push origin main

# 3. Update feature branch
git checkout your-feature-branch
git rebase main
git push origin your-feature-branch --force-with-lease

# 4. Verify
git log --oneline --graph --all
```

---

## Part 4: Open Source Best Practices

### Before Contributing

**1. Read CONTRIBUTING.md**
- Every project has guidelines
- Follow their process!

**2. Check CODE_OF_CONDUCT.md**
- Be respectful and professional

**3. Search existing issues**
- Bug already reported?
- Feature already requested?

**4. Start with "good first issue" label**
- Beginner-friendly tasks

**[IMAGE: Good first issue label]**  
*Description: GitHub issue with label*

### Creating Good Issues

**Bad Issue:**
```
Title: Doesn't work
Body: It crashes
```

❌ Not helpful!

**Good Issue:**
```
Title: sort_items() crashes with empty list

Body:
## Description
The `sort_items()` function in `data_tools/sorting.py` 
raises an AttributeError when passed an empty list.

## Steps to Reproduce
1. Call `sort_items([])`
2. Function crashes

## Expected Behavior
Should return empty list

## Actual Behavior
```
AttributeError: 'NoneType' object has no attribute 'sort'
```

## Environment
- Python: 3.9
- data-tools: 1.2.0
- OS: Ubuntu 20.04

## Possible Fix
Add check for empty list before sorting.
```

✅ Clear, actionable, detailed!

**[IMAGE: Well-written issue]**  
*Description: Issue with all sections filled*

### Writing Good PRs

**Structure:**

```markdown
## Summary
[One-line description]

## Motivation
[Why is this change needed?]

## Changes
- [Bullet list of changes]

## Testing
[How did you test this?]

## Screenshots
[If UI change]

## Checklist
- [ ] Tests pass
- [ ] Docs updated
- [ ] Breaking changes noted
```

### Commit Message Style

Many projects follow **Conventional Commits:**

```
type(scope): short description

Longer explanation if needed.

- Detail 1
- Detail 2

Closes #issue-number
```

**Types:**
- `feat`: New feature
- `fix`: Bug fix
- `docs`: Documentation
- `style`: Formatting
- `refactor`: Code restructure
- `test`: Add tests
- `chore`: Maintenance

**Examples:**
```
feat(auth): add OAuth login support

fix(api): handle timeout errors correctly

docs(readme): update installation instructions

test(sorting): add tests for edge cases
```

**[IMAGE: Conventional commit format]**  
*Description: Commit message template with examples*

---

## Part 5: GitHub Actions — Automation Superpowers

### What is GitHub Actions?

**GitHub Actions** automates workflows:
- ✅ Run tests on every push
- ✅ Deploy to production automatically
- ✅ Check code style
- ✅ Build and publish packages
- ✅ Send notifications

**[IMAGE: GitHub Actions concept]**  
*Description: Trigger → Action → Result flow*

### Anatomy of a Workflow

**Workflow file:** `.github/workflows/name.yml`

```yaml
name: Workflow Name           # What it's called
on: [push, pull_request]      # When it runs
jobs:                          # What it does
  job-name:
    runs-on: ubuntu-latest     # Where it runs
    steps:                     # Steps to execute
      - uses: actions/checkout@v3
      - name: Step name
        run: command
```

**[IMAGE: Workflow structure]**  
*Description: YAML file breakdown with annotations*

---

## Part 6: Your First GitHub Action — CI Testing

### Create Test Workflow

Create `.github/workflows/test.yml`:

```yaml
name: Run Tests

on:
  push:
    branches: [ main, develop ]
  pull_request:
    branches: [ main ]

jobs:
  test:
    runs-on: ubuntu-latest
    
    strategy:
      matrix:
        python-version: ['3.8', '3.9', '3.10', '3.11']
    
    steps:
    - name: Checkout code
      uses: actions/checkout@v3
    
    - name: Set up Python ${{ matrix.python-version }}
      uses: actions/setup-python@v4
      with:
        python-version: ${{ matrix.python-version }}
    
    - name: Install dependencies
      run: |
        python -m pip install --upgrade pip
        pip install -r requirements.txt
        pip install pytest pytest-cov
    
    - name: Run tests
      run: |
        pytest tests/ -v --cov=src --cov-report=term
    
    - name: Upload coverage
      uses: codecov/codecov-action@v3
      if: matrix.python-version == '3.11'
```

**[IMAGE: Test workflow file]**  
*Description: Complete YAML with syntax highlighting*

### What This Does

1. ✅ Runs on push to main/develop
2. ✅ Runs on pull requests to main
3. ✅ Tests with Python 3.8, 3.9, 3.10, 3.11
4. ✅ Installs dependencies
5. ✅ Runs pytest with coverage
6. ✅ Uploads coverage report

**[IMAGE: GitHub Actions tab]**  
*Description: Actions running with matrix*

### Add Status Badge

In `README.md`:

```markdown
![Tests](https://github.com/username/repo/actions/workflows/test.yml/badge.svg)
```

**[IMAGE: Status badge]**  
*Description: Green passing badge in README*

---

## PRACTICE 3: Set Up CI/CD

```bash
# 1. Create workflow directory
mkdir -p .github/workflows

# 2. Create test workflow
cat > .github/workflows/test.yml << 'EOF'
name: Tests

on: [push, pull_request]

jobs:
  test:
    runs-on: ubuntu-latest
    steps:
    - uses: actions/checkout@v3
    - uses: actions/setup-python@v4
      with:
        python-version: '3.11'
    - name: Install deps
      run: |
        pip install pytest
    - name: Run tests
      run: pytest tests/ -v
EOF

# 3. Commit and push
git add .github/
git commit -m "ci: add GitHub Actions workflow"
git push origin main

# 4. Check Actions tab on GitHub
# Should see workflow running!

# 5. Break a test (on purpose)
# Push again
# Watch workflow fail (red X)

# 6. Fix test
# Push again
# Watch workflow pass (green checkmark)
```

---

## Part 7: Deployment with GitHub Actions

### Deploy to GitHub Pages

Create `.github/workflows/deploy.yml`:

```yaml
name: Deploy to GitHub Pages

on:
  push:
    branches: [ main ]

permissions:
  contents: read
  pages: write
  id-token: write

jobs:
  build:
    runs-on: ubuntu-latest
    steps:
    - uses: actions/checkout@v3
    
    - name: Setup Python
      uses: actions/setup-python@v4
      with:
        python-version: '3.11'
    
    - name: Install dependencies
      run: |
        pip install mkdocs mkdocs-material
    
    - name: Build documentation
      run: mkdocs build
    
    - name: Upload artifact
      uses: actions/upload-pages-artifact@v2
      with:
        path: ./site
  
  deploy:
    needs: build
    runs-on: ubuntu-latest
    environment:
      name: github-pages
      url: ${{ steps.deployment.outputs.page_url }}
    steps:
    - name: Deploy to GitHub Pages
      id: deployment
      uses: actions/deploy-pages@v2
```

**[IMAGE: Deployment workflow]**  
*Description: Two-job workflow (build and deploy)*

---

## Part 8: GitHub Pages — Your Project Website

### What is GitHub Pages?

Free hosting for static websites directly from your repository!

**Use cases:**
- Project documentation
- 🌐 Portfolio website
- Blog
- Project demos

**[IMAGE: GitHub Pages examples]**  
*Description: Various Pages sites*

### Method 1: Simple HTML

**Structure:**
```
repo/
├── index.html
├── style.css
├── script.js
└── README.md
```

**index.html:**
```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>My Awesome Project</title>
    <link rel="stylesheet" href="style.css">
</head>
<body>
    <header>
        <h1>🚀 My Awesome Project</h1>
        <p>Solving real-world problems with Python</p>
    </header>
    
    <main>
        <section class="features">
            <div class="feature">
                <h2>⚡ Fast</h2>
                <p>Optimized for performance</p>
            </div>
            <div class="feature">
                <h2>🔒 Secure</h2>
                <p>Built with security in mind</p>
            </div>
            <div class="feature">
                <h2>📦 Easy</h2>
                <p>Simple to install and use</p>
            </div>
        </section>
        
        <section class="install">
            <h2>Installation</h2>
            <pre><code>pip install my-awesome-project</code></pre>
        </section>
        
        <section class="example">
            <h2>Quick Example</h2>
            <pre><code>from awesome import solve

result = solve("problem")
print(result)  # "solution"</code></pre>
        </section>
    </main>
    
    <footer>
        <p>Created by <a href="https://github.com/username">@username</a></p>
        <p>
            <a href="https://github.com/username/repo">GitHub</a> |
            <a href="https://github.com/username/repo/issues">Issues</a> |
            <a href="docs.html">Documentation</a>
        </p>
    </footer>
</body>
</html>
```

**[IMAGE: Simple Pages site]**  
*Description: Clean project landing page*

### Enable GitHub Pages

1. Settings → Pages
2. Source: Deploy from a branch
3. Branch: `main`, folder: `/ (root)` or `/docs`
4. Click **Save**
5. Wait ~1 minute
6. Visit `https://username.github.io/repo/`

**[IMAGE: Pages settings]**  
*Description: Settings page with branch selection*

### Method 2: MkDocs (Documentation)

**Install:**
```bash
pip install mkdocs mkdocs-material
```

**Initialize:**
```bash
mkdocs new .
```

**Structure:**
```
repo/
├── docs/
│   ├── index.md
│   ├── installation.md
│   ├── usage.md
│   └── api.md
├── mkdocs.yml
└── README.md
```

**mkdocs.yml:**
```yaml
site_name: My Awesome Project
site_url: https://username.github.io/repo/
theme:
  name: material
  palette:
    primary: indigo
    accent: indigo
  features:
    - navigation.tabs
    - navigation.sections
    - toc.integrate
    - search.suggest

nav:
  - Home: index.md
  - Installation: installation.md
  - Usage Guide: usage.md
  - API Reference: api.md

markdown_extensions:
  - pymdownx.highlight
  - pymdownx.superfences
  - pymdownx.tabbed
  - admonition
  - codehilite
```

**docs/index.md:**
```markdown
# Welcome to My Awesome Project

A powerful tool for solving problems efficiently.

## Features

- **Fast**: Optimized algorithms
- 🔒 **Secure**: Input validation
- 📦 **Simple**: Easy API

## Quick Start

\```python
from awesome import solve

result = solve("problem")
print(result)
\```

## Installation

\```bash
pip install my-awesome-project
\```

See [Installation Guide](installation.md) for details.
```

**Build and serve:**
```bash
# Local preview
mkdocs serve
# Visit http://127.0.0.1:8000/

# Build for production
mkdocs build
# Output in site/ directory
```

**Deploy:**
```bash
mkdocs gh-deploy
```

**Automatically deploys to GitHub Pages!**

**[IMAGE: MkDocs site]**  
*Description: Professional documentation site*

---

## PRACTICE 4: Create Project Website

```bash
# 1. Install MkDocs
pip install mkdocs mkdocs-material

# 2. Initialize docs
mkdocs new .

# 3. Configure mkdocs.yml
# (Edit as shown above)

# 4. Write documentation
echo "# My Project" > docs/index.md
echo "## Features" >> docs/index.md
echo "- Feature 1" >> docs/index.md

# 5. Preview locally
mkdocs serve
# Open browser to localhost:8000

# 6. Deploy to Pages
mkdocs gh-deploy

# 7. Visit your site!
# https://YOUR_USERNAME.github.io/YOUR_REPO/
```

---

## Part 9: Git Submodules — Managing Dependencies

### What are Submodules?

**Submodules** let you include one Git repository inside another.

**Use cases:**
- Shared libraries
- 🔌 Third-party components
- 🏗️ Microservices

**[IMAGE: Submodule concept]**  
*Description: Main repo containing other repos*

### Adding a Submodule

```bash
# Add external library as submodule
git submodule add https://github.com/user/library.git libs/library

# Creates:
# - libs/library/ directory (the external repo)
# - .gitmodules file (tracking info)
```

**.gitmodules:**
```
[submodule "libs/library"]
    path = libs/library
    url = https://github.com/user/library.git
```

**[IMAGE: Directory with submodule]**  
*Description: Folder structure showing submodule*

### Cloning Repository with Submodules

```bash
# Regular clone doesn't get submodules
git clone https://github.com/user/main-repo.git
# libs/library/ is EMPTY!

# Solution 1: Clone with submodules
git clone --recurse-submodules https://github.com/user/main-repo.git

# Solution 2: Initialize after cloning
git clone https://github.com/user/main-repo.git
cd main-repo
git submodule init
git submodule update
```

**[IMAGE: Cloning with submodules]**  
*Description: Terminal showing recursive clone*

### Updating Submodules

```bash
# Update all submodules to latest
git submodule update --remote

# Update specific submodule
git submodule update --remote libs/library

# Commit the update
git add .gitmodules libs/library
git commit -m "Update library submodule"
git push
```

### Working Inside Submodules

```bash
# Go into submodule
cd libs/library

# It's a full Git repository!
git status
git log
git checkout some-branch

# Go back to main repo
cd ../..

# Commit the submodule state change
git add libs/library
git commit -m "Update library to feature branch"
```

**[IMAGE: Submodule as independent repo]**  
*Description: Git operations inside submodule*

### Removing Submodules

```bash
# 1. Deinitialize
git submodule deinit libs/library

# 2. Remove from Git
git rm libs/library

# 3. Commit
git commit -m "Remove library submodule"

# 4. Clean up (optional)
rm -rf .git/modules/libs/library
```

---

## PRACTICE 5: Work with Submodules

```bash
# 1. Create main project
mkdir main-project
cd main-project
git init

# 2. Add submodule (use a small public repo)
git submodule add https://github.com/github/training-kit.git external/training

# 3. Check status
ls external/training/  # Should have files
cat .gitmodules        # Should show config

# 4. Commit
git add .
git commit -m "Add training-kit submodule"

# 5. Simulate another developer cloning
cd ..
git clone main-project main-project-2
cd main-project-2
ls external/training/  # EMPTY!

# 6. Initialize submodules
git submodule init
git submodule update
ls external/training/  # NOW has files!

# 7. Update submodule
git submodule update --remote external/training
git add external/training
git commit -m "Update training submodule"
```

---

## Part 10: Repository Security

### Security Best Practices

#### 1. Never Commit Secrets

**Bad:**
```python
# config.py
API_KEY = "sk_live_abcdef123456"  # ❌ NEVER DO THIS!
DATABASE_URL = "postgres://user:password@host/db"
```

**Good:**
```python
# config.py
import os

API_KEY = os.getenv("API_KEY")  # ✅ From environment variable
DATABASE_URL = os.getenv("DATABASE_URL")
```

**.env:**
```
API_KEY=sk_live_abcdef123456
DATABASE_URL=postgres://user:password@host/db
```

**.gitignore:**
```
.env
*.key
*.pem
secrets/
```

**[IMAGE: Secrets in environment variables]**  
*Description: Code using os.getenv()*

#### 2. GitHub Secrets

For GitHub Actions:

1. Settings → Secrets and variables → Actions
2. Click **"New repository secret"**
3. Name: `API_KEY`
4. Value: `sk_live_abcdef123456`
5. Add secret

**Use in workflow:**
```yaml
steps:
- name: Deploy
  env:
    API_KEY: ${{ secrets.API_KEY }}
  run: |
    echo "Deploying with API key"
    ./deploy.sh
```

**[IMAGE: GitHub Secrets page]**  
*Description: Adding repository secret*

#### 3. Dependabot

**Enable Dependabot:**

1. Settings → Code security and analysis
2. Enable **Dependabot alerts**
3. Enable **Dependabot security updates**
4. Enable **Dependabot version updates**

**Create `.github/dependabot.yml`:**
```yaml
version: 2
updates:
  - package-ecosystem: "pip"
    directory: "/"
    schedule:
      interval: "weekly"
    open-pull-requests-limit: 10
```

**[IMAGE: Dependabot alerts]**  
*Description: Security alerts in repository*

#### 4. Code Scanning

**Enable CodeQL:**

Create `.github/workflows/codeql.yml`:
```yaml
name: CodeQL

on:
  push:
    branches: [ main ]
  pull_request:
    branches: [ main ]
  schedule:
    - cron: '0 0 * * 1'  # Weekly

jobs:
  analyze:
    runs-on: ubuntu-latest
    permissions:
      security-events: write
    steps:
    - uses: actions/checkout@v3
    - uses: github/codeql-action/init@v2
      with:
        languages: python
    - uses: github/codeql-action/analyze@v2
```

**[IMAGE: CodeQL scanning results]**  
*Description: Security scan showing issues*

---

## PRACTICE 6: Implement Security

```bash
# 1. Create .env file for secrets
cat > .env << EOF
API_KEY=test_key_12345
DATABASE_URL=sqlite:///db.sqlite
SECRET_KEY=super_secret_key
EOF

# 2. Add to .gitignore
echo ".env" >> .gitignore
echo "*.key" >> .gitignore

# 3. Use environment variables in code
cat > config.py << 'EOF'
import os
from dotenv import load_dotenv

load_dotenv()

API_KEY = os.getenv("API_KEY")
DATABASE_URL = os.getenv("DATABASE_URL")

if not API_KEY:
    raise ValueError("API_KEY not set in environment")
EOF

# 4. Install python-dotenv
echo "python-dotenv" >> requirements.txt

# 5. Commit (secrets are protected!)
git add .gitignore config.py requirements.txt
git commit -m "Add secure config management"

# .env is NOT committed! ✅
```

---

## Part 11: Building Your Developer Portfolio

### Essential Components

**1. Pinned Repositories**
- Pin your 6 best projects
- Show diversity (web, CLI, ML, etc.)

**2. Professional README**
```markdown
# Hi, I'm Sara Ahmed! 👋

## About Me
Computer Science student at Cairo University, passionate about 
backend development and data science.

## Skills
**Languages:** Python, JavaScript, SQL  
**Frameworks:** Django, Flask, React  
**Tools:** Git, Docker, PostgreSQL  
**Cloud:** AWS, Heroku

## 📊 GitHub Stats
![Stats](https://github-readme-stats.vercel.app/api?username=sara-ahmed&show_icons=true)

## Featured Projects
- [Student Management System](link) - Full-stack Django app
- [Data Analysis Tool](link) - Python CLI with pandas
- [Portfolio Website](link) - React + TailwindCSS

## 📫 Contact
- LinkedIn: [sara-ahmed](link)
- Email: sara@example.com
- Portfolio: [saraahmed.dev](link)
```

**[IMAGE: Professional GitHub profile]**  
*Description: Well-designed profile README*

**3. Contribution Graph**
- Contribute consistently
- Open source projects
- Personal projects
- Code challenges

**[IMAGE: Active contribution graph]**  
*Description: Green squares showing regular commits*

**4. Project READMEs**

Every project should have:
```markdown
# Project Name

Brief description (1-2 sentences)

![Screenshot](screenshot.png)

## Features
- Feature 1
- Feature 2

## Installation
\```bash
pip install -r requirements.txt
\```

## Usage
\```python
from project import main
main()
\```

## Demo
[Live Demo](link) | [Video](link)

## Technologies
- Python 3.11
- Django 4.2
- PostgreSQL

## Contributing
Pull requests welcome! See [CONTRIBUTING.md](CONTRIBUTING.md)

## License
MIT License
```

**[IMAGE: Excellent project README]**  
*Description: README with badges, screenshot, clear sections*

---

## PRACTICE 7: Improve Your Profile

```bash
# 1. Create profile README
# Create repository: YOUR_USERNAME/YOUR_USERNAME
# Add README.md with bio, skills, projects

# 2. Add GitHub Stats
# Use: https://github.com/anuraghazra/github-readme-stats
# ![Stats](https://github-readme-stats.vercel.app/api?username=YOUR_USERNAME)

# 3. Pin 6 best repositories
# Go to profile → Customize pins

# 4. Improve project READMEs
# Add screenshots, clear installation, usage examples

# 5. Add badges to projects
# shields.io for status badges
```

---

## MEGA CHALLENGE: Complete Open Source Contribution

**Time:** 60 minutes  
**Difficulty:** Advanced

### Mission: Find, Fix, Contribute

**Part 1: Find a Project (10 min)**

```bash
# Search for beginner-friendly projects:
# https://github.com/topics/good-first-issue
# https://github.com/topics/beginner-friendly
# https://firsttimersonly.com/

# Criteria:
# ✅ Has "good first issue" label
# ✅ Active maintainers (recent commits)
# ✅ Clear CONTRIBUTING.md
# ✅ Friendly community
# ✅ Technologies you know
```

**Part 2: Fork and Setup (5 min)**

```bash
# 1. Fork repository
# 2. Clone your fork
git clone git@github.com:YOUR_USERNAME/PROJECT.git
cd PROJECT

# 3. Add upstream
git remote add upstream git@github.com:ORIGINAL/PROJECT.git

# 4. Install dependencies
pip install -r requirements.txt
# OR
npm install
```

**Part 3: Choose an Issue (5 min)**

```bash
# Find issue with label:
# - good first issue
# - help wanted
# - beginner-friendly

# Comment on issue:
# "Hi! I'd like to work on this. Is it still available?"

# Wait for maintainer confirmation
```

**Part 4: Implement Fix (25 min)**

```bash
# 1. Create branch
git checkout -b fix/issue-123

# 2. Understand the problem
# Read issue carefully
# Reproduce the bug/understand feature

# 3. Implement solution
# Write code
# Add/update tests
# Update documentation

# 4. Test thoroughly
pytest tests/
# OR
npm test

# 5. Check code style
flake8 .
# OR
npm run lint
```

**Part 5: Create Pull Request (10 min)**

```bash
# 1. Commit changes
git add .
git commit -m "fix: resolve issue #123

- Detailed explanation
- What changed
- Why it works

Closes #123"

# 2. Push to your fork
git push origin fix/issue-123

# 3. Create PR
# - Clear title
# - Reference issue
# - Explain changes
# - Add screenshots if UI

# 4. Wait for review
# Be patient and responsive
```

**Part 6: Respond to Feedback (5 min)**

```bash
# If changes requested:
# 1. Make requested changes
# 2. Commit
git commit -am "address review feedback"

# 3. Push
git push origin fix/issue-123

# PR updates automatically!
```

### Success Criteria

- ✅ Found suitable project
- ✅ Issue assigned to you
- ✅ Solution implemented
- ✅ Tests pass
- ✅ PR created with clear description
- ✅ Followed project guidelines
- ✅ Responded professionally to feedback

**🎉 Bonus:**
- PR gets merged!
- You're now an open source contributor!

---

## PRACTICE 8: Set Up Complete CI/CD

**Create full pipeline:**

```yaml
# .github/workflows/ci-cd.yml
name: CI/CD Pipeline

on:
  push:
    branches: [ main ]
  pull_request:
    branches: [ main ]

jobs:
  test:
    runs-on: ubuntu-latest
    strategy:
      matrix:
        python-version: ['3.9', '3.10', '3.11']
    steps:
    - uses: actions/checkout@v3
    - uses: actions/setup-python@v4
      with:
        python-version: ${{ matrix.python-version }}
    - name: Install dependencies
      run: |
        pip install -r requirements.txt
        pip install pytest pytest-cov flake8
    - name: Lint
      run: flake8 src/ --max-line-length=120
    - name: Test
      run: pytest tests/ -v --cov=src
  
  build:
    needs: test
    if: github.ref == 'refs/heads/main'
    runs-on: ubuntu-latest
    steps:
    - uses: actions/checkout@v3
    - name: Build package
      run: python -m build
    - name: Upload artifact
      uses: actions/upload-artifact@v3
      with:
        name: package
        path: dist/
  
  deploy-docs:
    needs: test
    if: github.ref == 'refs/heads/main'
    runs-on: ubuntu-latest
    permissions:
      contents: write
    steps:
    - uses: actions/checkout@v3
    - uses: actions/setup-python@v4
    - name: Install MkDocs
      run: pip install mkdocs mkdocs-material
    - name: Deploy docs
      run: mkdocs gh-deploy --force
```

---

## 📚 Session Summary

### What You Mastered

✅ **Open Source Contribution**
- Forking repositories
- Contributing to projects
- Following best practices
- Writing good issues/PRs

✅ **GitHub Actions**
- CI/CD pipelines
- Automated testing
- Automated deployment
- Custom workflows

✅ **GitHub Pages**
- Hosting websites
- Documentation sites
- Project landing pages

✅ **Git Submodules**
- Adding external repos
- Managing dependencies
- Updating submodules

✅ **Security**
- Protecting secrets
- Dependabot
- Code scanning
- Best practices

✅ **Portfolio Building**
- Professional profile
- Project documentation
- Contribution history

### Command Reference

```bash
# FORKING
git clone git@github.com:YOUR_USERNAME/repo.git
git remote add upstream git@github.com:ORIGINAL/repo.git
git fetch upstream
git merge upstream/main

# SUBMODULES
git submodule add URL path
git submodule init
git submodule update
git submodule update --remote
git clone --recurse-submodules URL

# PAGES
mkdocs new .
mkdocs serve
mkdocs build
mkdocs gh-deploy

# WORKFLOWS
# Create .github/workflows/name.yml
git add .github/
git commit -m "Add workflow"
git push
```

---

## Congratulations! You're a Git/GitHub Expert!

You've completed the workshop and can now:
- ✅ Use Git like a professional
- ✅ Collaborate effectively with teams
- ✅ Contribute to open source projects
- ✅ Automate workflows with GitHub Actions
- ✅ Build professional portfolios
- ✅ Follow industry best practices

### Next Steps

1. **Contribute to open source** (start today!)
2. **Build your portfolio** (3-5 strong projects)
3. **Automate everything** (CI/CD for all projects)
4. **Stay active** (commit regularly)
5. **Help others** (answer questions, review PRs)

### Resources

- [GitHub Docs](https://docs.github.com)
- [Git Book](https://git-scm.com/book/en/v2)
- [GitHub Skills](https://skills.github.com/)
- [First Timers Only](https://www.firsttimersonly.com/)
- [Good First Issue](https://goodfirstissue.dev/)

**Keep coding! Keep contributing! Keep learning!** 🚀

---

**End of Session 4 — End of Workshop**

Thank you for participating! We hope you enjoyed the workshop and learned valuable skills. Now go out there and make amazing contributions to the world of software development!

---

**Workshop Complete! 🎊**
