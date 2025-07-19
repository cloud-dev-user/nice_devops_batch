**GitHub-compatible hands-on guide** for:

# ☁️ Building a Java Project using **AWS CodeBuild**

This guide helps you integrate AWS CodeBuild to compile, test, and package a **Java Maven project** in a fully automated CI pipeline.

---

## 🧩 Prerequisites

| Requirement          | Description                     |
| -------------------- | ------------------------------- |
| AWS Account          | With access to CodeBuild & S3   |
| AWS CLI              | Installed and configured        |
| Git                  | Installed locally               |
| Java Project         | Maven-based (`pom.xml` present) |
| S3 Bucket (optional) | To store build artifacts        |

---

## 🚀 Part 1: Prepare a Sample Java Maven Project

```bash
git clone https://github.com/aws-samples/aws-codebuild-samples.git
cd aws-codebuild-samples/java/simple-java-maven
```

Or create your own Maven project:

```bash
mvn archetype:generate -DgroupId=com.demo -DartifactId=myapp \
  -DarchetypeArtifactId=maven-archetype-quickstart -DinteractiveMode=false
cd myapp
```

---

## 📁 Project Structure

```
myapp/
├── src/
├── pom.xml
├── buildspec.yml   <-- Add this file
```

---

## 🛠️ Part 2: Add `buildspec.yml` for CodeBuild

Create a file named `buildspec.yml` in the root of your project:

```yaml
version: 0.2

phases:
  install:
    runtime-versions:
      java: corretto11
    commands:
      - echo Installing dependencies...
      - mvn install -DskipTests=true
  build:
    commands:
      - echo Building the project...
      - mvn package
  post_build:
    commands:
      - echo Build completed.

artifacts:
  files:
    - target/*.jar
```

---

## 🧰 Part 3: Create CodeBuild Project (Console or CLI)

### 🔹 Option 1: Console

1. Go to **AWS CodeBuild → Create project**
2. Project Name: `java-maven-build`
3. Source: GitHub (connect your repo)
4. Environment:

   * Managed image
   * OS: Ubuntu
   * Runtime: `Standard`
   * Image: `aws/codebuild/standard:5.0` (or latest)
   * Privileged: No
5. Buildspec: Use buildspec.yml from source
6. Artifacts:

   * Type: No artifacts or Amazon S3

Click **Create build project** → **Start build**

---

### 🔹 Option 2: AWS CLI

```bash
aws codebuild create-project \
  --name java-maven-build \
  --source type=GITHUB,location=https://github.com/YOUR_USERNAME/YOUR_REPO \
  --artifacts type=NO_ARTIFACTS \
  --environment type=LINUX_CONTAINER,computeType=BUILD_GENERAL1_SMALL,image=aws/codebuild/standard:5.0,environmentVariables=[] \
  --service-role arn:aws:iam::YOUR_ACCOUNT_ID:role/service-role/codebuild-role
```

---

## 🔄 Part 4: Trigger Build

* Push code to GitHub:

```bash
git add .
git commit -m "Add buildspec for CodeBuild"
git push origin master
```

* Go to CodeBuild → Start build
* Monitor logs in **Phases** tab

---

## 📦 Output

If successful, you’ll see:

* JAR built in `target/` directory
* Optional: Artifact stored in S3 if configured

---

## 🧪 Testing Locally (Optional)

Install CodeBuild local agent (Docker-based):
👉 [https://docs.aws.amazon.com/codebuild/latest/userguide/use-codebuild-agent.html](https://docs.aws.amazon.com/codebuild/latest/userguide/use-codebuild-agent.html)

```bash
codebuild_build.sh -i aws/codebuild/standard:5.0 -a /tmp/artifacts
```

---

## ✅ Summary

You learned how to:

* Build a Java project using Maven
* Use AWS CodeBuild to automate compilation and packaging
* Track and debug builds via CodeBuild console

---

## 🧠 Bonus: Add Test Phase and S3 Artifacts

Update `buildspec.yml`:

```yaml
phases:
  build:
    commands:
      - mvn test
      - mvn package

artifacts:
  files:
    - target/*.jar
  discard-paths: yes
  base-directory: target
```
