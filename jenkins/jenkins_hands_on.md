---

# Jenkins Hands-On Lab: Install with Docker + Configure + Create & Run Pipeline

---

## 🛠️ Step 1: Install Jenkins using Docker

Run Jenkins official container:

```bash
docker run -d -p 8080:8080 -p 50000:50000 \
  -v jenkins_home:/var/jenkins_home \
  --name jenkins \
  jenkins/jenkins:lts
```

* Jenkins UI: [http://localhost:8080](http://localhost:8080)

### Unlock Jenkins

Get initial admin password:

```bash
docker exec jenkins cat /var/jenkins_home/secrets/initialAdminPassword
```

Paste this on Jenkins UI to unlock.

---

## ⚙️ Step 2: Configure Jenkins

1. **Install suggested plugins**
2. **Create your admin user**
3. Go to **Manage Jenkins → Global Tool Configuration**

   * Add **JDK** (e.g., name: `JDK_11`)
   * Add **Maven** (e.g., name: `Maven_3`)
4. Ensure **Git** is installed on your host or in Jenkins container.

---

## 📁 Step 3: Prepare Your Java Maven Project

Example project structure in GitHub repo:

```
jenkins-java-pipeline-demo/
├── pom.xml
├── src/main/java/com/demo/App.java
├── src/test/java/com/demo/AppTest.java
├── Jenkinsfile
```

**Minimal `pom.xml`** includes JUnit for tests.

---

## 📜 Step 4: Add Declarative Pipeline (`Jenkinsfile`)

Place this at project root in your GitHub repo:

```groovy
pipeline {
  agent any
  tools {
    jdk 'JDK_11'
    maven 'Maven_3'
  }
  stages {
    stage('Checkout') {
      steps {
        git 'https://github.com/YOUR_USERNAME/jenkins-java-pipeline-demo.git'
      }
    }
    stage('Build') {
      steps {
        sh 'mvn clean package'
      }
    }
    stage('Test') {
      steps {
        sh 'mvn test'
        junit '**/target/surefire-reports/*.xml'
      }
    }
  }
}
```

---

## 🔨 Step 5: Create Jenkins Pipeline Job

1. On Jenkins dashboard, click **New Item**
2. Enter job name: `java-maven-pipeline`
3. Select **Pipeline** → Click **OK**
4. Under **Pipeline** section:

   * Definition: **Pipeline script from SCM**
   * SCM: **Git**
   * Repository URL: `https://github.com/YOUR_USERNAME/jenkins-java-pipeline-demo.git`
   * Branch: `main` (or your branch)
   * Script Path: `Jenkinsfile`
5. Click **Save**

---

## ▶️ Step 6: Run and Monitor Pipeline

* Click **Build Now**
* View build progress and logs via **Console Output**
* Confirm Maven build, tests, and JUnit report published

---
