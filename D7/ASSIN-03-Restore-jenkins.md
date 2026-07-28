# Lab 3 – Restore Jenkins

## Lab Title

**Lab 3 – Disaster Recovery: Restoring Jenkins from Backup**

Duration

75 Minutes

Difficulty

Intermediate–Advanced

---

# Objectives

Students will

* Simulate failure
* Restore Jenkins
* Validate recovery
* Verify Jobs
* Verify Plugins
* Verify Credentials

---

# Scenario

Production Jenkins has failed.

Recover from backup.

---

# Prerequisites

Lab 2 completed.

---

# Task 1 – Stop Jenkins

```
sudo systemctl stop jenkins
```

---

# Task 2 – Simulate Failure

Rename

```
sudo mv \
/var/lib/jenkins \
/var/lib/jenkins.old
```

Verify

```
ls /var/lib
```

---

# Task 3 – Create Empty Directory

```
mkdir /var/lib/jenkins
```

---

# Task 4 – Restore Backup

```
sudo tar -xzvf \
/backup/jenkins-backup.tar.gz \
-C /
```

---

# Task 5 – Restore Ownership

```
sudo chown -R \
jenkins:jenkins \
/var/lib/jenkins
```

---

# Task 6 – Verify Permissions

```
ls -ld \
/var/lib/jenkins
```

Expected

```
jenkins jenkins
```

---

# Task 7 – Start Jenkins

```
sudo systemctl start jenkins
```

---

# Task 8 – Verify Service

```
systemctl status jenkins
```

---

# Task 9 – Login

Open

```
http://<IP>:8080
```

Verify

Dashboard opens.

---

# Task 10 – Verify Jobs

Confirm

* Pipelines
* Freestyle Jobs
* Build History

---

# Task 11 – Verify Plugins

Navigate

```
Manage Jenkins

↓

Plugins
```

Ensure

Installed plugins restored.

---

# Task 12 – Verify Users

Navigate

```
Manage Jenkins

↓

Users
```

Verify

```
developer1

developer2

qauser

auditor
```

---

# Task 13 – Verify Credentials

Navigate

```
Manage Jenkins

↓

Credentials
```

Ensure

All credentials are available.

---

# Task 14 – Disaster Recovery Validation

Execute

One Pipeline Job.

Confirm

* Workspace
* Console Output
* Artifacts
* Credentials usage
* Plugin functionality

---

# Validation Checklist

Students should verify:

* ✓ Jenkins service starts successfully
* ✓ Dashboard loads without errors
* ✓ All users can log in
* ✓ Jobs and pipelines are restored
* ✓ Build history is preserved
* ✓ Plugins are available
* ✓ Credentials are intact
* ✓ System configuration matches the pre-backup state
* ✓ A sample pipeline executes successfully

---

These three labs closely mirror **enterprise Jenkins operational procedures**, giving students hands-on experience with **hardening**, **backup**, and **disaster recovery** workflows that are commonly used in production environments.
