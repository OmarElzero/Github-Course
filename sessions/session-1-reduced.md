# Session 1 — Git & GitHub Basics (Quick Reference)

**Duration:** 20-30 minutes  
**Format:** Summary + Commands + Challenge

---

## Quick Summary

**Git** is version control software that tracks code changes on your computer. **GitHub** is a cloud platform that hosts your Git repositories online.

**Key Concepts:**
- Repository = folder Git tracks
- Commit = snapshot of your code
- Working Directory → Staging Area → Repository
- Remote = connection to GitHub

---

## Part 1: Installation & Setup

### Install Git

**Windows:** Download from https://git-scm.com/download/win  
**Mac:** `brew install git`  
**Linux:** `sudo apt-get install git`

**Verify installation:**
```bash
git --version
```

### Configure Git

```bash
# Set your identity
git config --global user.name "Your Name"
git config --global user.email "your.email@example.com"

# Verify
git config --list
```

---

## Part 2: Create Local Repository

### Initialize Repository

```bash
# Create project folder
mkdir my-project
cd my-project

# Initialize Git
git init
```

### Check Status

```bash
git status
```

---

## Part 3: Making Commits

### The Commit Workflow

```bash
# 1. Create/modify files
echo "print('Hello World')" > app.py

# 2. Check what changed
git status
git diff

# 3. Stage files
git add app.py
# Or stage all: git add .

# 4. Commit with message
git commit -m "Add hello world program"

# 5. View history
git log
git log --oneline
```

### Good Commit Messages

✅ "Add user authentication feature"  
✅ "Fix division by zero bug"  
✅ "Update README with setup instructions"  

❌ "update"  
❌ "fix stuff"  
❌ "changes"  

---

## Part 4: Working with GitHub

### Create GitHub Account

1. Go to https://github.com
2. Sign up with same email used in Git config
3. Verify your email

### Create Personal Access Token

1. Go to: https://github.com/settings/tokens
2. Click "Generate new token (classic)"
3. Select scopes: check **"repo"**
4. Copy token (save it securely!)
5. Use token as password when pushing

### Create Repository on GitHub

1. Click **"+"** → **"New repository"**
2. Name: `my-project`
3. Don't check "Add README" (you have code already)
4. Click **"Create repository"**

### Connect Local to GitHub

```bash
# Add remote connection
git remote add origin https://github.com/username/my-project.git

# Verify
git remote -v

# Push code
git branch -M main
git push -u origin main
```

**First push will ask for credentials:**
- Username: your GitHub username
- Password: your personal access token

---

## Part 5: .gitignore File

### Why .gitignore?

Tells Git which files to ignore:
- Compiled code (`.pyc`, `.exe`)
- Dependencies (`node_modules/`, `venv/`)
- Secrets (`.env`, API keys)
- IDE files (`.vscode/`, `.idea/`)

### Create .gitignore

```bash
# Create file
touch .gitignore

# Add patterns (example for Python)
echo "__pycache__/" >> .gitignore
echo "*.pyc" >> .gitignore
echo "venv/" >> .gitignore
echo ".env" >> .gitignore

# Commit it
git add .gitignore
git commit -m "Add .gitignore"
git push
```

---

## Complete Command Reference

```bash
# SETUP
git config --global user.name "Name"    # Set username
git config --global user.email "email"  # Set email
git config --list                       # View config

# REPOSITORY
git init                                # Initialize repository
git status                              # Check status
git log                                 # View history
git log --oneline                       # Compact history

# STAGING & COMMITTING
git add filename                        # Stage specific file
git add .                               # Stage all changes
git commit -m "message"                 # Commit staged changes
git diff                                # Show unstaged changes
git diff --staged                       # Show staged changes

# UNDOING
git restore filename                    # Discard working changes
git restore --staged filename           # Unstage file
git reset --soft HEAD~1                 # Undo last commit, keep staged

# REMOTE (GITHUB)
git remote add origin url               # Connect to GitHub
git remote -v                           # List remotes
git branch -M main                      # Rename branch to main
git push -u origin main                 # Push (first time)
git push                                # Push (after first time)
git pull                                # Download and merge updates

# VIEWING
git log --oneline --graph --all         # Visual commit history
```

---

## Comprehensive Challenge (30 minutes)

### Task: Create and Publish a Python Project

**Requirements:**

1. **Create a temperature converter** (or any Python program):
   - Celsius to Fahrenheit
   - Fahrenheit to Celsius
   - Kelvin conversions (bonus)

2. **Use proper Git workflow:**
   - Initialize repository
   - Make at least 3 commits showing progression:
     - Commit 1: Basic structure
     - Commit 2: Add conversion functions
     - Commit 3: Add user interface
   - Write good commit messages

3. **Add .gitignore:**
   - Include Python patterns
   - Include IDE folders

4. **Create README.md:**
   ```markdown
   # Temperature Converter
   
   Brief description of your program.
   
   ## Features
   - Feature 1
   - Feature 2
   
   ## How to Run
   ```bash
   python converter.py
   ```
   
   ## Author
   Your Name
   ```

5. **Push to GitHub:**
   - Create GitHub repository
   - Connect local to remote
   - Push all commits
   - Verify README displays on GitHub

**Success Criteria:**
- ✅ Repository has 3+ commits
- ✅ .gitignore file present
- ✅ README.md displays on GitHub
- ✅ Code runs without errors
- ✅ Good commit messages used

---

## Common Issues & Solutions

**Issue:** `git: command not found`  
**Solution:** Git not installed or not in PATH. Reinstall Git.

**Issue:** Can't push (403 error)  
**Solution:** Using password instead of token. Generate personal access token.

**Issue:** "Updates were rejected"  
**Solution:** Local branch behind remote. Run `git pull` then push.

**Issue:** Accidentally committed wrong file  
**Solution:** `git reset --soft HEAD~1` (undo commit, keep changes)

**Issue:** Want to discard all local changes  
**Solution:** `git restore .`

---

## Tips for Success

**Commit Often:**
- After completing a feature
- Before taking a break
- When code is in working state

**Write Clear Messages:**
- Use imperative mood ("Add" not "Added")
- Be specific about what changed
- Explain why if not obvious

**Use .gitignore:**
- Add it before first commit
- Never commit secrets or passwords
- Check `.gitignore` templates for your language

**Pull Before Push:**
- Always `git pull` before starting work
- Reduces merge conflicts
- Stays in sync with team

---

## What You've Learned

✅ Git installation and configuration  
✅ Creating and managing local repositories  
✅ Making commits and viewing history  
✅ Creating GitHub account and tokens  
✅ Pushing code to GitHub  
✅ Using .gitignore effectively  

**Next:** Session 2 covers branching and advanced Git workflows!

---

**End of Session 1 Quick Reference**
