# Session 2 — Branching (Quick Reference)

**Duration:** 30-45 minutes  
**Format:** Summary + Commands + Single Challenge

---

## Quick Summary

**Branching** allows you to work on multiple features simultaneously without breaking your main code. Each branch is an independent line of development.

**Key Concepts:**
- Branch = pointer to a commit
- Work on features in isolation
- Merge branches when done
- Resolve conflicts when changes overlap

---

## Part 1: Basic Branching

### Create & Switch Branches

```bash
# Check current branch
git branch

# Create new branch
git branch feature-name

# Switch to branch
git switch feature-name

# Create and switch (recommended)
git switch -c feature-name

# Switch back to main
git switch main
```

### View All Branches

```bash
# List local branches
git branch

# List with last commit
git branch -v

# Visual graph
git log --oneline --graph --all
```

---

## Part 2: Merging

### Fast-Forward Merge

When no changes on main since branch creation:

```bash
git switch main
git merge feature-name
```

### Three-Way Merge

When both branches have new commits:

```bash
git switch main
git merge feature-name
# Git creates merge commit automatically
```

---

## Part 3: Handling Conflicts

### When Conflict Occurs

```bash
git merge feature-name
# CONFLICT message appears
```

### Conflict Markers in File

```python
<<<<<<< HEAD
your code
=======
their code
>>>>>>> feature-name
```

### Resolve Conflict

```bash
# 1. Edit file, remove markers, keep desired code
# 2. Stage resolved file
git add filename

# 3. Complete merge
git commit
```

---

## Part 4: Rebasing

### Basic Rebase

Creates linear history by replaying commits:

```bash
git switch feature-branch
git rebase main
```

### Interactive Rebase (Clean History)

```bash
# Cleanup last 3 commits
git rebase -i HEAD~3

# In editor:
# pick = keep commit
# squash = combine with previous
# reword = change message
# drop = delete commit
```

**Golden Rule:** Never rebase public/shared branches!

---

## Part 5: Git Stash

### Save Work Temporarily

```bash
# Save all changes
git stash

# Save with message
git stash save "work in progress"

# List stashes
git stash list

# Apply latest stash (and remove)
git stash pop

# Apply specific stash (keep it)
git stash apply stash@{0}

# Delete stash
git stash drop stash@{0}

# Clear all stashes
git stash clear
```

**Use Case:** Switch branches without committing incomplete work.

---

## Part 6: Undoing Changes

### Three Undo Methods

| Command | Use When | History? |
|---------|----------|----------|
| `git restore file` | Discard uncommitted changes | No change |
| `git reset HEAD~1` | Undo local commit | Rewrites |
| `git revert HEAD` | Undo pushed commit | Adds new commit |

### Restore (Discard Changes)

```bash
# Discard changes in file
git restore filename

# Discard all changes
git restore .

# Unstage file
git restore --staged filename
```

### Reset (Undo Commits)

```bash
# Undo commit, keep changes staged
git reset --soft HEAD~1

# Undo commit, keep changes unstaged (default)
git reset HEAD~1

# Undo commit, discard changes (dangerous!)
git reset --hard HEAD~1
```

### Revert (Safe for Pushed Commits)

```bash
# Create new commit that undoes previous commit
git revert HEAD

# Revert specific commit
git revert commit-hash
```

---

## Part 7: Remotes & Synchronization

### View Remotes

```bash
# List remotes
git remote -v

# Add remote
git remote add upstream URL
```

### Fetch vs Pull

```bash
# Download changes only (safe)
git fetch origin

# Download and merge (fast)
git pull origin main

# Safe workflow:
git fetch origin
git diff main origin/main  # Review changes
git merge origin/main      # Merge when ready
```

---

## Part 8: Branch Cleanup

### Delete Branches

```bash
# Delete merged branch (safe)
git branch -d feature-name

# Force delete unmerged branch
git branch -D feature-name

# Delete remote branch
git push origin --delete feature-name

# Remove stale remote-tracking branches
git fetch --prune
```

---

## Complete Command Reference

```bash
# BRANCHING
git branch                        # List branches
git switch -c feature-name        # Create and switch
git switch main                   # Switch to main

# MERGING
git merge feature-name            # Merge branch
git merge --abort                 # Cancel merge

# REBASING
git rebase main                   # Rebase onto main
git rebase -i HEAD~3              # Interactive rebase
git rebase --continue             # Continue after fix
git rebase --abort                # Cancel rebase

# STASHING
git stash                         # Save changes
git stash pop                     # Restore changes
git stash list                    # List stashes

# UNDOING
git restore file                  # Discard changes
git reset --soft HEAD~1           # Undo commit, keep staged
git reset HEAD~1                  # Undo commit, keep changes
git reset --hard HEAD~1           # Undo commit, discard all
git revert HEAD                   # Safe undo for pushed commits

# REMOTES
git fetch origin                  # Download updates
git pull origin main              # Fetch and merge

# CLEANUP
git branch -d feature-name        # Delete branch
git push origin --delete feature # Delete remote branch
```

---

## CHALLENGE: Multi-Feature Development

**Scenario:** Build a task management app with 4 features using proper branching workflow.

### Setup (2 min)

```bash
# Create project
mkdir task-manager
cd task-manager
git init

# Initial commit
echo "# Task Manager" > README.md
echo "tasks = []" > app.py
git add .
git commit -m "Initial commit"
```

### Task 1: Add Features (10 min)

```bash
# Feature 1: Add task
git switch -c feature/add-task
echo "def add_task(name): tasks.append(name)" >> app.py
git add app.py
git commit -m "Add task creation"

# Feature 2: Delete task (from main)
git switch main
git switch -c feature/delete-task
echo "def delete_task(index): tasks.pop(index)" >> app.py
git add app.py
git commit -m "Add task deletion"

# Feature 3: List tasks (from main)
git switch main
git switch -c feature/list-tasks
echo "def list_tasks(): print(tasks)" >> app.py
git add app.py
git commit -m "Add task listing"
```

### Task 2: Create Conflict (3 min)

```bash
# Update app.py on main
git switch main
echo "# Main application" >> app.py
git add app.py
git commit -m "Update main"

# Update app.py on feature (same line area)
git switch feature/add-task
echo "# Add task feature" >> app.py
git add app.py
git commit -m "Update add task"
```

### Task 3: Merge & Resolve (8 min)

```bash
# Merge features
git switch main

# This should work
git merge feature/delete-task

# This should work
git merge feature/list-tasks

# This will conflict!
git merge feature/add-task

# Resolve conflict:
# 1. Open app.py
# 2. Remove conflict markers
# 3. Keep both changes combined
# 4. Stage and commit
git add app.py
git commit -m "Merge add-task feature"
```

### Task 4: Practice Stash (3 min)

```bash
# Start new feature
git switch -c feature/search
echo "def search_task(keyword): pass" >> app.py

# Don't commit! Emergency bug fix needed:
git stash save "Search feature in progress"

# Fix bug on main
git switch main
echo "# Bug fix" >> README.md
git add README.md
git commit -m "Fix bug"

# Return to feature
git switch feature/search
git stash pop
git add app.py
git commit -m "Add search feature"
```

### Task 5: Clean History (3 min)

```bash
# Make messy commits
git switch -c feature/messy
echo "line 1" >> temp.txt
git add temp.txt
git commit -m "temp"

echo "line 2" >> temp.txt
git add temp.txt
git commit -m "more temp"

echo "line 3" >> temp.txt
git add temp.txt
git commit -m "final"

# Clean up with rebase
git rebase -i HEAD~3
# In editor: squash all into one commit
# Save with message "Add temp file"
```

### Task 6: Undo Mistake (2 min)

```bash
git switch main
echo "MISTAKE" >> app.py
git add app.py
git commit -m "Bad commit"

# Undo it safely
git revert HEAD
```

### Task 7: Cleanup (2 min)

```bash
# Merge remaining features
git switch main
git merge feature/search
git merge feature/messy

# Delete merged branches
git branch -d feature/add-task
git branch -d feature/delete-task
git branch -d feature/list-tasks
git branch -d feature/search
git branch -d feature/messy

# View final history
git log --oneline --graph --all
```

### Success Criteria

Complete the challenge if you successfully:
- ✅ Created 5 feature branches
- ✅ Made at least 10 commits
- ✅ Resolved 1 merge conflict
- ✅ Used stash to save work
- ✅ Used interactive rebase to clean history
- ✅ Used revert to undo a commit
- ✅ Deleted all merged branches
- ✅ Have clean commit history

**Time Limit:** 30 minutes

---

## Tips for Success

1. **Always branch from main** - Start each feature from updated main
2. **Commit often** - Small, focused commits are easier to manage
3. **Descriptive branch names** - Use `feature/`, `bugfix/`, `hotfix/` prefixes
4. **Pull before merge** - Update main before merging features
5. **Test before merge** - Make sure feature works before merging
6. **Delete merged branches** - Keep workspace clean

---

## Common Mistakes to Avoid

❌ Committing directly to main  
❌ Creating branches from other feature branches  
❌ Rebasing public/shared branches  
❌ Using `reset --hard` on pushed commits  
❌ Forgetting to pull before pushing  
❌ Keeping too many stale branches  

---

## Quick Decision Guide

**When to merge?** Feature is complete and tested  
**When to rebase?** Update feature branch with main changes  
**When to stash?** Need to switch branches with uncommitted changes  
**When to restore?** Want to discard uncommitted changes  
**When to reset?** Want to undo local commits (not pushed)  
**When to revert?** Want to undo pushed commits safely  

---

## Next Steps

After mastering these commands:
1. Practice the challenge 2-3 times
2. Apply to your own projects
3. Learn Pull Requests (Session 3)
4. Master team collaboration workflows

**End of Quick Reference**
