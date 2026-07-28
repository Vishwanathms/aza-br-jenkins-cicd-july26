---

# Lab 2 – Backup Jenkins

## Lab Title

**Lab 2 – Performing Backup of Jenkins Configuration and Jobs**

Duration

60 Minutes

Difficulty

Intermediate

---

# Objectives

Students will learn

* Jenkins Home
* Backup Strategy
* Backup Plugins
* Manual Backup
* Scheduled Backup
* Verify Backup

---

# Prerequisites

Completed Lab 1

---

# Task 1 – Locate Jenkins Home

SSH

```
sudo su -

cd /var/lib/jenkins
```

Verify

```
pwd
```

---

# Task 2 – Review Jenkins Home

Execute

```
ls -lh
```

Observe

```
jobs/

plugins/

workspace/

users/

secrets/

credentials.xml

config.xml
```

---

# Task 3 – Identify Critical Files

Important

```
config.xml

credentials.xml

plugins/

users/

jobs/

nodes/

secrets/
```

Discuss why each is important.

---

# Task 4 – Stop Jenkins

```
sudo systemctl stop jenkins
```

Verify

```
systemctl status jenkins
```

---

# Task 5 – Create Backup

```
mkdir -p /backup
```

Run

```
sudo tar -czvf /backup/jenkins-backup.tar.gz \
/var/lib/jenkins
```

---

# Task 6 – Verify Backup

```
ls -lh /backup
```

Check

```
tar -tvf \
/backup/jenkins-backup.tar.gz
```

---

# Task 7 – Start Jenkins

```
sudo systemctl start jenkins
```

Verify

```
systemctl status jenkins
```

---

# Task 8 – Backup Plugins List

```
ls /var/lib/jenkins/plugins \
> plugins.txt
```

---

# Task 9 – Automate Backup

Create script

```
backup.sh
```

Script

* Stop Jenkins
* Backup
* Start Jenkins
* Log output

---

# Task 10 – Schedule Backup

Edit cron

```
crontab -e
```

Example

```
0 2 * * * /backup/backup.sh
```

Runs every day

2 AM.

---

# Validation

Student should show

```
jenkins-backup.tar.gz
```

Backup size

Backup contents

---

# Deliverables

* Backup archive
* Backup script
* Cron configuration

---

# Challenge

Create backup with

Date

Time

Example

```
jenkins-2026-07-28.tar.gz
```

---

---

