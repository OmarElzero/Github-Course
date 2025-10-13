# Session 4 — The Job: Open Source, Forking & Advanced GitHub

**Duration:** 2 hours 30 minutes  
**Venue:** Creativa Lab D  
**Instructors:** Omar Betawy, Amr Khaled

---

## 📖 Story Introduction

**Ahmed**, Sara's classmate and friend, has been inspired by her journey. While working on a personal project, he discovers a bug in a popular Python library that thousands of developers use. "I can fix this!" he thinks excitedly. But how do you contribute to a project you don't own?

Meanwhile, **Sara** wants to build a portfolio website to showcase all the projects she's built during her internship. She's heard about GitHub Pages but isn't sure how to use it.

Both are about to discover the true power of GitHub: **contributing to the global open-source community** and **sharing their work with the world**.

Today, you'll learn:
- How to contribute to open source projects
- The forking workflow used by millions
- Advanced GitHub features for collaboration
- Automation with GitHub Actions
- Building and hosting websites with GitHub Pages
- Security best practices

By the end of this session, you'll be ready to join the global developer community as a professional contributor! 🌍

---

## 🎯 Session Objectives

By the end of this session, you will be able to:
- Fork repositories and understand upstream remotes
- Contribute to open source projects professionally
- Keep your fork synchronized with upstream
- Use GitHub Projects for project management
- Set up GitHub Actions for CI/CD
- Deploy websites using GitHub Pages
- Implement security best practices
- Use advanced GitHub features
- Build a professional developer portfolio

---

## 🍴 Part 1: Forking - Your Copy of Any Project

### What is Forking?

**Forking** creates your personal copy of someone else's repository on GitHub. Unlike cloning (which creates a local copy), forking creates a **remote copy** under your GitHub account.

**[IMAGE: Fork workflow diagram showing original repo being copied to user's account]**  
*Description: Arrow from original repository to forked copy with different owner*

### Fork vs Clone vs Branch

| Operation | Where | Purpose | Permission Needed |
|-----------|-------|---------|-------------------|
| **Fork** | GitHub → GitHub | Your copy of someone's repo | None (public repos) |
| **Clone** | GitHub → Local | Get code on your computer | None (public repos) |
| **Branch** | Same repo | Work on features | Write access required |

**[IMAGE: Visual comparison of fork, clone, and branch]**  
*Description: Three diagrams showing each operation*

### When to Fork

✅ **Fork when:**
- Contributing to open source projects
- You don't have write access
- You want to experiment without affecting original
- Creating your own version of a project

❌ **Don't fork when:**
- You're on the team (use branches instead)
- You just want to view code (just browse on GitHub)
- You want to use a library (use package manager)

---

## 🔧 Part 2: Contributing to Open Source

### Ahmed's First Contribution

Ahmed found a bug in `awesome-python-lib`. Let's walk through his contribution.

**[IMAGE: GitHub repository with obvious issue]**  
*Description: Bug or typo visible in code/docs*

### Step 1: Fork the Repository

1. Go to the repository: `https://github.com/original-author/awesome-python-lib`
2. Click the **"Fork"** button in the top-right corner
3. GitHub creates a copy at: `https://github.com/ahmed/awesome-python-lib`

**[IMAGE: GitHub Fork button highlighted]**  
*Description: Fork button location on repository page*

**[IMAGE: Forking in progress animation]**  
*Description: GitHub's fork animation/message*

**[IMAGE: Forked repository showing "forked from" label]**  
*Description: Your forked repo with attribution to original*

### Step 2: Clone Your Fork

```bash
# Clone YOUR fork (not the original)
git clone https://github.com/ahmed/awesome-python-lib.git
cd awesome-python-lib
```

**Expected Output:**
```
Cloning into 'awesome-python-lib'...
remote: Enumerating objects: 250, done.
remote: Counting objects: 100% (250/250), done.
remote: Compressing objects: 100% (180/180), done.
remote: Total 250 (delta 95), reused 200 (delta 60)
Receiving objects: 100% (250/250), 500.00 KiB | 2.00 MiB/s, done.
Resolving deltas: 100% (95/95), done.
```

**[IMAGE: Terminal showing clone of forked repo]**  
*Description: Clone command with output*

### Step 3: Add Upstream Remote

Connect to the original repository to sync updates:

```bash
# Add original repo as "upstream"
git remote add upstream https://github.com/original-author/awesome-python-lib.git

# Verify remotes
git remote -v
```

**Expected Output:**
```
origin    https://github.com/ahmed/awesome-python-lib.git (fetch)
origin    https://github.com/ahmed/awesome-python-lib.git (push)
upstream  https://github.com/original-author/awesome-python-lib.git (fetch)
upstream  https://github.com/original-author/awesome-python-lib.git (push)
```

**Understanding the setup:**
- **origin**: Your fork (you can push here)
- **upstream**: Original repo (you can only pull from here)

**[IMAGE: Diagram showing local repo connected to origin and upstream]**  
*Description: Three boxes showing upstream, your fork, and local with arrows*

### Step 4: Create a Feature Branch

```bash
# Update your main branch first
git checkout main
git pull upstream main

# Create branch for your fix
git checkout -b fix/documentation-typo
```

**[IMAGE: Terminal showing branch creation]**  
*Description: Commands and output for branch setup*

### Step 5: Make Your Changes

Ahmed fixes a typo in `README.md`:

**Before:**
```markdown
## Installaton

To instal the library, run:
```

**After:**
```markdown
## Installation

To install the library, run:
```

**[IMAGE: Side-by-side diff showing the fix]**  
*Description: Red showing old text, green showing corrected text*

### Step 6: Commit and Push

```bash
# Stage and commit
git add README.md
git commit -m "Fix typos in installation section

- Fix 'Installaton' → 'Installation'
- Fix 'instal' → 'install'
"

# Push to YOUR fork
git push origin fix/documentation-typo
```

**[IMAGE: Terminal showing commit and push]**  
*Description: Successful push to fork*

### Step 7: Create Pull Request to Upstream

1. Go to your fork on GitHub
2. You'll see: **"fix/documentation-typo had recent pushes"**
3. Click **"Compare & pull request"**

**[IMAGE: GitHub banner prompting to create PR]**  
*Description: Yellow banner with compare button*

4. **Important:** Make sure the base repository is the **original**, not your fork:
   - Base repository: `original-author/awesome-python-lib`
   - Base branch: `main`
   - Head repository: `ahmed/awesome-python-lib`
   - Compare branch: `fix/documentation-typo`

**[IMAGE: PR creation form with base/head repository selection]**  
*Description: Dropdown showing correct repository selection*

5. Fill in PR details:

```markdown
## Description
Fixed spelling errors in the Installation section of README.

## Changes
- Corrected "Installaton" to "Installation"
- Corrected "instal" to "install"

## Type of Change
- [x] Documentation update
- [ ] Bug fix
- [ ] New feature

## Checklist
- [x] I have read the contributing guidelines
- [x] My changes follow the project's style guidelines
- [x] I have checked my spelling and grammar
```

6. Click **"Create pull request"**

**[IMAGE: Completed PR form]**  
*Description: All fields filled out professionally*

✅ **Ahmed's first open source contribution is submitted!**

**[IMAGE: Created PR page showing status]**  
*Description: PR with "Open" status and conversation tab*

### Step 8: Responding to Feedback

The maintainer comments:

> "Thanks for the contribution! Could you also check the 'Configuration' section? I think there might be similar typos there."

Ahmed responds:

```bash
# Make additional fixes
# Edit README.md to fix Configuration section

git add README.md
git commit -m "Fix typos in Configuration section as well"
git push origin fix/documentation-typo
```

The PR automatically updates! 🎉

**[IMAGE: PR showing new commit added]**  
*Description: Timeline with additional commit*

### Step 9: Contribution Accepted!

The maintainer merges Ahmed's PR:

**[IMAGE: Merged PR with purple "Merged" badge]**  
*Description: Successful merge notification*

Ahmed is now an **open source contributor**! His name appears in:
- Repository contributors list
- Commit history
- The project's changelog

**[IMAGE: GitHub contributors page showing Ahmed]**  
*Description: Contributors list with avatar and contribution count*

---

## 🔄 Part 3: Keeping Your Fork Synchronized

### The Problem

After a few weeks, the original repository has new updates. Ahmed's fork is behind.

**[IMAGE: GitHub showing "This branch is 15 commits behind"]**  
*Description: Fork status message on GitHub*

### Method 1: Sync via Command Line

```bash
# Switch to main branch
git checkout main

# Fetch updates from upstream
git fetch upstream

# Merge upstream changes
git merge upstream/main

# Push updates to your fork
git push origin main
```

**[IMAGE: Terminal showing sync process]**  
*Description: Fetch, merge, push commands with output*

### Method 2: Sync via GitHub UI

1. Go to your fork on GitHub
2. Click **"Fetch upstream"** button
3. Click **"Fetch and merge"**

**[IMAGE: GitHub Sync fork button]**  
*Description: Fetch upstream dropdown on fork page*

**[IMAGE: Sync confirmation dialog]**  
*Description: Dialog showing commits to be synced*

### Handling Conflicts During Sync

If you made changes to your fork that conflict:

```bash
git fetch upstream
git merge upstream/main

# If conflicts occur:
# 1. Edit conflicting files
# 2. Remove conflict markers
# 3. Stage resolved files
git add .
git commit -m "Merge upstream changes"

# Push to your fork
git push origin main
```

---

## 🌟 Part 4: Finding and Choosing Open Source Projects

### Where to Find Projects

**Popular platforms:**
- GitHub Explore: https://github.com/explore
- GitHub Topics: https://github.com/topics
- First Timers Only: https://www.firsttimersonly.com
- Good First Issue: https://goodfirstissue.dev
- Up For Grabs: https://up-for-grabs.net

**[IMAGE: GitHub Explore page screenshot]**  
*Description: Trending repositories and topics*

### What to Look For

**Good beginner-friendly projects have:**
- ✅ Clear README with contribution guidelines
- ✅ `CONTRIBUTING.md` file
- ✅ Issues labeled `good first issue` or `beginner friendly`
- ✅ Active maintainers (recent commits)
- ✅ Welcoming community
- ✅ Clear code of conduct

**Red flags:**
- ❌ No documentation
- ❌ Ignored pull requests
- ❌ Rude maintainers
- ❌ No activity in months

**[IMAGE: Good repository example with labels]**  
*Description: Repo showing good first issues and documentation*

### Reading Contributing Guidelines

Before contributing, ALWAYS read:

1. **README.md**: Understand what the project does
2. **CONTRIBUTING.md**: Learn how to contribute
3. **CODE_OF_CONDUCT.md**: Understand community standards
4. **LICENSE**: Know the project's license

**Example CONTRIBUTING.md structure:**
```markdown
# Contributing to Awesome Python Lib

## Getting Started
- Fork the repository
- Clone your fork
- Create a branch

## Development Setup
- Install dependencies: `pip install -r requirements-dev.txt`
- Run tests: `pytest`
- Check style: `flake8`

## Making Changes
- Write clear commit messages
- Add tests for new features
- Update documentation

## Submitting Pull Requests
- Ensure all tests pass
- Describe your changes clearly
- Reference related issues

## Code Style
- Follow PEP 8
- Use meaningful variable names
- Add docstrings to functions
```

**[IMAGE: Well-structured CONTRIBUTING.md file]**  
*Description: Example of clear contribution guidelines*

---

## 📋 Part 5: GitHub Projects - Advanced Project Management

### What are GitHub Projects?

**GitHub Projects** is a built-in project management tool with Kanban boards, tables, and automation.

**[IMAGE: GitHub Projects board view]**  
*Description: Kanban board with Todo, In Progress, Done columns*

### Creating a Project

Sara wants to track her portfolio website development:

1. Go to repository
2. Click **"Projects"** tab
3. Click **"New project"**
4. Choose template: **"Board"** or **"Table"**
5. Name it: "Portfolio Website Development"

**[IMAGE: New project creation dialog]**  
*Description: Template selection screen*

### Board Layout

**Columns:**
- 📋 **Backlog**: Future features
- 📝 **Todo**: Ready to start
- 🚧 **In Progress**: Currently working
- 👀 **Review**: Awaiting review
- ✅ **Done**: Completed

**[IMAGE: Full project board with cards in different columns]**  
*Description: Populated Kanban board*

### Adding Issues to Projects

```markdown
1. Create an issue
2. On issue page, click "Projects" in right sidebar
3. Select your project
4. Issue appears as a card in the project
```

**[IMAGE: Issue page showing Projects sidebar]**  
*Description: Projects dropdown on issue page*

### Automation

Set up automation rules:

- **When issue is opened** → Add to "Todo"
- **When PR is opened** → Move to "In Progress"
- **When PR is merged** → Move to "Done"

**[IMAGE: Project automation settings]**  
*Description: Automation rules configuration*

### Project Views

**Board View:**
```
[📋 Backlog] [📝 Todo] [🚧 In Progress] [✅ Done]
   Card 1      Card 2      Card 3        Card 4
   Card 5                  Card 6        Card 7
```

**Table View:**
| Title | Status | Assignee | Priority | Due Date |
|-------|--------|----------|----------|----------|
| Add homepage | In Progress | Sara | High | Mar 15 |
| Create blog | Todo | Sara | Medium | Mar 20 |

**Roadmap View:**
Timeline visualization of milestones and deadlines.

**[IMAGE: Project table view]**  
*Description: Spreadsheet-like view with sortable columns*

---

## 🤖 Part 6: GitHub Actions - Automation & CI/CD

### What are GitHub Actions?

**GitHub Actions** automate workflows like:
- Running tests on every commit
- Deploying code automatically
- Checking code quality
- Sending notifications
- Building documentation

**[IMAGE: GitHub Actions workflow diagram]**  
*Description: Trigger → Runner → Actions → Results*

### Sara's First Workflow

Sara wants to automatically run tests when she pushes code.

#### Step 1: Create Workflow File

Create `.github/workflows/tests.yml`:

```yaml
name: Run Tests

# Trigger workflow on push or pull request
on:
  push:
    branches: [ main, develop ]
  pull_request:
    branches: [ main ]

# Jobs to run
jobs:
  test:
    # Run on Ubuntu
    runs-on: ubuntu-latest
    
    # Steps to execute
    steps:
      # Checkout code
      - name: Checkout repository
        uses: actions/checkout@v3
      
      # Set up Python
      - name: Set up Python 3.9
        uses: actions/setup-python@v4
        with:
          python-version: 3.9
      
      # Install dependencies
      - name: Install dependencies
        run: |
          python -m pip install --upgrade pip
          pip install -r requirements.txt
          pip install pytest pytest-cov
      
      # Run tests
      - name: Run tests with coverage
        run: |
          pytest --cov=. --cov-report=xml
      
      # Upload coverage report
      - name: Upload coverage to Codecov
        uses: codecov/codecov-action@v3
        with:
          file: ./coverage.xml
```

**[IMAGE: Workflow file in code editor]**  
*Description: YAML file with syntax highlighting*

#### Step 2: Commit and Push

```bash
git add .github/workflows/tests.yml
git commit -m "Add GitHub Actions workflow for automated testing"
git push origin main
```

**[IMAGE: Terminal showing workflow file commit]**  
*Description: Git commands for workflow setup*

#### Step 3: View Workflow Run

1. Go to repository on GitHub
2. Click **"Actions"** tab
3. See your workflow running

**[IMAGE: GitHub Actions tab showing running workflow]**  
*Description: Workflow with yellow "in progress" indicator*

**[IMAGE: Completed workflow with green checkmark]**  
*Description: All steps passed successfully*

#### Step 4: View Detailed Logs

Click on workflow run to see logs for each step:

**[IMAGE: Workflow run details with expandable steps]**  
*Description: Each step with timing and output logs*

### Common Workflow Examples

#### Linting and Code Quality

```yaml
name: Code Quality

on: [push, pull_request]

jobs:
  lint:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3
      - name: Set up Python
        uses: actions/setup-python@v4
        with:
          python-version: 3.9
      - name: Install linters
        run: |
          pip install flake8 black mypy
      - name: Run flake8
        run: flake8 .
      - name: Check formatting with black
        run: black --check .
      - name: Type checking with mypy
        run: mypy .
```

#### Automatic Deployment

```yaml
name: Deploy to Production

on:
  push:
    branches: [ main ]
    tags:
      - 'v*'

jobs:
  deploy:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3
      - name: Deploy to server
        env:
          DEPLOY_KEY: ${{ secrets.DEPLOY_KEY }}
        run: |
          echo "Deploying to production..."
          # Deployment commands here
```

#### Multi-Python Version Testing

```yaml
name: Test Multiple Python Versions

on: [push, pull_request]

jobs:
  test:
    runs-on: ubuntu-latest
    strategy:
      matrix:
        python-version: [3.7, 3.8, 3.9, 3.10, 3.11]
    
    steps:
      - uses: actions/checkout@v3
      - name: Set up Python ${{ matrix.python-version }}
        uses: actions/setup-python@v4
        with:
          python-version: ${{ matrix.python-version }}
      - name: Install and test
        run: |
          pip install -r requirements.txt
          pytest
```

**[IMAGE: Matrix build showing multiple Python versions]**  
*Description: Workflow running parallel jobs for different Python versions*

### Status Badges

Add badges to README.md:

```markdown
# My Project

![Tests](https://github.com/sara/my-project/workflows/Tests/badge.svg)
![Code Quality](https://github.com/sara/my-project/workflows/Code%20Quality/badge.svg)
[![codecov](https://codecov.io/gh/sara/my-project/branch/main/graph/badge.svg)](https://codecov.io/gh/sara/my-project)
```

**[IMAGE: README with status badges]**  
*Description: Green passing badges displayed in README*

---

## 🌐 Part 7: GitHub Pages - Host Your Website

### What is GitHub Pages?

**GitHub Pages** hosts static websites directly from your repository for FREE!

**Perfect for:**
- Personal portfolios
- Project documentation
- Blogs
- Landing pages

**[IMAGE: Example portfolio website hosted on GitHub Pages]**  
*Description: Professional-looking personal website*

### Sara's Portfolio Website

Sara wants to create: `https://sara-ahmed.github.io`

#### Step 1: Create Repository

**Important:** Name it `your-username.github.io`

```
Repository name: sara-ahmed.github.io
Description: Personal portfolio and blog
Public
```

**[IMAGE: Create repository form with username.github.io pattern]**  
*Description: Correctly named repository for GitHub Pages*

#### Step 2: Create Website Files

Create `index.html`:

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Sara Ahmed - Software Developer</title>
    <link rel="stylesheet" href="styles.css">
</head>
<body>
    <header>
        <nav>
            <h1>Sara Ahmed</h1>
            <ul>
                <li><a href="#about">About</a></li>
                <li><a href="#projects">Projects</a></li>
                <li><a href="#contact">Contact</a></li>
            </ul>
        </nav>
    </header>

    <section id="hero">
        <h2>Software Developer</h2>
        <p>Building elegant solutions with Python and Web Technologies</p>
        <a href="#projects" class="cta-button">View My Work</a>
    </section>

    <section id="about">
        <h2>About Me</h2>
        <p>I'm a computer science student at FCAI Cairo University with a passion 
        for creating impactful software. I recently completed an internship at 
        TechWave Solutions where I developed authentication systems and learned 
        professional Git workflows.</p>
    </section>

    <section id="projects">
        <h2>My Projects</h2>
        <div class="project-grid">
            <div class="project-card">
                <h3>Student Management System</h3>
                <p>A collaborative team project with CRUD operations, 
                authentication, and database integration.</p>
                <a href="https://github.com/sara-ahmed/student-management-system">View on GitHub</a>
            </div>
            <div class="project-card">
                <h3>Authentication Module</h3>
                <p>Secure user authentication system with session management 
                built during my internship.</p>
                <a href="https://github.com/sara-ahmed/auth-module">View on GitHub</a>
            </div>
            <div class="project-card">
                <h3>Calculator App</h3>
                <p>My first Git project - a simple calculator with Python.</p>
                <a href="https://github.com/sara-ahmed/my-calculator">View on GitHub</a>
            </div>
        </div>
    </section>

    <section id="contact">
        <h2>Get In Touch</h2>
        <p>Email: sara.ahmed@example.com</p>
        <p>GitHub: <a href="https://github.com/sara-ahmed">@sara-ahmed</a></p>
        <p>LinkedIn: <a href="https://linkedin.com/in/sara-ahmed">Sara Ahmed</a></p>
    </section>

    <footer>
        <p>&copy; 2025 Sara Ahmed. Built with ❤️ and GitHub Pages.</p>
    </footer>
</body>
</html>
```

Create `styles.css`:

```css
* {
    margin: 0;
    padding: 0;
    box-sizing: border-box;
}

body {
    font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
    line-height: 1.6;
    color: #333;
}

header {
    background: #2c3e50;
    color: white;
    padding: 1rem 0;
    position: fixed;
    width: 100%;
    top: 0;
    z-index: 1000;
}

nav {
    display: flex;
    justify-content: space-between;
    align-items: center;
    max-width: 1200px;
    margin: 0 auto;
    padding: 0 2rem;
}

nav ul {
    display: flex;
    list-style: none;
    gap: 2rem;
}

nav a {
    color: white;
    text-decoration: none;
    transition: color 0.3s;
}

nav a:hover {
    color: #3498db;
}

#hero {
    background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
    color: white;
    text-align: center;
    padding: 150px 20px 100px;
    margin-top: 60px;
}

#hero h2 {
    font-size: 3rem;
    margin-bottom: 1rem;
}

.cta-button {
    display: inline-block;
    background: white;
    color: #667eea;
    padding: 12px 30px;
    border-radius: 25px;
    text-decoration: none;
    font-weight: bold;
    margin-top: 2rem;
    transition: transform 0.3s;
}

.cta-button:hover {
    transform: translateY(-3px);
}

section {
    max-width: 1200px;
    margin: 0 auto;
    padding: 80px 20px;
}

h2 {
    font-size: 2.5rem;
    margin-bottom: 2rem;
    text-align: center;
}

.project-grid {
    display: grid;
    grid-template-columns: repeat(auto-fit, minmax(300px, 1fr));
    gap: 2rem;
    margin-top: 3rem;
}

.project-card {
    background: white;
    padding: 2rem;
    border-radius: 10px;
    box-shadow: 0 5px 15px rgba(0,0,0,0.1);
    transition: transform 0.3s;
}

.project-card:hover {
    transform: translateY(-5px);
}

.project-card h3 {
    color: #667eea;
    margin-bottom: 1rem;
}

.project-card a {
    color: #667eea;
    text-decoration: none;
    font-weight: bold;
}

#contact {
    background: #f8f9fa;
    text-align: center;
}

footer {
    background: #2c3e50;
    color: white;
    text-align: center;
    padding: 2rem;
}
```

**[IMAGE: Portfolio website preview]**  
*Description: Beautiful rendered website with hero section, projects, contact*

#### Step 3: Push to GitHub

```bash
git add .
git commit -m "Initial portfolio website"
git push origin main
```

#### Step 4: Enable GitHub Pages

1. Go to repository **Settings**
2. Scroll to **Pages** section
3. Under "Source", select **main** branch
4. Click **Save**

**[IMAGE: GitHub Pages settings]**  
*Description: Pages configuration with branch selection*

After a minute, your site is live at: `https://sara-ahmed.github.io` 🎉

**[IMAGE: GitHub Pages success message with URL]**  
*Description: Green banner showing site is published*

### Using Jekyll for Blogging

Create `_config.yml`:

```yaml
title: Sara Ahmed's Blog
description: My journey in software development
theme: minima
author: Sara Ahmed
email: sara.ahmed@example.com

# Social links
github_username: sara-ahmed
linkedin_username: sara-ahmed

# Build settings
markdown: kramdown
```

Create blog posts in `_posts/`:

`_posts/2025-02-15-my-first-open-source-contribution.md`:

```markdown
---
layout: post
title: "My First Open Source Contribution"
date: 2025-02-15 10:00:00 +0200
categories: open-source git
---

Today I made my first contribution to an open source project! 
Here's what I learned...

## Finding the Project

I was looking for beginner-friendly projects and found...

## The Process

1. Forked the repository
2. Created a branch
3. Made my changes
4. Submitted a PR

## What I Learned

Contributing to open source taught me...
```

**[IMAGE: Jekyll blog post rendered]**  
*Description: Beautiful blog layout with post*

### Custom Domain

To use your own domain (e.g., `saraahmed.com`):

1. Create file named `CNAME` with your domain:
   ```
   saraahmed.com
   ```

2. In your domain registrar's DNS settings, add:
   ```
   Type: A
   Name: @
   Value: 185.199.108.153
   
   Type: CNAME
   Name: www
   Value: sara-ahmed.github.io
   ```

3. Wait for DNS propagation (up to 24 hours)

**[IMAGE: Custom domain setup in GitHub Pages]**  
*Description: Custom domain field in Pages settings*

---

## 🔒 Part 8: Security Best Practices

### Never Commit Secrets!

**❌ Never commit:**
- API keys
- Passwords
- Database credentials
- Private keys
- Access tokens

**Example of what NOT to do:**

```python
# BAD - NEVER DO THIS!
API_KEY = "sk-1234567890abcdef"
DATABASE_URL = "postgresql://user:password@localhost/db"
```

**[IMAGE: Crossed out code showing exposed secrets]**  
*Description: Red X over code with sensitive data*

### Using Environment Variables

**Create `.env` file (add to .gitignore!):**

```bash
# .env
API_KEY=sk-1234567890abcdef
DATABASE_URL=postgresql://user:password@localhost/db
SECRET_KEY=your-secret-key-here
```

**Add to `.gitignore`:**
```
.env
.env.local
.env.production
*.key
secrets/
```

**Use in code:**

```python
import os
from dotenv import load_dotenv

# Load environment variables
load_dotenv()

# Access securely
API_KEY = os.getenv('API_KEY')
DATABASE_URL = os.getenv('DATABASE_URL')
```

**[IMAGE: Environment variable usage in code]**  
*Description: Code properly loading secrets from environment*

### GitHub Secrets for Actions

Store secrets for CI/CD:

1. Go to repository **Settings**
2. Click **Secrets and variables** → **Actions**
3. Click **New repository secret**
4. Name: `API_KEY`
5. Value: Your actual API key
6. Click **Add secret**

**[IMAGE: GitHub Secrets settings page]**  
*Description: Secrets management interface*

**Use in workflows:**

```yaml
- name: Deploy
  env:
    API_KEY: ${{ secrets.API_KEY }}
  run: |
    ./deploy.sh
```

### Branch Protection Rules

Protect important branches:

1. Go to **Settings** → **Branches**
2. Click **Add rule**
3. Branch name pattern: `main`
4. Enable:
   - ✅ Require pull request reviews
   - ✅ Require status checks to pass
   - ✅ Require conversation resolution
   - ✅ Require signed commits (advanced)

**[IMAGE: Branch protection rules configuration]**  
*Description: Checkboxes for various protection options*

### Security Advisories

GitHub can detect vulnerabilities in dependencies:

**[IMAGE: Security advisory alert]**  
*Description: Dependabot alert showing vulnerable package*

**Fix vulnerabilities:**
1. Go to **Security** tab
2. Review Dependabot alerts
3. Click **Create security update**
4. Review and merge the automated PR

**[IMAGE: Dependabot PR for security update]**  
*Description: Auto-generated PR updating vulnerable dependency*

### Two-Factor Authentication (2FA)

**Enable 2FA on your GitHub account:**

1. Click your profile → **Settings**
2. **Password and authentication**
3. **Enable two-factor authentication**
4. Choose: SMS or Authenticator app (recommended)
5. Save recovery codes securely!

**[IMAGE: 2FA setup screen]**  
*Description: QR code for authenticator app*

---

## 🎨 Part 9: Advanced GitHub Features

### GitHub CLI

Command-line tool for GitHub operations:

```bash
# Install
brew install gh  # Mac
sudo apt install gh  # Ubuntu

# Authenticate
gh auth login

# Create repo from terminal
gh repo create my-new-project --public

# Create issue
gh issue create --title "Bug: Login fails" --body "Description here"

# Create PR
gh pr create --title "Add feature" --body "Description"

# View PRs
gh pr list

# Checkout PR
gh pr checkout 123

# Merge PR
gh pr merge 123
```

**[IMAGE: Terminal showing gh CLI commands]**  
*Description: Various gh commands with output*

### GitHub Codespaces

Cloud development environment:

1. Go to your repository
2. Click **Code** → **Codespaces**
3. Click **Create codespace on main**

**[IMAGE: Codespaces interface]**  
*Description: VS Code running in browser*

**Benefits:**
- ✅ Code from any device
- ✅ Pre-configured environment
- ✅ No local setup needed
- ✅ Powerful cloud resources

### GitHub Discussions

Community forum for your project:

1. Go to repository
2. **Settings** → **Features**
3. Enable **Discussions**

**[IMAGE: GitHub Discussions tab]**  
*Description: Forum-like interface with categories*

**Use cases:**
- Q&A
- Feature requests discussion
- Community announcements
- Show and tell

### GitHub Sponsors

Support open source developers:

**[IMAGE: GitHub Sponsors profile]**  
*Description: Sponsor tiers and donation options*

**Set up sponsorship:**
1. Join GitHub Sponsors program
2. Create `.github/FUNDING.yml`:
   ```yaml
   github: [sara-ahmed]
   patreon: sara_dev
   ko_fi: sara_ahmed
   custom: ["https://paypal.me/sara"]
   ```

### Gists - Code Snippets

Share code snippets:

1. Go to https://gist.github.com
2. Paste your code
3. Add description
4. Choose public or secret
5. Click **Create gist**

**[IMAGE: Gist creation interface]**  
*Description: Code editor with gist options*

**Embed gists:**
```html
<script src="https://gist.github.com/sara-ahmed/1234567890abcdef.js"></script>
```

---

## 🏆 Part 10: Building Your Developer Brand

### The Perfect Profile

Optimize your GitHub profile:

**Profile README** (create repo named after your username):

Create `sara-ahmed/README.md`:

```markdown
# Hi there! 👋 I'm Sara Ahmed

### 💻 Software Developer | 🎓 CS Student at FCAI

I'm passionate about building elegant solutions and contributing to open source!

---

### 🚀 About Me

- 🔭 Currently working on: **TechWave Authentication System**
- 🌱 Learning: **Docker, Kubernetes, and DevOps**
- 👯 Open to collaborate on: **Python and Web Development projects**
- 💬 Ask me about: **Git, Python, Web Development**
- 📫 Reach me: sara.ahmed@example.com
- ⚡ Fun fact: I made my first open source contribution in 2025!

---

### 🛠️ Tech Stack

![Python](https://img.shields.io/badge/-Python-3776AB?style=flat-square&logo=python&logoColor=white)
![JavaScript](https://img.shields.io/badge/-JavaScript-F7DF1E?style=flat-square&logo=javascript&logoColor=black)
![Git](https://img.shields.io/badge/-Git-F05032?style=flat-square&logo=git&logoColor=white)
![GitHub](https://img.shields.io/badge/-GitHub-181717?style=flat-square&logo=github)
![VS Code](https://img.shields.io/badge/-VS%20Code-007ACC?style=flat-square&logo=visual-studio-code)

---

### 📊 GitHub Stats

![Sara's GitHub stats](https://github-readme-stats.vercel.app/api?username=sara-ahmed&show_icons=true&theme=radical)

![Top Languages](https://github-readme-stats.vercel.app/api/top-langs/?username=sara-ahmed&layout=compact&theme=radical)

---

### 📝 Latest Blog Posts

<!-- BLOG-POST-LIST:START -->
- [My First Open Source Contribution](https://saraahmed.com/blog/first-contribution)
- [Understanding Git Branching Strategies](https://saraahmed.com/blog/git-branching)
- [Building a Portfolio with GitHub Pages](https://saraahmed.com/blog/github-pages)
<!-- BLOG-POST-LIST:END -->

---

### 🤝 Connect With Me

[![LinkedIn](https://img.shields.io/badge/-LinkedIn-0077B5?style=flat-square&logo=linkedin&logoColor=white)](https://linkedin.com/in/sara-ahmed)
[![Twitter](https://img.shields.io/badge/-Twitter-1DA1F2?style=flat-square&logo=twitter&logoColor=white)](https://twitter.com/sara_dev)
[![Portfolio](https://img.shields.io/badge/-Portfolio-000000?style=flat-square&logo=vercel&logoColor=white)](https://sara-ahmed.github.io)

---

⭐️ From [sara-ahmed](https://github.com/sara-ahmed)
```

**[IMAGE: Rendered profile README on GitHub]**  
*Description: Beautiful profile page with stats and badges*

### Pinned Repositories

Pin your best projects:

1. Go to your profile
2. Click **Customize your pins**
3. Select up to 6 repositories
4. Drag to reorder

**[IMAGE: Pinned repositories section]**  
*Description: Grid of 6 featured projects*

### Contribution Graph

Make your contribution graph impressive:

**[IMAGE: Active contribution graph]**  
*Description: Green squares showing daily contributions*

**Tips:**
- Commit regularly (not just on weekends)
- Contribute to open source
- Work on side projects
- Document your learning

### GitHub Achievements

Unlock badges:

- 🌟 **Quickdraw**: Close issue/PR within 5 minutes
- 🎖️ **Starstruck**: 16+ starred repository
- ⭐ **Pull Shark**: Merge 2+ pull requests
- 🐙 **Arctic Code Vault Contributor**: 2020 Archive Program

**[IMAGE: Achievement badges on profile]**  
*Description: Various achievement badges displayed*

---

## 📝 Part 11: Comprehensive Exercise - Full Contribution Workflow

### The Challenge: Contribute to a Real Project

**Task:** Find an open source project and make a meaningful contribution.

#### Phase 1: Find a Project (10 minutes)

1. Go to https://goodfirstissue.dev
2. Filter by language: Python
3. Find a project with recent activity
4. Read the README and CONTRIBUTING.md

#### Phase 2: Set Up (15 minutes)

1. Fork the repository
2. Clone your fork
3. Add upstream remote
4. Create a feature branch
5. Set up development environment

#### Phase 3: Make Contribution (30 minutes)

Choose ONE:
- **Fix a bug** from the issues
- **Add documentation** for unclear sections
- **Write tests** for untested code
- **Improve error messages**
- **Add examples** to README

#### Phase 4: Submit (15 minutes)

1. Commit with clear messages
2. Push to your fork
3. Create Pull Request
4. Fill out PR template completely
5. Respond to any immediate feedback

#### Phase 5: Portfolio Update (20 minutes)

1. Add the contribution to your portfolio
2. Write a blog post about the experience
3. Share on social media
4. Update your GitHub profile README

---

## 📚 Session Summary

### What You Accomplished

✅ **Forking**: Created your own copies of repositories  
✅ **Open Source**: Made contributions to real projects  
✅ **Upstream Sync**: Kept forks up-to-date  
✅ **GitHub Projects**: Managed work with project boards  
✅ **GitHub Actions**: Automated testing and deployment  
✅ **GitHub Pages**: Built and deployed websites  
✅ **Security**: Protected secrets and secured repositories  
✅ **Advanced Features**: Used CLI, Codespaces, Discussions  
✅ **Developer Brand**: Built impressive profile and portfolio  

### Essential Commands

```bash
# Forking workflow
git clone https://github.com/YOU/repo.git
git remote add upstream https://github.com/ORIGINAL/repo.git
git fetch upstream
git merge upstream/main

# Keeping fork updated
git pull upstream main
git push origin main

# Creating releases
git tag -a v1.0.0 -m "Release message"
git push origin v1.0.0

# Using GitHub CLI
gh repo fork
gh pr create
gh issue list
```

---

## 🎉 Success Stories

### Ahmed's Journey

Ahmed fixed that typo in `awesome-python-lib`. The maintainer was so impressed with his thorough approach that they asked him to help review other PRs. Within a month, Ahmed became a core contributor with merge privileges!

**His stats:**
- 📊 15 merged pull requests
- ⭐ 3 repositories starred by 100+ people
- 👥 Connected with developers worldwide
- 💼 Job offers from reviewing his GitHub activity

### Sara's Impact

Sara's portfolio site attracted attention from recruiters. Her combination of:
- Professional portfolio website
- Active open source contributions
- Well-documented projects
- Comprehensive GitHub profile

...led to interviews with 5 companies!

She's now a junior developer at a tech company, and she **teaches Git to new interns** using the skills from this workshop!

---

## 🌍 Join the Community

### What's Next?

**Continue your journey:**

1. **Contribute regularly**: Make open source contributions a habit
2. **Build in public**: Share your projects and learning
3. **Help others**: Answer questions, review PRs
4. **Stay updated**: Follow GitHub blog, changelog
5. **Network**: Connect with other developers

### Resources to Explore

**Open Source Guides:**
- Open Source Guides: https://opensource.guide
- First Timers Only: https://www.firsttimersonly.com
- How to Contribute: https://opensource.guide/how-to-contribute/

**GitHub Features:**
- GitHub Blog: https://github.blog
- GitHub Skills: https://skills.github.com
- GitHub Education: https://education.github.com

**Communities:**
- Dev.to: https://dev.to
- Hashnode: https://hashnode.com
- Reddit: r/opensource, r/github
- Discord servers for your favorite projects

---

## 🎓 Workshop Completion

### You've Mastered Git & GitHub!

From zero to hero in 4 sessions:

**Session 1:** First repository, basic Git, pushing to GitHub  
**Session 2:** Collaboration, pull requests, merge conflicts  
**Session 3:** Branching strategies, Git Flow, rebasing  
**Session 4:** Open source, automation, GitHub Pages, security  

**You can now:**
- ✅ Manage complex codebases with Git
- ✅ Collaborate professionally with teams
- ✅ Contribute to open source projects
- ✅ Automate workflows with GitHub Actions
- ✅ Build and host websites
- ✅ Secure your code and projects
- ✅ Build an impressive developer portfolio

---

## 🏅 Certificate of Completion

**Congratulations!** You've completed:

**Mastering Git & GitHub — From Zero to Hero**  
Organized by: DSC Cairo University Chapter  
Duration: 4 Sessions, 10.5 hours  
Instructors: Omar Betawy, Amr Khaled  

**Skills Acquired:**
- Version Control with Git
- GitHub Collaboration
- Open Source Contribution
- CI/CD with GitHub Actions
- Professional Development Workflows

---

## 🚀 Final Challenge

### Build Your Portfolio & Contribute

**Before you leave today:**

1. ✅ Create your GitHub profile README
2. ✅ Set up your portfolio website on GitHub Pages
3. ✅ Find and fork one open source project
4. ✅ Enable GitHub Actions on one of your projects
5. ✅ Connect with workshop attendees on GitHub

**Share your work:**
- Post your GitHub profile URL in the workshop Discord/WhatsApp
- Share your portfolio website
- Tag us when you make your first open source contribution!

---

## 🌟 Closing Thoughts

You're now part of the **global developer community**. Every day, millions of developers use Git and GitHub to:
- Build amazing software
- Solve world problems
- Learn and grow together
- Share knowledge freely

**Your journey doesn't end here** — it's just beginning!

Whether you build the next big startup, contribute to important open source projects, or help others learn, you now have the tools and knowledge to make an impact.

**Keep coding. Keep sharing. Keep learning.** 🚀

---

## 📞 Stay Connected

**Workshop Repository:** https://github.com/dsc-cairo/git-workshop-2025  
**Discord Community:** [Link to Discord]  
**Follow us:**
- GitHub: @dsc-cairo
- Instagram: @dsccairo
- LinkedIn: DSC Cairo University

**Instructors:**
- Omar Betawy: [@omarbetawy](https://github.com/omarbetawy)
- Amr Khaled: [@amrkhaled](https://github.com/amrkhaled)

---

**Thank you for joining us on this journey! Now go build something amazing! 💫**

**End of Session 4**

**End of Workshop**
