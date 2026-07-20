# Lab Manual: Setup Jenkins Using Docker Container on Ubuntu 24.04

## Lab Objective

In this lab, you will:

* Install Docker on Ubuntu 24.04
* Download the Jenkins LTS Docker image
* Create persistent storage for Jenkins
* Run Jenkins as a Docker container
* Access Jenkins through a browser
* Unlock and configure Jenkins
* Create your first Jenkins job
* Manage and troubleshoot the Jenkins container

Official references: [Jenkins Docker installation guide](https://www.jenkins.io/doc/book/installing/docker/?utm_source=chatgpt.com) and [Docker Engine installation for Ubuntu](https://docs.docker.com/engine/install/ubuntu/?utm_source=chatgpt.com).

---

## Lab Requirements

| Component          | Requirement                     |
| ------------------ | ------------------------------- |
| Operating System   | Ubuntu 24.04 LTS                |
| RAM                | Minimum 2 GB; 4 GB recommended  |
| CPU                | 2 vCPU recommended              |
| Disk               | 20 GB+                          |
| Docker             | Docker Engine                   |
| Jenkins Image      | `jenkins/jenkins:lts-jdk21`     |
| Jenkins Web Port   | 8080                            |
| Jenkins Agent Port | 50000                           |
| Internet           | Required for images and plugins |

---

# Part 1: Verify Ubuntu

## Step 1: Check Ubuntu Version

Run:

```bash
lsb_release -a
```

Or:

```bash
cat /etc/os-release
```

Verify that the server is running:

```text
Ubuntu 24.04 LTS
```

---

# Part 2: Install Docker

## Step 2: Update Package Repository

```bash
sudo apt update
```

Install prerequisites:

```bash
sudo apt install -y ca-certificates curl
```

---

## Step 3: Add Docker GPG Key

Create the keyring directory:

```bash
sudo install -m 0755 -d /etc/apt/keyrings
```

Download the Docker signing key:

```bash
sudo curl -fsSL https://download.docker.com/linux/ubuntu/gpg \
-o /etc/apt/keyrings/docker.asc
```

Set permissions:

```bash
sudo chmod a+r /etc/apt/keyrings/docker.asc
```

---

## Step 4: Add Docker Repository

Run:

```bash
echo \
  "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.asc] https://download.docker.com/linux/ubuntu \
  $(. /etc/os-release && echo "${UBUNTU_CODENAME:-$VERSION_CODENAME}") stable" \
  | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
```

Update the package repository:

```bash
sudo apt update
```

---

## Step 5: Install Docker

```bash
sudo apt install -y docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin
```

Verify Docker:

```bash
docker --version
```

Check Docker service:

```bash
sudo systemctl status docker
```

Press `q` to exit.

Enable Docker at boot:

```bash
sudo systemctl enable docker
```

---

## Step 6: Allow Current User to Run Docker

Add the current user to the Docker group:

```bash
sudo usermod -aG docker $USER
```

Apply the group membership in the current shell:

```bash
newgrp docker
```

Test Docker:

```bash
docker run hello-world
```

> In a production or shared environment, note that membership in the `docker` group effectively grants root-level privileges on the host.

---

# Part 3: Download Jenkins Docker Image

## Step 7: Pull Jenkins LTS Image

Pull the Jenkins LTS image with JDK 21:

```bash
docker pull jenkins/jenkins:lts-jdk21
```

Verify:

```bash
docker images
```

You should see an image similar to:

```text
REPOSITORY        TAG
jenkins/jenkins   lts-jdk21
```

---

# Part 4: Create Persistent Jenkins Storage

Jenkins stores jobs, plugins, credentials, configuration, and other data under:

```text
/var/jenkins_home
```

inside the container.

We will use a Docker named volume to preserve this data.

## Step 8: Create Jenkins Volume

```bash
docker volume create jenkins_home
```

Verify:

```bash
docker volume ls
```

Inspect the volume:

```bash
docker volume inspect jenkins_home
```

---

# Part 5: Run Jenkins Container

## Step 9: Start Jenkins

Run:

```bash
docker run -d \
  --name jenkins \
  --restart unless-stopped \
  -p 8080:8080 \
  -p 50000:50000 \
  -v jenkins_home:/var/jenkins_home \
  jenkins/jenkins:lts-jdk21
```

### Command Explanation

| Option                              | Description                         |
| ----------------------------------- | ----------------------------------- |
| `-d`                                | Runs container in background        |
| `--name jenkins`                    | Names the container `jenkins`       |
| `--restart unless-stopped`          | Restarts Jenkins automatically      |
| `-p 8080:8080`                      | Exposes Jenkins Web UI              |
| `-p 50000:50000`                    | Exposes inbound Jenkins agent port  |
| `-v jenkins_home:/var/jenkins_home` | Provides persistent Jenkins storage |
| `jenkins/jenkins:lts-jdk21`         | Jenkins LTS image                   |

> Port `50000` is only necessary if you plan to use inbound TCP Jenkins agents. It can be omitted for a simple standalone Jenkins lab.

---

## Step 10: Verify Jenkins Container

Run:

```bash
docker ps
```

Expected output should show the Jenkins container as running.

Check all containers:

```bash
docker ps -a
```

---

## Step 11: Check Jenkins Logs

Run:

```bash
docker logs jenkins
```

Follow logs in real time:

```bash
docker logs -f jenkins
```

Press:

```text
Ctrl+C
```

to stop following the logs.

This will **not** stop the Jenkins container.

---

# Part 6: Access Jenkins

## Step 12: Find Server IP Address

Run:

```bash
hostname -I
```

For example:

```text
192.168.1.100
```

Open your browser:

```text
http://<SERVER-IP>:8080
```

Example:

```text
http://192.168.1.100:8080
```

You should see:

**Unlock Jenkins**

---

# Part 7: Get Jenkins Initial Admin Password

## Step 13: Retrieve Password from Container

Run:

```bash
docker exec jenkins cat /var/jenkins_home/secrets/initialAdminPassword
```

Alternatively, you can find it in the startup logs:

```bash
docker logs jenkins
```

Copy the password.

---

## Step 14: Unlock Jenkins

Go back to the browser.

1. Paste the administrator password
2. Click **Continue**

---

# Part 8: Configure Jenkins

## Step 15: Install Plugins

Select:

**Install suggested plugins**

Wait for Jenkins to download and install the plugins.

---

## Step 16: Create Admin User

Enter:

* Username
* Password
* Confirm password
* Full name
* Email address

Click:

**Save and Continue**

---

## Step 17: Configure Jenkins URL

Verify the Jenkins URL.

For example:

```text
http://192.168.1.100:8080/
```

Click:

**Save and Finish**

Then click:

**Start using Jenkins**

The Jenkins Dashboard should now appear.

---

# Part 9: Create Your First Jenkins Job

## Step 18: Create Freestyle Project

From the Jenkins Dashboard:

1. Click **New Item**
2. Enter:

```text
docker-jenkins-job
```

3. Select **Freestyle project**
4. Click **OK**


# Part 09: Jenkins Container Management

## Check Running Container

```bash
docker ps
```

## Stop Jenkins

```bash
docker stop jenkins
```

## Start Jenkins

```bash
docker start jenkins
```

## Restart Jenkins

```bash
docker restart jenkins
```

## View Logs

```bash
docker logs jenkins
```

Follow logs:

```bash
docker logs -f jenkins
```

## Access Jenkins Container Shell

```bash
docker exec -it jenkins bash
```

Inside the container:

```bash
whoami
```

Exit:

```bash
exit
```

---

# Part 10: Verify Persistent Storage

One major advantage of using a Docker volume is that Jenkins data survives container deletion.

## Step 19: Stop and Delete Jenkins Container

```bash
docker stop jenkins
```

Remove it:

```bash
docker rm jenkins
```

Verify:

```bash
docker ps -a
```

The container is gone, but the volume still exists:

```bash
docker volume ls
```

---

## Step 20: Recreate Jenkins Container

Run:

```bash
docker run -d \
  --name jenkins \
  --restart unless-stopped \
  -p 8080:8080 \
  -p 50000:50000 \
  -v jenkins_home:/var/jenkins_home \
  jenkins/jenkins:lts-jdk21
```

Access Jenkins again:

```text
http://<SERVER-IP>:8080
```

Your previous:

* Users
* Jobs
* Plugins
* Configuration
* Build history

should still be available.

This demonstrates **Jenkins data persistence**.


# Part 13: Jenkins Docker Architecture

```text
                User Browser
                     |
                     |
              http://IP:8080
                     |
                     v
        +---------------------------+
        |    Ubuntu 24.04 Host      |
        |                           |
        |      Docker Engine        |
        |            |              |
        |            v              |
        |   +-------------------+   |
        |   | Jenkins Container |   |
        |   |                   |   |
        |   | Jenkins           |   |
        |   | Java / JDK 21     |   |
        |   |                   |   |
        |   | Port 8080         |   |
        |   | Port 50000        |   |
        |   +---------+---------+   |
        |             |             |
        |             v             |
        |   +-------------------+   |
        |   | Docker Volume     |   |
        |   | jenkins_home      |   |
        |   |                   |   |
        |   | Jobs              |   |
        |   | Plugins           |   |
        |   | Configurations    |   |
        |   | Credentials       |   |
        |   +-------------------+   |
        +---------------------------+
```

---

# Part 14: Important Docker Commands

| Task                | Command                                                                  |
| ------------------- | ------------------------------------------------------------------------ |
| Pull Jenkins        | `docker pull jenkins/jenkins:lts-jdk21`                                  |
| List containers     | `docker ps`                                                              |
| List all containers | `docker ps -a`                                                           |
| View logs           | `docker logs jenkins`                                                    |
| Follow logs         | `docker logs -f jenkins`                                                 |
| Stop Jenkins        | `docker stop jenkins`                                                    |
| Start Jenkins       | `docker start jenkins`                                                   |
| Restart Jenkins     | `docker restart jenkins`                                                 |
| Enter container     | `docker exec -it jenkins bash`                                           |
| Get admin password  | `docker exec jenkins cat /var/jenkins_home/secrets/initialAdminPassword` |
| Inspect container   | `docker inspect jenkins`                                                 |
| List volumes        | `docker volume ls`                                                       |
| Inspect volume      | `docker volume inspect jenkins_home`                                     |

