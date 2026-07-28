# Lab Manual

# Lab 08 – Building a Java Application using Jenkins Pipeline

## Objective

In this lab, you will integrate a Java Maven application with GitHub and Jenkins.

By the end of this lab, you will be able to:

* Initialize a local Maven project as a Git repository.
* Create a new GitHub repository.
* Configure SSH authentication between your workstation and GitHub.
* Push the project to GitHub.
* Add the provided Jenkins Pipeline (`Jenkinsfile`) to the project.
* Configure a Jenkins Pipeline Job.
* Execute the pipeline.
* Verify successful execution.

---

# Lab Architecture

```text
                Student Workstation
        +------------------------------+
        | Sample Maven Project         |
        | Jenkinsfile                  |
        +--------------+---------------+
                       |
                       | Git Push (SSH)
                       |
                       ▼
              GitHub Repository
                       |
                       | Git Clone
                       ▼
              Jenkins Pipeline
                       |
         Checkout Source Code
                       |
                  Maven Build
                       |
                 Docker Build
                       |
                 Docker Push
                       |
                  Docker Run
                       |
                  curl Verify
```

---

# Prerequisites

Before starting this lab, ensure that:

* Java JDK is installed.
* Maven is installed.
* Git is installed.
* Docker is installed.
* Jenkins is available.
* GitHub account is available.
* SSH key pair has been generated (or create one during this lab).
* Sample Maven project builds successfully using:

```bash
mvn clean package
```

---

# Estimated Duration

**60–75 Minutes**

---

# Task 1 – Verify the Sample Maven Project

Navigate to the project directory.

```bash
cd ~/sample-maven-app
```

Verify the project builds successfully.

```bash
mvn clean package
```

Expected result:

* Build completes successfully.
* A JAR file is generated under:

```text
target/
```

---

# Task 2 – Create an Empty GitHub Repository

1. Log in to GitHub.
2. Click **New Repository**.
3. Enter a repository name.

Example:

```text
sample-maven-app
```

4. Select:

* Private or Public (as instructed)
* **Do NOT initialize with:**

  * README
  * .gitignore
  * License

The repository must remain **empty**.

Click **Create Repository**.

---

# Task 3 – Generate an SSH Key (If Not Already Available)

Check whether an SSH key already exists.

```bash
ls ~/.ssh
```

If you see:

```text
id_ed25519
id_ed25519.pub
```

You may reuse the existing key.

Otherwise, create a new key.

```bash
ssh-keygen -t ed25519 -C "your-email@example.com"
```

Accept the default location.

---

# Task 4 – Add the SSH Public Key to GitHub

Display the public key.

```bash
cat ~/.ssh/id_ed25519.pub
```

Copy the entire output.

In GitHub:

Settings

↓

SSH and GPG Keys

↓

New SSH Key

Paste the copied key.

Click **Add SSH Key**.

---

# Task 5 – Test SSH Connectivity

Verify SSH access.

```bash
ssh -T git@github.com
```

Expected output:

```text
Hi <username>!
You've successfully authenticated...
```

---

# Task 6 – Initialize the Local Maven Project as a Git Repository

Navigate to the project directory.

```bash
cd ~/sample-maven-app
```

Initialize Git.

```bash
git init
```

Check the repository status.

```bash
git status
```

---

# Task 7 – Create a .gitignore File

Create a `.gitignore` file.

```bash
touch .gitignore
```

Add the following content.

```text
target/
*.class
*.log
.idea/
.vscode/
```

Save the file.

---

# Task 8 – Add All Project Files

Stage all files.

```bash
git add .
```

Verify staged files.

```bash
git status
```

---

# Task 9 – Commit the Project

Create the first commit.

```bash
git commit -m "Initial Maven project"
```

---

# Task 10 – Configure the GitHub Remote Repository

Copy the SSH URL from GitHub.

Example:

```text
git@github.com:student/sample-maven-app.git
```

Add it as the remote.

```bash
git remote add origin git@github.com:student/sample-maven-app.git
```

Verify.

```bash
git remote -v
```

Expected:

```text
origin  git@github.com:student/sample-maven-app.git
```

---

# Task 11 – Push the Project to GitHub

Rename the branch to **main**.

```bash
git branch -M main
```

Push the project.

```bash
git push -u origin main
```

Refresh GitHub.

The repository should now contain:

```text
pom.xml

src/

.gitignore
```

---

# Task 12 – Add the Jenkinsfile

A Jenkins Pipeline (`Jenkinsfile`) has been provided separately by the instructor.

Copy the provided `Jenkinsfile` into the root of your Maven project.

The project structure should now look like:

```text
sample-maven-app/

├── src/
├── pom.xml
├── Dockerfile
├── Jenkinsfile
└── .gitignore
```

---

# Task 13 – Commit the Jenkinsfile

Stage the new file.

```bash
git add Jenkinsfile
```

Commit the change.

```bash
git commit -m "Added Jenkins pipeline"
```

Push the changes.

```bash
git push
```

Verify that the `Jenkinsfile` is visible in the GitHub repository.

---

# Task 14 – Create a Jenkins Pipeline Job

Open the Jenkins dashboard.

Select:

**New Item**

Enter a job name.

Example:

```text
sample-maven-pipeline
```

Select:

**Pipeline**

Click **OK**.

---

# Task 15 – Configure the Pipeline Job

Under **General**:

* Enable **Discard Old Builds** (optional, if instructed).

Under **Pipeline**:

Definition:

```text
Pipeline script from SCM
```

SCM:

```text
Git
```

Repository URL:

Use the SSH URL for your GitHub repository, for example:

```text
git@github.com:student/sample-maven-app.git
```

Credentials:

Select the configured **Git SSH credentials** provided by the instructor or previously added in Jenkins.

Branch Specifier:

```text
*/main
```

Script Path:

```text
Jenkinsfile
```

Click **Save**.

---

# Task 16 – Build the Pipeline

Click:

**Build Now**

Observe the pipeline stages as they execute.

Expected stages include:

* Checkout Source Code
* Build Java Application
* Run Unit Tests
* Build Docker Image
* Login to Docker Hub
* Push Docker Image
* Stop Existing Container
* Deploy Application
* Verify Deployment

If any stage fails, review the console output to identify the cause.

---

# Task 17 – Review the Console Output

Select the completed build.

Open:

**Console Output**

Review the logs for each stage and verify that all stages completed successfully.

---

# Task 18 – Verify the Running Container

On the Jenkins server, list the running containers.

```bash
docker ps
```

Verify that the application container is running.

Example output:

```text
CONTAINER ID   IMAGE                             PORTS
abc123         sample-java-app:latest            0.0.0.0:8081->8080/tcp
```

---

# Task 19 – Validate the Application

From the Jenkins server, execute:

```bash
curl http://localhost:8081
```

Expected response:

```text
Welcome to Sample Java Application

Application Running Successfully
```

You can also verify the application using a web browser:

```text
http://<jenkins-server-ip>:8081
```

---

# Lab Verification Checklist

| Task                               | Status |
| ---------------------------------- | ------ |
| Maven project builds successfully  | ☐      |
| Empty GitHub repository created    | ☐      |
| SSH key added to GitHub            | ☐      |
| Git repository initialized locally | ☐      |
| `.gitignore` created               | ☐      |
| Initial commit completed           | ☐      |
| Remote repository configured       | ☐      |
| Project pushed to GitHub           | ☐      |
| `Jenkinsfile` added and committed  | ☐      |
| Jenkins Pipeline job created       | ☐      |
| Pipeline configured from SCM       | ☐      |
| Pipeline executed successfully     | ☐      |
| Docker image built and pushed      | ☐      |
| Docker container deployed          | ☐      |
| Application verified using `curl`  | ☐      |

---

# Expected Outcome

At the end of this lab, you will have successfully:

* Connected a local Maven project to a new GitHub repository using SSH.
* Managed the project using Git version control.
* Added a Jenkins Declarative Pipeline to the project.
* Configured a Jenkins Pipeline job that retrieves the source code directly from GitHub.
* Executed an end-to-end CI/CD workflow including build, containerization, deployment, and application verification. This workflow forms the foundation for more advanced CI/CD practices such as automated testing, Kubernetes deployments, and GitOps.
