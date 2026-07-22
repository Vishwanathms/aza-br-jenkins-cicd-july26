---

# Lab Manual 3

# Scenario: Develop a Production-Ready Multi-Stage Pipeline

## Scenario

You are responsible for automating the build process for an enterprise application.

The application should only be deployed from the **main** branch.

Unit Testing and Security Scanning should execute simultaneously to reduce build time.

---

## Learning Objectives

Students learn

* parallel
* when
* branch conditions
* environment
* complete Pipeline syntax
* stage organization

---

## Estimated Duration

**75 Minutes**

---

# Pipeline Flow

```text
Checkout

↓

Build

↓

Parallel

──────────────

Unit Test

Security Scan

──────────────

↓

Approval

↓

Deploy

↓

Completed
```

---

# Task 1 – Build Stage

Display

```
Building Application
```

---

# Task 2 – Parallel Execution

Create

```
Unit Test

Security Scan
```

Run simultaneously.

Observe Stage View.

---

# Task 3 – Simulate Testing

Execute

```
echo Testing...
sleep 10
```

---

# Task 4 – Simulate Security Scan

Execute

```
echo Running Trivy...
sleep 10
```

Observe

Both stages run together.

---

# Task 5 – Conditional Deployment

Deploy only

```
main
```

using

```groovy
when {

branch 'main'

}
```

Test

```
feature

↓

No Deployment
```

```
main

↓

Deployment
```

---

# Task 6 – Environment Variables

Create

```
APP

VERSION

ENVIRONMENT
```

Display values during deployment.

---

# Task 7 – Complete Enterprise Pipeline

Students combine

* echo
* sh
* variables
* environment
* retry
* timeout
* input
* parallel
* when
* script

into a single Pipeline.
