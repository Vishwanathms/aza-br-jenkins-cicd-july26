# Lab Manual 1

# Jenkins Pipeline - Docker Integration

# Scenario 1: Containerize a Java Application

**Module:** Docker Integration with Jenkins Pipeline

**Lab Duration:** 60–75 Minutes

**Difficulty:** Beginner

**Prerequisites:**

* Jenkins Controller is running.
* Jenkins Agent (Ubuntu) is configured.
* Docker is installed on the Jenkins Agent.
* Maven is installed and configured in Jenkins.
* Git is installed.
* Java application source code is available in GitHub.
* The application builds successfully using `mvn clean package`.

---

# Lab Scenario

You have recently joined **ABC Retail Pvt Ltd** as a DevOps Engineer.

The development team has completed a Java-based Inventory Management application. The application has already been compiled using Maven, and the JAR file is generated successfully.

The Operations team has requested that the application be packaged as a Docker image so it can be deployed consistently across Development, QA, UAT, and Production environments.

Your task is to containerize the application using Docker and validate that it runs successfully.

---

# Learning Objectives

After completing this lab, you will be able to:

* Verify Docker installation
* Understand Dockerfile structure
* Create a Dockerfile
* Build Docker images
* List Docker images
* Run Docker containers
* Verify application accessibility
* View container logs
* Stop and remove containers

---

# Expected Architecture

```text
                     Developer

                         │
                         ▼

                  Java Source Code

                         │

                  mvn clean package

                         │

                    target/app.jar

                         │

                    Docker Build

                         │

                 Docker Image Created

                         │

                    Docker Run

                         │

                Application Container

                         │

                 Browser Verification
```

---

# Lab Environment

| Component     | Details                      |
| ------------- | ---------------------------- |
| OS            | Ubuntu 24.04                 |
| Jenkins       | Running as Docker Container  |
| Docker Engine | Installed on Ubuntu Host     |
| Java          | JDK 17                       |
| Maven         | Maven 3.x                    |
| Git           | Installed                    |
| Application   | Spring Boot Java Application |

---

# Scenario Files

Assume the following project structure.

```
inventory-app/

├── src/

├── pom.xml

├── Dockerfile

└── Jenkinsfile
```

After Maven build

```
inventory-app/

├── target/

│      inventory-app.jar

├── Dockerfile
```

---

# Task 1

# Verify Docker Installation

Login to the Jenkins Agent.

Execute

```bash
docker version
```

Expected Output

```
Client:
 Version: 27.x

Server:
 Docker Engine
```

---

Verify Docker Service

```bash
sudo systemctl status docker
```

Expected

```
Active: active (running)
```

---

Verify Docker Information

```bash
docker info
```

Observe

* Docker Root Directory
* Storage Driver
* Running Containers
* Images

---

Verify Current User

```bash
whoami
```

Verify Docker Access

```bash
docker ps
```

If permission denied

```
sudo usermod -aG docker jenkins
```

Reconnect.

---

# Validation Check

✔ Docker Installed

✔ Docker Running

✔ Jenkins User can execute Docker commands

---

# Task 2

# Verify Maven Build Output

Navigate to project.

```
cd inventory-app
```

Verify

```
target/
```

List files

```bash
ls target
```

Expected

```
inventory-app.jar
```

If JAR not present

Run

```bash
mvn clean package
```

Wait until

```
BUILD SUCCESS
```

---

# Validation

JAR file generated.

---

# Task 3

# Create Dockerfile

Navigate

```
inventory-app
```

Create Dockerfile

```bash
vi Dockerfile
```

Insert

```dockerfile
FROM eclipse-temurin:17-jre

WORKDIR /app

COPY target/inventory-app.jar app.jar

EXPOSE 8080

ENTRYPOINT ["java","-jar","app.jar"]
```

Save

```
:wq
```

---

# Dockerfile Explanation

### Base Image

```dockerfile
FROM eclipse-temurin:17-jre
```

Downloads Java Runtime Image.

---

### Working Directory

```dockerfile
WORKDIR /app
```

Creates

```
/app
```

inside container.

---

### Copy JAR

```dockerfile
COPY target/inventory-app.jar app.jar
```

Copies application.

---

### Expose Port

```dockerfile
EXPOSE 8080
```

Documents application port.

---

### Start Application

```dockerfile
ENTRYPOINT ["java","-jar","app.jar"]
```

Runs application automatically.

---

# Validation

Verify Dockerfile

```bash
cat Dockerfile
```

---

# Task 4

# Build Docker Image

Execute

```bash
docker build -t inventory-app:v1 .
```

Observe

```
Sending build context

Step 1

Step 2

...

Successfully built

Successfully tagged inventory-app:v1
```

---

Explain

```
-t
```

means

```
Tag Image
```

Dot

```
.
```

means

Current Directory

---

# Validation

Image created successfully.

---

# Task 5

# Verify Docker Images

Execute

```bash
docker images
```

Expected

```
REPOSITORY

inventory-app

TAG

v1

IMAGE ID

xxxxxx
```

Observe

* Repository
* Tag
* Image ID
* Size

---

Alternative

```bash
docker image ls
```

---

# Task 6

# Run Docker Container

Execute

```bash
docker run -d \
--name inventory-container \
-p 8080:8080 \
inventory-app:v1
```

Explain

```
-d
```

Detached Mode

```
--name
```

Container Name

```
-p
```

Port Mapping

Host

```
8080
```

Container

```
8080
```

---

# Validation

Container Started.

---

# Task 7

# Verify Running Container

Execute

```bash
docker ps
```

Expected

```
CONTAINER ID

IMAGE

STATUS

PORTS

NAMES
```

Observe

```
inventory-container
```

Status

```
Up
```

---

# Task 8

# Verify Application

Open Browser

```
http://<VM-IP>:8080
```

or

```
http://localhost:8080
```

Expected

Application Home Page.

If REST API

```
http://localhost:8080/actuator/health
```

Expected

```json
{
"status":"UP"
}
```

---

# Validation

Application is accessible.

---

# Task 9

# View Container Logs

Execute

```bash
docker logs inventory-container
```

Observe

```
Spring Boot Started

Tomcat Started

Application Ready
```

Live Logs

```bash
docker logs -f inventory-container
```

Press

```
CTRL+C
```

---

# Task 10

# Inspect Container

Execute

```bash
docker inspect inventory-container
```

Observe

* Container ID
* Network
* IP Address
* Mounts
* Ports

---

# Task 11

# Stop Container

Execute

```bash
docker stop inventory-container
```

Verify

```bash
docker ps
```

Container no longer running.

---

# Task 12

# Remove Container

Execute

```bash
docker rm inventory-container
```

Verify

```bash
docker ps -a
```

Container removed.

---

# Task 13

# Remove Docker Image (Optional)

Execute

```bash
docker rmi inventory-app:v1
```

Verify

```bash
docker images
```

Image removed.

---

# Challenge Activity

Perform the following enhancements:

1. Build the image with version `v2`.
2. Create an additional tag named `latest`.
3. Run two containers simultaneously using different host ports:

   * `8081:8080`
   * `8082:8080`
4. Verify both containers are running.
5. View logs for both containers.
6. Stop and remove both containers.
7. Remove all locally created images.

---

# Validation Checklist

| Task                            | Status |
| ------------------------------- | :----: |
| Docker installation verified    |    ☐   |
| Docker daemon running           |    ☐   |
| Maven JAR verified              |    ☐   |
| Dockerfile created              |    ☐   |
| Docker image built successfully |    ☐   |
| Docker image listed             |    ☐   |
| Container started               |    ☐   |
| Application accessible          |    ☐   |
| Container logs verified         |    ☐   |
| Container inspected             |    ☐   |
| Container stopped               |    ☐   |
| Container removed               |    ☐   |
| Image removed (optional)        |    ☐   |

---

# Expected Outcome

At the end of this lab, students will have:

* Verified a Docker-enabled Jenkins build environment.
* Understood the structure and purpose of a Dockerfile.
* Built a Docker image from a Maven-generated Java application.
* Executed and managed Docker containers using common CLI commands.
* Validated application accessibility and reviewed runtime logs.
* Cleaned up Docker resources following standard operational practices.

This lab provides the foundation for the next Docker lab, where students will extend the workflow by integrating **Jenkins Pipelines**, **Docker Hub authentication**, **image tagging**, and **automated image publishing**.
