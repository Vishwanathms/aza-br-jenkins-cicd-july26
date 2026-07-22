---

# Lab Manual 2

# Scenario: Build a Reliable Enterprise Pipeline

## Scenario

Your company's build server occasionally experiences network failures while downloading dependencies.

Additionally, production deployments require manager approval.

You need to make the Pipeline fault tolerant and interactive.

---

## Learning Objectives

Students learn

* input
* timeout
* retry
* sleep
* error handling
* script
* currentBuild

---

## Estimated Duration

**60 Minutes**

---

# Architecture

```text
Developer

↓

Jenkins

↓

Approval

↓

Deployment
```

---

# Task 1 – Retry Build

Create

```groovy
retry(3) {

    sh "false"

}
```

Observe

```
Attempt 1

Attempt 2

Attempt 3
```

---

# Task 2 – Timeout

Wrap

```groovy
timeout(time:1, unit:'MINUTES')
```

around

```groovy
sleep 120
```

Observe timeout.

---

# Task 3 – Sleep

Pause

```
20 Seconds
```

Display

```
Waiting...

Deployment Starting...
```

---

# Task 4 – Input Approval

Before deployment

Ask

```
Deploy to Production?
```

Approve

Reject

Observe Pipeline behavior.

---

# Task 5 – User Selection

Prompt user

```
Dev

QA

UAT

Production
```

Print selected value.

---

# Task 6 – Error Handling

Create

```groovy
try {

sh "abcd"

}

catch(Exception e){

echo "Command Failed"

}
```

Observe

Pipeline continues.

---

# Task 7 – Build Status

Display

```
currentBuild.number

currentBuild.result

currentBuild.duration
```

---

# Challenge Activity

Create a Pipeline that

* Waits 15 seconds
* Requests approval
* Retries build twice
* Handles failure gracefully

---

# Validation Checklist

✔ Retry working

✔ Timeout working

✔ Sleep executed

✔ User approval requested

✔ Error handled

✔ Build details displayed

---

# Expected Outcome

Students understand how to create resilient Jenkins Pipelines.

