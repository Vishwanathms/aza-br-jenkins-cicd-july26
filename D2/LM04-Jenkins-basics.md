# Lab Manual: Jenkins Freestyle Jobs, Folder Structure & Build Triggers

**Platform:** Jenkins running as a Docker container
**Lab Type:** Hands-on
**Prerequisite:** Jenkins server is already installed, running, and accessible through the browser.

---

# Lab Objectives

By the end of this lab, students will be able to:

1. Create and configure **Jenkins Freestyle Jobs**
2. Organize Jenkins jobs using **Folders**
3. Configure different **Build Triggers**
4. Trigger jobs manually
5. Configure scheduled builds using **Build periodically**
6. Configure **Poll SCM**
7. Understand GitHub webhook-based triggers
8. Verify build history and console output

---

# Lab 1 – Verify the Jenkins Docker Server

## Step 1: Check the Jenkins Container

On the Jenkins Docker host, run:

```bash
docker ps
```

Example:

```text
CONTAINER ID   IMAGE                 PORTS
a12bc34de56f   jenkins/jenkins:lts   0.0.0.0:8080->8080/tcp
```

If the container is stopped:

```bash
docker ps -a
```

Start it using:

```bash
docker start <jenkins-container-name>
```

Example:

```bash
docker start jenkins
```

---

## Step 2: Access Jenkins

Open:

```text
http://<JENKINS-SERVER-IP>:8080
```

Example:

```text
http://192.168.1.100:8080
```

Log in using your Jenkins administrator credentials.

---

# Lab 2 – Create Your First Freestyle Job

## Objective

Create a simple Jenkins Freestyle project and execute Linux commands inside the Jenkins container.

---

## Step 1: Create a New Job

From the Jenkins Dashboard:

1. Click **New Item**
2. Enter the item name:

```text
My-First-Freestyle-Job
```

3. Select:

**Freestyle project**

4. Click **OK**

---

## Step 2: Add a Description

Under **General → Description**, enter:

```text
My first Jenkins Freestyle Job running on Docker-based Jenkins.
```

---

## Step 3: Configure Build Step

Scroll down to:

**Build Steps**

Click:

**Add build step → Execute shell**

Enter:

```bash
echo "======================================"
echo "Welcome to Jenkins Freestyle Job"
echo "======================================"

echo "Build Number: $BUILD_NUMBER"

echo "Job Name: $JOB_NAME"

echo "Workspace: $WORKSPACE"

echo "Jenkins URL: $JENKINS_URL"

echo "Current User:"
whoami

echo "Current Directory:"
pwd

echo "Current Date:"
date

echo "======================================"
echo "Build Completed Successfully"
echo "======================================"
```

---

## Step 4: Save the Job

Click:

**Save**

You should now see the job page.

---

# Lab 3 – Run the Freestyle Job Manually

## Step 1: Trigger the Job

Click:

**Build Now**

A new build should appear under:

**Build History**

Example:

```text
#1
```

---

## Step 2: Check Console Output

Click:

**Build #1**

Then:

**Console Output**

You should see output similar to:

```text
Started by user admin

Welcome to Jenkins Freestyle Job

Build Number: 1
Job Name: My-First-Freestyle-Job

Current User:
jenkins

Current Directory:
/var/jenkins_home/workspace/My-First-Freestyle-Job

Build Completed Successfully

Finished: SUCCESS
```

---

# Lab 4 – Understanding Jenkins Workspace

Every Freestyle job gets its own workspace.

For example:

```text
/var/jenkins_home/workspace/My-First-Freestyle-Job
```

Let's verify this.

Go to:

**My-First-Freestyle-Job → Configure**

Under:

**Build Steps → Execute shell**

Add:

```bash
echo "Jenkins Workspace:"
echo $WORKSPACE

echo "Creating sample file..."

echo "Created by Jenkins Build $BUILD_NUMBER" > sample.txt

echo "Files available in workspace:"
ls -la

echo "Contents of sample.txt:"
cat sample.txt
```

Click:

**Save**

Then:

**Build Now**

Check:

**Console Output**

---

# Lab 5 – Verify the Workspace Inside the Docker Container

From the Docker host:

```bash
docker ps
```

Identify the Jenkins container.

Enter the container:

```bash
docker exec -it jenkins bash
```

If your container has another name:

```bash
docker exec -it <container-name> bash
```

Navigate to the workspace:

```bash
cd /var/jenkins_home/workspace
```

Run:

```bash
ls -l
```

You should see:

```text
My-First-Freestyle-Job
```

Enter the directory:

```bash
cd My-First-Freestyle-Job
```

Run:

```bash
ls -la
```

Check:

```bash
cat sample.txt
```

Exit:

```bash
exit
```

---

# Lab 6 – Create Jenkins Folder Structure

## Objective

Organize Jenkins jobs into folders.

A real project may have environments such as:

```text
Development
Testing
Production
```

Instead of putting every Jenkins job directly on the Dashboard, folders can be used to organize them.

---

# Step 1: Create Development Folder

From Jenkins Dashboard:

**New Item**

Enter:

```text
Development
```

Select:

**Folder**

Click:

**OK**

Add description:

```text
Jenkins jobs for Development Environment
```

Click:

**Save**

---

# Step 2: Create Testing Folder

Return to:

**Dashboard**

Click:

**New Item**

Enter:

```text
Testing
```

Select:

**Folder**

Click:

**OK → Save**

---

# Step 3: Create Production Folder

Create another folder:

```text
Production
```

Select:

**Folder**

Click:

**OK → Save**

---

# Expected Folder Structure

The Jenkins Dashboard should now contain:

```text
Jenkins
│
├── Development
│
├── Testing
│
├── Production
│
└── My-First-Freestyle-Job
```

---

# Lab 7 – Create Freestyle Jobs Inside Folders

Let's create jobs inside each folder.

---

## Step 1: Development Job

Open:

**Dashboard → Development**

Click:

**New Item**

Enter:

```text
Build-App
```

Select:

**Freestyle project**

Click:

**OK**

---

## Configure Build Step

Go to:

**Build Steps → Add build step → Execute shell**

Enter:

```bash
echo "================================="
echo "Development Build"
echo "================================="

echo "Job Name: $JOB_NAME"
echo "Build Number: $BUILD_NUMBER"

echo "Building Development Application..."

sleep 3

echo "Development Build Completed"

echo "================================="
```

Click:

**Save**

---

## Run the Job

Click:

**Build Now**

Check:

**Console Output**

Expected:

```text
Development Build

Building Development Application...

Development Build Completed

Finished: SUCCESS
```

---

# Lab 8 – Create Testing Job

Navigate to:

**Dashboard → Testing**

Click:

**New Item**

Create:

```text
Test-App
```

Select:

**Freestyle project**

Add:

**Build Steps → Execute shell**

```bash
echo "================================="
echo "Testing Environment"
echo "================================="

echo "Running Application Tests..."

sleep 3

echo "Test Case 1: PASS"
echo "Test Case 2: PASS"
echo "Test Case 3: PASS"

echo "All Tests Passed"

echo "================================="
```

Save and run the job.

---

# Lab 9 – Create Production Job

Navigate to:

**Dashboard → Production**

Create:

```text
Deploy-App
```

Select:

**Freestyle project**

Configure:

**Build Steps → Execute shell**

```bash
echo "================================="
echo "Production Deployment"
echo "================================="

echo "Starting deployment..."

sleep 3

echo "Deploying application..."

sleep 2

echo "Application successfully deployed."

echo "Deployment Build Number: $BUILD_NUMBER"

echo "================================="
```

Save and run.

---

# Final Folder Structure

You should now have:

```text
Jenkins
│
├── Development
│   └── Build-App
│
├── Testing
│   └── Test-App
│
├── Production
│   └── Deploy-App
│
└── My-First-Freestyle-Job
```

This is an example of a simple Jenkins job organization structure.

---

# Lab 10 – Introduction to Jenkins Build Triggers

Jenkins supports several methods for automatically starting jobs.

Open a Freestyle job:

**Job → Configure**

Find:

**Build Triggers**

Depending on installed plugins, options can include:

```text
Trigger builds remotely
Build after other projects are built
Build periodically
GitHub hook trigger for GITScm polling
Poll SCM
```

We will test the important trigger types.

---

# Lab 11 – Build Periodically

## Objective

Automatically execute a Jenkins job according to a schedule.

Open:

**Development → Build-App → Configure**

Under:

**Build Triggers**

Select:

**Build periodically**

A **Schedule** field will appear.

Jenkins uses cron-style syntax.

---

## Jenkins Cron Format

```text
MINUTE HOUR DAY-OF-MONTH MONTH DAY-OF-WEEK
```

Example:

```text
H/5 * * * *
```

This means approximately:

> Run every 5 minutes.

Jenkins recommends using `H` to distribute scheduled jobs rather than having many jobs execute at exactly the same minute.

---

## Configure the Job

Enter:

```text
H/5 * * * *
```

Click:

**Save**

Now wait for the scheduled execution.

Check:

**Build History**

New builds should automatically appear.

---

# Lab 12 – Common Jenkins Build Schedules

| Requirement                    | Jenkins Schedule |
| ------------------------------ | ---------------- |
| Approximately every 5 minutes  | `H/5 * * * *`    |
| Approximately every 15 minutes | `H/15 * * * *`   |
| Once every hour                | `H * * * *`      |
| Once every day                 | `H H * * *`      |
| Every weekday                  | `H H * * 1-5`    |
| Every Sunday                   | `H H * * 0`      |

### Important

Jenkins' `H` means **hash**, not literally "every".

It allows Jenkins to spread jobs across different times.

For example:

```text
H/5 * * * *
```

runs periodically at roughly 5-minute intervals based on the hash assigned to the job.

---

# Lab 13 – Build After Another Project Is Built

## Objective

Automatically start one Jenkins job after another Jenkins job completes.

We can create a simple workflow:

```text
Build
   ↓
Test
   ↓
Deploy
```

For this exercise, it is easiest to create the three jobs within the **same folder**.

---

# Step 1: Create Application Folder

From Dashboard:

**New Item → Folder**

Enter:

```text
MyApplication
```

Click:

**OK → Save**

---

# Step 2: Create Three Jobs

Inside `MyApplication`, create:

```text
01-Build
02-Test
03-Deploy
```

All three should be:

**Freestyle projects**

---

# Step 3: Configure Build Job

Open:

```text
MyApplication/01-Build
```

Add:

**Build Steps → Execute shell**

```bash
echo "============================"
echo "STEP 1 - BUILD"
echo "============================"

echo "Building application..."

sleep 5

echo "Application Build Successful"

echo "Build Number: $BUILD_NUMBER"
```

Save.

---

# Step 4: Configure Test Job Trigger

Open:

```text
MyApplication/02-Test
```

Go to:

**Configure → Build Triggers**

Select:

**Build after other projects are built**

Enter:

```text
01-Build
```

Select:

**Trigger only if build is stable**

---

## Add Test Build Step

```bash
echo "============================"
echo "STEP 2 - TEST"
echo "============================"

echo "Running automated tests..."

sleep 5

echo "Unit Test: PASS"
echo "Integration Test: PASS"

echo "Testing completed successfully"
```

Save.

---

# Step 5: Configure Deploy Job Trigger

Open:

```text
MyApplication/03-Deploy
```

Go to:

**Configure → Build Triggers**

Select:

**Build after other projects are built**

Enter:

```text
02-Test
```

Select:

**Trigger only if build is stable**

---

## Add Deploy Build Step

```bash
echo "============================"
echo "STEP 3 - DEPLOY"
echo "============================"

echo "Starting deployment..."

sleep 5

echo "Application deployed successfully"

echo "Deployment completed."
```

Save.

---

# Lab 14 – Test the Build → Test → Deploy Chain

Only manually trigger:

```text
01-Build
```

Click:

**Build Now**

Observe the jobs.

Expected workflow:

```text
01-Build
    ↓
 SUCCESS
    ↓
02-Test
    ↓
 SUCCESS
    ↓
03-Deploy
    ↓
 SUCCESS
```

You should not have to manually start `02-Test` or `03-Deploy`.

---

