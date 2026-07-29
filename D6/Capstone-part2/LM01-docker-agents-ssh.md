# Lab 04 – Creating a Docker-based Jenkins Build Agent

## Lab Objective

By the end of this lab, students will be able to:

* Create a custom Docker image for a Jenkins agent
* Install Java, Maven, Git, Docker CLI, and SSH inside the image
* Configure SSH key-based authentication
* Register the Docker container as a Jenkins Agent
* Execute Jenkins Pipelines on the Docker Agent
* Verify Docker commands from Jenkins

---

# Lab Architecture

```text
                   +--------------------------------+
                   |      Jenkins Controller        |
                   |                                |
                   |  SSH Private Key (id_rsa)      |
                   +---------------+----------------+
                                   |
                          SSH Authentication
                                   |
                    Port 2222 (SSH over Docker)
                                   |
          -------------------------------------------------
                                   |
                    Docker Container (Jenkins Agent)
          -------------------------------------------------
          Java 17
          Maven
          Git
          Docker CLI
          SSH Server
          Jenkins User
          authorized_keys
          -------------------------------------------------
```

---

# Lab Environment

| Component | Details               |
| --------- | --------------------- |
| OS        | Ubuntu 22.04          |
| Jenkins   | Installed and Running |
| Docker    | Installed             |
| Java      | OpenJDK 17            |
| Maven     | Latest                |
| SSH       | OpenSSH Server        |

---

# Lab Files

Create a working directory.

```bash
mkdir ~/jenkins-agent
cd ~/jenkins-agent
```

Verify

```bash
pwd
```

Expected

```
/home/student/jenkins-agent
```

---

# Task 1 – Verify LABVM SSH Keys

## Objective

The LABVM already has an SSH key pair.

Verify the keys.

```bash
ls ~/.ssh
```

Expected

```
id_rsa
id_rsa.pub
known_hosts
authorized_keys
```

Display the public key.

```bash
cat ~/.ssh/authorized_keys
```

Example

```
ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAACAQC...
```


---


# Task 2 – Create the Dockerfile

Create the Dockerfile.

```bash
vi Dockerfile
```

Paste the following content.

```dockerfile
FROM ubuntu:22.04

ENV DEBIAN_FRONTEND=noninteractive

# Install required packages
RUN apt-get update && apt-get install -y \
    openjdk-17-jdk \
    git \
    curl \
    wget \
    unzip \
    maven \
    openssh-server \
    docker.io \
    sudo \
 && rm -rf /var/lib/apt/lists/*

# Create SSH runtime directory
RUN mkdir /var/run/sshd

# Create Jenkins user
RUN useradd -m -s /bin/bash jenkins

# Add Jenkins user to sudo and docker groups
RUN usermod -aG sudo,docker jenkins

# Create SSH directory
RUN mkdir -p /home/jenkins/.ssh

# Copy Jenkins public key
COPY jenkins-agent.pub /home/jenkins/.ssh/authorized_keys

# Correct permissions
RUN chown -R jenkins:jenkins /home/jenkins/.ssh && \
    chmod 700 /home/jenkins/.ssh && \
    chmod 600 /home/jenkins/.ssh/authorized_keys

EXPOSE 22

CMD ["/usr/sbin/sshd","-D"]
```

---

# Task 3 – Understand the Dockerfile

## Base Image

```dockerfile
FROM ubuntu:22.04
```

Uses Ubuntu 22.04.

---

## Install Software

```dockerfile
RUN apt-get update
```

Installs

* Java
* Git
* Maven
* Docker CLI
* SSH Server

---

## Create Jenkins User

```dockerfile
useradd -m -s /bin/bash jenkins
```

Creates the build user.

---

## Docker Permissions

```dockerfile
usermod -aG docker jenkins
```

Allows Docker commands without root.

---


# Task 4 – Build the Docker Image

Build the image.

```bash
docker build -t jenkins-agent:v1 .
```

Expected

```
Successfully built

Successfully tagged jenkins-agent:v1
```

Verify

```bash
docker images
```

Expected

```
REPOSITORY          TAG

jenkins-agent       v1
```

---

# Task 5 – Run the Jenkins Agent Container

Run the container.

```bash
docker run -d \
  --name jenkins-agent \
  -p 2222:22 \
  -v /home/labuser/.ssh:/home/jenkins/.ssh:ro \
  -v /var/run/docker.sock:/var/run/docker.sock \
  jenkins-agent:v1
```

Explanation

| Option         | Description            |
| -------------- | ---------------------- |
| -d             | Detached mode          |
| --name         | Container Name         |
| -p             | Publish SSH Port       |
| -v .ssh        | uses the authorized_keys on host |
| -v docker.sock | Allows Docker commands |
---

Verify

```bash
docker ps
```

Expected

```
PORTS

0.0.0.0:2222->22
```

---

# Task 6 – Verify Passwordless SSH

Since the public key was copied into the image, Jenkins can immediately connect using its private key.

Connect.

```bash
ssh -p 2222 jenkins@localhost
```

Expected

```
Welcome to Ubuntu

jenkins@container:~$
```

No password should be requested.

Verify Java

```bash
java -version
```

Verify Maven

```bash
mvn -version
```

Verify Git

```bash
git --version
```

Verify Docker

```bash
docker version
```

---

# Task 7 – Configure Jenkins Credentials

Navigate

```
Manage Jenkins

↓

Credentials

↓

Global

↓

Add Credentials
```

Choose

```
SSH Username with Private Key
```

Fill

| Field         | Value              |
| ------------- | ------------------ |
| Username      | jenkins            |
| Private Key   | From ~/.ssh/id_rsa |
| Credential ID | docker-agent       |

Save.

---

# Task 9 – Create Jenkins Agent

Navigate

```
Manage Jenkins

↓

Nodes

↓

New Node
```

Node Name

```
docker-agent
```

Select

```
Permanent Agent
```

---

Configure

| Setting               | Value                             |
| --------------------- | --------------------------------- |
| Remote Root Directory | /home/jenkins                     |
| Labels                | docker                            |
| Usage                 | Use this node as much as possible |

Launch Method

```
Launch agents via SSH
```

Host

```
<Lab VM IP Address>
```

SSH Port

```
2222
```

Credentials

```
docker-agent
```

Save.

---

# Task 10 – Verify Agent Status

Navigate

```
Manage Jenkins

↓

Nodes
```

Expected

```
docker-agent

ONLINE
```

If Online, Jenkins has successfully authenticated using the embedded public key.

---

# Task 11 – Create a Pipeline Job

Create a Pipeline project.

Paste the following pipeline.

```groovy
pipeline {

    agent {

        label 'docker'

    }

    stages {

        stage('Verify Environment') {

            steps {

                sh 'hostname'

                sh 'java -version'

                sh 'git --version'

                sh 'mvn -version'

                sh 'docker version'

            }

        }

    }

}
```

Run the pipeline.

Expected Output

```
hostname

java version

Apache Maven

Git Version

Docker Version
```

---

# Task 12 – Verify Docker Access

Replace the pipeline with:

```groovy
pipeline {

    agent { label 'docker' }

    stages {

        stage('Docker Test') {

            steps {

                sh 'docker images'

                sh 'docker ps'

            }

        }

    }

}
```

Run the pipeline.

Expected

```
REPOSITORY

jenkins-agent

ubuntu

sample-java
```

---

# Task 13 – Validate the SSH Configuration

From the Jenkins controller, verify that the `authorized_keys` file exists inside the container.

```bash
docker exec -it jenkins-agent ls -l /home/jenkins/.ssh
```

Expected

```
authorized_keys
```

Display its contents.

```bash
docker exec -it jenkins-agent cat /home/jenkins/.ssh/authorized_keys
```

Verify that it matches the local public key.

```bash
cat ~/.ssh/id_rsa.pub
```

Both outputs should be identical.

---

# Troubleshooting

| Problem                   | Solution                                                                                       |
| ------------------------- | ---------------------------------------------------------------------------------------------- |
| Agent Offline             | Verify the container is running with `docker ps` and that port `2222` is exposed.              |
| SSH Authentication Failed | Confirm the public key in `authorized_keys` matches the controller's `id_rsa.pub`.             |
| Permission Denied         | Check `.ssh` permissions (`700`) and `authorized_keys` permissions (`600`).                    |
| Docker Permission Error   | Ensure the `jenkins` user belongs to the `docker` group and `/var/run/docker.sock` is mounted. |
| Docker Command Not Found  | Confirm `docker.io` was installed during the image build.                                      |

---

# Lab Summary

In this lab, you successfully:

* Verified the Jenkins SSH key pair.
* Embedded the Jenkins public key into a custom Docker image.
* Built a reusable Jenkins agent image with Java, Maven, Git, Docker CLI, and SSH.
* Started the Docker container as an SSH-based Jenkins agent.
* Configured Jenkins to authenticate using the existing private key.
* Registered the container as a permanent Jenkins agent.
* Executed Jenkins pipelines on the Docker agent.
* Verified that the agent can run Docker commands, making it suitable for CI/CD pipelines that build, push, and deploy containerized applications.
