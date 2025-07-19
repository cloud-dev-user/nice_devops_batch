Here is a **GitHub-compatible, markdown-based hands-on guide** for your topic:
**“Source Control Integration + NPM Overview, Installation, and Build Workflow”**
You can directly add this as a `README.md` file in a training or demo repository.

---

# 🚀 Source Control + NPM Hands-On Guide

This guide will walk you through integrating Git for source control, understanding and using Node Package Manager (npm), and working with npm build scripts in a project environment.

---

## 🧩 Prerequisites

* Node.js installed (v18+ recommended)
* Git installed (v2.30+)
* Basic understanding of CLI and JavaScript

---

## 📁 Part 1: Source Control Integration (GitHub + Git)

### ✅ Step 1: Initialize Git

```bash
mkdir my-npm-demo && cd my-npm-demo
git init
```

### ✅ Step 2: Create `.gitignore`

Create a file named `.gitignore` and add the following:

```bash
node_modules/
.env
dist/
```

### ✅ Step 3: Connect to GitHub

1. Create a new repository on GitHub (e.g., `my-npm-demo`).
2. Add the remote:

```bash
git remote add origin https://github.com/YOUR_USERNAME/my-npm-demo.git
```

3. Make your first commit:

```bash
git add .
git commit -m "Initial commit"
git push -u origin master
```

---

## 📦 Part 2: Overview of Node Package Manager (npm)

### What is npm?

* **npm (Node Package Manager)** is the default package manager for Node.js.
* Used to manage dependencies and scripts for JavaScript/Node applications.

---

## ⚙️ Part 3: Installing and Configuring npm

### ✅ Step 1: Initialize npm

```bash
npm init -y
```

This will create a `package.json` file with default values.

### ✅ Step 2: Add Dependencies

```bash
npm install express
```

### ✅ Step 3: Add Dev Dependencies

```bash
npm install nodemon --save-dev
```

Now, `package.json` includes:

```json
"dependencies": {
  "express": "^4.18.2"
},
"devDependencies": {
  "nodemon": "^3.0.1"
}
```

---

## 🔧 Part 4: Working with npm Build Scripts

### ✅ Step 1: Create Simple Express App

Create `index.js`:

```js
const express = require('express');
const app = express();
const PORT = process.env.PORT || 3000;

app.get('/', (req, res) => res.send('Hello, NPM World!'));

app.listen(PORT, () => console.log(`Server running on port ${PORT}`));
```

### ✅ Step 2: Add Scripts to `package.json`

```json
"scripts": {
  "start": "node index.js",
  "dev": "nodemon index.js",
  "build": "echo 'No build step yet - just a placeholder'"
}
```

### ✅ Step 3: Run Scripts

```bash
npm run start   # Run the app normally
npm run dev     # Run the app in dev mode with auto-reload
npm run build   # Run the placeholder build command
```

---

## ✅ Git + npm Together

### Step-by-Step Workflow:

```bash
# Work on code
git add .
git commit -m "Added express server and npm scripts"
npm install new-package
git add package.json package-lock.json
git commit -m "Added new-package to project"
git push origin master
```

---

## 📂 Final Project Structure

```
my-npm-demo/
├── .gitignore
├── index.js
├── package.json
├── package-lock.json
└── node_modules/
```

---

## 📌 Bonus: Common Commands

| Command               | Purpose                        |
| --------------------- | ------------------------------ |
| `npm install`         | Install all dependencies       |
| `npm update`          | Update packages                |
| `npm uninstall <pkg>` | Remove a package               |
| `npm run <script>`    | Run a script from package.json |
| `npm list`            | List installed packages        |

---

## 🧪 Try it Yourself

Create a new branch:

```bash
git checkout -b feature/add-route
```

Add another route in `index.js`:

```js
app.get('/about', (req, res) => res.send('About Page'));
```

Commit & push:

```bash
git add index.js
git commit -m "Added /about route"
git push origin feature/add-route
```

Open a Pull Request on GitHub for review.

---
