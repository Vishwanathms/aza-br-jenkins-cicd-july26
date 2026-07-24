Excellent. This is exactly how I would conclude **Module 4**. This lab feels like an **enterprise scenario**, where multiple Jenkins agents (Build Agent, Test Agent, Deploy Agent) work together. It also introduces students to concepts they'll use later in Docker, Kubernetes, and production CI/CD.

---

# Lab 3

# Multi-Agent Artifact Sharing

## Scenario

### Background

ABC Retail Pvt Ltd has expanded its Jenkins infrastructure to improve scalability and performance.

Instead of running every task on a single Jenkins agent, the organization now follows an enterprise build strategy:

* A **Build Agent** compiles the application.
* A **Testing Agent** performs quality checks.
* A **Deployment Agent** deploys the application.

Since each Jenkins agent has its own independent workspace, the application built on one agent is **not automatically available** on another.

As the DevOps Engineer, your responsibility is to modify the existing Pipeline so that build artifacts can be securely transferred between agents without rebuilding the application.

Additionally, the DevOps Manager wants to improve Pipeline readability by adding timestamps and colored console output while ensuring that only one build executes at a time.

---

# Learning Objectives

By completing this lab, students will learn to:

* Build Multi-Agent Pipelines
* Transfer files using `stash()`
* Retrieve files using `unstash()`
* Configure agent-specific stages
* Enable timestamps
* Enable ANSI colored console output
* Prevent concurrent Pipeline execution
* Build a production-style enterprise Pipeline

---

# Estimated Duration

**75 Minutes**

---

# Enterprise Architecture

```text id="g7jlfi"
              GitHub Repository

                     │

                     ▼

             Jenkins Controller

                     │

     ┌───────────────┼────────────────┐

     ▼               ▼                ▼

Build Agent      Test Agent      Deploy Agent

     │               │                │

     └────── Artifact Transfer ───────┘
```

---

# Prerequisites

Complete

* Lab 1
* Lab 2

The Pipeline should already perform:

* Source Code Checkout
* Workspace Validation
* Artifact Generation
* Artifact Archiving
* Workspace Cleanup

---

# Existing Pipeline

Continue using the Pipeline created during the previous labs.

This lab focuses on extending that Pipeline to support multiple Jenkins agents.

---

# Task 1

# Understand Agent Isolation

Discuss the following question before making any changes.

**Question**

Can the Deploy Agent automatically access files created on the Build Agent?

Expected Discussion

```text id="avh8zm"
Build Agent

Workspace A

≠

Deploy Agent

Workspace B
```

Each Jenkins agent has its own workspace.

Files created on one agent are **not automatically copied** to another.

---

# Learning Outcome

Students understand why artifact transfer is required.

---

# Task 2

# Convert to a Multi-Agent Pipeline

Modify the existing Pipeline.

Instead of using a single global agent, assign different agents to individual stages.

Suggested Stage Allocation

| Stage      | Agent        |
| ---------- | ------------ |
| Checkout   | Build Agent  |
| Build      | Build Agent  |
| Testing    | Test Agent   |
| Deployment | Deploy Agent |

Run the Pipeline.

Observe which Jenkins node executes each stage.

---

# Discussion

Explain how stage-level agents allow workloads to be distributed across multiple systems.

---

# Task 3

# Transfer Build Artifact using stash()

After the Build stage completes, the generated artifact must be preserved before switching agents.

Students should enhance the **Build Stage** by adding a step that stores the generated build artifact in temporary Jenkins storage.

Discussion

Explain that:

* `stash()` stores files temporarily.
* Files remain available only during the current Pipeline execution.
* It is designed specifically for transferring files between Jenkins agents.

Run the Pipeline.

Verify that the Build stage completes successfully.

---

# Learning Outcome

Students understand temporary artifact storage.

---

# Task 4

# Retrieve Artifact using unstash()

Inside the **Testing Stage**, retrieve the artifact stored during the previous task.

Verify that:

* The artifact becomes available in the Testing Agent workspace.
* No rebuild occurs.

Run the Pipeline.

Compare the workspaces of both agents.

---

# Discussion

Explain the difference between:

* Workspace
* Archived Artifact
* Stashed Files

---

# Task 5

# Verify Artifact Transfer

Inside the Testing stage, display the workspace contents.

Verify that the transferred artifact is available.

Questions

* Was the application rebuilt?
* Was the file copied?
* Which Jenkins feature performed the transfer?

---

# Task 6

# Enable Console Timestamps

The Operations team wants every log entry to include timestamps for troubleshooting.

Modify the Pipeline configuration to enable timestamps.

Run the Pipeline.

Observe the Console Output.

Example

```text id="z4h9m8"
10:15:21  Starting Build

10:15:23  Running Tests

10:15:30  Deployment Started
```

Discussion

Explain why timestamps are important when troubleshooting long-running Pipelines.

---

# Task 7

# Enable ANSI Color Output

The support team wants important messages to stand out in the Jenkins Console.

Enable ANSI Color support in the Pipeline.

Display colored messages for:

* Build Started
* Testing Started
* Deployment Started
* Pipeline Completed

Run the Pipeline.

Observe the console output.

---

# Discussion

Explain how colored output improves readability during production troubleshooting.

---

# Task 8

# Prevent Concurrent Builds

The organization has experienced failures caused by multiple users starting the same Pipeline simultaneously.

Update the Pipeline configuration to prevent concurrent execution.

Run one Pipeline.

Before it completes, start another build.

Observe Jenkins behavior.

Expected Result

```text id="3z4mkr"
Build #2

↓

Waiting

↓

Build #1 Completes

↓

Build #2 Starts
```

---

# Discussion

Explain why enterprise Pipelines often allow only one execution at a time.

Examples

* Production Deployments
* Database Migrations
* Infrastructure Changes

---

# Task 9

# Build a Complete Enterprise Pipeline

Review the existing Pipeline created across all three labs.

Ensure it now contains:

* Source Code Checkout
* Workspace Validation
* Build Report
* Artifact Generation
* Artifact Archiving
* Multi-Agent Execution
* Artifact Transfer
* Build Information
* Workspace Cleanup

Students should verify that every stage executes successfully.

---

# Challenge Activity 1

Extend the Pipeline by introducing a dedicated **Documentation Agent**.

The Documentation stage should execute on a separate Jenkins node after Testing completes.

Discussion

Would additional artifacts need to be transferred?

---

# Challenge Activity 2

Create three different Jenkins agents:

```text id="xk6d7w"
linux-build

linux-test

linux-deploy
```

Assign the appropriate stages to each agent.

Observe the Jenkins Stage View.

---

# Challenge Activity 3

Enhance the console output so that every stage displays:

* Current Agent Name
* Build Number
* Job Name
* Current Timestamp

Discuss how this information helps support engineers diagnose production issues.

---

# Challenge Activity 4

Simulate a deployment failure on the Deploy Agent.

Observe:

* Build Status
* Console Output
* Archived Artifacts
* Stashed Files

Discuss how the Pipeline could be enhanced to recover automatically.

---

# Enterprise Pipeline Flow

```text id="mnck0n"
Checkout Source (Build Agent)

          │

          ▼

Build Application

          │

          ▼

Stash Build Artifact

          │

          ▼

Testing (Test Agent)

          │

          ▼

Unstash Artifact

          │

          ▼

Deployment (Deploy Agent)

          │

          ▼

Archive Build Information

          │

          ▼

Cleanup Workspace

          │

          ▼

Pipeline Completed
```

---

# Validation Checklist

| Validation Item                           | Status |
| ----------------------------------------- | :----: |
| Multi-agent Pipeline configured           |    ☐   |
| Stage-level agents used                   |    ☐   |
| Artifact successfully stashed             |    ☐   |
| Artifact successfully unstashed           |    ☐   |
| No rebuild performed on second agent      |    ☐   |
| Timestamps enabled                        |    ☐   |
| ANSI color output enabled                 |    ☐   |
| Concurrent builds prevented               |    ☐   |
| Enterprise Pipeline executed successfully |    ☐   |

---

# Learning Outcomes

After completing this lab, students will be able to:

* Design and implement **multi-agent Jenkins Pipelines**.
* Transfer build artifacts efficiently between isolated Jenkins workspaces using **`stash()`** and **`unstash()`**.
* Configure **stage-specific agents** to distribute workloads across build, test, and deployment nodes.
* Improve pipeline observability with **timestamps** and **ANSI-colored console output**.
* Prevent conflicting executions using **`disableConcurrentBuilds()`**.
* Build and understand a complete **enterprise-grade Jenkins Pipeline** that mirrors real-world CI/CD environments.
