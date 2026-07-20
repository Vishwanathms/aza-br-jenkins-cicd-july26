---

# Part 2: Create Your First Jenkins Job

## Step 16: Create a Freestyle Project

From the Jenkins Dashboard, click:

**New Item**

Enter the item name:

```text
first-jenkins-job
```

Select:

**Freestyle project**

Click:

**OK**

---

## Step 17: Add a Build Step

Scroll to the **Build Steps** section.

Select:

**Add build step → Execute shell**

Enter:

```bash
echo "Hello from Jenkins"
echo "My first Jenkins Job"
echo "Build Number: $BUILD_NUMBER"
echo "Job Name: $JOB_NAME"
date
```

Click:

**Save**

---

## Step 18: Run the Jenkins Job

Click:

**Build Now**

Under **Build History**, click the build number, such as:

```text
#1
```

Click:

**Console Output**

Expected output:

```text
Hello from Jenkins
My first Jenkins Job
Build Number: 1
Job Name: first-jenkins-job
Finished: SUCCESS
```

---

# Part 3: Jenkins Service Management

### Stop Jenkins

```bash
sudo systemctl stop jenkins
```

### Start Jenkins

```bash
sudo systemctl start jenkins
```

### Restart Jenkins

```bash
sudo systemctl restart jenkins
```

### Check Status

```bash
sudo systemctl status jenkins
```

### View Jenkins Logs

```bash
sudo journalctl -u jenkins
```

Follow logs in real time:

```bash
sudo journalctl -u jenkins -f
```

---

# Part 4: Important Jenkins Directories

| Purpose               | Location                     |
| --------------------- | ---------------------------- |
| Jenkins Home          | `/var/lib/jenkins`           |
| Jenkins Configuration | `/etc/default/jenkins`       |
| Jenkins Service       | `jenkins.service`            |
| Jenkins Plugins       | `/var/lib/jenkins/plugins`   |
| Jenkins Jobs          | `/var/lib/jenkins/jobs`      |
| Jenkins Workspace     | `/var/lib/jenkins/workspace` |
| Jenkins Secrets       | `/var/lib/jenkins/secrets`   |

---

# Troubleshooting

### Jenkins is not starting

Check status:

```bash
sudo systemctl status jenkins
```

Check logs:

```bash
sudo journalctl -u jenkins --no-pager -n 100
```

### Java issue

Verify Java:

```bash
java -version
```

### Port 8080 not accessible

Check whether Jenkins is listening:

```bash
sudo ss -tulpn | grep 8080
```

Check UFW:

```bash
sudo ufw status
```

### Restart Jenkins

```bash
sudo systemctl restart jenkins
```

---