

### **Task: Feature Development and Bug Fixing**

#### **Scenario**
You are working on a project with two branches:
1. `main` - The stable branch.
2. `feature/login` - A feature branch where you’re developing a login functionality.

You will perform the following steps:

---

### **Step 1: Create a Repository and Initial Setup**
1. Initialize a new Git repository.
   ```bash
   git init
   ```
2. Create a `main` branch and commit an initial file (e.g., `README.md`).
   ```bash
   git checkout -b main
   echo "# My Project" > README.md
   git add README.md
   git commit -m "Initial commit"
   ```

---

### **Step 2: Create a Feature Branch**
1. Create and switch to a new branch `feature/login`.
   ```bash
   git checkout -b feature/login
   ```
2. Make multiple commits on this branch:
   - Commit 1: Add a login form.
     ```bash
     echo "<form>Login Form</form>" > login.html
     git add login.html
     git commit -m "Add login form"
     ```
   - Commit 2: Add CSS for the login form.
     ```bash
     echo "form { color: red; }" > styles.css
     git add styles.css
     git commit -m "Add CSS for login form"
     ```
   - Commit 3: Add JavaScript validation.
     ```bash
     echo "function validate() { console.log('Validated'); }" > script.js
     git add script.js
     git commit -m "Add JavaScript validation"
     ```

---

### **Step 3: Practice Git Reset**
1. Use `git reset --soft` to undo the last commit but keep changes in the staging area.
   ```bash
   git reset --soft HEAD~1
   ```
   - **When to use**: When you want to rework the last commit without losing changes.

2. Use `git reset --mixed` (default) to undo the last commit and unstage changes.
   ```bash
   git reset --mixed HEAD~1
   ```
   - **When to use**: When you want to unstage changes and re-commit them.

3. Use `git reset --hard` to completely discard the last commit and all changes.
   ```bash
   git reset --hard HEAD~1
   ```
   - **When to use**: When you want to discard all changes and go back to a previous state.

---

### **Step 4: Practice Git Rebase**
1. Rebase the `feature/login` branch onto `main` to incorporate any changes from `main`.
   ```bash
   git checkout main
   echo "Some changes in main" >> README.md
   git add README.md
   git commit -m "Update README in main"
   git checkout feature/login
   git rebase main
   ```
   - **When to use**: When you want to incorporate changes from another branch and maintain a linear history.

---

### **Step 5: Practice Git Merge**
1. Merge `feature/login` into `main`.
   ```bash
   git checkout main
   git merge feature/login
   ```
   - **When to use**: When you want to combine changes from a feature branch into the main branch.

---

### **Step 6: Practice Git Revert**
1. Revert the last commit on `main`.
   ```bash
   git revert HEAD
   ```
   - **When to use**: When you want to undo a commit by creating a new commit that reverses the changes.

---

### **Step 7: Practice Git Restore**
1. Make changes to a file and discard them using `git restore`.
   ```bash
   echo "Temporary changes" >> login.html
   git restore login.html
   ```
   - **When to use**: When you want to discard uncommitted changes in a file.

---

### **Step 8: Practice Git Cherry-Pick**
1. Create a new branch `feature/bugfix` and make a commit.
   ```bash
   git checkout -b feature/bugfix
   echo "Bug fix" >> bugfix.txt
   git add bugfix.txt
   git commit -m "Fix a bug"
   ```
2. Cherry-pick this commit into `main`.
   ```bash
   git checkout main
   git cherry-pick <commit-hash>
   ```
   - **When to use**: When you want to apply a specific commit from one branch to another.

---

### **Step 9: Practice Git Squash**
1. Squash the last three commits in `feature/login` into one commit.
   ```bash
   git checkout feature/login
   git rebase -i HEAD~3
   ```
   - In the interactive rebase editor, mark the first commit as `pick` and the others as `squash`.
   - **When to use**: When you want to combine multiple commits into one for a cleaner history.

---

### **Step 10: Practice Git Stash**
1. Make changes to a file and stash them.
   ```bash
   echo "Temporary work" >> login.html
   git stash
   ```
2. Apply the stashed changes later.
   ```bash
   git stash apply
   ```
   - **When to use**: When you want to temporarily save changes and switch branches.

---

### **Step 11: Delete a Feature Branch**
1. Delete the `feature/login` branch after merging it into `main`.
   ```bash
   git branch -d feature/login
   ```
   - **When to use**: When a feature branch is no longer needed after merging.

---

### **Summary of Commands and Their Uses**
- **`git reset`**: Undo commits (soft, mixed, hard).
- **`git rebase`**: Incorporate changes from another branch and maintain a linear history.
- **`git merge`**: Combine changes from one branch into another.
- **`git revert`**: Undo a commit by creating a new commit.
- **`git restore`**: Discard uncommitted changes in a file.
- **`git cherry-pick`**: Apply a specific commit from one branch to another.
- **`git squash`**: Combine multiple commits into one.
- **`git stash`**: Temporarily save changes.
- **`git branch -d`**: Delete a feature branch after merging.

