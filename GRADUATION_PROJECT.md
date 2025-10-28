# 🎓 Git & GitHub Workshop: Graduation Project

**Project:** Build, Deploy, and Contribute to a Personal Portfolio Website

**Objective:** Apply all the skills you've learned across our four sessions to create a professional developer portfolio, managed with Git and hosted on GitHub Pages.ls

**Deadline:** November 4, 2025, 11:59 PM

---

## 📋 Core Requirements

You must complete the following tasks. Use your Git history to document your progress. Your commit messages should be clear and follow best practices.

### **Part 1: Project Setup & Initial Content (Session 1 Skills)**

1.  **Create a New Local Repository:** Name it `dev-portfolio`.
2.  **Initialize Git:** Start tracking the project with Git.
3.  **Create Initial Files:**
    *   `index.html` (Your portfolio's main page)
    *   `style.css` (For styling)
    *   `.gitignore` (Add patterns for `.vscode/`, `node_modules/`, `.env`, etc.)
4.  **Make Your First Commits:**
    *   Commit 1: "Initial commit with project structure"
    *   Commit 2: "Add basic HTML structure to index.html"
    *   Commit 3: "Add .gitignore for common files"
5.  **Push to GitHub:**
    *   Create a new **public** repository on GitHub with the same name (`dev-portfolio`).
    *   Connect your local repository to the remote.
    *   Push your `main` branch to GitHub.

### **Part 2: Adding Features with Branches (Session 2 Skills)**

1.  **Create Feature Branches:**
    *   Create a branch named `feature/add-projects-section`.
    *   Create another branch named `feature/add-about-me-section`.
2.  **Develop in Branches:**
    *   On the `projects` branch, add a "My Projects" section to `index.html`.
    *   On the `about-me` branch, add an "About Me" section to `index.html`.
3.  **Merge Branches:**
    *   Merge the `about-me` branch into `main`.
    *   Merge the `projects` branch into `main`. You will likely encounter a **merge conflict**. Resolve it manually.
4.  **Visualize Your History:**
    *   Generate a graph of your commit history and save a screenshot of the output. Add it to a new `docs/` folder in your project.
    *   `git log --oneline --graph --decorate --all`

### **Part 3: Professional Workflows (Session 3 Skills)**

1.  **Create a Release:**
    *   Once `main` is stable, create an annotated tag `v1.0.0` for your initial version.
    *   Push the tag to GitHub.
    *   On GitHub, create a **Release** from this tag.
2.  **Simulate a Hotfix:**
    *   Create a branch `hotfix/fix-styling-bug` from `main`.
    *   Make a small change in `style.css` and commit it.
    *   Merge the hotfix back into `main`.
    *   Tag this new commit as `v1.0.1`.
3.  **Cherry-Pick the Fix:**
    *   Imagine you have a `develop` branch that also needs this fix. Create a `develop` branch from an older commit on `main`.
    *   **Cherry-pick** the hotfix commit from `main` into your `develop` branch.

### **Part 4: Automation & Open Source (Session 4 Skills)**

1.  **Set Up GitHub Pages:**
    *   Enable GitHub Pages in your repository settings to serve from the `main` branch.
    *   Your portfolio should now be live at `https://<your-username>.github.io/dev-portfolio/`.
2.  **Automate with GitHub Actions:**
    *   Create a workflow file `.github/workflows/main.yml`.
    *   This action should automatically run on every push to `main`.
    *   **Job:** Add a simple job that prints a message, like "Deploying website...". (A real-world example would be running tests or building assets).
3.  **Contribute to an "Open Source" Project:**
    *   Go to this repository: `https://github.com/OmarElzero/Github-Course/tree/master/community-themes`.
    *   **Fork** the repository.
    *   In your fork, add a new CSS file with a simple color theme (e.g., `dark-theme.css`).
    *   Submit a **Pull Request** to the original `community-themes` repository to add your theme.

---

## 🏆 Submission

To submit your project, post a link to your `dev-portfolio` GitHub repository in the designated submission channel.

We will review:
-   **Commit History:** Quality and clarity of messages.
-   **Branching Strategy:** Correct use of feature and hotfix branches.
-   **Pull Requests:** Your contribution to the community project.
-   **GitHub Actions:** A successful workflow run.
-   **GitHub Pages:** Your live portfolio website.

Good luck, and show us what you've learned!
