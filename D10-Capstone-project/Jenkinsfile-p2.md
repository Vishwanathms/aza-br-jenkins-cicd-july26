# Capstone Project – Student Configuration Guide

## Jenkins CI/CD Pipeline Configuration

Before running the capstone pipeline, students **must customize** the Jenkinsfile to match their own AWS account and application.

---

# 1. AWS Region

Locate the following line:

```groovy
AWS_REGION = 'ap-south-1'
```

Replace it with your AWS Region.

Examples

| Region      | Value          |
| ----------- | -------------- |
| Mumbai      | ap-south-1     |
| Singapore   | ap-southeast-1 |
| Oregon      | us-west-2      |
| N. Virginia | us-east-1      |

Example

```groovy
AWS_REGION = 'ap-south-1'
```

---

# 2. AWS Account ID

Locate

```groovy
AWS_ACCOUNT_ID = '123456789012'
```

Replace with your own AWS Account ID.

Example

```groovy
AWS_ACCOUNT_ID = '987654321098'
```

How to find it

AWS Console

Top Right Corner

↓

Account ID

Example

```
987654321098
```

---

# 3. Amazon ECR Repository Name

Locate

```groovy
ECR_REPOSITORY = 'python-flask-app'
```

Replace with the repository you created.

Example

```groovy
ECR_REPOSITORY = 'student01-python'
```

Verify

AWS Console

↓

Elastic Container Registry

↓

Repositories

↓

Repository Name

---

# 4. Jenkins Agent Label

Locate

```groovy
agent {
    label 'python'
}
```

Replace with the label of your Jenkins Agent.

Examples

```groovy
label 'docker'
```

or

```groovy
label 'linux'
```

or

```groovy
label 'student-agent'
```

Verify

Jenkins

↓

Manage Jenkins

↓

Nodes

↓

Labels

---

# 5. Docker Compose File Location (if required)

If docker-compose.yml is not present in the repository root, update the path.

Current

```groovy
docker compose up -d
```

Example

```groovy
cd deployment

docker compose up -d
```

---

# 6. Health Check URL

Locate

```groovy
curl http://localhost:8000
```

If your application uses another port, update it.

Examples

Port 8080

```groovy
curl http://localhost:8080
```

Port 8000

```groovy
curl http://localhost:8000
```

---

# 7. Application Port

Ensure the Docker Compose file exposes the correct port.

Example

```yaml
ports:
  - "5000:5000"
```

If your Flask app runs on 8000

```yaml
ports:
  - "8000:8000"
```

---

# 8. Docker Image Name

The image name is constructed automatically.

```groovy
IMAGE_URI =
"${AWS_ACCOUNT_ID}.dkr.ecr.${AWS_REGION}.amazonaws.com/${ECR_REPOSITORY}:${IMAGE_TAG}"
```

Normally, **no changes are required**.

---

# 9. Build Number

The image tag uses Jenkins Build Number.

```groovy
IMAGE_TAG = "${BUILD_NUMBER}"
```

Do **not** modify this.

Each build creates a unique image.

Example

```
Build 15

↓

python-flask-app:15
```

---

# 10. AWS CLI

Verify AWS CLI is installed.

```bash
aws --version
```

Expected

```
aws-cli/2.x.x
```

---

# 11. Docker

Verify Docker is installed.

```bash
docker --version
```

---

# 12. Docker Compose

Verify

```bash
docker compose version
```

---

# 13. IAM Permissions

The IAM User/Role configured on the Jenkins Agent must have permissions for:

* Amazon ECR
* Get Authorization Token
* Push Images
* List Images

Recommended managed policy:

```
AmazonEC2ContainerRegistryPowerUser
```

---

# 14. Existing Amazon ECR Repository

Ensure the repository already exists.

Example

```
python-flask-app
```

Verify

AWS Console

↓

Amazon ECR

↓

Repositories

---

# 15. Jenkins Credentials (If Using AWS Credentials Plugin)

If your Jenkins pipeline uses credentials instead of a configured AWS CLI profile, update the credential ID.

Example

```groovy
withCredentials([
    [$class: 'AmazonWebServicesCredentialsBinding',
    credentialsId: 'aws-prod']
])
```

Replace

```
aws-prod
```

with your own Jenkins Credential ID.

---

# Final Checklist

| Item                            | Student Action | Completed |
| ------------------------------- | -------------- | --------- |
| Update AWS Region               | ☐              |           |
| Update AWS Account ID           | ☐              |           |
| Create ECR Repository           | ☐              |           |
| Update Repository Name          | ☐              |           |
| Verify Jenkins Agent Label      | ☐              |           |
| Verify Docker Installed         | ☐              |           |
| Verify Docker Compose Installed | ☐              |           |
| Verify AWS CLI Installed        | ☐              |           |
| Verify IAM Permissions          | ☐              |           |
| Verify Health Check Port        | ☐              |           |
| Verify Docker Compose Path      | ☐              |           |
| Commit Jenkinsfile              | ☐              |           |
| Run Pipeline                    | ☐              |           |

---

# Example (Completed Configuration)

```groovy
environment {

    AWS_REGION      = 'ap-south-1'

    AWS_ACCOUNT_ID  = '987654321098'

    ECR_REPOSITORY  = 'student01-python'

    IMAGE_TAG       = "${BUILD_NUMBER}"

    IMAGE_URI       = "${AWS_ACCOUNT_ID}.dkr.ecr.${AWS_REGION}.amazonaws.com/${ECR_REPOSITORY}:${IMAGE_TAG}"
}
```

## Submission Requirements

Each student should submit:

1. **Updated `Jenkinsfile`** with their AWS configuration.
2. **`docker-compose.yml`** configured to pull the image from Amazon ECR.
3. Screenshot of the successful Jenkins pipeline.
4. Screenshot of the Amazon ECR repository showing the pushed image.
5. Output of `docker ps` showing the running Flask and Redis containers.
6. Browser screenshot showing the Flask application running successfully.
