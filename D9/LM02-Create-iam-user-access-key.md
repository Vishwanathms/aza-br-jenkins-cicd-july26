# Lab Manual: Create an IAM User for Amazon ECR (Elastic Container Registry)

## Lab Objective

In this lab, you will learn how to:

* Create an IAM User for Docker image operations
* Attach Amazon ECR Full Access permissions
* Generate Access Key and Secret Access Key
* Configure AWS CLI using the credentials
* Verify connectivity to Amazon ECR

---

# Lab Architecture

```
AWS Account
      │
      │
 ┌───────────────┐
 │   IAM User    │
 │ ecr-admin     │
 └──────┬────────┘
        │
Access Key + Secret Key
        │
        ▼
Ubuntu / Jenkins / Developer Laptop
        │
 AWS CLI
        │
 Login to Amazon ECR
        │
 Push / Pull Docker Images
```

---

# Prerequisites

* AWS Account
* Administrator access to AWS Console
* Browser
* Ubuntu Machine (Optional for verification)
* AWS CLI Installed

---

# Step 1 — Login to AWS Console

Open

```
https://console.aws.amazon.com
```

Login using your AWS Administrator account.

---

# Step 2 — Open IAM Service

Search

```
IAM
```

Open

```
Identity and Access Management (IAM)
```

---

# Step 3 — Navigate to Users

Left Menu

```
Users
```

Click

```
Create User
```

---

# Step 4 — Enter User Details

User name

```
ecr-admin
```

Leave all other options as default.

Click

```
Next
```

---

# Step 5 — Set Permissions

Select

```
Attach policies directly
```

Search

```
AmazonEC2ContainerRegistryFullAccess
```

Select the checkbox.

The policy grants permissions to:

* Create repositories
* Delete repositories
* Push Docker images
* Pull Docker images
* Create lifecycle policies
* Manage repository permissions

Click

```
Next
```

---

# Step 6 — Review

Review

```
Username:
ecr-admin

Permissions:
AmazonEC2ContainerRegistryFullAccess
```

Click

```
Create User
```

Expected Output

```
User created successfully.
```

---

# Step 7 — Open the User

Click

```
ecr-admin
```

You will see

```
Summary
Permissions
Groups
Security credentials
Tags
```

---

# Step 8 — Create Access Key

Open

```
Security Credentials
```

Scroll down to

```
Access Keys
```

Click

```
Create access key
```

---

# Step 9 — Choose Use Case

AWS asks

```
Best practices & alternatives
```

Select

```
Command Line Interface (CLI)
```

Tick

```
I understand the recommendation...
```

Click

```
Next
```

---

# Step 10 — Description (Optional)

Enter

```
Ubuntu Laptop
```

or

```
Jenkins Server
```

Click

```
Create Access Key
```

---

# Step 11 — Download Credentials

AWS displays

```
Access Key ID

Secret Access Key
```

Example

```
Access Key

AKIAIOSFODNN7EXAMPLE

Secret

wJalrXUtnFEMI/K7MDENG/bPxRfiCYEXAMPLEKEY
```

**Important:**

* Copy the Secret Access Key immediately.
* AWS will not display it again.
* Alternatively, download the CSV file.

Store these credentials securely. Do not commit them to source control or share them publicly.

---

# Step 12 — Verify User Permissions

Navigate to

```
Permissions
```

Verify

```
AmazonEC2ContainerRegistryFullAccess
```

Status

```
Attached
```

---

# Step 13 — Install AWS CLI (Ubuntu)

```bash
curl "https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip" -o awscliv2.zip

sudo apt update

sudo apt install unzip -y

unzip awscliv2.zip

sudo ./aws/install
```

Verify

```bash
aws --version
```

Expected

```
aws-cli/2.x.x
```

---

# Step 14 — Configure AWS CLI

Run

```bash
aws configure
```

Enter

```
AWS Access Key ID:
AKIAxxxxxxxxxxxxxxxx

AWS Secret Access Key:
xxxxxxxxxxxxxxxxxxxxxxxxxxxx

Default region:
ap-south-1

Default output:
json
```

---

# Step 15 — Verify Identity

Run

```bash
aws sts get-caller-identity
```

Example Output

```json
{
    "UserId": "AIDAxxxxxxxxxxxxx",
    "Account": "123456789012",
    "Arn": "arn:aws:iam::123456789012:user/ecr-admin"
}
```

This confirms the CLI is using the IAM user's credentials.

---

# Step 16 — Verify Amazon ECR Access

List repositories

```bash
aws ecr describe-repositories
```

If repositories exist

Example

```json
{
    "repositories": [
        {
            "repositoryName": "python-app"
        }
    ]
}
```

If none exist

```
RepositoryNotFoundException
```

or

```
{
    "repositories": []
}
```

Both indicate successful authentication if permissions are sufficient.

---

# Step 17 — Login to Amazon ECR

Replace the placeholders with your AWS Region and Account ID.

```bash
aws ecr get-login-password \
--region ap-south-1 \
| docker login \
--username AWS \
--password-stdin \
123456789012.dkr.ecr.ap-south-1.amazonaws.com
```

Expected

```
Login Succeeded
```

---

# Step 18 — Create a Test Repository

```bash
aws ecr create-repository \
--repository-name nginx-demo
```

Expected

```json
{
    "repository": {
        "repositoryName": "nginx-demo"
    }
}
```

---

# Step 19 — Verify Repository

```bash
aws ecr describe-repositories
```

Example

```json
{
    "repositories": [
        {
            "repositoryName": "nginx-demo"
        }
    ]
}
```

---

# Step 20 — Cleanup (Optional)

Delete the repository

```bash
aws ecr delete-repository \
--repository-name nginx-demo \
--force
```

Delete the IAM user from the AWS Console:

1. Remove the access key from **Security credentials**.
2. Detach the `AmazonEC2ContainerRegistryFullAccess` policy.
3. Delete the `ecr-admin` IAM user.

---

# Security Best Practices

* Follow the principle of least privilege. For production environments, prefer a custom policy that grants access only to required ECR repositories instead of full ECR access.
* Rotate access keys regularly.
* Never embed access keys in source code or Docker images.
* Store secrets in a secure secrets manager or CI/CD credential store (for example, Jenkins Credentials or AWS Secrets Manager).
* Delete unused access keys promptly.
* Consider using IAM roles instead of long-lived access keys when running workloads on AWS services such as EC2, ECS, or EKS.

---

# Lab Validation Checklist

| Task                                                   | Status |
| ------------------------------------------------------ | ------ |
| Logged into AWS Console                                | ☐      |
| Opened IAM                                             | ☐      |
| Created `ecr-admin` IAM user                           | ☐      |
| Attached `AmazonEC2ContainerRegistryFullAccess` policy | ☐      |
| Generated Access Key and Secret Access Key             | ☐      |
| Installed AWS CLI                                      | ☐      |
| Configured AWS CLI                                     | ☐      |
| Verified identity with `aws sts get-caller-identity`   | ☐      |
| Listed ECR repositories                                | ☐      |
| Logged in to Amazon ECR                                | ☐      |
| Created an ECR repository                              | ☐      |
| Verified repository creation                           | ☐      |

### Expected Learning Outcome

After completing this lab, you will be able to:

* Create an IAM user for Amazon ECR access.
* Generate and manage AWS access keys securely.
* Configure the AWS CLI with IAM credentials.
* Authenticate Docker with Amazon ECR.
* Create, list, and manage Amazon ECR repositories.
* Apply security best practices for IAM users and access keys.
