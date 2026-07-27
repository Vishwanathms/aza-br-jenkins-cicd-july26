I actually recommend making **Lab 2 continue from Lab 1**, just like a real DevOps project. Students should **reuse the same Pipeline Job** and progressively enhance it instead of creating a new one from scratch.

---

# Lab 2

# Build Artifacts & Workspace Maintenance

## Scenario

### Background

The DevOps team has successfully automated the source code checkout and project validation process.

Management now wants every successful build to generate a **Build Artifact** that developers can download later for testing and release purposes.

During testing, the team notices that old files remain in the Jenkins workspace, causing inconsistent build results.

As the DevOps Engineer, your next task is to enhance the existing Pipeline to:

* Generate build artifacts
* Archive them in Jenkins
* Monitor build information
* Understand build status
* Clean the workspace after every build

The objective is to ensure that every Pipeline execution starts with a clean environment while preserving important build outputs.

---

# Learning Objectives

By completing this lab, students will learn to:

* Generate build artifacts
* Archive artifacts using Jenkins
* View archived artifacts
* Understand `currentBuild`
* Monitor build status
* Clean the Jenkins workspace
* Compare `deleteDir()` and `cleanWs()`

---

# Estimated Duration

**60 Minutes**

---

# Lab Topology

```text
Developer

        │

        ▼

GitHub Repository

        │

        ▼

Jenkins Pipeline

        │

        ▼

Build Workspace

        │

        ├────────► Archive Artifacts

        │

        ▼

Workspace Cleanup
```

---

# Prerequisites

Complete **Lab 1**

The Pipeline should already perform:

* Git Checkout
* Workspace Inspection
* File Validation
* Build Report Generation

---

# Existing Pipeline

Students should **reuse the Pipeline created in Lab 1**.

Additional stages will be added as the lab progresses.

---

# Task 1

# Review Existing Workspace

Run the Pipeline created in Lab 1.

Observe

* Workspace
* Generated files
* Build Report

Discussion

Notice that files from previous builds still remain inside the workspace.

Explain why leftover files can create unreliable builds.

---

# Task 2

# Generate a Build Artifact

A build process normally produces files such as:

* JAR
* WAR
* ZIP
* Reports
* Configuration Packages

Since this lab focuses on Pipeline concepts rather than Maven, simulate an artifact.

Students should **add a new stage after the Build Report stage**.

Inside this stage, create a file named:

```text
application.jar
```

or

```text
artifact.zip
```

using the Pipeline file creation step.

Run the Pipeline.

Verify that the file exists inside the workspace.

---

# Learning Outcome

Students understand what a build artifact represents.

---

# Task 3

# Archive the Artifact

The DevOps Manager wants every successful build to preserve its output.

Students should **extend the Artifact stage created in Task 2**.

After generating the artifact, archive it using the Jenkins Pipeline archive step.

Run the Pipeline.

---

## Verification

Navigate to

```
Build History

↓

Latest Build

↓

Artifacts
```

Verify that the generated file can be downloaded.

Discussion

Explain why archived artifacts remain available even if the workspace is deleted.

---

# Task 4

# Generate Multiple Artifacts

Enhance the existing Artifact stage.

Instead of one file, create multiple artifacts such as:

```
application.jar

BuildReport.txt

release-notes.txt
```

Archive all of them together.

Run the Pipeline again.

Observe the Artifacts section.

---

# Learning Outcome

Students understand that multiple files can be archived in a single build.

---

# Task 5

# Understanding currentBuild

The Operations team wants every build log to include important build information.

Students should **add a new stage named "Build Information" after the Artifact stage**.

Inside this stage, display the following values:

* Build Number
* Build Result
* Build Duration
* Job Name

Run the Pipeline multiple times.

Observe how the values change for every build.

Discussion

Explain how `currentBuild` differs from environment variables.

---

# Task 6

# Understanding Build Status

Jenkins reports several build states.

Students should intentionally create different Pipeline outcomes.

### Exercise 1

Execute the Pipeline successfully.

Observe

```
SUCCESS
```

---

### Exercise 2

Introduce an invalid shell command into one stage.

Run the Pipeline.

Observe

```
FAILURE
```

After verification, remove the invalid command.

---

### Exercise 3

Instructor Demonstration

Discuss other possible build states.

```
UNSTABLE

ABORTED

NOT_BUILT
```

Explain when each status occurs in enterprise CI/CD environments.

---

# Task 7

# Cleaning the Workspace using deleteDir()

The DevOps team has identified that files from previous builds remain in the workspace.

Students should **add a new Cleanup stage at the end of the Pipeline**.

Inside this stage, delete the workspace using the appropriate Pipeline step.

Run the Pipeline.

Verify

```
Workspace

↓

Empty
```

Discussion

What happens to archived artifacts after deleting the workspace?

---

# Task 8

# Cleaning the Workspace using cleanWs()

Many enterprise environments prefer the Workspace Cleanup Plugin.

Replace the cleanup logic used in Task 7 with the Workspace Cleanup step.

Run the Pipeline again.

Observe the Console Output.

Discuss

Why do many organizations prefer `cleanWs()` over `deleteDir()`?

---

# Task 9

# Compare deleteDir() vs cleanWs()

Complete the following comparison table.

| Feature                    | deleteDir() | cleanWs() |
| -------------------------- | ----------- | --------- |
| Built into Pipeline        |             |           |
| Requires Plugin            |             |           |
| Deletes Entire Workspace   |             |           |
| Advanced Cleanup Options   |             |           |
| Recommended for Enterprise |             |           |

Discuss the advantages and limitations of each approach.

---

# Challenge Activity 1

Modify the Pipeline so that:

* Build artifacts are archived only when the build succeeds.
* Workspace cleanup always runs, regardless of success or failure.

Hint:

Students should explore the appropriate Pipeline section for post-build actions.

---

# Challenge Activity 2

Generate the following artifacts automatically:

```
application.jar

BuildReport.txt

DeploymentNotes.txt

Version.txt
```

Archive all files using a single archive step.

---

# Challenge Activity 3

Display the following information before cleanup:

* Current Build Number
* Build Result
* Workspace Path
* Number of Generated Artifacts

Discuss how this information can assist with troubleshooting.

---

# Expected Pipeline Flow

```text
Checkout Source

        │

        ▼

Workspace Validation

        │

        ▼

Generate Build Report

        │

        ▼

Generate Artifact

        │

        ▼

Archive Artifact

        │

        ▼

Display Build Information

        │

        ▼

Cleanup Workspace

        │

        ▼

Pipeline Completed
```

---

# Validation Checklist

| Validation Item                      | Status |
| ------------------------------------ | :----: |
| Source code checked out successfully |    ☐   |
| Build artifact generated             |    ☐   |
| Artifact archived in Jenkins         |    ☐   |
| Multiple artifacts archived          |    ☐   |
| Build information displayed          |    ☐   |
| Build status observed                |    ☐   |
| Workspace cleaned successfully       |    ☐   |
| Pipeline completes without errors    |    ☐   |

---

# Learning Outcomes

After completing this lab, students will be able to:

* Preserve build outputs using Jenkins artifacts.
* Distinguish between workspace files and archived artifacts.
* Retrieve build information using `currentBuild`.
* Interpret Jenkins build statuses such as **SUCCESS**, **FAILURE**, **UNSTABLE**, and **ABORTED**.
* Clean the Jenkins workspace using both `deleteDir()` and `cleanWs()`.
* Implement workspace maintenance practices commonly used in enterprise CI/CD pipelines.

This style keeps the lab **progressive**, avoids repeating complete Jenkinsfiles in every task, and encourages students to enhance the pipeline they built in Lab 1—mirroring how CI/CD pipelines evolve in real projects.
