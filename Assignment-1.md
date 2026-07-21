# Home Assignment 1

# Jenkins Installation and Build Automation

**Module:** Continuous Integration using Jenkins

**Difficulty:** Beginner to Intermediate

**Objective:**
The objective of this assignment is to install and configure Jenkins, integrate it with GitHub, and automate the build process for both Java and Python applications using Jenkins Freestyle Jobs.

---

# Learning Outcomes

After completing this assignment, the student will be able to:

* Install Jenkins LTS on Ubuntu Linux
* Configure Jenkins for production use
* Install and configure Jenkins plugins
* Configure Java, Git and Maven
* Integrate Jenkins with GitHub
* Create Jenkins Freestyle Jobs
* Build Java applications using Maven
* Execute Python applications using Jenkins
* Archive build artifacts
* Configure automatic build triggers
* Analyze Jenkins build logs

---

# Scenario

ABC Technologies has recently adopted Jenkins as its Continuous Integration (CI) server.

As a DevOps Engineer, your responsibility is to deploy Jenkins, integrate it with GitHub, automate the build process for Java and Python projects, and ensure builds are automatically triggered whenever developers push code to GitHub.

---

# Prerequisites

Before starting the assignment, ensure the following:

* Ubuntu Server (22.04 or 24.04)
* Internet Connectivity
* GitHub Account
* Basic Linux Knowledge
* Sudo Access
* Java JDK 17 or above
* Maven
* Git
* Python 3

---

# Assignment Tasks

---

# Task 1 – Install Java

### Objective

Install Java required for Jenkins.

### Deliverables

* Install OpenJDK
* Verify Java version

### Evidence Required

```
java -version
```

Screenshot Required

---

# Task 2 – Install Jenkins LTS

### Objective

Install Jenkins Long-Term Support Version.

### Deliverables

* Add Jenkins Repository
* Install Jenkins
* Enable Jenkins Service
* Start Jenkins

### Verify

```
systemctl status jenkins
```

### Evidence Required

Screenshot of

* Jenkins Service Running
* Jenkins Web Console

---

# Task 3 – Initial Jenkins Configuration

### Objective

Configure Jenkins after installation.

### Deliverables

* Unlock Jenkins
* Create Administrator User
* Configure Jenkins URL
* Install Suggested Plugins

### Screenshot Required

* Unlock Screen
* Plugin Installation
* Dashboard
* Admin User Created

---

# Task 4 – Install Required Jenkins Plugins

Install the following plugins.

| Plugin            | Purpose                |
| ----------------- | ---------------------- |
| Git               | Source Code Management |
| GitHub            | GitHub Integration     |
| Maven Integration | Maven Builds           |
| Pipeline          | Pipeline Support       |
| Workspace Cleanup | Cleanup                |
| Build Timestamp   | Build History          |
| SSH Agent         | Remote Authentication  |

### Evidence

Screenshot of Installed Plugins

---

# Task 5 – Configure Global Tools

Configure the following under

**Manage Jenkins → Tools**

Configure

* Git
* Maven
* JDK

### Verification

Screenshot of Tool Configuration

---

# Task 6 – Create GitHub Repository

Create one GitHub repository named

```
jenkins-build-demo
```

Repository Structure

```
jenkins-build-demo
│
├── java-app
│   ├── pom.xml
│   └── src
│       └── main
│            └── java
│                 └── App.java
│
├── python-app
│   └── app.py
│
└── README.md
```

---

# Task 7 – Push Source Code

Push the complete repository to GitHub.

### Evidence

* GitHub Repository URL
* Commit History

---

# Task 8 – Integrate Jenkins with GitHub

Configure Git credentials inside Jenkins.

Repository should be accessible from Jenkins.

### Verify

```
Test Connection
```

Screenshot Required

---

# Task 9 – Create Java Freestyle Job

Job Name

```
Java-Build
```

### Configure

Source Code

* Git Repository

Build Step

```
Invoke Top-Level Maven Targets
```

Goals

```
clean package
```

Post Build Action

```
Archive Artifacts

target/*.jar
```

### Expected Output

```
BUILD SUCCESS
```

### Deliverables

* Successful Build
* Archived JAR
* Console Output

Screenshots Required

---

# Task 10 – Create Python Freestyle Job

Job Name

```
Python-Build
```

Source Code

Git Repository

Build Step

```
Execute Shell
```

Example

```bash
python3 python-app/app.py
```

### Expected Output

```
Hello from Python
```

### Deliverables

* Successful Execution
* Console Output

Screenshot Required

---

# Task 11 – Configure Automatic Build Trigger

Configure **one** of the following:

### Option A

Poll SCM

Example

```
H/2 * * * *
```

OR

### Option B

GitHub Webhook

```
http://<jenkins-server>:8080/github-webhook/
```

### Verify

Push new code to GitHub.

Jenkins should automatically trigger the build.

---

# Task 12 – Verify Build Execution

Both jobs should execute successfully.

Verify

* Build Number
* Build Duration
* Build Status
* Console Output

Screenshots Required

---

# Deliverables

Students must submit:

* Jenkins Installation Screenshot
* Jenkins Dashboard Screenshot
* Installed Plugins Screenshot
* Global Tool Configuration Screenshot
* GitHub Repository URL
* Java Freestyle Job Configuration
* Python Freestyle Job Configuration
* Successful Java Build Screenshot
* Successful Python Build Screenshot
* Archived Maven Artifact Screenshot
* Automatic Build Trigger Screenshot
* Console Output of Java Build
* Console Output of Python Build

---

# Expected Directory Structure

```
jenkins-build-demo
│
├── java-app
│   ├── pom.xml
│   ├── src
│   │    └── main
│   │         └── java
│   │              └── App.java
│   ├── target
│   │     └── app.jar
│   │
│   └── README.md
│
├── python-app
│   ├── app.py
│   ├── requirements.txt
│   └── README.md
│
└── README.md
```

---

# Submission Checklist

| Item                               | Status |
| ---------------------------------- | ------ |
| Java Installed                     | ☐      |
| Jenkins Installed                  | ☐      |
| Administrator User Created         | ☐      |
| Required Plugins Installed         | ☐      |
| Git Configured                     | ☐      |
| Maven Configured                   | ☐      |
| GitHub Repository Created          | ☐      |
| Jenkins Connected to GitHub        | ☐      |
| Java Freestyle Job Created         | ☐      |
| Java Build Successful              | ☐      |
| JAR Archived                       | ☐      |
| Python Freestyle Job Created       | ☐      |
| Python Build Successful            | ☐      |
| Automatic Build Trigger Configured | ☐      |
| Build Logs Collected               | ☐      |
| Screenshots Attached               | ☐      |
