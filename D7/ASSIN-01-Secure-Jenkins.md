For a training program, these should be **three independent labs**, each with its own objectives and deliverables. The sequence should be:

1. **Lab 1 – Secure Jenkins**
2. **Lab 2 – Backup Jenkins**
3. **Lab 3 – Restore Jenkins (Disaster Recovery)**

This progression mimics a real enterprise environment: first harden the server, then protect it with backups, and finally recover from a failure.

---

# Lab 1 – Secure Jenkins

## Lab Title

**Lab 1 – Securing an Enterprise Jenkins Server**

**Duration:** 90 Minutes

**Difficulty:** Intermediate

---

# Lab Objectives

By the end of this lab, students will be able to:

* Configure Jenkins security
* Create administrator and developer users
* Disable anonymous access
* Configure Matrix-based authorization
* Configure Role-Based Authorization
* Enable CSRF Protection
* Configure Agent Security
* Disable CLI Remoting
* Review Security Warnings

---

# Lab Topology

```text
Student Laptop

        │

Browser

        │

Ubuntu VM

        │

Jenkins Controller
```

---

# Prerequisites

* Ubuntu VM
* Jenkins installed
* Java installed
* Browser access
* Administrator password

---

# Task 1 – Verify Jenkins Installation

Open browser

```
http://<jenkins-ip>:8080
```

Login as

```
admin
```

Verify Dashboard opens.

---

# Task 2 – Review Current Security

Navigate

```
Manage Jenkins

↓

Security
```

Observe

* Authentication
* Authorization
* CSRF
* Agent Protocols

Take a screenshot.

---

# Task 3 – Enable Authentication

Navigate

```
Manage Jenkins

↓

Security
```

Authentication

Select

```
Jenkins own user database
```

Enable

```
Allow users to sign up

OFF
```

Click

```
Save
```

---

# Task 4 – Create New Users

Navigate

```
Manage Jenkins

↓

Users

↓

Create User
```

Create

```
developer1
```

Password

```
Dev@12345
```

Repeat

```
developer2

qauser

auditor
```

Verify all users exist.

---

# Task 5 – Disable Anonymous Access

Navigate

```
Manage Jenkins

↓

Security
```

Authorization

Select

```
Matrix Authorization
```

Remove

```
Anonymous Read
```

Save.

Open another browser

```
Incognito Mode
```

Verify Jenkins asks for login.

---

# Task 6 – Configure Matrix Security

Grant permissions

Admin

```
Overall/Administer
```

Developer

```
Read

Job Build

Job Read

Workspace
```

QA

```
Read

Job Read
```

Auditor

```
Overall Read

View Read
```

Test each account.

---

# Task 7 – Install Role Strategy Plugin

Navigate

```
Manage Jenkins

↓

Plugins

↓

Available
```

Search

```
Role Strategy
```

Install

Restart Jenkins.

---

# Task 8 – Configure RBAC

Navigate

```
Manage Jenkins

↓

Security
```

Select

```
Role Based Strategy
```

Save.

---

# Task 9 – Create Roles

Navigate

```
Manage Jenkins

↓

Manage and Assign Roles
```

Create

```
Admin

Developer

QA

Auditor
```

Assign permissions.

Assign users.

Verify login.

---

# Task 10 – Enable CSRF Protection

Navigate

```
Manage Jenkins

↓

Security
```

Enable

```
Prevent Cross Site Request Forgery
```

Select

```
Default Crumb Issuer
```

Save.

---

# Task 11 – Configure Agent Security

Navigate

```
Manage Jenkins

↓

Security
```

Disable old protocols

Allow only

```
JNLP4

WebSocket
```

Disable

```
JNLP1

JNLP2

JNLP3
```

Save.

---

# Task 12 – Disable CLI Over Remoting

Navigate

```
Manage Jenkins

↓

Security
```

Disable

```
CLI over Remoting
```

Save.

---

# Task 13 – Review Security Warnings

Navigate

```
Manage Jenkins

↓

Administrative Monitors
```

Review

* Old Plugins
* Insecure Protocols
* Security Alerts

---

# Validation

Student should demonstrate

✅ Login with Admin

✅ Login with Developer

✅ Anonymous denied

✅ RBAC working

✅ CSRF enabled

✅ Agent protocols secured

---

# Deliverables

Capture screenshots of

* Matrix Security
* RBAC
* Users
* CSRF
* Security Configuration

---

# Challenge

Create

```
Intern
```

Role

with only

```
View Jobs
```

No Build permission.

---

