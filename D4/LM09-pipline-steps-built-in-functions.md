I like to design labs the same way they are performed in enterprise training—not as "copy and paste this Jenkinsfile", but as **a real business problem that evolves step by step**.

Below is a **complete scenario-based lab manual**.

---

# Lab 1

# Source Code & Workspace Management

## Scenario

### Background

ABC Retail Pvt Ltd has recently migrated its Java application from an internal Git server to GitHub.

The DevOps team has been asked to create the first Jenkins Pipeline that can automatically download the source code, inspect the Jenkins workspace, verify that all required project files are available, and generate a build information report for auditing purposes.

As a newly hired DevOps Engineer, your responsibility is to develop this Pipeline incrementally and validate each stage before moving to the next task.

---

# Lab Objectives

By the end of this lab students will be able to

* Clone source code from GitHub
* Understand Jenkins Workspace
* Navigate directories
* Verify project files
* Generate reports
* Read and write files inside Jenkins workspace

---

# Estimated Duration

**60 Minutes**

---

# Lab Architecture

```text
                   Developer

                        │

                        ▼

                 GitHub Repository

                        │

                        ▼

                Jenkins Controller

                        │

                        ▼

                Linux Jenkins Agent

                        │

                        ▼

              Jenkins Workspace

                        │

                        ▼

             Build Information Report
```

---

# Prerequisites

* Jenkins Installed
* Linux Agent Connected
* Git Installed
* Sample Java Repository in GitHub

Example Repository

```
https://github.com/jglick/simple-maven-project-with-tests.git
```

(Any Java repository can be used.)

---

# Expected Project Structure

After checkout

```
Workspace

│

├── pom.xml

├── src/

├── README.md

├── Jenkinsfile

└── target/
```

---

# Task 1

# Create Pipeline Job

Create a new Pipeline Job

```
lab4-source-management
```

Click

```
Build Now
```

No Jenkinsfile yet.

---

# Task 2

# Create the Basic Pipeline

Students begin with the smallest Pipeline.

```groovy
pipeline {

    agent any

    stages {

        stage('Welcome') {

            steps {

                echo "Starting Pipeline"

            }

        }

    }

}
```

---

## Expected Output

```
Starting Pipeline
```

---

# Learning Outcome

Students understand

* Pipeline syntax
* Stage execution
* Console output

---

# Task 3

# Clone the Git Repository

Now automate source code download.

```groovy
pipeline {

    agent any

    stages {

        stage('Checkout Source Code') {

            steps {

                git 'https://github.com/jglick/simple-maven-project-with-tests.git'

            }

        }

    }

}
```

---

## Observe

Console Output

```
Cloning repository...

Fetching origin...

Checking out Revision...
```

---

# Learning Outcome

Students learn

* git step
* automatic checkout
* workspace population

---

# Task 4

# Display Workspace Location

Students should now identify where Jenkins stores the project.

Update the Pipeline.

```groovy
stage('Workspace') {

    steps {

        script {

            echo pwd()

        }

    }

}
```

---

Expected Output

```
/var/lib/jenkins/workspace/lab4-source-management
```

Discussion

Explain

```
Workspace

↓

Temporary Build Directory

↓

Deleted after Cleanup
```

---

# Task 5

# Display Workspace Contents

Students now inspect downloaded files.

```groovy
stage('Workspace Files') {

    steps {

        sh "pwd"

        sh "ls -l"

    }

}
```

Expected Output

```
pom.xml

README.md

src

Jenkinsfile
```

---

Learning

Difference between

```
pwd()

and

sh "pwd"
```

---

# Task 6

# Navigate to a Directory

Students explore the source folder.

```groovy
stage('Navigate Directory') {

    steps {

        dir("src") {

            sh "pwd"

            sh "ls"

        }

    }

}
```

---

Observe

Workspace changes only inside the block.

After completion

Pipeline automatically returns to root workspace.

---

# Task 7

# Verify Project Files

The build cannot continue if **pom.xml** is missing.

Check its existence.

```groovy
stage('Verify Project') {

    steps {

        script {

            if(fileExists("pom.xml")){

                echo "Java Project Found"

            }

            else{

                error "pom.xml Missing"

            }

        }

    }

}
```

---

Experiment

Rename

```
pom.xml

↓

pom.xml.old
```

Observe Pipeline Failure.

---

Learning

Enterprise Pipelines always validate prerequisites.

---

# Task 8

# Generate Build Information Report

Create a report.

```groovy
stage('Generate Report') {

    steps {

        writeFile(

            file: "BuildReport.txt",

            text: """

Application : Demo

Job : ${env.JOB_NAME}

Build : ${env.BUILD_NUMBER}

Workspace : ${env.WORKSPACE}

"""

        )

    }

}
```

---

Students verify

```
BuildReport.txt
```

exists inside Workspace.

---

# Task 9

# Read Report File

Display report.

```groovy
stage('Read Report') {

    steps {

        script {

            def report = readFile("BuildReport.txt")

            echo report

        }

    }

}
```

---

Expected Console

```
Application : Demo

Job : lab4-source-management

Build : 4

Workspace :

/var/lib/jenkins/workspace/lab4-source-management
```

---

# Challenge Activity 1

Modify BuildReport

Include

```
Date

Node Name

Git Branch

Build URL
```

Hint

Use

```
env.NODE_NAME

env.BUILD_URL

env.GIT_BRANCH
```

---

# Challenge Activity 2

Before generating report

Verify

```
README.md

pom.xml

src/
```

If any file is missing

Pipeline should stop.

---

# Challenge Activity 3

Inside

```
src
```

Create

```
student.txt
```

using

```
writeFile()
```

Read it back

Display its contents.

---

# Final Jenkins Pipeline

Students should now have a Pipeline similar to:

```
Checkout

↓

Display Workspace

↓

Display Files

↓

Navigate Directory

↓

Verify Project

↓

Generate Report

↓

Read Report

↓

Pipeline Complete
```

---

# Expected Learning Outcomes

After completing this lab, students will be able to:

* Use the `git` step to clone source code from GitHub.
* Understand the purpose and location of the Jenkins workspace.
* Navigate directories using the `dir()` step.
* Display the current workspace with `pwd()`.
* Validate the existence of project files using `fileExists()`.
* Create files programmatically using `writeFile()`.
* Read file contents using `readFile()`.
* Build a structured, multi-stage Jenkins Pipeline that performs source code checkout, workspace inspection, validation, and reporting.

---

## Progressive Feature Mapping

| Task                 | Jenkins Feature Introduced        |
| -------------------- | --------------------------------- |
| Task 1               | Basic Pipeline Structure          |
| Task 2               | `echo`                            |
| Task 3               | `git`                             |
| Task 4               | `pwd()`                           |
| Task 5               | Shell commands (`sh`)             |
| Task 6               | `dir()`                           |
| Task 7               | `fileExists()`                    |
| Task 8               | `writeFile()`                     |
| Task 9               | `readFile()`                      |
| Challenge Activities | Combining multiple Pipeline steps |

This format mirrors a real-world enterprise workflow: students build the pipeline incrementally, verify each stage before proceeding, and finish with a complete, working solution rather than pasting a single large Jenkinsfile.
