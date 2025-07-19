**step-by-step Git hands-on project** that walks through GIT concept

---

# 🧪 Complete Git Hands-On Project  
### 🚀 Project Title: `vinsys-product-app`  
**Goal**: Simulate a collaborative software development project using Git and GitHub

---

## 🧰 Setup and Initialization

### 1️⃣ Create and Initialize Repository
```bash
mkdir Fin-product-app
cd Fin-product-app
git init
```
✅ Creates `.git` directory and starts tracking.

---

## ✍️ Stage and Commit

### 2️⃣ Add Basic Files and Stage
```bash
echo "<!DOCTYPE html>" > index.html
echo "# Product Info" > README.md

git status        # See untracked files
git add index.html README.md
git commit -m "Initial commit with homepage and readme"
```

---

## 🌿 Branching and Feature Development

### 3️⃣ Create a Branch and Add Feature
```bash
git checkout -b feature-login
echo "<form>Login</form>" >> index.html
git commit -am "Add login form"
```

### 4️⃣ List, Delete, Switch Branches
```bash
git branch                # List local branches
git branch -a             # All branches (remote + local)
git checkout main         # Switch back
git branch -D feature-login   # Delete local branch
```

---

## 🔀 Merging

### 5️⃣ Merge Feature into Main (Fast-Forward)
```bash
git checkout -b feature-header
echo "<header>Welcome</header>" >> index.html
git commit -am "Add header"

git checkout main
git merge feature-header   # Fast-forward merge
```

---

## ⚔️ Merge Conflicts (Recursive Strategy)

### 6️⃣ Simulate Conflict
```bash
git checkout -b feature-footer
echo "<footer>Footer A</footer>" >> index.html
git commit -am "Add footer A"

git checkout main
git checkout -b feature-footer-b
echo "<footer>Footer B</footer>" >> index.html
git commit -am "Add footer B"

git checkout main
git merge feature-footer          # Merge A
git merge feature-footer-b        # Conflicts with B
```

### 🔧 Resolve and Commit
```bash
# Edit index.html manually to fix conflict
git add index.html
git commit -m "Resolved footer conflict"
```

---

## 🧼 Undoing Commits

### 7️⃣ Checkout to Restore File
```bash
git checkout HEAD index.html
```

### 🔁 Revert a Commit (Safe Undo)
```bash
git log --oneline
git revert <commit-hash>
```

### 🧨 Reset to Remove Commits
```bash
git reset --soft HEAD~1     # Keeps changes staged
git reset --mixed HEAD~1    # Keeps changes but unstaged
git reset --hard HEAD~1     # Deletes everything (use cautiously)
```

---

## 📦 Stashing

### 8️⃣ Save and Restore Work
```bash
git stash           # Save uncommitted changes
git stash list      # See stash list
git stash apply     # Restore latest stash
git stash pop       # Restore and remove
```

---

## 🔁 Rebase

### 9️⃣ Linearize History Before Merge
```bash
git checkout -b feature-search
echo "<input>" >> index.html
git commit -am "Add search bar"

git checkout main
git pull            # Get latest main

git checkout feature-search
git rebase main     # Replay commits over updated main
```

---

## 🍒 Cherry-pick

### 🔟 Apply Specific Commit
```bash
git log --oneline   # Find commit to reuse
git checkout main
git cherry-pick <commit-hash>
```

---

## 🏷️ Tagging Releases

### 🔖 Create Tags
```bash
git tag v1.0
git tag -a v1.0 -m "Initial release"
git tag              # List tags
git push origin v1.0
```

---

## 🐙 GitHub Integration

### 👤 Sign In / Create Repo
- Sign up: [https://github.com](https://github.com)
- Create repo: `vinsys-product-app`

### 📤 Push Local Code
```bash
git remote add origin https://github.com/<your-username>/vinsys-product-app.git
git push -u origin main
```

### 🔄 Clone for Team Member
```bash
git clone https://github.com/<your-username>/vinsys-product-app.git
```

### 📎 Create Alias for URL
```bash
git remote add vinsys https://github.com/<your-username>/vinsys-product-app.git
git push vinsys main
```

---

## 🔄 Synchronizing Updates

### 🔃 Fetch vs Pull
```bash
git fetch origin      # Download without merge
git pull origin main  # Download and merge
```

---

## 🧪 Repo Integrity

### 📋 fsck – Check Consistency
```bash
git fsck
```

> Example Output:
```
dangling commit a1b2c3...
```

### 🛠 Recovery
```bash
git cat-file -p a1b2c3...
git branch recovered a1b2c3...
```

---

### 🧹 gc – Clean Up Repository
```bash
git gc
```

---

## 🪝 Git Hooks

### ✨ Pre-commit Hook Example
```bash
echo '#!/bin/sh
echo "Checking before commit..."' > .git/hooks/pre-commit
chmod +x .git/hooks/pre-commit
```

---

## 🚫 .gitignore

### 📁 Add a .gitignore File
```bash
echo "node_modules/" > .gitignore
echo "*.log" >> .gitignore
echo ".env" >> .gitignore
git add .gitignore
git commit -m "Add .gitignore to exclude unnecessary files"
```

---

## ✅ Project Summary Flow

```text
init → branch → add/commit → stash → rebase → merge/conflict → tag →
push/pull → revert/reset → fsck/gc → hook → ignore
```
