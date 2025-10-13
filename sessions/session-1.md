# Session 1 — The Beginning: First Assignment, First Repo

**Duration:** 2 hours 30 minutes  
**Venue:** Creativa Lab A  
**Instructors:** Omar Betawy, Amr Khaled

---

## 📖 Story Introduction

Meet **Sara**, a freshman computer science student at FCAI. She just received her first programming assignment: create a calculator program in Python. Excited but nervous, she starts coding on her laptop. After a few hours, she accidentally deletes a crucial function. Panic sets in — she has no backup! 

Her friend Ahmed notices her distress and introduces her to **Git** and **GitHub**. "This is how real developers work," he says. "You'll never lose your code again, and you'll look professional too!"

Today, you'll follow Sara's journey as she learns to manage her code like a pro.

---

## 🎯 Session Objectives

By the end of this session, you will be able to:
- Understand what version control is and why it's essential
- Install and configure Git on your computer
- Create your first local Git repository
- Make commits and track your code changes
- Create a GitHub account
- Push your code to GitHub
- Understand the `.gitignore` file

---

## 📚 Part 1: Understanding Version Control

### What is Version Control?

Version control is a system that records changes to files over time. Think of it like a time machine for your code — you can:
- Go back to any previous version
- See what changed and when
- Work with others without overwriting each other's work
- Keep your code safe in the cloud

**[IMAGE: Diagram showing code evolution over time with snapshots]**  
*Description: Visual timeline showing code versions with ability to travel back in time*

### Why Do Developers Use Git?

Git is the most popular version control system in the world. Here's why:

1. **Safety**: Never lose your work again
2. **History**: See every change you've ever made
3. **Collaboration**: Work with teammates seamlessly
4. **Professional**: Required skill for any developer job
5. **Free & Open Source**: Used by millions of developers

**[IMAGE: Logos of companies using Git - Google, Microsoft, Facebook, etc.]**  
*Description: Major tech companies that use Git daily*

### Git vs GitHub — What's the Difference?

- **Git**: The version control software that runs on your computer
- **GitHub**: A website that hosts your Git repositories online (like Google Drive for code)

**[IMAGE: Diagram showing Git (local) vs GitHub (cloud) relationship]**  
*Description: Computer with Git connecting to GitHub cloud*

---

## 💻 Part 2: Installing and Configuring Git

### Step 1: Check if Git is Already Installed

Open your terminal (Linux/Mac) or Command Prompt (Windows) and type:

```bash
git --version
```

**Expected Output:**
```
git version 2.40.0
```

If you see a version number, Git is installed! Skip to **Step 3: Configure Git**.

**[IMAGE: Terminal showing git --version command output]**  
*Description: Screenshot of successful git version check*

### Step 2: Install Git (If Not Installed)

#### For Windows:
1. Download Git from: https://git-scm.com/download/win
2. Run the installer
3. Use recommended settings (just click "Next")
4. **Important**: On "Adjusting your PATH environment" screen, select "Git from the command line and also from 3rd-party software"

**[IMAGE: Git Windows installer PATH selection screen]**  
*Description: Screenshot showing the PATH configuration option*

#### For Mac:
1. Install Homebrew first (if not installed): https://brew.sh
2. Run in terminal:
```bash
brew install git
```

#### For Linux:
```bash
# Ubuntu/Debian
sudo apt-get update
sudo apt-get install git

# Fedora
sudo dnf install git
```

**[IMAGE: Terminal showing successful Git installation]**  
*Description: Installation completion message*

### Step 3: Configure Git

Git needs to know who you are. Run these commands (replace with your info):

```bash
git config --global user.name "Sara Ahmed"
git config --global user.email "sara.ahmed@example.com"
```

**Important:** Use the email you'll use for GitHub!

**Verify your configuration:**
```bash
git config --list
```

**Expected Output:**
```
user.name=Sara Ahmed
user.email=sara.ahmed@example.com
```

**[IMAGE: Terminal showing git config commands and output]**  
*Description: Screenshot of configuration setup*

---

## 🚀 Part 3: Creating Your First Repository

### What is a Repository?

A **repository** (or "repo") is a folder that Git tracks. It contains:
- Your project files
- A hidden `.git` folder with all the version history
- Metadata about commits, branches, etc.

**[IMAGE: Folder structure showing project files and hidden .git folder]**  
*Description: File explorer showing .git folder (make hidden files visible)*

### Hands-On: Create Your First Project

#### Step 1: Create a Project Folder

```bash
# Navigate to where you want to create your project
cd ~/Documents

# Create a folder for your calculator project
mkdir my-calculator

# Enter the folder
cd my-calculator
```

**[IMAGE: Terminal showing folder creation and navigation]**  
*Description: Commands being executed in terminal*

#### Step 2: Initialize Git Repository

```bash
git init
```

**Expected Output:**
```
Initialized empty Git repository in /home/sara/Documents/my-calculator/.git/
```

🎉 Congratulations! You just created your first Git repository!

**[IMAGE: Terminal showing git init success message]**  
*Description: Successful repository initialization*

#### Step 3: Create Your Python Calculator

Create a file named `calculator.py`:

```bash
# Linux/Mac
nano calculator.py

# Or use VS Code, PyCharm, or any text editor
```

Add this code to `calculator.py`:

```python
# Simple Calculator
# Author: Sara Ahmed
# Date: February 2025

def add(a, b):
    """Add two numbers"""
    return a + b

def subtract(a, b):
    """Subtract two numbers"""
    return a - b

def multiply(a, b):
    """Multiply two numbers"""
    return a * b

def divide(a, b):
    """Divide two numbers"""
    if b == 0:
        return "Error: Cannot divide by zero"
    return a / b

def main():
    print("=== Simple Calculator ===")
    print("1. Add")
    print("2. Subtract")
    print("3. Multiply")
    print("4. Divide")
    
    choice = input("Enter choice (1-4): ")
    num1 = float(input("Enter first number: "))
    num2 = float(input("Enter second number: "))
    
    if choice == '1':
        print(f"Result: {add(num1, num2)}")
    elif choice == '2':
        print(f"Result: {subtract(num1, num2)}")
    elif choice == '3':
        print(f"Result: {multiply(num1, num2)}")
    elif choice == '4':
        print(f"Result: {divide(num1, num2)}")
    else:
        print("Invalid choice!")

if __name__ == "__main__":
    main()
```

Save the file.

**[IMAGE: Code editor showing calculator.py file]**  
*Description: Python code in a text editor*

---

## 📸 Part 4: Understanding Git Snapshots

### Key Git Concepts

Before we continue, understand these terms:

- **Working Directory**: Your project folder with files you can see and edit
- **Staging Area**: A "waiting room" where you prepare files for a commit
- **Repository**: Where Git stores all committed versions

**[IMAGE: Three-stage diagram: Working Directory → Staging Area → Repository]**  
*Description: Visual showing the three areas and how files move between them*

### The Git Workflow

1. **Modify** files in working directory
2. **Stage** changes (prepare them)
3. **Commit** changes (save snapshot)

---

## ✅ Part 5: Your First Commit

### Step 1: Check Repository Status

```bash
git status
```

**Expected Output:**
```
On branch main

No commits yet

Untracked files:
  (use "git add <file>..." to include in what will be committed)
        calculator.py

nothing added to commit but untracked files present (use "git add" to track)
```

**What this means:**
- Git sees your `calculator.py` file
- It's "untracked" — Git isn't watching it yet
- You need to use `git add` to start tracking it

**[IMAGE: Terminal showing git status with untracked files]**  
*Description: Red text showing untracked files*

### Step 2: Stage Your File

```bash
git add calculator.py
```

No output means success! Check status again:

```bash
git status
```

**Expected Output:**
```
On branch main

No commits yet

Changes to be committed:
  (use "git rm --cached <file>..." to unstage)
        new file:   calculator.py
```

Now the file is **staged** (ready to commit).

**[IMAGE: Terminal showing git status with staged files in green]**  
*Description: Green text showing files ready to commit*

### Step 3: Make Your First Commit

A **commit** is like taking a snapshot. Always include a descriptive message:

```bash
git commit -m "Add basic calculator with four operations"
```

**Expected Output:**
```
[main (root-commit) a1b2c3d] Add basic calculator with four operations
 1 file changed, 45 insertions(+)
 create mode 100644 calculator.py
```

🎉 **You made your first commit!**

**[IMAGE: Terminal showing successful commit with hash and statistics]**  
*Description: Commit confirmation message*

### Understanding the Commit Message

- `a1b2c3d`: Unique commit identifier (hash)
- `1 file changed`: Number of files modified
- `45 insertions(+)`: Lines of code added

### Step 4: View Your Commit History

```bash
git log
```

**Expected Output:**
```
commit a1b2c3d4e5f6g7h8i9j0k1l2m3n4o5p6q7r8s9t0
Author: Sara Ahmed <sara.ahmed@example.com>
Date:   Mon Feb 10 14:30:00 2025 +0200

    Add basic calculator with four operations
```

**[IMAGE: Terminal showing git log output with commit details]**  
*Description: Full commit history display*

For a prettier view:

```bash
git log --oneline
```

**Expected Output:**
```
a1b2c3d Add basic calculator with four operations
```

---

## 🔄 Part 6: Making More Changes

Let's see Git in action by making changes.

### Step 1: Add a New Feature

Add a power function to `calculator.py`:

```python
def power(a, b):
    """Raise a to the power of b"""
    return a ** b
```

And update the menu in `main()`:

```python
def main():
    print("=== Simple Calculator ===")
    print("1. Add")
    print("2. Subtract")
    print("3. Multiply")
    print("4. Divide")
    print("5. Power")  # NEW LINE
    
    choice = input("Enter choice (1-5): ")  # UPDATED
    # ... rest of code
    
    # Add this elif before the else:
    elif choice == '5':
        print(f"Result: {power(num1, num2)}")
```

### Step 2: Check What Changed

```bash
git status
```

**Expected Output:**
```
On branch main
Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git restore <file>..." to discard changes in working directory)
        modified:   calculator.py
```

**See the exact changes:**

```bash
git diff
```

**Expected Output:**
```diff
diff --git a/calculator.py b/calculator.py
index a1b2c3d..e5f6g7h 100644
--- a/calculator.py
+++ b/calculator.py
@@ -18,6 +18,10 @@ def divide(a, b):
         return "Error: Cannot divide by zero"
     return a / b
 
+def power(a, b):
+    """Raise a to the power of b"""
+    return a ** b
+
 def main():
     print("=== Simple Calculator ===")
     print("1. Add")
```

**[IMAGE: Terminal showing git diff output with green additions]**  
*Description: Diff showing added lines in green with + symbols*

### Step 3: Stage and Commit

```bash
git add calculator.py
git commit -m "Add power function to calculator"
```

### Step 4: View History

```bash
git log --oneline
```

**Expected Output:**
```
b2c3d4e Add power function to calculator
a1b2c3d Add basic calculator with four operations
```

**[IMAGE: Terminal showing git log with multiple commits]**  
*Description: Commit history with two commits*

---

## 🌐 Part 7: Creating a GitHub Account

### Step 1: Sign Up for GitHub

1. Go to: https://github.com
2. Click "Sign up"
3. Enter your email (use the same one from Git config!)
4. Create a strong password
5. Choose a username (this will be public)
6. Verify you're human
7. Click "Create account"

**[IMAGE: GitHub signup page]**  
*Description: Screenshot of GitHub registration form*

### Step 2: Verify Your Email

1. Check your email inbox
2. Click the verification link from GitHub
3. Complete the setup questionnaire (optional)

**[IMAGE: GitHub welcome screen after verification]**  
*Description: Post-verification dashboard*

---

## ☁️ Part 8: Pushing Your Code to GitHub

### Step 1: Create a New Repository on GitHub

1. Click the **"+"** icon in the top-right corner
2. Select **"New repository"**

**[IMAGE: GitHub new repository button location]**  
*Description: Screenshot highlighting the + icon and dropdown menu*

3. Fill in the details:
   - **Repository name:** `my-calculator`
   - **Description:** "A simple calculator program in Python"
   - **Visibility:** Public (or Private if you prefer)
   - **DO NOT** check "Add a README file" (we already have code)
   - **DO NOT** add .gitignore or license yet

4. Click **"Create repository"**

**[IMAGE: GitHub create repository form filled out]**  
*Description: Form with all fields completed as described*

### Step 2: Connect Your Local Repo to GitHub

After creating the repo, GitHub shows you commands. Copy the URL that looks like:
```
https://github.com/sara-ahmed/my-calculator.git
```

**[IMAGE: GitHub quick setup page with repository URL]**  
*Description: Page showing HTTPS URL and push commands*

In your terminal, run:

```bash
git remote add origin https://github.com/sara-ahmed/my-calculator.git
```

**What this does:**
- `git remote add`: Adds a connection to a remote server
- `origin`: A name for this connection (standard convention)
- URL: Where your GitHub repo lives

**Verify the connection:**

```bash
git remote -v
```

**Expected Output:**
```
origin  https://github.com/sara-ahmed/my-calculator.git (fetch)
origin  https://github.com/sara-ahmed/my-calculator.git (push)
```

**[IMAGE: Terminal showing git remote commands]**  
*Description: Remote configuration confirmation*

### Step 3: Push Your Code to GitHub

```bash
git branch -M main
git push -u origin main
```

**What these commands do:**
- `git branch -M main`: Renames your branch to "main" (GitHub's default)
- `git push`: Sends your commits to GitHub
- `-u origin main`: Sets "origin/main" as the default upstream branch

**You'll be prompted for credentials:**
- Username: Your GitHub username
- Password: Your GitHub **personal access token** (not your password!)

**[IMAGE: Terminal showing git push in progress]**  
*Description: Upload progress and success message*

### Step 4: Creating a Personal Access Token

GitHub no longer accepts passwords for Git operations. You need a token:

1. Go to: https://github.com/settings/tokens
2. Click "Generate new token" → "Generate new token (classic)"
3. Give it a name: "Git Access"
4. Set expiration: 90 days (or your preference)
5. Select scopes: Check **"repo"** (full control of private repositories)
6. Click "Generate token"
7. **COPY THE TOKEN IMMEDIATELY** (you won't see it again!)

**[IMAGE: GitHub Personal Access Token creation page]**  
*Description: Token generation form with repo scope selected*

**[IMAGE: Generated token with copy button]**  
*Description: Success page showing the token (blurred)*

Use this token as your password when pushing!

**Expected Push Output:**
```
Enumerating objects: 6, done.
Counting objects: 100% (6/6), done.
Delta compression using up to 8 threads
Compressing objects: 100% (4/4), done.
Writing objects: 100% (6/6), 1.23 KiB | 1.23 MiB/s, done.
Total 6 (delta 0), reused 0 (delta 0), pack-reused 0
To https://github.com/sara-ahmed/my-calculator.git
 * [new branch]      main -> main
Branch 'main' set up to track remote branch 'main' from 'origin'.
```

🎉 **Your code is now on GitHub!**

### Step 5: View Your Code on GitHub

1. Go to your repository URL: `https://github.com/sara-ahmed/my-calculator`
2. You should see your `calculator.py` file
3. Click on it to view the code
4. Check the commit history by clicking on the commit count

**[IMAGE: GitHub repository page showing files]**  
*Description: Main repo page with file list and commit history*

**[IMAGE: GitHub code view with syntax highlighting]**  
*Description: calculator.py displayed on GitHub with Python syntax highlighting*

---

## 📁 Part 9: Understanding .gitignore

### Why Do We Need .gitignore?

Not all files should be tracked by Git:
- **Compiled code** (`.pyc`, `.exe`)
- **Dependencies** (`node_modules/`, `venv/`)
- **Sensitive data** (`.env` files, API keys)
- **IDE files** (`.vscode/`, `.idea/`)
- **OS files** (`.DS_Store`, `Thumbs.db`)

### Creating a .gitignore File

Create a file named `.gitignore` in your project root:

```bash
# For Linux/Mac
touch .gitignore

# For Windows
type nul > .gitignore
```

Add these lines to `.gitignore`:

```
# Python
__pycache__/
*.py[cod]
*$py.class
*.so
.Python
venv/
env/
ENV/

# IDE
.vscode/
.idea/
*.swp
*.swo

# OS
.DS_Store
Thumbs.db

# Project specific
config.ini
*.log
```

**[IMAGE: .gitignore file in text editor]**  
*Description: Example .gitignore with comments*

### Testing .gitignore

Create a test file that should be ignored:

```bash
mkdir __pycache__
touch __pycache__/test.pyc
```

Check status:

```bash
git status
```

You should only see `.gitignore` as untracked, not the `__pycache__` folder!

**Commit the .gitignore:**

```bash
git add .gitignore
git commit -m "Add .gitignore for Python project"
git push
```

**[IMAGE: Terminal showing .gitignore working correctly]**  
*Description: git status showing __pycache__ is ignored*

---

## 🏗️ Part 10: Understanding Repository Structure

### What's Inside the .git Folder?

Your `.git` folder contains all version history. Never modify it manually!

**View it (Linux/Mac):**
```bash
ls -la .git
```

**View it (Windows):**
```bash
dir .git /a
```

**Contents:**
- `HEAD`: Points to current branch
- `config`: Repository configuration
- `objects/`: All commits, files, and trees
- `refs/`: Pointers to commits (branches, tags)

**[IMAGE: .git folder structure in file explorer]**  
*Description: Tree view showing .git subfolders*

### How Git Stores Data

Git uses a **snapshot model**, not a delta model:
- Each commit is a full snapshot of your project
- Unchanged files are stored as links to previous versions
- Very efficient due to compression

**[IMAGE: Diagram showing commit snapshots over time]**  
*Description: Visual showing three commits, each as a complete snapshot*

---

## 🎓 Part 11: Essential Git Commands Summary

### Basic Commands Cheat Sheet

| Command | Description |
|---------|-------------|
| `git init` | Initialize a new Git repository |
| `git status` | Show working tree status |
| `git add <file>` | Stage file for commit |
| `git add .` | Stage all changed files |
| `git commit -m "message"` | Commit staged changes |
| `git log` | Show commit history |
| `git log --oneline` | Show condensed history |
| `git diff` | Show unstaged changes |
| `git diff --staged` | Show staged changes |

### Remote Commands

| Command | Description |
|---------|-------------|
| `git remote add origin <url>` | Connect to remote repository |
| `git remote -v` | List remote connections |
| `git push -u origin main` | Push commits to GitHub |
| `git push` | Push commits (after first push) |

**[IMAGE: Infographic of Git workflow with command placement]**  
*Description: Visual flowchart showing when to use each command*

---

## 💡 Part 12: Best Practices

### Writing Good Commit Messages

**Bad examples:**
- ❌ "update"
- ❌ "fix bug"
- ❌ "changed stuff"

**Good examples:**
- ✅ "Add power function to calculator"
- ✅ "Fix division by zero error handling"
- ✅ "Update README with installation instructions"

**Rules:**
1. Use imperative mood ("Add feature" not "Added feature")
2. Keep first line under 50 characters
3. Be specific about what changed
4. Explain **why** if not obvious

### How Often Should You Commit?

**Commit when you:**
- Complete a feature
- Fix a bug
- Reach a working state
- Before taking a break

**Don't commit:**
- Broken code (unless it's a WIP branch)
- Every single line change
- Files that belong in .gitignore

### Repository Naming Conventions

**Good names:**
- `my-calculator`
- `todo-app`
- `weather-api`

**Avoid:**
- Spaces: ❌ "my calculator"
- Special characters: ❌ "my_calculator!"
- Vague names: ❌ "project1"

---

## 🎯 Hands-On Practice Exercise

### Your Challenge

Create a new project from scratch and push it to GitHub:

1. **Create a new Python program** (choose one):
   - Todo list application
   - Number guessing game
   - Temperature converter

2. **Initialize Git repository**
3. **Make at least 3 commits** showing progression
4. **Create .gitignore file**
5. **Create GitHub repository**
6. **Push your code**
7. **Add a README.md** describing your project

### README.md Template

Create `README.md`:

```markdown
# Project Name

Brief description of what your project does.

## Features

- Feature 1
- Feature 2
- Feature 3

## How to Run

```bash
python your_file.py
```

## Author

Your Name
```

**[IMAGE: Example GitHub repository with nice README]**  
*Description: Professional-looking repo with README rendered*

---

## 🔍 Troubleshooting Common Issues

### Issue 1: "git: command not found"

**Solution:** Git is not installed or not in PATH. Reinstall Git and restart terminal.

### Issue 2: Can't Push to GitHub (403 Error)

**Solution:** You're using your password instead of a personal access token. Generate a token and use it as password.

### Issue 3: "Updates were rejected"

**Solution:** Your local branch is behind the remote. Run:
```bash
git pull origin main
```
Then push again.

### Issue 4: Accidentally Committed Wrong File

**Solution:** Undo the last commit (keeps changes):
```bash
git reset --soft HEAD~1
```

### Issue 5: Want to Discard All Local Changes

**Solution:** 
```bash
git restore .
```

**[IMAGE: Common error messages with solutions flowchart]**  
*Description: Decision tree for solving frequent Git errors*

---

## 📝 Session Review Quiz

Test your understanding:

1. What's the difference between Git and GitHub?
2. What are the three stages of Git workflow?
3. What command stages a file for commit?
4. What's the purpose of a .gitignore file?
5. How do you view your commit history?
6. What's a commit hash?
7. Why do we use `git push -u origin main` the first time?
8. What should you never commit to a public repository?

**Answers:**
1. Git is local version control software; GitHub is a cloud hosting service
2. Working Directory → Staging Area → Repository
3. `git add <filename>`
4. To tell Git which files to ignore (like compiled code, dependencies)
5. `git log` or `git log --oneline`
6. A unique identifier for each commit
7. `-u` sets upstream tracking so future pushes don't need to specify remote/branch
8. Passwords, API keys, sensitive data, large binary files

---

## 🎉 Congratulations!

You've completed Session 1! You can now:
- ✅ Install and configure Git
- ✅ Create and manage local repositories
- ✅ Make commits and track changes
- ✅ Create a GitHub account
- ✅ Push code to GitHub
- ✅ Use .gitignore effectively

**Sara's Journey:** Sara can now safely work on her calculator project. She commits her changes regularly, and even when she accidentally deleted that function again, she simply ran `git restore calculator.py` and got it back instantly. Her professor was impressed when she shared her GitHub repository link in her submission!

---

## 📚 Additional Resources

### Documentation
- Official Git Documentation: https://git-scm.com/doc
- GitHub Guides: https://guides.github.com
- Git Cheat Sheet: https://education.github.com/git-cheat-sheet-education.pdf

### Interactive Learning
- Learn Git Branching: https://learngitbranching.js.org
- GitHub Skills: https://skills.github.com

### Videos
- Git & GitHub for Beginners (Traversy Media)
- Git Tutorial for Beginners (Programming with Mosh)

---

## 📅 Next Session Preview

**Session 2 — Team Chaos: Collaboration & Access Control**

Sara's professor assigns a team project. She needs to work with three other students on the same codebase. Chaos ensues when everyone tries to edit the same files! 

You'll learn:
- How to collaborate with others
- Managing permissions and access
- Handling merge conflicts
- Pull requests and code reviews

See you next week! 🚀

---

**End of Session 1**
