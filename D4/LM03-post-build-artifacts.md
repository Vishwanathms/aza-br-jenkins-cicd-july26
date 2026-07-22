---

# Exercise 13 – Post Build Actions

Post Build Actions execute after build completion.

Common examples

* Archive Artifacts
* Publish Test Reports
* Email Notification
* Trigger another Job
* Fingerprint Files
* Build Name Setter

---

# Exercise 14 – Archive Build Artifacts

Click

```
Post Build Actions

↓

Archive the Artifacts
```

Enter

```
output/*
```

Save.

Run the build.

---

Verify

Navigate to

```
Build History

↓

Latest Build

↓

Artifacts
```

You should see

```
result.txt
```

---

# Exercise 15 – Fingerprint Files

Add

```
Record fingerprints of files
```

Files

```
output/*
```

Run the build.

Observe

Jenkins tracks artifacts across builds.

---

# Exercise 16 – Build Name Setter

Install Plugin

```
Build Name Setter
```

Configure

```
Build Name

↓

Build-${BUILD_NUMBER}-${USERNAME}
```

Run

Observe

```
Build-15-John
```

instead of

```
#15
```

---

# Exercise 17 – Trigger Another Job

Create another job

```
child-job
```

Add Execute Shell

```bash
echo "This is Child Job"
```

Return to

```
parameter-demo
```

Post Build Actions

↓

```
Build Other Projects
```

Project

```
child-job
```

Run

Observe

Parent Job

↓

Child Job

---

# Exercise 18 – Email Notification (Optional)

Install Plugin

```
Email Extension Plugin
```

Configure SMTP

Manage Jenkins

↓

System

↓

Extended Email Notification

Add

```
SMTP Server

Username

Password
```

Post Build Action

↓

```
Editable Email Notification
```

Run build

Verify email delivery.

---

# Exercise 19 – Console Output Review

Open

```
Build History

↓

Latest Build

↓

Console Output
```

Verify

```
Hello John

Deploying to TEST

Run Tests : true

Executing Test Cases

Build Successful
```

---

# Exercise 20 – Challenge Activity

Create a job with the following requirements:

### Parameters

* Application Name (String)
* Version (String)
* Deployment Environment (Choice)
* Run Unit Tests (Boolean)
* Release Notes (Text)

---

### Build Environment

* Delete Workspace Before Build
* Add Timestamps
* Disable Concurrent Builds

---

### Build Steps

Print all parameter values.

Create directory

```
build-output
```

Create file

```
build-output/deployment.txt
```

Store

* Application Name
* Version
* Environment
* Build Number
* Build Date

---

### Post Build Actions

* Archive Artifacts
* Fingerprint Files
* Build Name Setter
* Trigger another Job

---
