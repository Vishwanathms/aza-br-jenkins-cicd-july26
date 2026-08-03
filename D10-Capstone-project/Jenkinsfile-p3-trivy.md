I completely agree. **Do not remove anything from the capstone Jenkinsfile.** One of the key DevSecOps concepts is that **security becomes another automated stage in the CI/CD pipeline**, not a replacement for existing stages.

For your progressive labs, I recommend introducing Trivy in **two phases**, exactly as it is done in enterprise environments.

## Updated Pipeline Flow

```
GitHub
    │
Checkout
    │
Filesystem Scan (Trivy)
    │
Docker Build
    │
Docker Image Scan (Trivy)
    │
Amazon ECR Push
    │
Docker Compose Deploy
    │
Health Check
```

This demonstrates the two major Trivy scan types:

* **Filesystem Scan** – scans the application source code and dependency files before building the image.
* **Image Scan** – scans the final Docker image before pushing it to the registry.

---

# Lab Manual

# Lab 7 – Integrating Trivy into Jenkins CI/CD Pipeline

## Objective

After completing this lab, students will be able to:

* Install Trivy
* Verify Trivy installation
* Perform filesystem vulnerability scanning
* Perform Docker image scanning
* Generate HTML reports
* Fail a Jenkins build based on vulnerability severity
* Integrate Trivy into an existing Jenkins pipeline

---

# Lab Architecture

```
Developer
      │
      ▼
GitHub
      │
      ▼
Jenkins
      │
      ├──────────────┐
      │              │
      ▼              ▼
Filesystem Scan   Docker Image Scan
      │              │
      └──────┬───────┘
             ▼
        Amazon ECR
             │
             ▼
     Docker Compose
```

---

# Prerequisites

Students should already have:

* Jenkins installed
* Docker installed
* AWS CLI installed
* Docker Compose installed
* Existing Capstone Jenkinsfile
* Running Jenkins Agent
* Amazon ECR Repository

---

# Step 1 – Install Trivy

Ubuntu

```bash
sudo apt-get update

sudo apt-get install wget apt-transport-https gnupg lsb-release -y

wget -qO - https://aquasecurity.github.io/trivy-repo/deb/public.key \
| sudo gpg --dearmor \
-o /usr/share/keyrings/trivy.gpg

echo "deb [signed-by=/usr/share/keyrings/trivy.gpg] \
https://aquasecurity.github.io/trivy-repo/deb \
$(lsb_release -sc) main" \
| sudo tee /etc/apt/sources.list.d/trivy.list

sudo apt update

sudo apt install trivy -y
```

---

# Step 2 – Verify Installation

```
trivy --version
```

Expected

```
Version: 0.xx.x
```

---

# Step 3 – Verify Database Download

Run

```
trivy image alpine:latest
```

The first execution downloads the vulnerability database.

---

# Step 4 – Test Filesystem Scan

Navigate to your project.

```
cd python-flask-app
```

Run

```
trivy fs .
```

Example Output

```
Total: 6

HIGH : 2

MEDIUM : 3

LOW : 1
```

Explain to students that Trivy scans:

* requirements.txt
* package-lock.json
* pom.xml
* go.mod
* Pipfile
* yarn.lock
* Dockerfile
* OS packages
* Secrets (if enabled)

---

# Step 5 – Test Docker Image Scan

Build the image.

```
docker build -t python-app .
```

Scan it.

```
trivy image python-app
```

Expected Output

```
Critical : 0

High : 4

Medium : 10
```

---

# Step 6 – Add Filesystem Scan Stage to Jenkins

Insert this stage **after Checkout** and **before Docker Build**.

```groovy
stage('Trivy Filesystem Scan') {

    steps {

        sh '''
        trivy fs \
        --severity HIGH,CRITICAL \
        --exit-code 0 \
        .
        '''
    }
}
```

### Explanation

| Option          | Purpose                                          |
| --------------- | ------------------------------------------------ |
| `fs`            | Scan project files                               |
| `--severity`    | Show only HIGH and CRITICAL vulnerabilities      |
| `--exit-code 0` | Report vulnerabilities but do not fail the build |

---

# Step 7 – Add Docker Image Scan

Insert this stage **after Docker Build** and **before Login to Amazon ECR**.

```groovy
stage('Trivy Image Scan') {

    steps {

        sh '''
        trivy image \
        --severity HIGH,CRITICAL \
        --exit-code 0 \
        ${ECR_REPOSITORY}:${IMAGE_TAG}
        '''
    }
}
```

---

# Step 8 – Pipeline Order

Students should now have the following stages:

```
Checkout

↓

Filesystem Scan

↓

Docker Build

↓

Image Scan

↓

Login ECR

↓

Tag Image

↓

Push Image

↓

Docker Compose

↓

Health Check
```

---

# Step 9 – Generate HTML Reports (Optional)

Filesystem report

```bash
trivy fs \
--format html \
-o fs-report.html \
.
```

Docker image report

```bash
trivy image \
--format html \
-o image-report.html \
python-app
```

These reports can be archived as Jenkins build artifacts.

---

# Step 10 – Publish Reports in Jenkins

Add this to the `post` section:

```groovy
archiveArtifacts artifacts: '*.html', fingerprint: true
```

Students can then download the reports from the Jenkins build page.

---

# Step 11 – Failing the Pipeline (Enterprise Policy)

Once students are comfortable with Trivy, demonstrate enforcing a security gate.

```groovy
trivy image \
--severity CRITICAL \
--exit-code 1 \
${ECR_REPOSITORY}:${IMAGE_TAG}
```

Behavior:

* No critical vulnerabilities → Pipeline continues.
* One or more critical vulnerabilities → Pipeline stops before pushing to Amazon ECR.

This illustrates the concept of "shift-left security" by preventing vulnerable images from reaching the registry.

---

# Expected Console Output

```
Checkout Source
      ✓

Trivy Filesystem Scan
      ✓

Docker Build
      ✓

Trivy Image Scan
      ✓

Login Amazon ECR
      ✓

Push Image
      ✓

Docker Compose
      ✓

Health Check
      ✓
```

---

# Learning Outcomes

By the end of this lab, students will be able to:

* Install and configure Trivy.
* Perform filesystem and container image vulnerability scans.
* Integrate Trivy into a Jenkins pipeline without disrupting existing stages.
* Generate and archive scan reports.
* Understand how security gates can block deployments based on vulnerability severity.
