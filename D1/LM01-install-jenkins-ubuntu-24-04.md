# Lab Manual: Installing Jenkins on Ubuntu 24.04 LTS

## Lab Objective

In this lab, you will install and configure Jenkins on an Ubuntu 24.04 LTS server.

### Lab Environment

| Component        | Requirement                    |
| ---------------- | ------------------------------ |
| Operating System | Ubuntu 24.04 LTS               |
| RAM              | Minimum 2 GB, 4 GB recommended |
| CPU              | 2 vCPU recommended             |
| Disk             | 20 GB+                         |
| Java             | OpenJDK 21                     |
| Jenkins Port     | TCP 8080                       |
| Access           | sudo/root privileges           |

---

## Step 1: Verify Ubuntu Version

```bash
lsb_release -a
```

Or:

```bash
cat /etc/os-release
```

Confirm that the server is running:

```text
Ubuntu 24.04 LTS
```

---

## Step 2: Update the System

Update the package repository:

```bash
sudo apt update
```

Upgrade installed packages if required:

```bash
sudo apt upgrade -y
```

---

## Step 3: Install Java

Jenkins requires Java. Install OpenJDK 21:

```bash
sudo apt install openjdk-21-jdk -y
```

Verify the Java installation:

```bash
java -version
```

Expected output will be similar to:

```text
openjdk version "21.x.x"
OpenJDK Runtime Environment
OpenJDK 64-Bit Server VM
```

Check the Java path:

```bash
which java
```

---

## Step 4: Add the Jenkins Repository Key

Install required packages:

```bash
sudo apt install wget gnupg -y
```

Download and install the Jenkins repository signing key:

```bash
sudo wget -O /usr/share/keyrings/jenkins-keyring.asc \
https://pkg.jenkins.io/debian-stable/jenkins.io-2026.key
```

> **Note:** Jenkins periodically updates its repository signing keys. If this key URL changes, use the current instructions from the official Jenkins installation documentation.

---

## Step 5: Add the Jenkins Repository

Add the Jenkins stable repository:

```bash
echo "deb [signed-by=/usr/share/keyrings/jenkins-keyring.asc] https://pkg.jenkins.io/debian-stable binary/" \
| sudo tee /etc/apt/sources.list.d/jenkins.list > /dev/null
```

Update the package repository:

```bash
sudo apt update
```

---

## Step 6: Install Jenkins

Install Jenkins:

```bash
sudo apt install jenkins -y
```

Check the installed Jenkins version:

```bash
jenkins --version
```

---

## Step 7: Start Jenkins Service

Start Jenkins:

```bash
sudo systemctl start jenkins
```

Enable Jenkins to automatically start after server reboot:

```bash
sudo systemctl enable jenkins
```

Check Jenkins service status:

```bash
sudo systemctl status jenkins
```

The expected status should show:

```text
Active: active (running)
```

Press `q` to exit the status screen.

You can also verify using:

```bash
sudo systemctl is-active jenkins
```

Expected output:

```text
active
```

---

## Step 8: Verify Jenkins Port 8080

Jenkins uses port **8080** by default.

Run:

```bash
sudo ss -tulpn | grep 8080
```

You should see that port `8080` is listening.

---

## Step 9: Configure Ubuntu Firewall

Check firewall status:

```bash
sudo ufw status
```

If UFW is enabled, allow Jenkins port 8080:

```bash
sudo ufw allow 8080/tcp
```

Reload UFW:

```bash
sudo ufw reload
```

Verify:

```bash
sudo ufw status
```

> If Jenkins is running on AWS, Azure, GCP, VMware, or another environment with an external firewall, make sure **TCP port 8080** is also permitted there. Restrict access to trusted IP addresses where possible rather than exposing Jenkins publicly.

---

## Step 10: Find the Server IP Address

Run:

```bash
hostname -I
```

For example:

```text
192.168.1.100
```

---

## Step 11: Access Jenkins

Open a web browser and navigate to:

```text
http://<JENKINS-SERVER-IP>:8080
```

Example:

```text
http://192.168.1.100:8080
```

You should see the **Unlock Jenkins** page.

---

## Step 12: Get the Initial Administrator Password

On the Jenkins server, run:

```bash
sudo cat /var/lib/jenkins/secrets/initialAdminPassword
```

You will see a password similar to:

```text
xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx
```

Copy the password.

Paste it into the **Administrator password** field on the Jenkins web interface.

Click:

**Continue**

---

## Step 13: Install Jenkins Plugins

Jenkins will display the **Customize Jenkins** screen.

Select:

**Install suggested plugins**

Jenkins will automatically download and install commonly used plugins.

Wait until the installation completes.

---

## Step 14: Create the First Admin User

Enter the required information:

* Username
* Password
* Confirm password
* Full name
* Email address

Click:

**Save and Continue**

---

## Step 15: Configure Jenkins URL

Jenkins will display the instance configuration.

The Jenkins URL may look like:

```text
http://192.168.1.100:8080/
```

Verify the URL and click:

**Save and Finish**

Then click:

**Start using Jenkins**

The Jenkins Dashboard should appear.

