# 🚀 Source Control + Maven Hands-On Guide

This guide provides step-by-step instructions to set up a Maven-based Java project with Git integration and build automation.

---

## 🧩 Prerequisites

* Java JDK (v11+ recommended)
* Maven installed (`mvn -v` to verify)
* Git installed (`git --version`)

---

## 📁 Part 1: Source Control Integration (GitHub + Git)

### ✅ Step 1: Create Project Directory

```bash
mkdir my-maven-demo && cd my-maven-demo
git init
```

### ✅ Step 2: Add `.gitignore`

Create a `.gitignore` file with the following content:

```bash
target/
.idea/
*.iml
*.log
*.class
```

### ✅ Step 3: Create GitHub Repo and Connect

1. Create a new GitHub repository: `my-maven-demo`
2. Link to GitHub:

```bash
git remote add origin https://github.com/YOUR_USERNAME/my-maven-demo.git
```

3. Make initial commit:

```bash
git add .
git commit -m "Initial commit"
git push -u origin master
```

---

## 📦 Part 2: Overview of Apache Maven

### What is Maven?

* A **build automation tool** primarily for Java projects.
* Manages dependencies, compiles code, runs tests, and packages deliverables.
* Uses `pom.xml` to define project structure and configuration.

---

## ⚙️ Part 3: Installing and Configuring Maven

### ✅ Step 1: Verify Maven Installation

```bash
mvn -v
```

If not installed, download and install from:
👉 [https://maven.apache.org/download.cgi](https://maven.apache.org/download.cgi)

### ✅ Step 2: Generate Maven Project

```bash
git clone https://github.com/cloud-dev-user/javademo.git
```

### ✅ Step 3: Project Structure

```
my-maven-app/
├── pom.xml
└── src/
    ├── main/java/com/demo/app/App.java
    └── test/java/com/demo/app/AppTest.java
```

---

## 🔧 Part 4: Working with Maven Build

### ✅ Step 1: Compile the Code

```bash
mvn compile
```

### ✅ Step 2: Run Unit Tests

```bash
mvn test
```

### ✅ Step 3: Package the Application

```bash
mvn package
```

This generates a `.jar` file in the `target/` directory.

---

## ✅ Git + Maven Workflow

```bash
git status
git add .
git commit -m "Added initial Maven project with App.java"
git push origin master
```

Make a change in the Java code, then repeat `mvn package` and push changes.

---

## 🧪 Try It Yourself

### Step 1: Edit `App.java`

```java
public class App {
    public static void main(String[] args) {
        System.out.println("Hello, Maven World!");
    }
}
```

### Step 2: Run App from JAR

```bash
java -cp target/my-maven-app-1.0-SNAPSHOT.jar com.demo.app.App
```

---

## 📌 Common Maven Commands

| Command               | Purpose                     |
| --------------------- | --------------------------- |
| `mvn clean`           | Remove `target/` folder     |
| `mvn compile`         | Compile source code         |
| `mvn test`            | Run unit tests              |
| `mvn package`         | Package into `.jar`         |
| `mvn install`         | Install to local Maven repo |
| `mvn dependency:tree` | View dependency hierarchy   |

---

## 📂 Final Project Tree

```
my-maven-app/
├── pom.xml
├── .gitignore
├── src/
│   ├── main/java/com/demo/app/App.java
│   └── test/java/com/demo/app/AppTest.java
├── target/
```
