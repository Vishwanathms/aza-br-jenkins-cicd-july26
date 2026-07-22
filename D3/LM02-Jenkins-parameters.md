# Lab Manual: Jenkins Jobs – Build Parameters, Build Environment & Post Build Actions

## Lab Information

**Lab Title:** Jenkins Job Configuration – Build Parameters, Build Environment & Post Build Actions

**Duration:** 3 Hours

**Difficulty:** Beginner to Intermediate

**Prerequisites**

* Ubuntu 24.04 Server
* Jenkins installed and running
* Administrator access to Jenkins
* Git installed on Jenkins server
* Java installed
* Docker is optional (not required for this lab)

---

# Lab Objectives

After completing this lab, students will be able to:

* Create and configure Jenkins Freestyle Jobs
* Use different Build Parameters
* Configure Build Environment options
* Execute Shell Scripts
* Archive Build Artifacts
* Send Build Notifications
* Understand Post Build Actions
* Build reusable parameterized jobs

---

# Lab Architecture

```
+-------------------------+
|      Jenkins Server     |
|                         |
|  Freestyle Job          |
|        │                |
|        ▼                |
| Build Parameters        |
|        │                |
|        ▼                |
| Build Environment       |
|        │                |
|        ▼                |
| Execute Shell           |
|        │                |
|        ▼                |
| Post Build Actions      |
+-------------------------+
```

---

# Exercise 1 – Create a New Freestyle Job

## Step 1

Open Jenkins

```
http://<jenkins-server>:8080
```

Login using your administrator credentials.

---

## Step 2

Click

```
New Item
```

Enter

```
parameter-demo
```

Select

```
Freestyle Project
```

Click

```
OK
```

---

# Exercise 2 – Understanding Build Parameters

Build Parameters allow users to provide input during build execution.

Instead of modifying scripts every time, Jenkins asks the user for values before starting the build.

---

## Enable Build Parameters

Scroll to

```
General
```

Enable

```
✓ This project is parameterized
```

You will now be able to add multiple parameter types.

---

# Exercise 3 – String Parameter

Click

```
Add Parameter
```

Choose

```
String Parameter
```

Configure

| Field         | Value           |
| ------------- | --------------- |
| Name          | USERNAME        |
| Default Value | Student         |
| Description   | Enter your name |

Save.

---

## Build the Job

Click

```
Build with Parameters
```

Enter

```
John
```

Run the build.

---

## Add Execute Shell

Navigate to

```
Build Steps

↓

Execute Shell
```

Add

```bash
echo "Hello $USERNAME"

echo "Welcome to Jenkins"
```

Run again.

Expected Output

```
Hello John

Welcome to Jenkins
```

---

# Exercise 4 – Choice Parameter

Add another parameter

```
Choice Parameter
```

Configuration

| Field | Value       |
| ----- | ----------- |
| Name  | ENVIRONMENT |

Choices

```
DEV
TEST
UAT
PROD
```

Description

```
Deployment Environment
```

---

Modify Execute Shell

```bash
echo "Deploying to $ENVIRONMENT"
```

Run the build.

Select

```
TEST
```

Expected

```
Deploying to TEST
```

---

# Exercise 5 – Boolean Parameter

Add

```
Boolean Parameter
```

Configuration

| Field   | Value     |
| ------- | --------- |
| Name    | RUN_TESTS |
| Default | Checked   |

---

Update Execute Shell

```bash
echo "Run Tests : $RUN_TESTS"

if [ "$RUN_TESTS" = "true" ]
then
    echo "Executing Test Cases..."
else
    echo "Skipping Tests"
fi
```

Execute twice.

Observe output.

---

# Exercise 6 – Password Parameter

Add

```
Password Parameter
```

Configuration

| Field | Value    |
| ----- | -------- |
| Name  | PASSWORD |

---

Execute Shell

```bash
echo "Password Length"

echo ${#PASSWORD}
```

Observe

Password value is hidden in Jenkins UI.

---

# Exercise 7 – Multi-Line Text Parameter

Add

```
Text Parameter
```

Name

```
COMMENTS
```

Execute Shell

```bash
echo "Comments"

echo "$COMMENTS"
```

Run with multiple lines.

Example

```
Deployment Started

Database Updated

Application Successful
```

---

# Exercise 8 – Build Environment

Build Environment prepares the workspace before the build begins.

Navigate to

```
Build Environment
```

Observe the available options.

---

# Exercise 9 – Delete Workspace Before Build

Enable

```
Delete workspace before build starts
```

Create a temporary file.

```bash
touch demo.txt

ls
```

Run build.

Execute again.

Notice

Workspace gets cleaned before every build.

---

# Exercise 10 – Add Timestamps

Enable

```
Add timestamps to the Console Output
```

Run the build.

Observe

```
07:10:15

07:10:16

07:10:17
```

Each console message now contains timestamps.

---

# Exercise 11 – Abort Previous Builds

Enable

```
Do not allow concurrent builds
```

Start one build.

Immediately trigger another.

Observe

Second build waits until first build completes.

---

# Exercise 12 – Execute Shell Script

Replace script

```bash
echo "User : $USERNAME"

echo "Environment : $ENVIRONMENT"

echo "Run Tests : $RUN_TESTS"

echo "Comments"

echo "$COMMENTS"

mkdir output

echo "Build Successful" > output/result.txt
```

Run the build.

