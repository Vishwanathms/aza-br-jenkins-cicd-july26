# Lab Manual

# Docker Installation on Ubuntu 24.04 LTS

**Course:** Docker Fundamentals

**Lab No.:** 01

**Lab Title:** Installing Docker Engine on Ubuntu 24.04 LTS

**Duration:** 45–60 Minutes

**Difficulty:** Beginner

---

# Lab Objective

In this lab, you will learn how to install Docker Engine on Ubuntu 24.04 LTS, verify the installation, manage Docker services, and run your first container.

---

# Learning Outcomes

After completing this lab, you will be able to:

* Understand Docker architecture
* Install Docker Engine on Ubuntu 24.04
* Configure Docker repositories
* Manage the Docker service
* Run Docker containers
* Download Docker images from Docker Hub
* Execute commands inside containers
* Remove containers and images

---

# Lab Architecture

```text
                    +-----------------------+
                    |   Docker Hub          |
                    +-----------+-----------+
                                |
                           Pull Images
                                |
                                |
                    +-----------v-----------+
                    | Ubuntu 24.04 Server   |
                    |                       |
                    |  Docker Engine        |
                    |  Docker CLI           |
                    |  containerd           |
                    +-----------+-----------+
                                |
                  +-------------+-------------+
                  |                           |
          +-------v-------+           +-------v-------+
          | nginx         |           | ubuntu        |
          | Container     |           | Container     |
          +---------------+           +---------------+
```

---

# Prerequisites

* Ubuntu 24.04 LTS
* Internet Connection
* Sudo User
* Minimum 2 GB RAM
* 20 GB Disk Space

---

# Lab Tasks

---

# Task 1 – Verify Ubuntu Version

### Check Operating System

```bash
cat /etc/os-release
```

or

```bash
lsb_release -a
```

Expected Output

```text
Ubuntu 24.04 LTS
```

---

# Task 2 – Update the Server

Update package information.

```bash
sudo apt update
```

Upgrade packages.

```bash
sudo apt upgrade -y
```

---

# Task 3 – Install Required Packages

Install prerequisites required by Docker.

```bash
sudo apt install -y \
ca-certificates \
curl \
gnupg \
lsb-release
```

Verify

```bash
curl --version
```

---

# Task 4 – Create Docker Keyring Directory

```bash
sudo install -m 0755 -d /etc/apt/keyrings
```

Verify

```bash
ls -ld /etc/apt/keyrings
```

---

# Task 5 – Download Docker GPG Key

```bash
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | \
sudo gpg --dearmor -o /etc/apt/keyrings/docker.gpg
```

Set permissions.

```bash
sudo chmod a+r /etc/apt/keyrings/docker.gpg
```

Verify

```bash
ls -l /etc/apt/keyrings
```

---

# Task 6 – Add Docker Repository

```bash
echo \
"deb [arch=$(dpkg --print-architecture) \
signed-by=/etc/apt/keyrings/docker.gpg] \
https://download.docker.com/linux/ubuntu \
$(. /etc/os-release && echo "$VERSION_CODENAME") stable" | \
sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
```

Update repositories.

```bash
sudo apt update
```

Verify

```bash
apt-cache policy docker-ce
```

---

# Task 7 – Install Docker Engine

Install Docker packages.

```bash
sudo apt install -y \
docker-ce \
docker-ce-cli \
containerd.io \
docker-buildx-plugin \
docker-compose-plugin
```

---

# Task 8 – Verify Installation

Check Docker version.

```bash
docker --version
```

Expected

```text
Docker version XX.XX
```

---

# Task 9 – Start Docker Service

Enable Docker during boot.

```bash
sudo systemctl enable docker
```

Start Docker.

```bash
sudo systemctl start docker
```

Check status.

```bash
sudo systemctl status docker
```

Expected

```text
Active: active (running)
```

---

# Task 10 – Verify Docker Engine

Run

```bash
docker info
```

Check

* Storage Driver
* Server Version
* Runtime
* Images
* Containers

