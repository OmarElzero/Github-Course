# Session 4 — Open Source & Advanced GitHub (Quick Reference)

**Duration:** 30-40 minutes  
**Format:** Summary + Commands + Challenge

---

## Quick Summary

**Open source contribution** involves forking repositories, making improvements, and submitting Pull Requests. Advanced GitHub features include Actions (CI/CD), Pages (hosting), and Submodules (dependencies).

**Key Concepts:**
- Fork = your copy of someone's repository
- Upstream = original repository
- GitHub Actions = automated workflows
- GitHub Pages = free website hosting
- Submodules = Git repositories inside repositories

---

## Part 1: Forking & Contributing

### Fork Workflow

```bash
# 1. Fork on GitHub (click Fork button)

# 2. Clone YOUR fork
git clone https://github.com/YOUR-USERNAME/project.git
cd project

# 3. Add upstream remote
git remote add upstream https://github.com/ORIGINAL-OWNER/project.git

# 4. Verify remotes
git remote -v
# origin    -> your fork
# upstream  -> original repo

# 5. Create feature branch
git switch -c fix-bug

# 6. Make changes and commit
git add .
git commit -m "Fix bug in validation logic"

# 7. Push to YOUR fork
git push origin fix-bug

# 8. Create Pull Request on GitHub
# Go to original repo and click "New Pull Request"
```

### Keeping Fork Updated

```bash
# Fetch upstream changes
git fetch upstream

# Update your main branch
git switch main
git merge upstream/main

# Push updates to your fork
git push origin main

# Update feature branch with latest main
git switch fix-bug
git rebase main
```

---

## Part 2: Open Source Best Practices

### Before Contributing

**Research:**
1. Read `CONTRIBUTING.md`
2. Check `CODE_OF_CONDUCT.md`
3. Review existing issues/PRs
4. Check if issue is already reported
5. Read project style guide

### Good Contribution

**Issue Report:**
```markdown
## Bug Description
Clear description of the problem

## Steps to Reproduce
1. Do this
2. Then this
3. See error

## Expected Behavior
What should happen

## Actual Behavior
What actually happens

## Environment
- OS: Ubuntu 22.04
- Python: 3.10
- Version: 1.2.3
```

**Pull Request:**
```markdown
## Description
Fixes #123 - Add input validation

## Changes Made
- Added email validation
- Added phone number validation
- Updated tests

## Testing
- [ ] All tests pass
- [ ] Added new tests
- [ ] Manually tested

## Screenshots (if applicable)
```

---

## Part 3: GitHub Actions

### What are GitHub Actions?

Automated workflows that run on specific events:
- Push to repository
- Pull request created
- Schedule (cron)
- Manual trigger

### Basic Workflow Structure

**File:** `.github/workflows/test.yml`

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
    
    steps:
    - name: Checkout code
      uses: actions/checkout@v3
    
    - name: Set up Python
      uses: actions/setup-python@v4
      with:
        python-version: '3.10'
    
    - name: Install dependencies
      run: |
        pip install -r requirements.txt
    
    - name: Run tests
      run: |
        pytest
```

### Common Workflow Examples

**Python Testing:**
```yaml
name: Python Tests

on: [push, pull_request]

jobs:
  test:
    runs-on: ubuntu-latest
    strategy:
      matrix:
        python-version: [3.8, 3.9, '3.10']
    
    steps:
    - uses: actions/checkout@v3
    - name: Set up Python ${{ matrix.python-version }}
      uses: actions/setup-python@v4
      with:
        python-version: ${{ matrix.python-version }}
    - name: Install dependencies
      run: pip install pytest
    - name: Test
      run: pytest
```

**Auto Deploy:**
```yaml
name: Deploy

on:
  push:
    branches: [ main ]

jobs:
  deploy:
    runs-on: ubuntu-latest
    steps:
    - uses: actions/checkout@v3
    - name: Deploy to server
      run: |
        echo "Deploying to production..."
```

### Workflow Commands

```bash
# Workflows are triggered automatically
# View in GitHub: Actions tab

# Local testing (use act tool)
act -l  # List workflows
act     # Run workflows locally
```

---

## Part 4: GitHub Pages

### What is GitHub Pages?

Free static website hosting directly from your repository.

### Setup Options

**Option 1: User/Organization Site**
- Repository name: `username.github.io`
- URL: `https://username.github.io`

**Option 2: Project Site**
- Any repository
- URL: `https://username.github.io/repo-name`

### Quick Setup

```bash
# 1. Create/clone repository
git clone https://github.com/username/repo.git
cd repo

# 2. Create index.html
cat > index.html << 'EOF'
<!DOCTYPE html>
<html>
<head>
    <title>My Project</title>
</head>
<body>
    <h1>Welcome to My Project</h1>
    <p>This is hosted on GitHub Pages!</p>
</body>
</html>
EOF

# 3. Commit and push
git add index.html
git commit -m "Add GitHub Pages site"
git push

# 4. Enable Pages in Settings
# Repo → Settings → Pages
# Source: main branch
# Save
```

### Using Jekyll (Static Site Generator)

```bash
# Create _config.yml
echo "title: My Project" > _config.yml
echo "theme: jekyll-theme-minimal" >> _config.yml

# Create index.md
cat > index.md << 'EOF'
# Welcome

This is my project documentation.

## Features
- Feature 1
- Feature 2
EOF

# Push
git add _config.yml index.md
git commit -m "Add Jekyll site"
git push
```

### Custom Domain

```bash
# Create CNAME file
echo "www.yourdomain.com" > CNAME
git add CNAME
git commit -m "Add custom domain"
git push

# Configure DNS:
# Add CNAME record: www -> username.github.io
```

---

## Part 5: Git Submodules

### What are Submodules?

Git repositories nested inside other repositories. Used for managing dependencies.

### Adding Submodules

```bash
# Add submodule
git submodule add https://github.com/user/library.git libs/library

# This creates:
# - libs/library/ directory
# - .gitmodules file

# Commit
git add .gitmodules libs/library
git commit -m "Add library submodule"
git push
```

### Cloning Repository with Submodules

```bash
# Option 1: Clone then initialize
git clone https://github.com/user/project.git
cd project
git submodule init
git submodule update

# Option 2: Clone with submodules
git clone --recurse-submodules https://github.com/user/project.git
```

### Updating Submodules

```bash
# Update to latest commit
git submodule update --remote

# Update specific submodule
git submodule update --remote libs/library

# Commit the update
git add libs/library
git commit -m "Update library submodule"
git push
```

### Removing Submodules

```bash
# 1. Remove from .gitmodules
git config -f .gitmodules --remove-section submodule.libs/library

# 2. Stage .gitmodules
git add .gitmodules

# 3. Remove from .git/config
git config -f .git/config --remove-section submodule.libs/library

# 4. Remove cached files
git rm --cached libs/library

# 5. Remove directory
rm -rf libs/library

# 6. Remove from .git/modules
rm -rf .git/modules/libs/library

# 7. Commit
git commit -m "Remove library submodule"
```

---

## Part 6: Repository Security

### Security Best Practices

**1. Never commit secrets:**
```bash
# Use .gitignore
echo ".env" >> .gitignore
echo "config.ini" >> .gitignore
echo "secrets.json" >> .gitignore
```

**2. Use environment variables:**
```python
import os
API_KEY = os.getenv('API_KEY')
```

**3. Enable GitHub security features:**
- Dependabot alerts
- Code scanning
- Secret scanning
- Branch protection rules

### Branch Protection Rules

**GitHub → Settings → Branches → Add rule:**

- ✅ Require pull request reviews
- ✅ Require status checks to pass
- ✅ Require branches to be up to date
- ✅ Include administrators
- ✅ Restrict who can push

### Scanning for Secrets

```bash
# Use git-secrets tool
git clone https://github.com/awslabs/git-secrets.git
cd git-secrets
make install

# Install hooks in repo
git secrets --install

# Scan repository
git secrets --scan-history
```

---

## Complete Command Reference

```bash
# FORKING
# (Fork on GitHub first, then:)
git clone https://github.com/YOU/project.git     # Clone fork
git remote add upstream URL                      # Add upstream
git fetch upstream                                # Fetch upstream
git merge upstream/main                           # Merge upstream

# SUBMODULES
git submodule add URL path                       # Add submodule
git submodule init                               # Initialize submodules
git submodule update                             # Update submodules
git clone --recurse-submodules URL               # Clone with submodules
git submodule update --remote                    # Update to latest
git submodule foreach git pull origin main       # Pull in all submodules

# GITHUB PAGES
# Enable in: Settings → Pages → Source: main branch
# Access at: https://username.github.io/repo

# ACTIONS
# Create: .github/workflows/name.yml
# View: Actions tab on GitHub

# SECURITY
git secrets --scan                               # Scan for secrets
git log -p -S "password"                         # Find string in history
git filter-branch --tree-filter 'rm -f file'     # Remove file from history
```

---

## Comprehensive Challenge (40 minutes)

### Task: Contribute to Open Source & Set Up CI/CD

**Part 1: Fork and Contribute (15 min)**

1. **Find a project or create mock repository**
   ```bash
   # Create a simple Python project
   mkdir open-source-demo
   cd open-source-demo
   git init
   
   # Create app.py with a deliberate bug
   cat > app.py << 'EOF'
   def calculate_average(numbers):
       return sum(numbers) / len(numbers)  # Bug: no check for empty list
   
   if __name__ == "__main__":
       print(calculate_average([1, 2, 3, 4, 5]))
   EOF
   
   git add app.py
   git commit -m "Initial commit"
   ```

2. **Fork workflow**
   ```bash
   # Push to GitHub as "original" repo
   # Fork it (or simulate with another clone)
   # Clone your fork
   # Add upstream remote
   ```

3. **Fix the bug**
   ```bash
   git switch -c fix-division-by-zero
   
   # Fix app.py
   cat > app.py << 'EOF'
   def calculate_average(numbers):
       if not numbers:
           return 0
       return sum(numbers) / len(numbers)
   
   if __name__ == "__main__":
       print(calculate_average([1, 2, 3, 4, 5]))
       print(calculate_average([]))  # Test empty list
   EOF
   
   git add app.py
   git commit -m "Fix division by zero for empty lists"
   git push origin fix-division-by-zero
   # Create PR on GitHub
   ```

**Part 2: GitHub Actions (15 min)**

1. **Create test workflow**
   ```bash
   mkdir -p .github/workflows
   
   cat > .github/workflows/test.yml << 'EOF'
   name: Python Tests
   
   on: [push, pull_request]
   
   jobs:
     test:
       runs-on: ubuntu-latest
       steps:
       - uses: actions/checkout@v3
       - name: Set up Python
         uses: actions/setup-python@v4
         with:
           python-version: '3.10'
       - name: Run application
         run: python app.py
   EOF
   
   git add .github/workflows/test.yml
   git commit -m "Add CI workflow"
   git push
   ```

2. **Verify workflow runs**
   - Go to Actions tab
   - Check workflow execution
   - Fix any failures

**Part 3: GitHub Pages (10 min)**

1. **Create documentation site**
   ```bash
   mkdir docs
   cd docs
   
   cat > index.html << 'EOF'
   <!DOCTYPE html>
   <html>
   <head>
       <title>Average Calculator</title>
       <style>
           body { font-family: Arial; margin: 40px; }
           code { background: #f4f4f4; padding: 2px 5px; }
       </style>
   </head>
   <body>
       <h1>Average Calculator</h1>
       <p>A simple Python utility for calculating averages.</p>
       
       <h2>Usage</h2>
       <pre><code>from app import calculate_average
   
   result = calculate_average([1, 2, 3, 4, 5])
   print(result)  # 3.0</code></pre>
       
       <h2>Features</h2>
       <ul>
           <li>Handles empty lists safely</li>
           <li>Returns 0 for empty input</li>
       </ul>
   </body>
   </html>
   EOF
   
   cd ..
   git add docs/
   git commit -m "Add documentation site"
   git push
   ```

2. **Enable GitHub Pages**
   - Settings → Pages
   - Source: main branch, /docs folder
   - Save and visit your site!

**Success Criteria:**
- ✅ Fork created and cloned
- ✅ Bug fixed with descriptive commit
- ✅ Pull Request created
- ✅ GitHub Actions workflow running
- ✅ Tests passing
- ✅ GitHub Pages site live
- ✅ Documentation accessible

---

## Real-World Contribution Tips

**Finding Projects:**
- GitHub Explore
- "good first issue" label
- "help wanted" label
- Your own dependencies with bugs

**Making Impact:**
- Start small (docs, tests)
- Fix bugs you encounter
- Add missing features
- Improve error messages
- Write examples

**Etiquette:**
- Be respectful and patient
- Follow project guidelines
- Respond to feedback
- Don't demand attention
- Thank maintainers

**Building Reputation:**
- Consistent contributions
- Quality over quantity
- Help others in issues
- Write good documentation
- Maintain your own projects

---

## Advanced GitHub Features

**GitHub Discussions:**
- Community forum
- Q&A, ideas, announcements

**GitHub Projects:**
- Kanban boards
- Issue tracking
- Roadmap planning

**GitHub Sponsors:**
- Support maintainers
- Receive funding

**GitHub Packages:**
- Host packages (npm, pip, etc.)
- Private registries

**GitHub Codespaces:**
- Cloud development environment
- VS Code in browser

---

## What You've Learned

✅ Forking and contributing to open source  
✅ Maintaining fork sync with upstream  
✅ Creating GitHub Actions workflows  
✅ Setting up CI/CD pipelines  
✅ Building sites with GitHub Pages  
✅ Managing dependencies with submodules  
✅ Repository security best practices  
✅ Professional open source contribution  

**You're now ready to contribute to real projects!**

---

**End of Session 4 Quick Reference**
