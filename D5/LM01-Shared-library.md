---

# Lab Manual 1

# Create Your First Jenkins Shared Library

---

# Business Scenario

## Background

ABC Retail Pvt Ltd has more than **40 Jenkins Pipelines** supporting different development teams.

During a recent audit, the DevOps team discovered that many Pipelines contain identical build logic, resulting in duplicated code, inconsistent implementations, and increased maintenance effort.

To standardize Pipeline development, the DevOps Architect has decided to introduce **Jenkins Shared Libraries**. The objective is to move commonly used Pipeline functions into a reusable library that can be shared across all Jenkins Pipelines.

As a DevOps Engineer, your task is to create the organization's first Shared Library, configure Jenkins to use it, and verify that a Pipeline can successfully execute a reusable function from the library.

---

# Learning Objectives

After completing this lab, students will be able to:

* Create a Jenkins Shared Library repository
* Understand the standard Shared Library directory structure
* Create reusable functions inside the **vars** directory
* Configure a Global Shared Library in Jenkins
* Import a Shared Library into a Jenkins Pipeline
* Execute reusable Pipeline functions

---

# Estimated Duration

**75 Minutes**

---

# Enterprise Architecture

```text
                GitHub

                     │

        jenkins-shared-library

                     │

                     ▼

          Jenkins Controller

                     │

      Global Shared Library

                     │

                     ▼

           Jenkins Pipeline

                     │

                     ▼

             buildApp()
```

---

# Prerequisites

Students should have completed

* Jenkins Installation
* Pipeline Fundamentals
* GitHub Integration
* Multibranch Pipelines

---

# Task 1

# Create the Shared Library Repository

The DevOps team has decided to keep reusable Pipeline code in a dedicated Git repository.

---

## Step 1

Login to GitHub.

---

## Step 2

Create a new repository.

Repository Name

```text
jenkins-shared-library
```

Repository Type

```text
Private
```

Initialize with

```text
README.md
```

---

## Validation

Verify the repository has been created successfully.

---

# Task 2

# Clone the Repository

Clone the repository to your local machine.

Reference Command

```bash
git clone https://github.com/<username>/jenkins-shared-library.git
```

Navigate into the repository.

Reference Command

```bash
cd jenkins-shared-library
```

---

## Validation

Verify the repository contents.

Expected Output

```text
README.md
```

---

# Task 3

# Create the Standard Shared Library Structure

Jenkins expects Shared Libraries to follow a predefined directory structure.

Create the following directories.

```text
jenkins-shared-library

│

├── vars

├── src

└── resources
```

> **Note:** For this lab, only the **vars** directory will be used.

---

## Validation

Verify the directory structure.

---

# Task 4

# Create Your First Shared Function

Inside the **vars** directory, create a new Groovy file.

Filename

```text
buildApp.groovy
```

---

The Shared Library function must expose a **call()** method so that Jenkins can invoke it like a Pipeline step.

Reference Code

```groovy
def call() {

    echo "Starting application build..."

    echo "Compiling source code..."

    echo "Build completed successfully."

}
```

---

## Discussion

Explain

* Why the filename becomes the Pipeline function name.
* Why the `call()` method is mandatory for simple reusable steps.
* How Jenkins converts `buildApp.groovy` into the `buildApp()` Pipeline step.

---

## Validation

Verify that

```text
vars/

└── buildApp.groovy
```

exists.

---

# Task 5

# Commit and Push the Library

Review repository status.

Reference Command

```bash
git status
```

Add the newly created files.

Reference Command

```bash
git add .
```

Commit the changes.

Reference Command

```bash
git commit -m "Initial Shared Library"
```

Push the repository.

Reference Command

```bash
git push origin main
```

---

## Validation

Verify that the **vars** directory and **buildApp.groovy** are visible in GitHub.

---

# Task 6

# Configure the Global Shared Library

Open Jenkins.

Navigate to

```text
Manage Jenkins

↓

System

↓

Global Trusted Pipeline Libraries
```

---

Click

```text
Add
```

Configure the following values.

| Field                  | Value                  |
| ---------------------- | ---------------------- |
| Name                   | shared-lib             |
| Default Version        | main                   |
| Retrieval Method       | Modern SCM             |
| Source Code Management | Git                    |
| Repository URL         | Your GitHub Repository |
| Credentials            | GitHub PAT             |
| Load Implicitly        | Disabled               |

Click

```text
Save
```

---

## Validation

Verify that the Shared Library appears in the Global Libraries list.

---

# Task 7

# Create a Pipeline Job

Create a new Pipeline Job.

Suggested Name

```text
shared-library-demo
```

Use

```text
Pipeline Script
```

instead of Pipeline Script from SCM.

---

## Validation

Verify the Pipeline Job is created successfully.

---

# Task 8

# Import the Shared Library

Modify the Pipeline by importing the Shared Library.

Reference Code

```groovy
@Library('shared-lib') _
```

Place the annotation at the beginning of the Jenkinsfile.

---

## Discussion

Explain

* Purpose of the `@Library` annotation.
* Difference between implicit and explicit library loading.

---

# Task 9

# Execute the Shared Function

Inside an existing Pipeline stage, replace the repeated build logic with a call to the Shared Library function.

Instead of multiple `echo` statements, invoke the reusable function.

Reference Function

```groovy
buildApp()
```

full pipeline code 
```bash
@Library('myLibrary') _

pipeline {

    agent any

    stages {

        stage('Build') {

            steps {

                buildApp()

            }

        }

    }

}
```

Run the Pipeline.

---

## Expected Console Output

```text
Starting application build...

Compiling source code...

Build completed successfully.
```

---

## Validation

Verify that the output originates from the Shared Library rather than directly from the Jenkinsfile.

---

# Task 10

# Verify Reusability

Create a second Pipeline Job.

Import the same Shared Library.

Reuse the **buildApp()** function without copying any code from the first Pipeline.

Run the Pipeline.

---

## Discussion

Explain how Shared Libraries eliminate code duplication across multiple Jenkins Pipelines.

---

# Challenge Activity 1

Create another reusable function named

```text
testApp()
```

The function should simulate application testing.

Import and execute both functions in the same Pipeline.

---

# Challenge Activity 2

Create a reusable function named

```text
deployApp()
```

Modify the Pipeline so that it performs:

```text
buildApp()

↓

testApp()

↓

deployApp()
```

using only Shared Library functions.

---

# Challenge Activity 3

Modify the **buildApp()** function to accept an application name as a parameter.

Example

```text
buildApp("Inventory Service")
```

Display the application name during execution.

---

# Challenge Activity 4

Create a second Git repository named

```text
enterprise-shared-library
```

Discuss how organizations manage multiple Shared Libraries for different teams or business units.

---

# Repository Structure (Final)

```text
jenkins-shared-library

│

├── vars

│     ├── buildApp.groovy

│     ├── testApp.groovy

│     └── deployApp.groovy

│

├── src

│

├── resources

│

└── README.md
```

---

# Validation Checklist

| Validation Item                         | Status |
| --------------------------------------- | :----: |
| Shared Library repository created       |    ☐   |
| Standard directory structure created    |    ☐   |
| `buildApp.groovy` added                 |    ☐   |
| Repository pushed to GitHub             |    ☐   |
| Global Shared Library configured        |    ☐   |
| Library imported successfully           |    ☐   |
| `buildApp()` executed                   |    ☐   |
| Second Pipeline reused the same library |    ☐   |

---

# Expected Learning Outcomes

After completing this lab, students will be able to:

* Design a reusable Jenkins Shared Library repository using the standard Jenkins directory layout.
* Create custom Pipeline steps using the `vars` directory and the `call()` method.
* Configure and manage a **Global Shared Library** in Jenkins.
* Import Shared Libraries into Pipelines using the `@Library` annotation.
* Replace duplicated Pipeline code with reusable functions.
* Apply Shared Libraries to multiple Jenkins Pipelines, following enterprise CI/CD best practices.
