# Session 2 — Team Chaos: Collaboration & Access Control

**Duration:** 3 hours  
**Venue:** Creativa Lab B  
**Instructors:** Omar Betawy, Amr Khaled

---

## 📖 Story Introduction

After acing her first assignment with Git and GitHub, **Sara** feels confident. But then Professor Hassan announces a new challenge: a **team project**. Sara is grouped with three other students: **Ahmed**, **Mona**, and **Youssef**.

Excited, they decide to build a **Student Management System** together. Ahmed shares his GitHub repository link, and everyone starts coding. But disaster strikes:

- **Youssef** overwrites Sara's code by accident
- **Mona** can't push her changes (permission denied!)
- **Ahmed** makes changes that break everyone's work
- When they try to combine their code, nothing works!

The team is in chaos. They need to learn how to collaborate properly using GitHub's collaboration features, permissions, and workflows. Today, you'll master these skills and save the project!

---

## 🎯 Session Objectives

By the end of this session, you will be able to:
- Understand different collaboration models on GitHub
- Add collaborators and manage permissions
- Clone remote repositories
- Pull and fetch changes from remote repositories
- Handle and resolve merge conflicts
- Create and review Pull Requests
- Use GitHub Issues for project management
- Write effective commit messages and documentation
- Understand forking vs collaboration

---

## 🤝 Part 1: GitHub Collaboration Models

### Model 1: Direct Collaboration (Shared Repository)

**Best for:** Small teams, trusted collaborators

**How it works:**
- One person owns the repository
- They add other team members as **collaborators**
- Everyone can push directly to the repository

**[IMAGE: Diagram showing multiple users with direct push access to one repo]**  
*Description: Central repository with arrows from multiple developers*

### Model 2: Fork & Pull Request (Open Source Model)

**Best for:** Open source projects, large teams

**How it works:**
- Contributors **fork** (copy) the repository to their account
- They make changes in their fork
- They submit a **Pull Request** to the original repo
- Owner reviews and merges

**[IMAGE: Diagram showing fork workflow with original repo and multiple forks]**  
*Description: Original repo at top, multiple forked copies, PRs going back*

We'll focus on **Model 1** today and cover **Model 2** in Session 4.

---

## 🔐 Part 2: Understanding Roles & Permissions

GitHub has different permission levels for repository access:

### Permission Levels Explained

| Role | Read | Clone | Issues/PRs | Push to Repo | Settings | Delete Repo |
|------|------|-------|------------|--------------|----------|-------------|
| **Read** | ✅ | ✅ | Comment | ❌ | ❌ | ❌ |
| **Triage** | ✅ | ✅ | Manage | ❌ | ❌ | ❌ |
| **Write** | ✅ | ✅ | Manage | ✅ | ❌ | ❌ |
| **Maintain** | ✅ | ✅ | Manage | ✅ | Some | ❌ |
| **Admin** | ✅ | ✅ | Manage | ✅ | ✅ | ✅ |

**Most common for team projects:** **Write** permission (can push code)

**[IMAGE: GitHub permissions hierarchy pyramid]**  
*Description: Pyramid showing permission levels from Read at bottom to Admin at top*

### Real-World Examples

- **Read Access**: External reviewers, clients who want to see code
- **Write Access**: Team members actively coding
- **Admin Access**: Project lead, repository owner

---

## 👥 Part 3: Adding Collaborators to Your Repository

### Step 1: Create a Team Repository

One team member (let's say **Ahmed**) creates the repository:

1. Go to GitHub and create a new repository
2. Name it: `student-management-system`
3. Description: "Team project for managing student records"
4. Make it **Public** or **Private** (Private is better for team work)
5. Check "Add a README file"
6. Choose Python for .gitignore
7. Click "Create repository"

**[IMAGE: GitHub create repository form with team project settings]**  
*Description: Form filled out as described above*

### Step 2: Add Team Members as Collaborators

Ahmed needs to give access to Sara, Mona, and Youssef:

1. Go to repository **Settings** tab
2. Click **Collaborators** in left sidebar
3. Click **Add people**
4. Search by username or email
5. Select the person
6. Choose permission level: **Write**
7. Click **Add [username] to this repository**

**[IMAGE: GitHub Settings > Collaborators page]**  
*Description: Screenshot showing Add people button*

**[IMAGE: Add collaborator dialog with Write permission selected]**  
*Description: Dialog showing user search and permission dropdown*

### Step 3: Accept Collaboration Invitation

Sara, Mona, and Youssef will receive:
- Email notification
- Notification on GitHub

They must:
1. Check their GitHub notifications (bell icon)
2. Click on the invitation
3. Click **Accept invitation**

**[IMAGE: GitHub invitation notification]**  
*Description: Email showing collaboration invitation*

**[IMAGE: Accept invitation page on GitHub]**  
*Description: Green Accept invitation button*

Now everyone has **Write** access! 🎉

---

## 📥 Part 4: Cloning a Remote Repository

### What is Cloning?

**Cloning** creates a complete local copy of a remote repository, including:
- All files
- Complete history
- All branches
- Remote connection configured

**[IMAGE: Diagram showing GitHub repo being copied to local computer]**  
*Description: Cloud repo with arrow pointing to local computer folder*

### Hands-On: Clone the Team Repository

Sara needs to get a copy of Ahmed's repository:

#### Step 1: Get the Repository URL

1. Go to the repository page on GitHub
2. Click the green **Code** button
3. Make sure **HTTPS** is selected
4. Copy the URL (looks like: `https://github.com/ahmed/student-management-system.git`)

**[IMAGE: GitHub Code button dropdown showing HTTPS URL]**  
*Description: Dropdown with URL and copy button highlighted*

#### Step 2: Clone the Repository

Open terminal and navigate to where you want the project:

```bash
cd ~/Documents/Projects
```

Clone the repository:

```bash
git clone https://github.com/ahmed/student-management-system.git
```

**Expected Output:**
```
Cloning into 'student-management-system'...
remote: Enumerating objects: 5, done.
remote: Counting objects: 100% (5/5), done.
remote: Compressing objects: 100% (4/4), done.
remote: Total 5 (delta 0), reused 0 (delta 0), pack-reused 0
Receiving objects: 100% (5/5), done.
```

**[IMAGE: Terminal showing successful clone operation]**  
*Description: Clone progress and completion message*

#### Step 3: Navigate into the Repository

```bash
cd student-management-system
```

Check what's inside:

```bash
ls -la
```

You should see:
- `README.md`
- `.git/` folder
- `.gitignore`

#### Step 4: Verify Remote Connection

```bash
git remote -v
```

**Expected Output:**
```
origin  https://github.com/ahmed/student-management-system.git (fetch)
origin  https://github.com/ahmed/student-management-system.git (push)
```

The remote connection is automatically set up! ✅

**[IMAGE: Terminal showing remote configuration]**  
*Description: git remote -v output*

---

## 💻 Part 5: Working Together - First Collaboration

### The Scenario

The team divides the work:
- **Ahmed**: Creates the main menu (main.py)
- **Sara**: Creates student class (student.py)
- **Mona**: Creates database functions (database.py)
- **Youssef**: Updates README with instructions

Let's follow Sara's workflow:

### Step 1: Sara Creates student.py

```python
# student.py
# Author: Sara Ahmed
# Description: Student class definition

class Student:
    """Represents a student in the management system"""
    
    def __init__(self, student_id, name, email, major):
        self.student_id = student_id
        self.name = name
        self.email = email
        self.major = major
        self.courses = []
    
    def enroll_course(self, course_name):
        """Enroll student in a course"""
        if course_name not in self.courses:
            self.courses.append(course_name)
            return True
        return False
    
    def drop_course(self, course_name):
        """Drop a course"""
        if course_name in self.courses:
            self.courses.remove(course_name)
            return True
        return False
    
    def get_info(self):
        """Return student information as a dictionary"""
        return {
            'id': self.student_id,
            'name': self.name,
            'email': self.email,
            'major': self.major,
            'courses': self.courses
        }
    
    def __str__(self):
        return f"Student({self.student_id}, {self.name}, {self.major})"
    
    def __repr__(self):
        return self.__str__()
```

**[IMAGE: Code editor showing student.py]**  
*Description: Python class code in editor*

### Step 2: Sara Commits Her Changes

```bash
# Check status
git status

# Stage the file
git add student.py

# Commit with descriptive message
git commit -m "Add Student class with enroll and drop methods"
```

**[IMAGE: Terminal showing add and commit commands]**  
*Description: Successful commit message*

### Step 3: Sara Pushes to GitHub

```bash
git push origin main
```

**Expected Output:**
```
Enumerating objects: 4, done.
Counting objects: 100% (4/4), done.
Delta compression using up to 8 threads
Compressing objects: 100% (3/3), done.
Writing objects: 100% (3/3), 1.05 KiB | 1.05 MiB/s, done.
Total 3 (delta 0), reused 0 (delta 0), pack-reused 0
To https://github.com/ahmed/student-management-system.git
   a1b2c3d..e4f5g6h  main -> main
```

✅ **Sara's code is now on GitHub!**

**[IMAGE: GitHub repository showing new student.py file]**  
*Description: File list with student.py added*

---

## 🔄 Part 6: Pull, Fetch, and Merge

### The Problem

While Sara was working, **Ahmed** already pushed `main.py`. Sara's local repository is now **behind** the remote.

**[IMAGE: Diagram showing local repo behind remote repo]**  
*Description: Timeline showing diverged commits*

### Understanding Pull vs Fetch

**`git fetch`**: Downloads changes but doesn't merge them
- Safe, lets you review changes first
- Updates `origin/main` but not your local `main`

**`git pull`**: Downloads changes AND merges them automatically
- Convenient but can cause unexpected merges
- Equivalent to: `git fetch` + `git merge`

**[IMAGE: Diagram comparing fetch vs pull operations]**  
*Description: Two flowcharts showing fetch+merge vs pull*

### Step 1: Fetch Changes

Ahmed pushed new code. Mona wants to get it:

```bash
git fetch origin
```

**Expected Output:**
```
remote: Enumerating objects: 5, done.
remote: Counting objects: 100% (5/5), done.
remote: Compressing objects: 100% (3/3), done.
remote: Total 3 (delta 1), reused 0 (delta 0), pack-reused 0
Unpacking objects: 100% (3/3), done.
From https://github.com/ahmed/student-management-system
   e4f5g6h..i7j8k9l  main       -> origin/main
```

**Check what changed:**

```bash
git log main..origin/main
```

This shows commits on `origin/main` that aren't in your local `main`.

**[IMAGE: Terminal showing fetch and log output]**  
*Description: New commits from remote*

### Step 2: Merge the Changes

```bash
git merge origin/main
```

**Expected Output:**
```
Updating e4f5g6h..i7j8k9l
Fast-forward
 main.py | 45 +++++++++++++++++++++++++++++++++++++++++++++
 1 file changed, 45 insertions(+)
 create mode 100644 main.py
```

Now Mona has Ahmed's code! ✅

### Step 3: Using Pull (Shortcut)

Instead of fetch + merge, you can use:

```bash
git pull origin main
```

This does both operations at once.

**[IMAGE: Terminal showing git pull output]**  
*Description: Pull downloading and merging changes*

### Best Practice

Before pushing your work, always pull first:

```bash
# 1. Pull latest changes
git pull origin main

# 2. Resolve any conflicts (if they occur)

# 3. Push your work
git push origin main
```

---

## ⚔️ Part 7: Handling Merge Conflicts

### What is a Merge Conflict?

A conflict occurs when:
- Two people edit the **same lines** in the **same file**
- Git can't automatically decide which change to keep

**[IMAGE: Diagram showing two developers editing same file lines]**  
*Description: Split screen showing conflicting edits*

### The Scenario: Sara and Youssef Conflict

Both Sara and Youssef edit `README.md` at the same time:

**Sara's change:**
```markdown
## Team Members
- Ahmed Hassan (Team Lead)
- Sara Ahmed (Backend Developer)
```

**Youssef's change:**
```markdown
## Team Members
- Ahmed Hassan
- Youssef Ali (Database Specialist)
```

Sara pushes first. When Youssef tries to push:

```bash
git push origin main
```

**Error:**
```
To https://github.com/ahmed/student-management-system.git
 ! [rejected]        main -> main (fetch first)
error: failed to push some refs to 'https://github.com/ahmed/student-management-system.git'
hint: Updates were rejected because the remote contains work that you do
hint: not have locally. This is usually caused by another repository pushing
hint: to the same ref. You may want to first integrate the remote changes
hint: (e.g., 'git pull ...') before pushing again.
```

**[IMAGE: Terminal showing push rejection error]**  
*Description: Error message with hints*

### Step 1: Pull the Latest Changes

```bash
git pull origin main
```

**Output:**
```
Auto-merging README.md
CONFLICT (content): Merge conflict in README.md
Automatic merge failed; fix conflicts and then commit the result.
```

🚨 **Conflict detected!**

**[IMAGE: Terminal showing merge conflict message]**  
*Description: Red warning about conflict*

### Step 2: View the Conflict

Open `README.md`:

```markdown
## Team Members
<<<<<<< HEAD
- Ahmed Hassan
- Youssef Ali (Database Specialist)
=======
- Ahmed Hassan (Team Lead)
- Sara Ahmed (Backend Developer)
>>>>>>> e4f5g6h1i2j3k4l5m6n7o8p9q0r1s2t3u4v5w6x7
```

**Understanding conflict markers:**
- `<<<<<<< HEAD`: Your local changes start here
- `=======`: Separator between changes
- `>>>>>>> [commit hash]`: Remote changes end here

**[IMAGE: Code editor showing conflict markers with annotations]**  
*Description: README with conflict markers highlighted and labeled*

### Step 3: Resolve the Conflict Manually

Edit the file to include BOTH changes:

```markdown
## Team Members
- Ahmed Hassan (Team Lead)
- Sara Ahmed (Backend Developer)
- Youssef Ali (Database Specialist)
```

Remove the conflict markers completely!

**[IMAGE: Code editor showing resolved conflict]**  
*Description: Clean file with both changes integrated*

### Step 4: Mark as Resolved

```bash
# Stage the resolved file
git add README.md

# Check status
git status
```

**Output:**
```
On branch main
All conflicts fixed but you are still merging.
  (use "git commit" to conclude merge)

Changes to be committed:
        modified:   README.md
```

### Step 5: Complete the Merge

```bash
git commit -m "Merge remote changes and resolve team members conflict"
```

**No need for a commit message actually** — Git will provide a default merge message. Just run:

```bash
git commit
```

This opens an editor with:
```
Merge branch 'main' of https://github.com/ahmed/student-management-system

# Conflicts:
#       README.md
```

Save and close (`:wq` in vim, or `Ctrl+X` in nano).

**[IMAGE: Terminal showing successful merge commit]**  
*Description: Merge commit completion message*

### Step 6: Push the Resolved Changes

```bash
git push origin main
```

✅ **Conflict resolved and pushed!**

**[IMAGE: GitHub network graph showing merge commit]**  
*Description: Visualization of branching and merging*

---

## 🔀 Part 8: Pull Requests (PRs) - The Professional Way

### What is a Pull Request?

A **Pull Request** is a GitHub feature that:
- Lets you propose changes before merging
- Allows team review and discussion
- Shows exactly what changed
- Enables approval workflow

**Even in direct collaboration**, PRs are best practice!

**[IMAGE: Pull Request workflow diagram]**  
*Description: Feature branch → PR → Review → Merge to main*

### Why Use Pull Requests?

✅ **Code Review**: Team can check your code before merging  
✅ **Discussion**: Comment on specific lines  
✅ **Quality Control**: Catch bugs early  
✅ **Documentation**: History of why changes were made  
✅ **CI/CD Integration**: Automated tests run before merge  

### Workflow with Pull Requests

Instead of pushing directly to `main`, use branches:

```
main branch (protected)
    ↑
    | Pull Request (review & merge)
    |
feature branch (your work)
```

### Step 1: Create a Feature Branch

Mona wants to add database functionality. Instead of working on `main`:

```bash
# Create and switch to new branch
git checkout -b add-database-functions
```

**Expected Output:**
```
Switched to a new branch 'add-database-functions'
```

**[IMAGE: Terminal showing branch creation]**  
*Description: Branch creation and switch confirmation*

### Step 2: Make Changes and Commit

Mona creates `database.py`:

```python
# database.py
# Author: Mona Ibrahim
# Description: Database operations for student management

import json
import os

DATABASE_FILE = 'students.json'

def load_students():
    """Load students from JSON file"""
    if not os.path.exists(DATABASE_FILE):
        return []
    
    with open(DATABASE_FILE, 'r') as f:
        return json.load(f)

def save_students(students):
    """Save students to JSON file"""
    with open(DATABASE_FILE, 'w') as f:
        json.dump(students, f, indent=4)
    return True

def add_student(student_data):
    """Add a new student to database"""
    students = load_students()
    students.append(student_data)
    save_students(students)
    return True

def get_student_by_id(student_id):
    """Retrieve student by ID"""
    students = load_students()
    for student in students:
        if student['id'] == student_id:
            return student
    return None

def update_student(student_id, updated_data):
    """Update student information"""
    students = load_students()
    for i, student in enumerate(students):
        if student['id'] == student_id:
            students[i] = updated_data
            save_students(students)
            return True
    return False

def delete_student(student_id):
    """Delete student from database"""
    students = load_students()
    students = [s for s in students if s['id'] != student_id]
    save_students(students)
    return True
```

Commit the changes:

```bash
git add database.py
git commit -m "Add database functions for student CRUD operations"
```

**[IMAGE: Code editor showing database.py]**  
*Description: Database functions code*

### Step 3: Push the Branch to GitHub

```bash
git push origin add-database-functions
```

**Expected Output:**
```
Enumerating objects: 4, done.
Counting objects: 100% (4/4), done.
Delta compression using up to 8 threads
Compressing objects: 100% (3/3), done.
Writing objects: 100% (3/3), 1.23 KiB | 1.23 MiB/s, done.
Total 3 (delta 1), reused 0 (delta 0), pack-reused 0
remote: 
remote: Create a pull request for 'add-database-functions' on GitHub by visiting:
remote:      https://github.com/ahmed/student-management-system/pull/new/add-database-functions
remote: 
To https://github.com/ahmed/student-management-system.git
 * [new branch]      add-database-functions -> add-database-functions
```

**[IMAGE: Terminal showing branch push with PR link]**  
*Description: Push output with helpful PR creation link*

### Step 4: Create Pull Request on GitHub

1. Go to the repository on GitHub
2. You'll see a banner: **"add-database-functions had recent pushes"**
3. Click **"Compare & pull request"**

**[IMAGE: GitHub banner prompting to create PR]**  
*Description: Yellow banner with Compare & pull request button*

4. Fill in the PR form:
   - **Title**: "Add database CRUD operations"
   - **Description**:
     ```markdown
     ## Changes
     - Added database.py with CRUD functions
     - Uses JSON for data persistence
     - Functions for add, get, update, delete students
     
     ## Testing
     - Tested all functions manually
     - No dependencies required (uses standard library)
     
     ## Related Issue
     Closes #3
     ```

5. Assign reviewers (select team members)
6. Click **"Create pull request"**

**[IMAGE: GitHub PR creation form]**  
*Description: Form with title, description, and reviewers*

**[IMAGE: Created PR page]**  
*Description: PR showing files changed, commits, and conversation*

### Step 5: Code Review

Ahmed reviews Mona's PR:

**View the changes:**
- Click **"Files changed"** tab
- See side-by-side diff

**[IMAGE: GitHub Files changed tab showing diff]**  
*Description: Green additions highlighted in code*

**Add comments:**
- Hover over a line number
- Click the **+** icon
- Write a comment
- Click **"Start a review"**

**[IMAGE: Inline comment on code]**  
*Description: Comment bubble on specific line*

Ahmed adds a comment:
```
Good work! Consider adding error handling for file operations.
Could you add try-except blocks?
```

**Submit review:**
- Click **"Review changes"**
- Select: **Comment**, **Approve**, or **Request changes**
- Add summary
- Click **"Submit review"**

**[IMAGE: Review submission dialog]**  
*Description: Three review options with summary textbox*

### Step 6: Address Review Comments

Mona updates the code:

```python
def save_students(students):
    """Save students to JSON file"""
    try:
        with open(DATABASE_FILE, 'w') as f:
            json.dump(students, f, indent=4)
        return True
    except IOError as e:
        print(f"Error saving students: {e}")
        return False
```

Commit and push:

```bash
git add database.py
git commit -m "Add error handling to save_students function"
git push origin add-database-functions
```

The PR automatically updates! ✨

**[IMAGE: PR showing new commit added]**  
*Description: Timeline showing additional commit*

### Step 7: Merge the Pull Request

After approval, Ahmed merges:

1. Click **"Merge pull request"**
2. Choose merge method (usually "Create a merge commit")
3. Click **"Confirm merge"**
4. Optionally: **"Delete branch"** (cleanup)

**[IMAGE: Merge pull request button]**  
*Description: Green merge button with merge method dropdown*

**[IMAGE: Merged PR confirmation with purple badge]**  
*Description: Merged status with option to delete branch*

✅ **The code is now in main branch!**

### Step 8: Update Local Repository

Everyone else needs to pull the changes:

```bash
# Switch to main branch
git checkout main

# Pull the merged changes
git pull origin main

# Delete local feature branch (optional cleanup)
git branch -d add-database-functions
```

**[IMAGE: Terminal showing branch switch and pull]**  
*Description: Successful pull with fast-forward merge*

---

## 📋 Part 9: GitHub Issues - Project Management

### What are Issues?

GitHub Issues are like a to-do list for your project:
- Track bugs
- Plan features
- Discuss ideas
- Assign tasks to team members

**[IMAGE: GitHub Issues tab showing list of issues]**  
*Description: Issues page with open and closed issues*

### Creating an Issue

1. Go to **Issues** tab
2. Click **"New issue"**
3. Fill in:
   - **Title**: "Add input validation for student email"
   - **Description**:
     ```markdown
     ## Description
     Currently, the system accepts any string as email.
     We need to validate email format.
     
     ## Acceptance Criteria
     - Email must contain @
     - Email must have domain
     - Reject invalid formats
     
     ## Additional Context
     Use regex or email validation library
     ```

4. Assign to: Youssef
5. Labels: `enhancement`, `priority: medium`
6. Click **"Submit new issue"**

**[IMAGE: New issue creation form]**  
*Description: Form with all fields filled*

**[IMAGE: Created issue with #2 number]**  
*Description: Issue page showing full details*

### Issue Labels

Common labels:
- `bug` 🐛: Something isn't working
- `enhancement` ✨: New feature
- `documentation` 📝: Documentation improvements
- `help wanted` 🙋: Need assistance
- `good first issue` 🌱: Easy for beginners

**[IMAGE: GitHub labels list]**  
*Description: Colorful labels with descriptions*

### Closing Issues with Commits

Link issues to commits using keywords:

```bash
git commit -m "Add email validation, closes #2"
```

When this commit is merged, issue #2 automatically closes!

**Magic keywords:**
- `closes #2`
- `fixes #2`
- `resolves #2`

**[IMAGE: Commit message linked to issue]**  
*Description: Issue showing linked commit*

### Using Issue Templates

Create `.github/ISSUE_TEMPLATE/bug_report.md`:

```markdown
---
name: Bug Report
about: Create a report to help us improve
title: '[BUG] '
labels: bug
assignees: ''
---

## Bug Description
A clear description of what the bug is.

## Steps to Reproduce
1. Go to '...'
2. Click on '...'
3. See error

## Expected Behavior
What you expected to happen.

## Screenshots
If applicable, add screenshots.

## Environment
- OS: [e.g. Windows 10]
- Python Version: [e.g. 3.9.0]
```

**[IMAGE: New issue button showing template options]**  
*Description: Dropdown with Bug Report and Feature Request templates*

---

## ✍️ Part 10: Commit Message Best Practices

### The Seven Rules

1. **Separate subject from body with a blank line**
2. **Limit the subject line to 50 characters**
3. **Capitalize the subject line**
4. **Do not end the subject line with a period**
5. **Use the imperative mood** ("Add feature" not "Added feature")
6. **Wrap the body at 72 characters**
7. **Use the body to explain what and why, not how**

### Example: Bad Commit Message

```
fixed stuff
```

❌ Vague, no context, lowercase

### Example: Good Commit Message

```
Add email validation to Student class

The system was accepting invalid email addresses, causing issues
with notifications. This commit adds regex validation to ensure
emails follow proper format before accepting them.

Closes #2
```

✅ Clear, detailed, references issue

**[IMAGE: Comparison of bad vs good commit messages]**  
*Description: Side-by-side showing the difference*

### Commit Message Template

Create `.gitmessage` in your home directory:

```
# <type>: <subject> (Max 50 characters)

# <body> (Wrap at 72 characters)

# <footer>
# BREAKING CHANGE: description
# Closes #issue-number

# Type can be:
#   feat     (new feature)
#   fix      (bug fix)
#   docs     (documentation)
#   style    (formatting, missing semi colons, etc)
#   refactor (refactoring code)
#   test     (adding tests)
#   chore    (maintain)
```

Configure Git to use it:

```bash
git config --global commit.template ~/.gitmessage
```

---

## 📝 Part 11: Markdown for README Files

### Why Markdown Matters

Your `README.md` is the first thing people see. Make it professional!

**[IMAGE: Example of well-formatted README on GitHub]**  
*Description: Professional README with sections and badges*

### Essential Markdown Syntax

```markdown
# Heading 1
## Heading 2
### Heading 3

**Bold text**
*Italic text*
~~Strikethrough~~

- Bullet point
- Another bullet
  - Nested bullet

1. Numbered list
2. Second item

[Link text](https://example.com)

![Image alt text](image-url.png)

`inline code`

​```python
# Code block
def hello():
    print("Hello, World!")
​```

> Blockquote for important notes

| Column 1 | Column 2 |
|----------|----------|
| Data 1   | Data 2   |

---

Horizontal rule above
```

**[IMAGE: Split screen showing Markdown syntax and rendered output]**  
*Description: Left side code, right side how it looks*

### Professional README Template

```markdown
# Student Management System

![Python Version](https://img.shields.io/badge/python-3.8+-blue.svg)
![License](https://img.shields.io/badge/license-MIT-green.svg)

A simple yet effective student management system built with Python.

## Features

- ✅ Add, update, and delete student records
- ✅ Enroll students in courses
- ✅ JSON-based data persistence
- ✅ Email validation
- ✅ Search functionality

## Installation

​```bash
# Clone the repository
git clone https://github.com/ahmed/student-management-system.git

# Navigate to directory
cd student-management-system

# Run the application
python main.py
​```

## Usage

​```python
from student import Student

# Create a student
student = Student(1, "Sara Ahmed", "sara@example.com", "CS")

# Enroll in course
student.enroll_course("Data Structures")

# Get student info
print(student.get_info())
​```

## Project Structure

​```
student-management-system/
│
├── main.py              # Main application
├── student.py           # Student class
├── database.py          # Database operations
├── README.md            # Documentation
└── students.json        # Data file
​```

## Contributing

1. Fork the repository
2. Create feature branch (`git checkout -b feature/AmazingFeature`)
3. Commit changes (`git commit -m 'Add some AmazingFeature'`)
4. Push to branch (`git push origin feature/AmazingFeature`)
5. Open a Pull Request

## Team Members

- Ahmed Hassan - Team Lead
- Sara Ahmed - Backend Developer
- Mona Ibrahim - Database Specialist
- Youssef Ali - Frontend Developer

## License

This project is licensed under the MIT License - see the LICENSE file for details.

## Acknowledgments

- Thanks to DSC Cairo University for the workshop
- Inspired by real-world student management needs
```

**[IMAGE: Rendered professional README]**  
*Description: Beautiful formatted README with all sections*

---

## 🎯 Hands-On Team Exercise

### Challenge: Build a Feature Together

**Task**: As a team, add a "Course" class to the project using proper collaboration workflow.

#### Requirements:

1. **Plan**: Create an issue for the feature
2. **Branch**: Each team member creates a branch
3. **Develop**: Write the Course class
4. **Review**: Create Pull Request
5. **Merge**: After approval, merge to main

#### Course Class Specifications:

```python
class Course:
    - course_id
    - course_name
    - instructor
    - credits
    - enrolled_students (list)
    
    Methods:
    - add_student(student_id)
    - remove_student(student_id)
    - get_enrollment_count()
    - get_course_info()
```

#### Steps:

**Step 1**: Create issue "Add Course class"  
**Step 2**: Assign to a team member  
**Step 3**: Create branch `feature/add-course-class`  
**Step 4**: Implement the class  
**Step 5**: Push and create PR  
**Step 6**: Team reviews and approves  
**Step 7**: Merge to main  
**Step 8**: Everyone pulls latest changes  

---

## 🔧 Part 12: Collaboration Troubleshooting

### Problem 1: Permission Denied When Pushing

**Error:**
```
remote: Permission to ahmed/student-management-system.git denied to sara-ahmed.
fatal: unable to access 'https://github.com/ahmed/student-management-system.git/':
The requested URL returned error: 403
```

**Solutions:**
1. Check if you're added as collaborator (and accepted invitation)
2. Verify you're logged in with correct account
3. Use personal access token, not password
4. Check if repository is your fork (push to your fork, then PR)

### Problem 2: Diverged Branches

**Error:**
```
Your branch and 'origin/main' have diverged,
and have 3 and 2 different commits each, respectively.
```

**Solution:**
```bash
# Option 1: Pull and merge
git pull origin main

# Option 2: Pull and rebase (cleaner history)
git pull --rebase origin main
```

### Problem 3: Accidental Commit to Main

**You wanted to use a branch but committed to main:**

```bash
# Create branch from current state
git branch feature/my-feature

# Reset main to remote state
git reset --hard origin/main

# Switch to feature branch
git checkout feature/my-feature
```

### Problem 4: Need to Undo a Push

**Already pushed bad code:**

```bash
# Revert the commit (creates new commit that undoes it)
git revert HEAD

# Push the revert
git push origin main
```

**Never use `git push --force` on shared branches!** ⚠️

**[IMAGE: Flowchart for common Git problems and solutions]**  
*Description: Decision tree for troubleshooting*

---

## 📊 Part 13: Visualizing Collaboration

### Git Network Graph

GitHub shows visual history:
1. Go to **Insights** tab
2. Click **Network** in left sidebar

**[IMAGE: GitHub network graph showing branches and merges]**  
*Description: Visual representation of repository history*

### Contributors Page

See who contributed what:
1. Go to **Insights** tab
2. Click **Contributors**

Shows:
- Number of commits per person
- Lines added/deleted
- Contribution timeline

**[IMAGE: GitHub contributors graph]**  
*Description: Bar chart showing team contributions*

---

## 📚 Session Summary

### What You Learned

✅ **Collaboration Models**: Direct vs Fork & Pull Request  
✅ **Permissions**: Understanding GitHub roles  
✅ **Adding Collaborators**: Managing team access  
✅ **Cloning**: Getting a copy of remote repository  
✅ **Pull & Fetch**: Staying in sync with team  
✅ **Merge Conflicts**: Resolving conflicting changes  
✅ **Pull Requests**: Professional code review workflow  
✅ **GitHub Issues**: Project management and tracking  
✅ **Commit Messages**: Writing clear, helpful messages  
✅ **Markdown**: Creating professional documentation  

### Key Commands

```bash
# Collaboration
git clone <url>              # Copy remote repository
git pull origin main         # Get latest changes
git fetch origin             # Download without merging
git push origin <branch>     # Send your changes

# Branch workflow for PRs
git checkout -b <branch>     # Create feature branch
git push origin <branch>     # Push branch to GitHub

# Conflict resolution
git pull origin main         # Get changes (may cause conflict)
# Edit files to resolve
git add <file>               # Stage resolved files
git commit                   # Complete the merge

# Remote management
git remote -v                # View remotes
git remote add <name> <url>  # Add remote
```

---

## 🎉 Success Story: From Chaos to Order

Remember the chaos at the beginning?

**Before:**
- ❌ Overwritten code
- ❌ Permission errors
- ❌ Merge disasters
- ❌ No code review
- ❌ Lost work

**After:**
- ✅ Protected code with branches
- ✅ Proper access control
- ✅ Conflict resolution skills
- ✅ Professional PR workflow
- ✅ Full project history

Ahmed's team completed their Student Management System successfully! The professor was impressed not just with the code, but with their **professional Git workflow**. They even got bonus points for their commit history and documentation!

---

## 🏆 Challenge: Contribute to a Real Project

Before next session, try this:

1. **Find a project** on GitHub (search for "good first issue")
2. **Fork** the repository
3. **Create a branch** for your contribution
4. **Make a small improvement** (fix typo, update docs)
5. **Submit a Pull Request**

This is your first step into open source! 🌟

---

## 📖 Additional Resources

### Documentation
- GitHub Collaboration Guide: https://docs.github.com/en/pull-requests/collaborating-with-pull-requests
- Git Branching: https://git-scm.com/book/en/v2/Git-Branching-Branches-in-a-Nutshell
- Markdown Guide: https://www.markdownguide.org

### Tools
- GitKraken: Visual Git client
- GitHub Desktop: GitHub's official GUI
- VS Code Git Integration: Built-in Git features

---

## 🔮 Next Session Preview

**Session 3 — The Internship: Branching & Professional Workflow**

Sara lands an internship at a tech company! But the codebase is huge, with multiple developers working on different features simultaneously. She needs to master:

- Advanced branching strategies
- Git Flow vs GitHub Flow
- Rebasing vs merging
- Stashing work in progress
- Cherry-picking commits
- Tags and releases

See you next week! 🚀

---

**End of Session 2**
