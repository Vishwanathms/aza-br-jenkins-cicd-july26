---

# Lab Manual 1

# Scenario: Build Your First Jenkins Pipeline

## Scenario

You have recently joined the DevOps team at **ABC Retail Pvt Ltd**.

The development team has created a simple application repository and wants the DevOps team to automate the build process.

Your task is to create your first Jenkins Pipeline that prints useful information, executes Linux commands, and demonstrates the use of variables.

---

## Learning Objectives

By completing this lab, students will learn:

* Pipeline syntax
* echo
* sh
* Groovy variables
* Environment variables
* String interpolation
* script block
* Console Output

---

## Estimated Duration

**45 Minutes**

---

# Lab Topology

```text
Developer

↓

Git Repository

↓

Jenkins Controller

↓

Linux Agent

↓

Console Output
```

---

# Prerequisites

* Jenkins Controller
* Ubuntu Jenkins Agent
* Git Installed
* Pipeline Plugin Installed

---

# Task 1 – Create a New Pipeline Job

Create a Pipeline Job

Name

```
pipeline-syntax-lab1
```

---

# Task 2 – Create a Jenkinsfile

Students create

```groovy
pipeline {

    agent any

    stages {

        stage('Welcome') {

            steps {

                echo "Welcome to Jenkins Pipeline"

            }

        }

    }

}
```

Run Build

Observe Console Output.

---

# Task 3 – Execute Linux Commands

Add

```groovy
sh "pwd"

sh "hostname"

sh "whoami"

sh "date"
```

Questions

* What is the current workspace?
* Which user executed the build?
* Which machine executed the Pipeline?

---

# Task 4 – Create Variables

Inside script block

```groovy
script {

    def app="Shopping"

    def version="1.0"

    echo app

    echo version

}
```

Students modify

```
Shopping

↓

Inventory

↓

Payroll
```

Observe output.

---

# Task 5 – Environment Variables

Create

```groovy
environment {

APP="Inventory"

ENV="Development"

}
```

Print values.

---

# Task 6 – Built-in Jenkins Variables

Display

```
BUILD_NUMBER

JOB_NAME

NODE_NAME

WORKSPACE

BUILD_ID
```

---

# Task 7 – String Interpolation

Generate

```
Building Inventory Version 1.0
```

using

```
${}
```

---

# Challenge Activity

Display

```
Application : Inventory

Environment : Development

Build Number : xx

Workspace : xxxxxx
```

---

# Validation Checklist

✔ Pipeline executes

✔ Echo displayed

✔ Linux commands executed

✔ Variables created

✔ Environment variables printed

✔ String interpolation working

---

# Expected Outcome

Students understand the basic syntax required to write Jenkins Pipelines.

