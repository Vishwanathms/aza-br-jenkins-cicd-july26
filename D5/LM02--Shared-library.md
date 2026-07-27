
---

# Lab Manual 2

# Build a Reusable Enterprise CI/CD Shared Library

---

# Business Scenario

## Background

ABC Retail Pvt Ltd now has more than **60 development teams** building Java, Python, Node.js, and .NET applications.

During a DevOps maturity assessment, it was discovered that each team has implemented its own Jenkins Pipeline, resulting in:

* Different build processes
* Inconsistent testing
* Multiple deployment methods
* Difficult maintenance
* Duplicate Pipeline code

To solve this problem, the DevOps Center of Excellence (CoE) has decided to standardize the CI/CD process using **Jenkins Shared Libraries**.

Every application team must now use the same reusable functions for:

* Build
* Test
* Deployment

Your task is to extend the Shared Library created in Lab 1 and build a reusable enterprise CI/CD framework that can be consumed by multiple Jenkins Pipelines.

---

# Learning Objectives

After completing this lab, students will be able to:

* Create multiple reusable Shared Library functions
* Organize CI/CD logic using the `vars` directory
* Execute multiple Shared Library functions from a Pipeline
* Understand Shared Library versioning
* Standardize CI/CD processes across multiple projects

---

# Estimated Duration

**90 Minutes**

---

# Enterprise Architecture

```text
                     GitHub

        jenkins-shared-library

                  │

                  ▼

        Global Shared Library

                  │

    ┌─────────────┴──────────────┐

    ▼                            ▼

Pipeline A                  Pipeline B

Inventory Service          Payment Service

    │                            │

    └─────────────┬──────────────┘

                  ▼

      buildApp()

      testApp()

      deployApp()
```

---

# Prerequisites

Students must complete

✅ Lab Manual 1 – Create Your First Jenkins Shared Library

Verify

* Shared Library configured
* buildApp() working
* Pipeline successfully imports the library

---

# Existing Repository

Continue using

```text
jenkins-shared-library
```

created during Lab 1.

No new repository should be created.

---

# Task 1

# Review Existing Shared Library

Open the repository created during Lab 1.

Verify the existing structure.

Expected Structure

```text
jenkins-shared-library

│

├── vars

│      buildApp.groovy

│

├── src

└── resources
```

---

## Validation

Confirm that `buildApp.groovy` executes successfully from the previous lab.

---

# Task 2

# Design the Enterprise CI/CD Library

The organization has standardized its Pipeline into three logical stages.

Students should identify the reusable functions required.

Required Functions

```text
buildApp()

testApp()

deployApp()
```

Discuss

Why should each Pipeline stage become an independent reusable function?

---

# Task 3

# Create testApp()

Inside the **vars** directory, create a new Shared Library function.

Filename

```text
testApp.groovy
```

The function should simulate the testing phase of an application.

Suggested activities include:

* Display testing messages
* Simulate execution of unit tests
* Print test completion status

Use the same structure introduced in Lab 1 by exposing a `call()` method.

---

## Validation

Verify that

```text
vars/

testApp.groovy
```

exists in the repository.

---

# Task 4

# Create deployApp()

Create another reusable function.

Filename

```text
deployApp.groovy
```

The function should simulate application deployment.

Suggested activities include:

* Display deployment start
* Simulate deployment
* Display deployment completion

Follow the same Shared Library conventions used previously.

---

## Validation

Verify that all three reusable functions now exist.

Expected Structure

```text
vars

│

├── buildApp.groovy

├── testApp.groovy

└── deployApp.groovy
```

---

# Task 5

# Update the Shared Library Repository

Review repository changes.

Commit all newly created Shared Library functions.

Push the updated repository to GitHub.

---

## Validation

Verify the latest commit is visible in GitHub.

Confirm that Jenkins can access the updated repository.

---

# Task 6

# Update an Existing Pipeline

Open the Pipeline created during Lab 1.

Instead of performing the build directly inside the Jenkinsfile, modify the Pipeline to execute the three reusable Shared Library functions.

The Pipeline flow should become:

```text
Build

↓

Test

↓

Deploy
```

using only Shared Library functions.

---

## Validation

Execute the Pipeline.

Observe the Console Output.

Verify that all three functions execute successfully.

---

# Task 7

# Create a Second Pipeline

Create a second Pipeline Job.

Suggested Name

```text
payment-service-pipeline
```

Import the same Shared Library.

Reuse the existing functions without copying any Pipeline code from the first job.

---

## Validation

Execute the Pipeline.

Verify that both Pipeline Jobs use the same Shared Library.

---

# Task 8

# Verify Library Reuse

Compare both Pipeline Jobs.

Questions

* Did either Pipeline duplicate any CI/CD logic?
* Where is the actual build logic stored?
* Which repository contains the reusable functions?

Discuss why Shared Libraries simplify enterprise Pipeline management.

---

# Task 9

# Modify buildApp()

The DevOps Architect wants to improve the build process.

Update

```text
buildApp.groovy
```

by adding additional logging or build information.

Examples

* Display application version
* Display timestamp
* Display Jenkins Job Name
* Display Build Number

Commit the changes.

Push the updated Shared Library.

---

## Validation

Without modifying either Jenkins Pipeline,

run both Pipeline Jobs again.

Observe the updated output.

---

# Discussion

Explain

Why did both Pipelines immediately use the updated build logic?

Discuss how this centralizes maintenance in enterprise environments.

---

# Task 10

# Understanding Library Versioning

The DevOps team plans to introduce multiple versions of the Shared Library.

Discuss:

* Why versioning is important
* Benefits of using Git branches or tags
* Risks of updating the `main` branch directly

Instructor Demonstration (Optional)

Create a new Git tag or branch for the Shared Library and explain how Jenkins can reference specific versions.

---

# Challenge Activity 1

Create an additional Shared Library function named

```text
securityScan()
```

Integrate it into both Pipeline Jobs after the Test stage.

---

# Challenge Activity 2

Create a new application Pipeline named

```text
inventory-service
```

Reuse the existing Shared Library without writing any new CI/CD logic.

Verify that all stages execute successfully.

---

# Challenge Activity 3

Modify

```text
deployApp()
```

to display the deployment environment.

Examples

```text
Development

Testing

Production
```

Discuss how the function could later be enhanced to accept parameters.

---

# Challenge Activity 4

Research and document the following Shared Library concepts:

* Global Shared Library
* Folder-Level Shared Library
* Implicit Library Loading
* Explicit Library Loading
* Trusted vs Untrusted Libraries

Prepare a short summary for classroom discussion.

---

# Final Repository Structure

```text
jenkins-shared-library

│

├── vars

│     ├── buildApp.groovy

│     ├── testApp.groovy

│     ├── deployApp.groovy

│     └── securityScan.groovy (Challenge)

│

├── src

│

├── resources

│

└── README.md
```

---

# Enterprise CI/CD Flow

```text
Pipeline A

        │

        ▼

buildApp()

        │

        ▼

testApp()

        │

        ▼

deployApp()

────────────────────────────

Pipeline B

        │

        ▼

buildApp()

        │

        ▼

testApp()

        │

        ▼

deployApp()
```

---

# Validation Checklist

| Validation Item                      | Status |
| ------------------------------------ | :----: |
| Existing Shared Library verified     |    ☐   |
| `testApp()` created                  |    ☐   |
| `deployApp()` created                |    ☐   |
| Repository updated in GitHub         |    ☐   |
| Pipeline A uses Shared Library       |    ☐   |
| Pipeline B uses Shared Library       |    ☐   |
| No duplicate Pipeline code           |    ☐   |
| `buildApp()` updated successfully    |    ☐   |
| Both Pipelines reflect updated logic |    ☐   |
| Shared Library versioning discussed  |    ☐   |

---

# Expected Learning Outcomes

After completing this lab, students will be able to:

* Extend a Jenkins Shared Library with multiple reusable CI/CD functions.
* Organize reusable Pipeline logic using the **`vars`** directory.
* Build standardized **build**, **test**, and **deployment** workflows shared across multiple Jenkins Pipelines.
* Update Shared Library functions centrally and observe the changes propagate automatically to all consuming Pipelines.
* Understand the role of Shared Library versioning in enterprise CI/CD environments.
* Apply DevOps Platform Engineering principles by promoting consistency, reuse, and maintainability across application teams.
