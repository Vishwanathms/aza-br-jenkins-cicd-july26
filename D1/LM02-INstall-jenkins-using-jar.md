Here is a step-by-step lab manual for installing and running **Jenkins using the WAR/JAR method** on Ubuntu 24.04.

# Lab Manual: Install Jenkins Using WAR File on Ubuntu 24.04

## Lab Objective

In this lab, you will:

* Install Java on Ubuntu 24.04
* Download the Jenkins WAR file
* Run Jenkins manually using Java
* Access Jenkins on port 8080
* Complete the initial Jenkins configuration
* Create and run a sample Jenkins job

> **Note:** Jenkins is distributed as a `.war` file for standalone execution. Although you run it with the `java -jar` command, the downloaded file is normally `jenkins.war`.

---

## Lab Requirements

| Component    | Requirement                       |
| ------------ | --------------------------------- |
| OS           | Ubuntu 24.04 LTS                  |
| RAM          | Minimum 2 GB                      |
| CPU          | 2 vCPU recommended                |
| Disk         | 10 GB+                            |
| Java         | OpenJDK 21                        |
| Jenkins Port | 8080                              |
| Internet     | Required for download and plugins |
| Privileges   | sudo access                       |

---

# Part 1: Prepare the Ubuntu Server

## Step 1: Check Ubuntu Version

```bash
lsb_release -a
```

Alternatively:

```bash
cat /etc/os-release
```

Verify that the system is running Ubuntu 24.04.

---

## Step 2: Update Package Repository

Run:

```bash
sudo apt update
```

Optionally upgrade installed packages:

```bash
sudo apt upgrade -y
```

---

# Part 2: Install Java

## Step 3: Install OpenJDK 21

Run:

```bash
sudo apt install openjdk-21-jdk -y
```

Verify Java:

```bash
java -version
```

Expected output should indicate Java 21.

You can also check:

```bash
javac -version
```

---

# Part 3: Download Jenkins

## Step 4: Create a Directory for Jenkins

Create a dedicated directory:

```bash
sudo mkdir -p /opt/jenkins
```

Change ownership to the current user:

```bash
sudo chown $USER:$USER /opt/jenkins
```

Navigate to the directory:

```bash
cd /opt/jenkins
```

---

## Step 5: Download Jenkins WAR File

The safest approach is to download the current Jenkins LTS WAR file from the official Jenkins download page.

[Jenkins Download Page](https://www.jenkins.io/download/?utm_source=chatgpt.com)

For example, the general LTS WAR download endpoint is:

```bash
wget https://get.jenkins.io/war-stable/latest/jenkins.war
```

Verify that the file exists:

```bash
ls -lh
```

You should see:

```text
jenkins.war
```

---

# Part 4: Run Jenkins

## Step 6: Start Jenkins Using Java

Run:

```bash
java -jar jenkins.war
```

Jenkins will start and display logs directly in the terminal.

By default, Jenkins runs on:

```text
Port 8080
```

Look for startup messages indicating Jenkins is ready.

---

## Step 7: Access Jenkins

Find your server's IP address:

```bash
hostname -I
```

Open a browser and enter:

```text
http://<SERVER-IP>:8080
```

For example:

```text
http://192.168.1.100:8080
```

You should see:

**Unlock Jenkins**

---

# Part 5: Unlock Jenkins

## Step 8: Find the Initial Administrator Password

When Jenkins starts, the initial password location is displayed in the terminal.

If Jenkins is running with the default home directory for your current user, run:

```bash
cat ~/.jenkins/secrets/initialAdminPassword
```

Alternatively, check the Jenkins startup console output for the exact path.

Copy the password.

---

## Step 9: Unlock Jenkins

Return to the browser.

Paste the initial administrator password.

Click:

**Continue**

---

# Part 6: Configure Jenkins

## Step 10: Install Plugins

On the **Customize Jenkins** screen, select:

**Install suggested plugins**

Wait for the plugins to download and install.

---

## Step 11: Create Admin User

Enter:

* Username
* Password
* Confirm password
* Full name
* Email address

Click:

**Save and Continue**

---

## Step 12: Configure Jenkins URL

Verify the Jenkins URL.

For example:

```text
http://192.168.1.100:8080/
```

Click:

**Save and Finish**

Then click:

**Start using Jenkins**

You should now see the Jenkins Dashboard.

---

# Part 7: Run Jenkins in the Background

If you start Jenkins with:

```bash
java -jar jenkins.war
```

Jenkins is attached to your terminal.

Pressing `Ctrl+C` will stop Jenkins.

To run Jenkins in the background for a simple lab environment, use:

```bash
nohup java -jar /opt/jenkins/jenkins.war > /opt/jenkins/jenkins.log 2>&1 &
```

Verify the process:

```bash
ps -ef | grep jenkins
```

Check Jenkins logs:

```bash
tail -f /opt/jenkins/jenkins.log
```

Press `Ctrl+C` to stop following the logs. This does **not** stop Jenkins.

---

# Part 8: Run Jenkins on a Custom Port

To run Jenkins on port `9090`:

```bash
java -jar jenkins.war --httpPort=9090
```

Access Jenkins using:

```text
http://<SERVER-IP>:9090
```

To disable HTTP entirely, additional HTTPS or reverse-proxy configuration is required.

---

# Part 9: Configure a Custom Jenkins Home

By default, when running Jenkins manually as your current Linux user, Jenkins typically uses:

```text
~/.jenkins
```

Check:

```bash
ls -la ~/.jenkins
```

You can define a custom `JENKINS_HOME`:

```bash
mkdir -p /opt/jenkins/jenkins_home
```

Set the environment variable:

```bash
export JENKINS_HOME=/opt/jenkins/jenkins_home
```

Start Jenkins:

```bash
java -jar /opt/jenkins/jenkins.war
```

Verify:

```bash
echo $JENKINS_HOME
```

The Jenkins configuration, plugins, jobs, workspaces, and other persistent data will now be stored under:

```text
/opt/jenkins/jenkins_home
```

