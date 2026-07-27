# Home Assignment – Enterprise Docker Build and Publish Pipeline

## Module 10 – Docker Integration with Jenkins Pipeline

**Assignment Type:** Individual Practical Assignment

**Difficulty:** Intermediate

**Estimated Duration:** 2–3 Hours

**Marks:** 100

---

# Assignment Scenario

You have recently joined **ABC Retail Pvt Ltd** as a DevOps Engineer.

The organization has decided to standardize its application packaging process using Docker containers. Every successful build should automatically generate a Docker image, assign appropriate version tags, publish the image to Docker Hub, and clean up the Jenkins build server after completion.

As the DevOps Engineer, you have been assigned to automate this entire workflow using a Jenkins Declarative Pipeline.

---

# Business Requirements

Your Jenkins Pipeline must:

* Retrieve the application source code from GitHub.
* Build the Java application using Maven.
* Generate the application JAR file.
* Build a Docker image.
* Tag the image using:

  * Jenkins Build Number
  * latest
* Authenticate with Docker Hub using Jenkins Credentials.
* Push both image tags to Docker Hub.
* Remove unused Docker images and stopped containers.
* Display useful build information during execution.

---

# Assignment Objectives

After completing this assignment, students should be able to:

* Create an enterprise Jenkins Pipeline
* Build Docker images automatically
* Use Docker Hub credentials securely
* Implement Docker image versioning
* Publish Docker images
* Perform Docker housekeeping
* Troubleshoot Docker build failures

---

# Lab Environment

| Component          | Details                      |
| ------------------ | ---------------------------- |
| Jenkins            | Running inside Docker        |
| Jenkins Agent      | Ubuntu Linux                 |
| GitHub             | Private or Public Repository |
| Docker Engine      | Installed                    |
| Maven              | Installed                    |
| JDK                | Version 17                   |
| Docker Hub Account | Required                     |

---

# Prerequisites

Before starting the assignment, ensure:

* Docker is installed on the Jenkins Agent.
* Jenkins can execute Docker commands.
* Maven is configured.
* JDK is configured.
* Git is configured.
* Docker Hub account is available.
* Jenkins credentials are created for Docker Hub.

---

# Assignment Tasks

## Task 1 – Verify Environment (10 Marks)

Verify the following software:

```bash
docker version

docker info

git --version

mvn -version

java -version
```

Capture screenshots.

---

## Task 2 – Configure Jenkins Credentials (10 Marks)

Create Docker Hub credentials.

Credential Type

```
Username with Password
```

Credential ID

```
dockerhub
```

Take screenshots.

---

## Task 3 – Create Pipeline Job (10 Marks)

Create a Jenkins Pipeline named

```
docker-enterprise-pipeline
```

Configure the job to retrieve the Jenkinsfile from GitHub.

---

## Task 4 – Checkout Source Code (10 Marks)

The Pipeline must:

* Clone the GitHub repository.
* Display the current workspace.
* Display the Git branch.
* Display the latest commit ID.

Expected console output:

```
Workspace:
/var/lib/jenkins/workspace/docker-enterprise-pipeline

Branch:
main

Commit:
xxxxxxxx
```

---

## Task 5 – Package Application (10 Marks)

Build the application using Maven.

Command

```bash
mvn clean package
```

Verify

```
target/*.jar
```

is generated.

---

## Task 6 – Build Docker Image (15 Marks)

Create a Docker image named

```
studentapp
```

Tag

```
studentapp:${BUILD_NUMBER}
```

Verify

```bash
docker images
```

Take screenshots.

---

## Task 7 – Create Multiple Image Tags (10 Marks)

Create another tag.

```
studentapp:latest
```

Verify both tags exist.

Expected

```
studentapp      35

studentapp      latest
```

---

## Task 8 – Push Images to Docker Hub (15 Marks)

Authenticate using Jenkins Credentials.

Push

```
studentapp:${BUILD_NUMBER}

studentapp:latest
```

Verify

Images are available in Docker Hub.

---

## Task 9 – Cleanup (10 Marks)

Remove

* Stopped Containers
* Dangling Images
* Unused Images

Commands may include

```bash
docker image prune

docker container prune

docker system prune
```

Verify cleanup.

---

## Task 10 – Validate Deployment (10 Marks)

On another Docker host (or another VM):

Pull

```
studentapp:latest
```

Run

```bash
docker run
```

Verify

Application starts successfully.

Capture screenshots.

---

# Challenge Tasks (Bonus – 20 Marks)

Implement the following enhancements:

### Challenge 1

Use the Git Commit ID as an image tag.

Example

```
studentapp:ab45de7
```

---

### Challenge 2

Display build information.

```
Application Name

Image Name

Image Version

Build Number

Git Branch

Docker Image ID
```

---

### Challenge 3

Automatically remove previous image versions after a successful push while keeping only:

* Current Build Number tag
* latest

---

### Challenge 4

Measure the total Pipeline execution time and display it at the end of the build.

---

# Expected Pipeline Flow

```text
Developer

        │

        ▼

GitHub Repository

        │

        ▼

Jenkins Pipeline

        │

        ▼

Git Checkout

        │

        ▼

Maven Package

        │

        ▼

Docker Build

        │

        ▼

Tag Image

(Build Number)

        │

        ▼

Tag Image

(latest)

        │

        ▼

Docker Login

        │

        ▼

Push Build Tag

        │

        ▼

Push latest Tag

        │

        ▼

Cleanup

        │

        ▼

Build Success
```

---

# Deliverables

Students must submit:

* Jenkinsfile
* Dockerfile
* GitHub Repository URL
* Docker Hub Repository URL
* Screenshots of:

  * Successful Jenkins Build
  * Docker Images
  * Docker Hub Repository
  * Running Container
  * Console Output
* A short report (2–3 pages) describing:

  * Pipeline stages
  * Challenges encountered
  * How issues were resolved

---

# Evaluation Rubric

| Task                     |   Marks |
| ------------------------ | ------: |
| Environment Verification |      10 |
| Jenkins Credentials      |      10 |
| Pipeline Configuration   |      10 |
| Git Checkout             |      10 |
| Maven Build              |      10 |
| Docker Build             |      15 |
| Image Tagging            |      10 |
| Docker Hub Push          |      15 |
| Cleanup                  |      10 |
| Validation               |      10 |
| **Total**                | **100** |
| Bonus Challenges         |     +20 |

---

# Expected Learning Outcome

After completing this assignment, students will have implemented a production-style Docker build and publish pipeline using Jenkins. They will understand how to automate image creation, apply dynamic versioning, securely authenticate with Docker Hub, publish container images, and maintain a clean build environment—skills that form the foundation for the next module on **Trivy Container Security Scanning**, where these published Docker images will be analyzed for vulnerabilities before deployment.
