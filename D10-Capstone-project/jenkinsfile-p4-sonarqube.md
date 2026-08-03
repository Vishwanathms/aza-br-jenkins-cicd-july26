Yes, this is exactly the next logical step. I would also slightly change the pipeline order to align with industry standards.

> **Code Quality (SonarQube Cloud) should happen before Trivy.**

The reason is simple:

* **SonarQube** analyzes source code quality, bugs, code smells, and security hotspots before any build starts.
* **Trivy** scans dependencies and container images after code quality checks.

## Enterprise Pipeline Flow

```text
Developer
    │
    ▼
GitHub
    │
Checkout
    │
SonarQube Cloud Analysis
    │
Trivy Filesystem Scan
    │
Docker Build
    │
Trivy Image Scan
    │
Amazon ECR Push
    │
Docker Compose Deploy
    │
Health Check
```

This is very close to what you'll find in organizations like AT&T, Ericsson, Cisco, and many cloud-native teams.

---

# Lab 6 – Integrating SonarQube Cloud into Jenkins

## Objective

In this lab students will learn how to:

* Create a SonarQube Cloud account
* Create an organization
* Create a project
* Generate a Sonar Token
* Configure SonarScanner
* Configure Jenkins credentials
* Integrate SonarQube Cloud into an existing Jenkins Pipeline
* Understand Quality Gates

---

# Lab Architecture

```text
Developer
      │
      ▼
GitHub Repository
      │
      ▼
Jenkins
      │
      ▼
SonarQube Cloud
      │
      ▼
Code Quality Report
```

---

# Prerequisites

Students should already have:

* GitHub account
* Jenkins server
* Java installed
* Docker installed
* Existing Flask/Python project
* Existing Jenkins Pipeline

---

# Step 1 – Create a SonarQube Cloud Account

Open:

**[https://sonarcloud.io](https://sonarcloud.io)**

Click

```text
Log In
```

Choose

```text
Continue with GitHub
```

Authorize GitHub.

After login you should see the SonarQube Cloud dashboard.

---

# Step 2 – Create an Organization

Click

```text
+
```

Select

```text
Create Organization
```

Choose

```text
GitHub Organization
```

or

```text
Personal Account
```

Example

```text
Organization

student01
```

---

# Step 3 – Import Repository

Click

```text
Analyze New Project
```

Select your GitHub repository.

Example

```text
python-flask-app
```

Click

```text
Set Up
```

---

# Step 4 – Choose Analysis Method

Select

```text
Previous Version
```

Then choose

```text
With SonarScanner CLI
```

We'll run the scanner from Jenkins rather than GitHub Actions.

---

# Step 5 – Generate Project Token

Navigate to:

```text
Administration

↓

Analysis Method

↓

Generate Token
```

Example

```text
jenkins-token
```

Copy the token.

> **Important:** The token is displayed only once. Store it securely.

---

# Step 6 – Note the Project Information

Record the following values:

```text
Organization

student01
```

```text
Project Key

student01_python-flask
```

```text
Project Name

python-flask
```

These values will be used in the Jenkins pipeline.

---

# Step 7 – Install SonarScanner on Jenkins Agent

Ubuntu

```bash
wget https://binaries.sonarsource.com/Distribution/sonar-scanner-cli/sonar-scanner-7.2.0.5079-linux-x64.zip

sudo apt install unzip -y

unzip sonar-scanner-*.zip

sudo mv sonar-scanner-* /opt/sonar-scanner
```

Add it to the PATH:

```bash
sudo nano /etc/profile
```

Append:

```bash
export PATH=$PATH:/opt/sonar-scanner/bin
```

Reload:

```bash
source /etc/profile
```

Verify:

```bash
sonar-scanner --version
```

---

# Step 8 – Configure Jenkins Credentials

In Jenkins:

```text
Manage Jenkins

↓

Credentials

↓

Global

↓

Add Credentials
```

Choose:

```text
Kind

Secret Text
```

Secret

```text
Paste Sonar Token
```

ID

```text
sonar-token
```

Description

```text
SonarQube Cloud Token
```

Save.

---

# Step 9 – Create `sonar-project.properties`

In the project root, create:

```properties
sonar.projectKey=student01_python-flask
sonar.organization=student01

sonar.sources=.

sonar.python.version=3

sonar.sourceEncoding=UTF-8

sonar.host.url=https://sonarcloud.io
```

Commit this file to GitHub.

---

# Step 10 – Add SonarQube Stage to Jenkinsfile

Insert this stage **after Checkout** and **before Trivy Filesystem Scan**.

```groovy
stage('SonarQube Cloud Analysis') {

    steps {

        withCredentials([string(credentialsId: 'sonar-token', variable: 'SONAR_TOKEN')]) {

            sh '''
            sonar-scanner \
              -Dsonar.token=$SONAR_TOKEN
            '''
        }
    }
}
```

---

# Updated Pipeline Order

```text
Checkout

↓

SonarQube Cloud Analysis

↓

Trivy Filesystem Scan

↓

Docker Build

↓

Trivy Image Scan

↓

Login Amazon ECR

↓

Tag Image

↓

Push Image

↓

Docker Compose Deploy

↓

Health Check
```

---

# Step 11 – Run the Pipeline

Expected Jenkins console output:

```text
Checking out source...

SonarScanner 7.x

INFO: Project Key

INFO: Organization

INFO: Analysis successful

More details at

https://sonarcloud.io/dashboard?id=student01_python-flask
```

---

# Step 12 – View Results

Open your project in SonarQube Cloud.

Students can explore:

* **Overall Quality Rating**
* **Bugs**
* **Vulnerabilities**
* **Code Smells**
* **Security Hotspots**
* **Coverage** (if tests are configured)
* **Duplicated Code**

Explain what each metric means and why teams monitor them continuously.

---

# Step 13 – Understanding Quality Gates

A **Quality Gate** is a set of rules that determines whether code is acceptable for promotion.

Typical checks include:

* No new critical bugs
* No new vulnerabilities
* Maintainability rating of A
* Security rating of A
* Coverage above the team's threshold (when tests are present)

In enterprise pipelines, a failed Quality Gate typically blocks deployment until issues are resolved.

