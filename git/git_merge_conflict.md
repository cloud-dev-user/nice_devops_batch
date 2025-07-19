# ⚔️ Git Merge Conflict Resolution — Full Hands-on Lab

### 💼 Scenario: Two developers modified the same line of `index.html` in separate branches. You will simulate the conflict and resolve it manually.

---

### 🧰 Setup and Initial Commit

```bash
mkdir vinsys-conflict-lab
cd vinsys-conflict-lab
git init
echo "<h1>Welcome</h1>" > index.html
git add index.html
git commit -m "Initial homepage"
```

---

### 🌿 Create Two Feature Branches with Conflicting Changes

```bash
# Branch A — adds a login form
git checkout -b feature-login
echo "<form>Login</form>" >> index.html
git commit -am "Add login form"

# Branch B — adds a signup form
git checkout main
git checkout -b feature-signup
echo "<form>Signup</form>" >> index.html
git commit -am "Add signup form"
```

> Both branches modified `index.html` → will result in conflict upon merging.

---

### 🔁 Merge with Conflict

```bash
git checkout main
git merge feature-login       # Fast-forward — no issues
git merge feature-signup      # Conflict occurs here
```

---

### ❌ Expected Output

```text
Auto-merging index.html
CONFLICT (content): Merge conflict in index.html
Automatic merge failed; fix conflicts and then commit the result.
```

---

### 📂 File Contents Now — `index.html` will show:

```html
<h1>Welcome</h1>
<<<<<<< HEAD
<form>Login</form>
=======
<form>Signup</form>
>>>>>>> feature-signup
```

---

## 🛠 Step-by-Step Conflict Resolution

### ✏️ 1. Open `index.html` in your editor.

Decide which content to keep, or merge both like this:

```html
<h1>Welcome</h1>
<form>Login</form>
<form>Signup</form>
```

### 🧼 2. Remove all conflict markers:  
Remove `<<<<<<<`, `=======`, and `>>>>>>>` lines.

---

### 💾 3. Stage the Resolved File
```bash
git add index.html
```

### ✅ 4. Commit the Merge
```bash
git commit -m "Merged feature-signup with resolved index.html conflict"
```

---

## 🔍 Final Verification

```bash
git log --oneline
git status
```
Your codebase now includes both login and signup features, and the conflict has been resolved properly.

