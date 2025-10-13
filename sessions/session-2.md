# Session 2 — Branching: Parallel Development Mastery

**Duration:** 3 hours  
**Venue:** Creativa Lab B  
**Instructors:** Omar Betawy, Amr Khaled

---

## 📖 Story Introduction

**Sara** is working on her calculator project when her professor asks: "Can you add a scientific calculator feature? But keep the basic calculator working too — I need to test both versions!"

Sara panics. If she starts adding scientific functions, she'll break the basic calculator. If she makes a copy of the folder, she'll have duplicated code to maintain. What if she needs to fix a bug in both?

Her mentor **Ahmed** introduces her to **Git branches**: "Think of branches as parallel universes. You can create a new reality where you experiment with the scientific calculator, while the original calculator continues to exist perfectly. When you're done, you can merge the universes together!"

Today, you'll master Git's most powerful feature: **branching**. This is how professional developers work on multiple features simultaneously without breaking production code.

---

## 🎯 Session Objectives

By the end of this session, you will be able to:
- ✅ Understand what branches are and why they're critical
- ✅ Create and switch between branches confidently
- ✅ Visualize branch structures
- ✅ Merge branches with different strategies
- ✅ Resolve complex merge conflicts
- ✅ Use rebase for clean history
- ✅ Stash and restore work in progress
- ✅ Undo mistakes safely (restore, reset, revert)
- ✅ Understand remotes (origin, upstream)
- ✅ Master fetch vs pull
- ✅ Delete and clean branches properly

---

## Part 1: What is a Git Branch?

### The Concept

A **branch** is an independent line of development. It's a pointer to a specific commit that moves forward as you make new commits.

**Analogy:**
```
Imagine writing a story:
- Main plot (main branch)
- What-if scenario 1 (feature branch 1)
- What-if scenario 2 (feature branch 2)

You can work on all three simultaneously, and later decide which scenarios to merge into the main plot!
```

**[IMAGE: Tree with main trunk and multiple branches]**  
*Description: Visual metaphor of tree branches*

### How Branches Work Internally

```
Commit History:
A ← B ← C ← D (main branch)
         ↑
         └─ E ← F (feature branch)
```

- Each commit points to its parent
- Branches are just pointers to commits
- HEAD points to current branch

**[IMAGE: Diagram showing commit graph with branch pointers]**  
*Description: Commits A-F with branch and HEAD pointers*

### Why Use Branches?

**Without branches:**
```
main: feature1 → bug → feature2 → broken → fix → feature3
```
❌ Everything mixed together  
❌ Can't test features independently  
❌ Hard to track what changed  
❌ Can't revert specific features  

**With branches:**
```
main:          A ─────────────→ merge ─────→ release
                ↓                ↑
feature-1:      └─→ B → C ──────┘
bug-fix:              └→ D ──────────────→ merge
feature-2:                └─→ E → F ──────┘
```
✅ Isolated features  
✅ Independent testing  
✅ Clear history  
✅ Easy rollback  

**[IMAGE: Comparison diagram of linear vs branched development]**  
*Description: Messy timeline vs organized branches*

---

## Part 2: Creating and Switching Branches

### Check Current Branch

```bash
git branch
```

**Output:**
```
* main
```

The `*` shows your current branch.

**[IMAGE: Terminal showing git branch output]**  
*Description: Branch list with asterisk highlighting current branch*

### Method 1: Traditional Approach

```bash
# Create new branch
git branch feature-login

# Switch to it
git checkout feature-login
```

### Method 2: Combined Command

```bash
# Create and switch in one command
git checkout -b feature-login
```

### Method 3: Modern Git (2.23+)

```bash
# Create and switch
git switch -c feature-login

# Just switch
git switch main
```

**[IMAGE: Three methods shown side by side in terminal]**  
*Description: All three branch creation methods*

### PRACTICE 1: Create Your First Branch

**Task:** Create a branch for adding a square root function to your calculator.

```bash
# 1. Check you're on main
git branch

# 2. Create feature branch
git switch -c feature/square-root

# 3. Verify you switched
git branch
```

**Expected Output:**
```
* feature/square-root
  main
```

---

## Part 3: Working on a Branch

### Scenario: Add Square Root Function

**On branch `feature/square-root`, edit calculator.py:**

```python
import math

def square_root(a):
    """Calculate square root"""
    if a < 0:
        return "Error: Cannot calculate square root of negative number"
    return math.sqrt(a)

def main():
    print("=== Calculator ===")
    print("1. Add")
    print("2. Subtract")
    print("3. Multiply")
    print("4. Divide")
    print("5. Square Root")  # NEW
    
    choice = input("Enter choice (1-5): ")
    
    if choice == '5':  # NEW
        num = float(input("Enter number: "))
        print(f"Result: {square_root(num)}")
    # ... rest of code
```

**[IMAGE: Code editor showing changes]**  
*Description: New square_root function highlighted*

### Commit on Branch

```bash
git add calculator.py
git commit -m "Add square root function"
```

### Switch Back to Main

```bash
git switch main
```

**Look at calculator.py — the square root function is GONE!** 😱

That's the power of branches — changes are isolated!

**[IMAGE: Split screen showing file with/without function in different branches]**  
*Description: Same file, different content in main vs feature branch*

### Switch Back to Feature Branch

```bash
git switch feature/square-root
```

**The square root function is BACK!** ✨

---

## PRACTICE 2: Multiple Branches

**Challenge:** Create multiple features simultaneously.

```bash
# Switch to main
git switch main

# Create power function branch
git switch -c feature/power-function

# Add this to calculator.py
def power(base, exponent):
    """Raise base to exponent power"""
    return base ** exponent

# Commit
git add calculator.py
git commit -m "Add power function"

# Switch to main
git switch main

# Create log function branch
git switch -c feature/log-function

# Add this to calculator.py
import math

def logarithm(a, base=10):
    """Calculate logarithm"""
    if a <= 0:
        return "Error: Logarithm undefined for non-positive numbers"
    return math.log(a, base)

# Commit
git add calculator.py
git commit -m "Add logarithm function"
```

**Now you have 3 branches with different features!**

```bash
git branch
```

**Output:**
```
* feature/log-function
  feature/power-function
  feature/square-root
  main
```

**[IMAGE: Branch list showing all created branches]**  
*Description: Terminal with multiple branches listed*

---

## Part 4: Visualizing Branches

### Text-Based Visualization

```bash
git log --oneline --graph --all --decorate
```

**Output:**
```
* a7f8b9c (feature/log-function) Add logarithm function
| * d4e5f6g (feature/power-function) Add power function
|/
| * b2c3d4e (feature/square-root) Add square root function
|/
* 1a2b3c4 (HEAD -> main) Initial calculator
```

**[IMAGE: Git log graph in terminal]**  
*Description: ASCII art showing branching structure*

### Visual Git Tools

**Install Git Graph (VS Code Extension):**
1. Open VS Code
2. Extensions → Search "Git Graph"
3. Install
4. Click "Git Graph" in bottom status bar

**[IMAGE: VS Code Git Graph extension interface]**  
*Description: Visual representation of branches and commits*

**GitKraken (Desktop App):**
- Beautiful visual interface
- Drag-and-drop branching
- Interactive rebase

**[IMAGE: GitKraken interface showing branch visualization]**  
*Description: Colorful branch diagram with node connections*

---

## Part 5: Merging Branches

### Understanding Merge Types

#### Type 1: Fast-Forward Merge

**Scenario:** No commits on main since branch creation.

```
Before merge:
main:     A ─ B
              └─ C ─ D (feature)

After merge:
main:     A ─ B ─ C ─ D
```

**Command:**
```bash
git switch main
git merge feature/square-root
```

**Output:**
```
Updating 1a2b3c4..b2c3d4e
Fast-forward
 calculator.py | 8 ++++++++
 1 file changed, 8 insertions(+)
```

✅ **Clean, linear history**

**[IMAGE: Fast-forward merge diagram]**  
*Description: Simple forward movement of main pointer*

#### Type 2: Three-Way Merge

**Scenario:** Both main and feature have new commits.

```
Before merge:
         ┌─ C ─ D (feature)
main: A ─ B
         └─ E ─ F (main)

After merge:
         ┌─ C ─ D
main: A ─ B       ─ G (merge commit)
         └─ E ─ F ┘
```

**Command:**
```bash
git switch main
git merge feature/power-function
```

**Git opens editor for merge commit message:**
```
Merge branch 'feature/power-function'
```

Save and close.

**[IMAGE: Three-way merge diagram with merge commit]**  
*Description: Two lines converging into merge commit*

---

## PRACTICE 3: Your First Merge

**Task:** Merge the square root feature into main.

```bash
# 1. Switch to main
git switch main

# 2. Merge feature
git merge feature/square-root

# 3. Verify
git log --oneline

# 4. Test the code
python calculator.py
```

**Challenge Questions:**
1. Was it a fast-forward or three-way merge?
2. How many commits are in main now?
3. What happened to the feature branch?

**[IMAGE: Before and after merge comparison]**  
*Description: Branch structure before and after merge*

---

## Part 6: Merge Conflicts — The Challenge

### What Causes Conflicts?

**Conflict occurs when:**
- Two branches modify the **same lines** in the **same file**
- Git can't automatically decide which change to keep

### Scenario: The Classic Conflict

**Sara and Ahmed both edit the same function:**

**Sara's branch:**
```python
def add(a, b):
    """Add two numbers - improved version"""
    return float(a) + float(b)
```

**Ahmed's branch:**
```python
def add(a, b):
    """Add two numbers together"""
    return round(a + b, 2)
```

Both commit and try to merge... **CONFLICT!** 💥

**[IMAGE: Two developers editing same code]**  
*Description: Split screen showing different changes to same function*

### Creating a Conflict (Practice Setup)

Let's intentionally create a conflict to practice:

```bash
# On main branch
git switch main

# Edit calculator.py - change the add function
def add(a, b):
    """Add two numbers - main branch version"""
    return a + b

git add calculator.py
git commit -m "Update add function docstring on main"

# Create conflict branch
git switch -c conflict-branch

# Edit calculator.py - change the SAME add function differently
def add(a, b):
    """Add two numbers - conflict branch version"""
    return a + b

git add calculator.py
git commit -m "Update add function docstring on conflict branch"

# Try to merge back to main
git switch main
git merge conflict-branch
```

**BOOM! Conflict:**
```
Auto-merging calculator.py
CONFLICT (content): Merge conflict in calculator.py
Automatic merge failed; fix conflicts and then commit the result.
```

**[IMAGE: Terminal showing conflict error]**  
*Description: Red text showing merge conflict message*

### Anatomy of a Conflict

**Open calculator.py:**

```python
def add(a, b):
<<<<<<< HEAD
    """Add two numbers - main branch version"""
=======
    """Add two numbers - conflict branch version"""
>>>>>>> conflict-branch
    return a + b
```

**Understanding conflict markers:**
- `<<<<<<< HEAD`: Start of YOUR changes (current branch)
- `=======`: Separator
- `>>>>>>> conflict-branch`: End of THEIR changes (branch being merged)

**[IMAGE: Conflict markers annotated]**  
*Description: Each marker labeled with its meaning*

### Resolving the Conflict

**Option 1: Keep your version**
```python
def add(a, b):
    """Add two numbers - main branch version"""
    return a + b
```

**Option 2: Keep their version**
```python
def add(a, b):
    """Add two numbers - conflict branch version"""
    return a + b
```

**Option 3: Combine both (usually best)**
```python
def add(a, b):
    """Add two numbers - combined improved version"""
    return a + b
```

**Option 4: Write something completely new**
```python
def add(a, b):
    """Add two numbers and return the sum"""
    return a + b
```

**Remove ALL conflict markers!**

**[IMAGE: Resolved conflict - clean code]**  
*Description: Final code with no markers*

### Complete the Merge

```bash
# 1. Stage the resolved file
git add calculator.py

# 2. Check status
git status
```

**Output:**
```
All conflicts fixed but you are still merging.
  (use "git commit" to conclude merge)
```

```bash
# 3. Complete the merge
git commit
```

Git provides a default message:
```
Merge branch 'conflict-branch'

Conflicts:
	calculator.py
```

Save and close. ✅ **Conflict resolved!**

**[IMAGE: Successful merge commit]**  
*Description: Green success message*

---

## PRACTICE 4: Conflict Resolution Challenge

**Setup a multi-line conflict:**

```bash
# Create two branches with conflicting changes

# Branch 1: Add error handling
git switch main
git switch -c feature/error-handling

# Edit calculator.py
def divide(a, b):
    """Divide two numbers with error handling"""
    if b == 0:
        return "Error: Division by zero!"
    return a / b

git add calculator.py
git commit -m "Add error handling to divide"

# Branch 2: Add precision control
git switch main
git switch -c feature/precision

# Edit calculator.py (same function!)
def divide(a, b):
    """Divide two numbers with precision control"""
    if b == 0:
        return "Cannot divide by zero"
    return round(a / b, 4)

git add calculator.py
git commit -m "Add precision control to divide"

# Now merge both into main - CONFLICT!
git switch main
git merge feature/error-handling  # This works
git merge feature/precision        # CONFLICT!
```

**Your Task:**
1. Identify the conflict
2. Combine BOTH features (error handling AND precision)
3. Keep both improvements
4. Test the code works
5. Complete the merge

**Solution should look like:**
```python
def divide(a, b):
    """Divide two numbers with error handling and precision control"""
    if b == 0:
        return "Error: Division by zero!"
    return round(a / b, 4)
```

---

## PRACTICE 5: Three-Way Conflict

**Complex scenario with multiple files:**

```bash
# Create scenario
git switch main

# File 1: calculator.py
# File 2: utils.py (create it)

# Branch A: Modify both files
git switch -c feature/enhance-calculator
# Edit calculator.py and utils.py
# Commit

# Branch B: Also modify both files differently
git switch main
git switch -c feature/add-validation
# Edit calculator.py and utils.py differently
# Commit

# Merge both - handle conflicts in both files!
```

**Challenge:** Resolve conflicts in multiple files while keeping improvements from both branches.

---

## Part 7: Rebasing — The Clean History Technique

### What is Rebasing?

**Rebasing** replays your commits on top of another branch, creating a linear history.

**Merge creates:**
```
       ┌─ C ─ D
A ─ B ─┤       ─ F (merge commit)
       └─ E ───┘
```

**Rebase creates:**
```
A ─ B ─ E ─ C' ─ D'
```

**[IMAGE: Merge vs Rebase comparison]**  
*Description: Two diagrams showing different results*

### When to Rebase

✅ **Use rebase when:**
- Cleaning up local commits before pushing
- Updating feature branch with latest main
- Want linear, clean history

❌ **NEVER rebase:**
- Public/shared branches
- Commits others depend on
- After pushing to shared repository

**Golden Rule:** Never rebase commits that exist outside your local repository!

**[IMAGE: Warning sign for public branch rebasing]**  
*Description: Red warning about rebasing shared branches*

### Basic Rebase Example

```bash
# You're on feature branch
git switch feature/analytics

# Rebase onto main
git rebase main
```

**What happens:**
1. Git finds common ancestor
2. Saves your feature commits
3. Fast-forwards to main's latest
4. Replays your commits one by one

**[IMAGE: Rebase process step-by-step]**  
*Description: Four stages of rebasing*

### Interactive Rebase — The Power Tool

**Clean up your commit history before merging:**

```bash
git log --oneline
```

**Output:**
```
a1b2c3d Fix typo
b2c3d4e Add feature
c3d4e5f Fix bug
d4e5f6g Add tests
e5f6g7h Add feature implementation
```

Messy! Let's clean it up:

```bash
git rebase -i HEAD~5
```

**Editor opens:**
```
pick e5f6g7h Add feature implementation
pick b2c3d4e Add feature
pick d4e5f6g Add tests
pick c3d4e5f Fix bug
pick a1b2c3d Fix typo

# Commands:
# p, pick = use commit
# r, reword = use commit, but edit message
# e, edit = use commit, but stop for amending
# s, squash = meld into previous commit
# f, fixup = like squash, but discard commit message
# d, drop = remove commit
```

**[IMAGE: Interactive rebase editor]**  
*Description: Vim/nano showing rebase commands*

**Let's clean it:**
```
pick e5f6g7h Add feature implementation
squash b2c3d4e Add feature
pick d4e5f6g Add tests
fixup c3d4e5f Fix bug
fixup a1b2c3d Fix typo
```

**Result:**
```
e5f6g7h Add complete feature with tests
d4e5f6g Add tests
```

**[IMAGE: Before and after commit history]**  
*Description: Messy vs clean commit log*

---

## PRACTICE 6: Interactive Rebase

**Task:** Clean up a messy commit history.

```bash
# Create messy commits
git switch -c feature/messy

echo "line 1" >> test.txt
git add test.txt
git commit -m "Add line 1"

echo "line 2" >> test.txt
git add test.txt
git commit -m "Add line 2"

echo "line 3" >> test.txt
git add test.txt
git commit -m "typo fix"

echo "line 4" >> test.txt
git add test.txt
git commit -m "oops forgot this"

echo "line 5" >> test.txt
git add test.txt
git commit -m "final line"

# Now clean them up with interactive rebase
git rebase -i HEAD~5
```

**Your Task:**
1. Combine all commits into one meaningful commit
2. Message should be: "Add test file with 5 lines"
3. Verify with `git log --oneline`

---

## PRACTICE 7: Rebase Conflict Resolution

**Setup:**
```bash
# Main branch has updates
git switch main
echo "main update" >> README.md
git add README.md
git commit -m "Update README on main"

# Your feature branch has conflicting changes
git switch -c feature/readme
echo "feature update" >> README.md
git add README.md
git commit -m "Update README on feature"

# Rebase onto main - CONFLICT!
git rebase main
```

**When conflict occurs during rebase:**
```bash
# 1. Fix the conflict in files
# 2. Stage the fixed files
git add README.md

# 3. Continue rebase (NOT commit!)
git rebase --continue

# If things go wrong:
git rebase --abort
```

**Challenge:** Successfully rebase with conflict resolution.

---

## 💾 Part 8: Git Stash — Save Work in Progress

### The Problem

```bash
# You're working on a feature
git switch feature/payment
# ...making changes, not ready to commit...

# Suddenly: URGENT BUG on main!
git switch main
```

**ERROR:**
```
error: Your local changes to the following files would be overwritten by checkout:
	payment.py
Please commit your changes or stash them before you switch branches.
```

You can't switch with uncommitted changes! But you're not ready to commit yet...

**[IMAGE: Error message about uncommitted changes]**  
*Description: Terminal showing checkout error*

### Solution: Git Stash

**Save your work temporarily:**

```bash
git stash
```

**Or with description:**
```bash
git stash save "WIP: Payment integration half done"
```

**Output:**
```
Saved working directory and index state WIP on feature/payment: a1b2c3d Last commit
```

Now your working directory is clean! ✅

**[IMAGE: Stash save confirmation]**  
*Description: Success message for stash*

### Switch and Fix Bug

```bash
# Switch to main
git switch main

# Fix the urgent bug
echo "fix" >> bug.py
git add bug.py
git commit -m "Fix critical bug"

# Go back to feature
git switch feature/payment
```

### Restore Stashed Work

**Method 1: Pop (apply and remove)**
```bash
git stash pop
```

**Method 2: Apply (keep stash)**
```bash
git stash apply
```

Your work is back!

**[IMAGE: Stash pop restoring changes]**  
*Description: Changes reappearing in working directory*

### Managing Multiple Stashes

```bash
# List all stashes
git stash list
```

**Output:**
```
stash@{0}: WIP on feature/payment: a1b2c3d Payment integration
stash@{1}: WIP on feature/auth: b2c3d4e Auth system
stash@{2}: WIP on feature/ui: c3d4e5f UI updates
```

**Apply specific stash:**
```bash
git stash apply stash@{1}
```

**Delete specific stash:**
```bash
git stash drop stash@{0}
```

**Clear all stashes:**
```bash
git stash clear
```

**[IMAGE: Stash list with multiple entries]**  
*Description: Terminal showing numbered stashes*

### Advanced Stash Options

**Stash untracked files too:**
```bash
git stash -u
```

**Stash including ignored files:**
```bash
git stash -a
```

**Create branch from stash:**
```bash
git stash branch feature/from-stash stash@{0}
```

---

## PRACTICE 8: Stash Workflow

**Scenario: Interrupted work**

```bash
# 1. Start working on feature
git switch -c feature/notifications
echo "notification_system = True" >> config.py
# Don't commit yet!

# 2. Urgent: need to fix bug on main
# Stash your work
git stash save "Notification system in progress"

# 3. Switch and fix bug
git switch main
echo "bug_fixed = True" >> hotfix.py
git add hotfix.py
git commit -m "Hotfix: Critical bug"

# 4. Return to feature
git switch feature/notifications
git stash pop

# 5. Continue working
echo "notification_enabled = True" >> config.py
git add config.py
git commit -m "Add notification system"
```

**Challenge:** Do this 3 times with different files to build muscle memory.

---

## PRACTICE 9: Multiple Stashes

**Task:** Create and manage 3 different stashes.

```bash
# Stash 1: Feature A
git switch -c feature-a
echo "feature a" >> a.txt
git stash save "Feature A work"

# Stash 2: Feature B
git switch -c feature-b
echo "feature b" >> b.txt
git stash save "Feature B work"

# Stash 3: Feature C
git switch -c feature-c
echo "feature c" >> c.txt
git stash save "Feature C work"

# Now:
# 1. List all stashes
# 2. Apply stash for Feature B
# 3. Drop stash for Feature A
# 4. Pop stash for Feature C
```

---

## Part 9: Undoing Mistakes

### Three Ways to Undo

| Command | Scope | Rewrites History? | Use Case |
|---------|-------|-------------------|----------|
| `git restore` | Working directory | ❌ No | Discard uncommitted changes |
| `git reset` | Commits | ⚠️ Yes (local only) | Undo commits not pushed |
| `git revert` | Commits | ❌ No | Undo pushed commits safely |

**[IMAGE: Flowchart for choosing undo method]**  
*Description: Decision tree based on commit status*

### 1. Git Restore — Discard Working Changes

**Scenario: You made changes but want to discard them**

```bash
# Modified file
echo "bad change" >> calculator.py

# Discard changes
git restore calculator.py
```

**Restore all files:**
```bash
git restore .
```

**Restore staged files:**
```bash
git add calculator.py
git restore --staged calculator.py  # Unstage
```

**[IMAGE: Before and after restore]**  
*Description: Modified file returning to original state*

### 2. Git Reset — Undo Local Commits

**Three modes:**

#### Soft Reset (keep changes, undo commit)
```bash
git reset --soft HEAD~1
```

**Result:**
- Commit removed
- Changes stay staged
- Working directory unchanged

**[IMAGE: Soft reset diagram]**  
*Description: Commit removed, files stay in staging*

#### Mixed Reset (default - unstage changes)
```bash
git reset HEAD~1
# Same as: git reset --mixed HEAD~1
```

**Result:**
- Commit removed
- Changes unstaged
- Working directory unchanged

**[IMAGE: Mixed reset diagram]**  
*Description: Commit removed, files in working directory*

#### Hard Reset (destroy everything)
```bash
git reset --hard HEAD~1
```

**Result:**
- Commit removed
- Changes discarded
- Working directory clean

⚠️ **DANGER: Can't undo this!**

**[IMAGE: Hard reset warning]**  
*Description: Red warning sign*

### 3. Git Revert — Safe Undo for Pushed Commits

**Scenario: You pushed a bad commit**

```bash
# Bad commit is pushed
git log --oneline
# a1b2c3d (HEAD) Bad commit
# b2c3d4e Good commit

# Create reverse commit
git revert a1b2c3d
```

**Result:**
```
c3d4e5f (HEAD) Revert "Bad commit"
a1b2c3d Bad commit
b2c3d4e Good commit
```

History preserved! Safe for shared branches! ✅

**[IMAGE: Revert creating new commit]**  
*Description: Timeline showing revert commit*

---

## PRACTICE 10: Restore, Reset, Revert

**Part A: Restore**
```bash
# Make changes
echo "mistake" >> calculator.py

# Discard
git restore calculator.py

# Verify it's gone
cat calculator.py
```

**Part B: Reset**
```bash
# Make a commit you want to undo
echo "test" >> test.txt
git add test.txt
git commit -m "Test commit"

# Undo with soft reset
git reset --soft HEAD~1

# Verify: file still staged
git status

# Undo with hard reset
git add test.txt
git commit -m "Test commit again"
git reset --hard HEAD~1

# Verify: file gone
ls test.txt  # File not found
```

**Part C: Revert**
```bash
# Make and push a commit
echo "public mistake" >> public.txt
git add public.txt
git commit -m "Public mistake"
git push origin main

# Revert it (safe for public branches)
git revert HEAD
git push origin main
```

---

## PRACTICE 11: Undo Disasters

**Scenario 1: Committed to wrong branch**
```bash
# You're on main but should be on feature
git switch main
echo "feature code" >> feature.py
git add feature.py
git commit -m "Add feature"

# Oops! Move it to correct branch
git branch feature/correct-branch  # Create branch with commit
git reset --hard HEAD~1            # Remove from main
git switch feature/correct-branch  # Switch to new branch
```

**Scenario 2: Recover deleted commits**
```bash
# You did hard reset and lost commits
git reset --hard HEAD~3

# Panic! Find the lost commits
git reflog

# Find the commit hash before reset
# a1b2c3d HEAD@{1}: commit: Important work

# Recover it
git reset --hard a1b2c3d
```

**Scenario 3: Undo multiple commits**
```bash
# Undo last 3 commits but keep changes
git reset --soft HEAD~3

# Recommit as one
git add .
git commit -m "Combined feature"
```

---

## 🌐 Part 10: Understanding Remotes

### What are Remotes?

**Remotes** are versions of your repository hosted elsewhere (GitHub, GitLab, etc.).

```bash
# List remotes
git remote -v
```

**Output:**
```
origin  https://github.com/sara/calculator.git (fetch)
origin  https://github.com/sara/calculator.git (push)
```

**[IMAGE: Local repo connected to remote]**  
*Description: Computer linked to GitHub cloud*

### Common Remote Names

- **origin**: Your fork (where you push)
- **upstream**: Original repo (for contributions)
- **production**: Production server
- **staging**: Staging server

### Adding Remotes

```bash
# Add upstream for open source
git remote add upstream https://github.com/original/repo.git

# Add production server
git remote add production git@production.com:app.git

# Verify
git remote -v
```

**Output:**
```
origin     https://github.com/sara/calculator.git (fetch)
origin     https://github.com/sara/calculator.git (push)
upstream   https://github.com/original/calculator.git (fetch)
upstream   https://github.com/original/calculator.git (push)
```

**[IMAGE: Multiple remotes diagram]**  
*Description: Local repo connected to origin and upstream*

---

## Part 11: Fetch vs Pull — The Critical Difference

### Git Fetch

**Downloads changes but DOESN'T merge:**

```bash
git fetch origin
```

**What it does:**
1. Downloads commits from remote
2. Updates `origin/main` pointer
3. Does NOT change your local `main`
4. Safe — just downloads data

**[IMAGE: Fetch operation diagram]**  
*Description: Remote commits downloaded, local branch unchanged*

### Git Pull

**Downloads AND merges:**

```bash
git pull origin main
```

**Equivalent to:**
```bash
git fetch origin
git merge origin/main
```

**[IMAGE: Pull operation diagram]**  
*Description: Download + merge in one step*

### When to Use Each

**Use `fetch` when:**
- ✅ You want to see changes before merging
- ✅ You're reviewing others' work
- ✅ You want to be careful

**Use `pull` when:**
- ✅ You trust the changes
- ✅ You want quick updates
- ✅ You're syncing your fork

### Safe Pull Workflow

```bash
# 1. See what's new
git fetch origin

# 2. Check differences
git log main..origin/main

# 3. See actual changes
git diff main origin/main

# 4. Merge if safe
git merge origin/main

# OR combine: git pull origin main
```

**[IMAGE: Safe workflow diagram]**  
*Description: Fetch → Review → Merge process*

---

## PRACTICE 12: Fetch vs Pull

**Setup (need GitHub repo):**
```bash
# Create test repo on GitHub
# Clone it locally

# Make changes on GitHub (edit file in browser)
# Don't pull yet!

# 1. Fetch only
git fetch origin

# 2. Compare
git log main..origin/main
git diff main origin/main

# 3. Merge manually
git merge origin/main

# Now try with pull:
# Make another change on GitHub

# 4. Pull directly
git pull origin main
```

**Challenge:** Can you tell the difference?

---

## PRACTICE 13: Remote Management

**Task: Set up multiple remotes**

```bash
# 1. Fork a repository on GitHub
# 2. Clone YOUR fork
git clone https://github.com/YOUR_USERNAME/repo.git

# 3. Add upstream
git remote add upstream https://github.com/ORIGINAL_OWNER/repo.git

# 4. Fetch from both
git fetch origin
git fetch upstream

# 5. Compare
git log origin/main
git log upstream/main

# 6. Update your fork with upstream
git merge upstream/main
git push origin main
```

---

## Part 12: Deleting & Cleaning Up Branches

### Delete Merged Branch

**Safe delete (only if merged):**
```bash
git branch -d feature/completed
```

**If not merged:**
```
error: The branch 'feature/completed' is not fully merged.
If you are sure you want to delete it, run 'git branch -D feature/completed'.
```

**[IMAGE: Safe delete error message]**  
*Description: Warning when trying to delete unmerged branch*

### Force Delete Branch

**Delete even if not merged:**
```bash
git branch -D feature/abandoned
```

⚠️ **Warning:** This deletes ALL commits unique to that branch!

### Delete Remote Branch

```bash
# Delete from GitHub
git push origin --delete feature/completed
```

**OR shorter:**
```bash
git push origin :feature/completed
```

**[IMAGE: Branch deletion confirmation]**  
*Description: Terminal showing successful deletion*

### Clean Up Tracking Branches

**List all branches including remote:**
```bash
git branch -a
```

**Output:**
```
* main
  feature/active
  remotes/origin/main
  remotes/origin/feature/deleted-on-github
```

**Prune deleted remote branches:**
```bash
git fetch --prune
```

**[IMAGE: Before and after pruning]**  
*Description: Branch list cleaned up*

---

## PRACTICE 14: Branch Cleanup

**Task:**
```bash
# 1. Create 3 feature branches
git branch feature/temp-1
git branch feature/temp-2  
git branch feature/temp-3

# 2. Merge one into main
git switch feature/temp-1
echo "content" >> file1.txt
git add file1.txt
git commit -m "Add feature 1"
git switch main
git merge feature/temp-1

# 3. Try to delete merged branch (safe)
git branch -d feature/temp-1  # Works!

# 4. Try to delete unmerged branch
git branch -d feature/temp-2  # Error!

# 5. Force delete
git branch -D feature/temp-2  # Works!

# 6. Push branch to GitHub and delete remotely
git push origin feature/temp-3
git push origin --delete feature/temp-3
```

---

## 🏆 MEGA CHALLENGE: Complete Branching Workflow

**Scenario: Build a Blog Application**

**Your Tasks:**

### Part 1: Setup (5 min)
```bash
# 1. Create repository
mkdir blog-app
cd blog-app
git init

# 2. Create initial files
echo "# Blog Application" > README.md
echo "print('Blog App')" > app.py
git add .
git commit -m "Initial commit"

# 3. Push to GitHub (create repo first)
git remote add origin YOUR_GITHUB_URL
git push -u origin main
```

### Part 2: Feature Development (15 min)
```bash
# Feature 1: User authentication
git switch -c feature/auth
# Add authentication code
echo "def login(): pass" >> auth.py
git add auth.py
git commit -m "Add login function"

echo "def register(): pass" >> auth.py
git add auth.py
git commit -m "Add register function"

# Feature 2: Blog posts (parallel development!)
git switch main
git switch -c feature/posts
# Add post management
echo "def create_post(): pass" >> posts.py
git add posts.py
git commit -m "Add create post"

echo "def delete_post(): pass" >> posts.py
git add posts.py
git commit -m "Add delete post"

# Feature 3: Comments
git switch main
git switch -c feature/comments
echo "def add_comment(): pass" >> comments.py
git add comments.py
git commit -m "Add comments"
```

### Part 3: Create Conflicts (10 min)
```bash
# Modify app.py on main
git switch main
echo "# Main application" >> app.py
git add app.py
git commit -m "Update main app"

# Modify app.py on feature
git switch feature/auth
echo "# Authentication app" >> app.py
git add app.py
git commit -m "Update auth app"
```

### Part 4: Merging & Conflicts (15 min)
```bash
# Merge features one by one
git switch main
git merge feature/posts      # Should work
git merge feature/comments   # Should work  
git merge feature/auth       # CONFLICT!

# Resolve conflict
# Keep both comments combined
git add app.py
git commit

# Clean up merged branches
git branch -d feature/posts
git branch -d feature/comments
git branch -d feature/auth
```

### Part 5: Rebase Practice (10 min)
```bash
# Create new feature
git switch -c feature/admin

# Make messy commits
echo "admin 1" >> admin.py
git add admin.py
git commit -m "admin stuff"

echo "admin 2" >> admin.py
git add admin.py
git commit -m "more admin"

echo "admin 3" >> admin.py
git add admin.py
git commit -m "oops"

# Clean up with interactive rebase
git rebase -i HEAD~3
# Squash all into one commit

# Update with main changes
git rebase main
```

### Part 6: Stash Practice (5 min)
```bash
# Start new feature
git switch -c feature/search
echo "def search(): pass" >> search.py
# Don't commit!

# Emergency bug fix needed
git stash save "Search feature WIP"
git switch main
echo "# Bug fix" >> app.py
git add app.py
git commit -m "Critical bug fix"

# Return to feature
git switch feature/search
git stash pop
# Complete and commit
git add search.py
git commit -m "Add search feature"
```

### Part 7: Undo Practice (5 min)
```bash
# Make a mistake
git switch main
echo "MISTAKE" >> app.py
git add app.py
git commit -m "Bad commit"
git push origin main

# Revert it (safe for pushed commits)
git revert HEAD
git push origin main
```

### Part 8: Final Integration (5 min)
```bash
# Merge all remaining features
git switch main
git merge feature/admin
git merge feature/search

# Push everything
git push origin main

# Visualize your work
git log --oneline --graph --all
```

**Success Criteria:**
- ✅ At least 5 feature branches created
- ✅ At least 15 commits total
- ✅ At least 1 merge conflict resolved
- ✅ At least 1 rebase performed
- ✅ At least 1 stash used
- ✅ At least 1 commit reverted
- ✅ All branches cleaned up
- ✅ Clean commit history

**[IMAGE: Final git log graph]**  
*Description: Beautiful branching structure with all merges*

---

## BONUS CHALLENGES

### Challenge 1: The Time Traveler
```bash
# Create 5 commits
# Use reset to go back to commit 2
# Create new branch from there
# Make different changes
# Merge back creating alternative timeline
```

### Challenge 2: The Archaeologist
```bash
# Someone deleted important commits
# Use git reflog to find them
# Recover the lost work
# Cherry-pick important commits to new branch
```

### Challenge 3: The Perfectionist
```bash
# Make 10 messy commits
# Use interactive rebase to create 3 perfect commits
# Reword all commit messages
# Create perfect linear history
```

### Challenge 4: The Juggler
```bash
# Work on 3 features simultaneously
# Use stash to switch between them
# Don't lose any work
# Complete all 3 features
```

### Challenge 5: The Merger
```bash
# Create 3 branches with overlapping changes
# Merge all 3 into main one by one
# Resolve all conflicts
# No history should be lost
```

---

## 📚 Session Summary

### What You Mastered

✅ **Branching Fundamentals**
- What branches are and why use them
- Creating and switching branches
- Branch naming conventions

✅ **Merging Strategies**
- Fast-forward merges
- Three-way merges
- Merge conflict resolution

✅ **Advanced Techniques**
- Rebasing for clean history
- Interactive rebase for cleanup
- Stashing work in progress

✅ **Safety & Undo**
- git restore for working directory
- git reset for local commits
- git revert for pushed commits

✅ **Remote Operations**
- Understanding remotes
- fetch vs pull
- Branch management on remotes

### Command Reference

```bash
# BRANCHING
git branch                    # List branches
git branch feature-name       # Create branch
git switch feature-name       # Switch branch
git switch -c feature-name    # Create and switch
git branch -d feature-name    # Delete merged branch
git branch -D feature-name    # Force delete branch

# MERGING
git merge feature-name        # Merge branch
git merge --abort             # Abort merge

# REBASING
git rebase main              # Rebase onto main
git rebase -i HEAD~3         # Interactive rebase
git rebase --continue        # Continue after conflict
git rebase --abort           # Abort rebase

# STASHING
git stash                    # Stash changes
git stash save "message"     # Stash with description
git stash list               # List stashes
git stash pop                # Apply and remove stash
git stash apply              # Apply keep stash
git stash drop stash@{0}     # Delete stash
git stash clear              # Delete all stashes

# UNDOING
git restore file.py          # Discard working changes
git restore --staged file.py # Unstage file
git reset --soft HEAD~1      # Undo commit, keep staged
git reset HEAD~1             # Undo commit, keep changes
git reset --hard HEAD~1      # Undo commit, discard all
git revert HEAD              # Create reverse commit

# REMOTES
git remote -v                # List remotes
git remote add name url      # Add remote
git fetch origin             # Download from remote
git pull origin main         # Fetch and merge
git push origin branch       # Push branch

# CLEANUP
git fetch --prune            # Remove deleted remote branches
git branch -d feature        # Delete local branch
git push origin --delete feature  # Delete remote branch
```

---

## You've Mastered Branching!

You can now:
- ✅ Create and manage multiple branches
- ✅ Merge changes from different branches
- ✅ Resolve complex merge conflicts
- ✅ Use rebase for clean history
- ✅ Stash work and switch contexts
- ✅ Undo mistakes safely
- ✅ Work with remote repositories
- ✅ Handle professional branching workflows

**Next session:** We'll use these skills for real team collaboration with Pull Requests, code reviews, and professional workflows!

---

**End of Session 2**
