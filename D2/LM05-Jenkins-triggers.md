# Lab 15 – Poll SCM Build Trigger

## Objective

Configure Jenkins to check a Git repository periodically and start a build when changes are detected.

---

## Important Difference

`Build periodically` means:

```text
Run the job according to the schedule
regardless of whether source code changed.
```

`Poll SCM` means:

```text
Check the source repository according to the schedule.

If Jenkins detects changes → run the job.

If there are no changes → don't run the job.
```

---

# Step 1: Create a GitHub Repository

Create a repository containing a simple file such as:

```text
README.md
```

Example repository structure:

```text
jenkins-freestyle-demo/
│
├── README.md
└── app.txt
```

---

# Step 2: Create Jenkins Job

Create a Freestyle project:

```text
Git-Poll-Demo
```

---

# Step 3: Configure Source Code Management

Go to:

**Configure → Source Code Management**

Select:

**Git**

Enter your Git repository URL.

For example:

```text
https://github.com/<username>/jenkins-freestyle-demo.git
```

For a private repository, configure appropriate Jenkins Git credentials.

---

# Step 4: Configure Branch

Under:

**Branches to build**

Specify:

```text
*/main
```

If your repository uses `master`:

```text
*/master
```

---

# Step 5: Configure Poll SCM

Under:

**Build Triggers**

Select:

**Poll SCM**

Schedule:

```text
H/2 * * * *
```

This tells Jenkins to check the Git repository approximately every two minutes.

---

# Step 6: Configure Build Step

Add:

**Execute shell**

```bash
echo "================================"
echo "Git Repository Change Detected"
echo "================================"

echo "Job: $JOB_NAME"
echo "Build: $BUILD_NUMBER"

echo ""
echo "Repository files:"
ls -la

echo ""
echo "Latest Git Commit:"
git log -1 --oneline

echo ""
echo "Build completed."
```

Click:

**Save**

---

# Step 7: Initial Build

Click:

**Build Now**

This verifies that Jenkins can successfully clone the repository.

Check:

**Console Output**

Confirm that the repository is successfully downloaded.

---

# Step 8: Make a Git Change

On your machine:

```bash
git clone https://github.com/<username>/jenkins-freestyle-demo.git
```

Enter the directory:

```bash
cd jenkins-freestyle-demo
```

Make a change:

```bash
echo "Jenkins Poll SCM Test" >> README.md
```

Commit:

```bash
git add .
git commit -m "Testing Jenkins Poll SCM"
```

Push:

```bash
git push origin main
```

---

# Step 9: Observe Jenkins

Wait approximately 2–5 minutes.

Jenkins should detect the new commit.

A new build should automatically start.

Check:

**Build History → Console Output**

You should see the latest Git commit.

---

# Lab 16 – GitHub Webhook Trigger

## Objective

Trigger Jenkins immediately whenever code is pushed to GitHub.

This is generally preferable to frequent SCM polling because GitHub informs Jenkins when a change happens.

---

# Architecture

```text
Developer
    |
    | git push
    v
GitHub Repository
    |
    | Webhook
    v
Jenkins Server
    |
    v
Freestyle Job
    |
    v
Build
```

---

# Step 1: Jenkins Must Be Reachable

For GitHub webhook integration, GitHub must be able to reach the Jenkins server.

For example:

```text
https://jenkins.example.com
```

A Jenkins server accessible only through an internal/private IP generally cannot receive GitHub.com webhooks directly.

---

# Step 2: Configure Jenkins Job

Open:

```text
Git-Poll-Demo
```

Go to:

**Configure → Build Triggers**

Enable:

**GitHub hook trigger for GITScm polling**

Disable `Poll SCM` if you want to test only the webhook mechanism.

Click:

**Save**

---

# Step 3: Configure GitHub Webhook

Open your GitHub repository.

Navigate to:

**Settings → Webhooks → Add webhook**

Payload URL:

```text
https://<JENKINS-DOMAIN>/github-webhook/
```

For example:

```text
https://jenkins.example.com/github-webhook/
```

The trailing `/` is recommended.

---

## Content Type

Select:

```text
application/json
```

---

## Events

Select:

**Just the push event**

Make sure:

**Active**

is enabled.

Click:

**Add webhook**

---

# Step 4: Test the Webhook

Make another change:

```bash
echo "Webhook Test" >> README.md
```

Commit:

```bash
git add .
git commit -m "Testing Jenkins GitHub Webhook"
```

Push:

```bash
git push origin main
```

Immediately check Jenkins.

A new build should start.

---

# Lab 17 – Trigger Build Remotely

## Objective

Understand how Jenkins jobs can be triggered using an authentication token.

Open a Freestyle job.

Go to:

**Configure → Build Triggers**

Select:

**Trigger builds remotely (e.g., from scripts)**

Enter an authentication token such as:

```text
jenkins-lab-token
```

Save the job.

This mechanism allows external automation/scripts to initiate Jenkins builds.

> In production, do not treat the job token as sufficient security by itself. Jenkins authentication/API tokens and appropriate permissions should also be used.

---

# Lab 18 – Build Trigger Comparison

| Trigger                    | Purpose                  | Build happens when         |
| -------------------------- | ------------------------ | -------------------------- |
| Manual                     | Testing/manual execution | User clicks Build Now      |
| Build periodically         | Scheduled automation     | Schedule is reached        |
| Poll SCM                   | Source-change detection  | SCM changes are detected   |
| GitHub Webhook             | Event-based automation   | GitHub sends push event    |
| Build after other projects | Job chaining             | Upstream job completes     |
| Remote Trigger             | External automation      | API/remote request arrives |

---

# Build Periodically vs Poll SCM

This is an important Jenkins concept.

### Build Periodically

```text
Schedule
   ↓
Jenkins Job
   ↓
Build
```

Even if there is no Git change, the job executes.

Example:

```text
H/5 * * * *
```

---

### Poll SCM

```text
Schedule
   ↓
Check Git
   ↓
Any changes?
   |
   ├── NO → Do nothing
   |
   └── YES
          ↓
        Build
```

---

### GitHub Webhook

```text
Developer
   ↓
git push
   ↓
GitHub
   ↓
Webhook
   ↓
Jenkins
   ↓
Build
```

This is event-driven rather than based on Jenkins repeatedly checking GitHub.

---

# Lab 19 – Final Hands-On Assignment

## Scenario

Your organization has an application called:

```text
CloudPortal
```

You need to organize its Jenkins jobs and configure automated build triggers.

---

## Requirement 1 – Folder Structure

Create:

```text
CloudPortal
│
├── Development
├── Testing
└── Production
```

---

## Requirement 2 – Development Job

Inside:

```text
CloudPortal/Development
```

Create:

```text
Build-Application
```

Type:

**Freestyle Project**

Build command:

```bash
echo "CloudPortal Development Build"

echo "Build Number: $BUILD_NUMBER"

echo "Building application..."

sleep 5

echo "Build successful"
```

---

## Requirement 3 – Testing Job

Inside:

```text
CloudPortal/Testing
```

Create:

```text
Test-Application
```

Build command:

```bash
echo "CloudPortal Testing"

echo "Running tests..."

sleep 5

echo "Test 1: PASS"
echo "Test 2: PASS"
echo "Test 3: PASS"

echo "Testing completed successfully"
```

---

## Requirement 4 – Production Job

Inside:

```text
CloudPortal/Production
```

Create:

```text
Deploy-Application
```

Build command:

```bash
echo "CloudPortal Production Deployment"

echo "Deployment started..."

sleep 5

echo "Application deployed successfully"

echo "Deployment Build: $BUILD_NUMBER"
```

---

# Expected Structure

```text
Jenkins
│
└── CloudPortal
    │
    ├── Development
    │   └── Build-Application
    │
    ├── Testing
    │   └── Test-Application
    │
    └── Production
        └── Deploy-Application
```

---

# Requirement 5 – Configure Build Trigger

Configure the Development job with:

```text
Poll SCM
```

Schedule:

```text
H/5 * * * *
```

Connect it to a Git repository.

---

# Requirement 6 – Test Git Change

Modify a file in Git:

```bash
echo "New Application Feature" >> README.md
```

Commit:

```bash
git add .
git commit -m "Added new application feature"
```

Push:

```bash
git push origin main
```

Verify that Jenkins detects the change and starts:

```text
Build-Application
```

---

# Lab Validation Checklist

Students should verify the following:

* [ ] Jenkins Docker container is running
* [ ] Jenkins Dashboard is accessible
* [ ] Freestyle project created
* [ ] Execute Shell build step configured
* [ ] Manual build completed successfully
* [ ] Console Output verified
* [ ] Jenkins workspace understood
* [ ] Jenkins folders created
* [ ] Jobs created inside folders
* [ ] Build periodically tested
* [ ] Upstream/downstream build trigger tested
* [ ] Git repository configured
* [ ] Poll SCM configured
* [ ] Git change automatically detected
* [ ] GitHub webhook concept understood/tested
* [ ] Remote build trigger understood

---

# Final Lab Outcome

After completing this lab, students should understand the following Jenkins workflow:

```text
                   JENKINS
                      |
             +--------+--------+
             |                 |
          Folders         Freestyle Jobs
             |                 |
       +-----+-----+       Build Steps
       |     |     |            |
      DEV   TEST  PROD      Execute Shell
                              |
                         Build Triggers
                              |
             +----------------+----------------+
             |                |                |
         Scheduled         Poll SCM         Webhook
             |                |                |
             +----------------+----------------+
                              |
                            BUILD
                              |
                       Console Output
```

