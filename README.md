
A **production-ready, fully automated CI/CD pipeline** built on AWS using Terraform, featuring containerized deployment with ECS Fargate, automated builds with CodePipeline, and a modern responsive web interface.

![application](screenshots/1.png)

## Table of Contents

- [Overview](#overview)
- [Architecture](#architecture)
- [Features](#features)
- [Prerequisites](#prerequisites)
- [Quick Start](#quick-start)
- [Project Structure](#project-structure)
- [CI/CD Pipeline](#cicd-pipeline)
- [Contributing](#contributing)
- [License](#license)
- [Acknowledgments](#acknowledgments)

---

## Overview

This project demonstrates a **complete AWS DevOps pipeline** that automatically builds, tests, and deploys a Node.js application to ECS Fargate whenever code is pushed to GitHub. Everything is provisioned using Infrastructure as Code (Terraform) following AWS best practices.


---

## Prerequisites

### Required Tools
- **AWS CLI** (v2.x) - [Install Guide](https://docs.aws.amazon.com/cli/latest/userguide/getting-started-install.html)
- **Terraform** (v1.0+) - [Install Guide](https://learn.hashicorp.com/tutorials/terraform/install-cli)
- **Git**
- **Node.js** (v18+) - For local development (optional)
- **Docker** - For local testing (optional)

### AWS Account Requirements
- Active AWS account with admin access
- AWS credentials configured (`aws configure`)
- GitHub Personal Access Token with `repo` and `admin:repo_hook` permissions

---

## Quick Start

### 1. Clone the Repository

```bash
git clone https://github.com/Amitabh-DevOps/aws-devops.git
cd aws-devops
```

### 2. Configure AWS Credentials

```bash
aws configure
# Enter your AWS Access Key ID
# Enter your AWS Secret Access Key
# Default region: us-east-1
# Default output format: json
```

### 3. Create GitHub Personal Access Token

1. Go to GitHub → Settings → Developer settings → Personal access tokens → Tokens (classic)
2. Generate new token with scopes: `repo` and `admin:repo_hook`
3. Copy the token (you'll need it in the next step)

### 4. Configure Terraform Variables

```bash
cd terraform
cp terraform.tfvars.example terraform.tfvars
```

Edit `terraform.tfvars`:

```hcl
aws_region      = "us-east-1"
project_name    = "aws-devops"
github_repo     = "YourUsername/aws-devops"  # Change this!
github_branch   = "main"
github_token    = "ghp_your_token_here"      # Paste your token!
```

### 5. Deploy Infrastructure

```bash
# Initialize Terraform
terraform init

# Validate the configuration
terraform validate

# Review the execution plan
terraform plan

# Apply the configuration with auto-approve
terraform apply --auto-approve
```

**Deployment takes ~5-10 minutes**

- ECR Repository

  ![ecr](screenshots/7.png)

- ECS Cluster

  ![ecs](screenshots/8.png)

- ALB

  ![alb](screenshots/9.png)

- S3 Bucket

  ![s3](screenshots/10.png)

### 6. Access Your Application

After deployment completes, Terraform will output:



### Local Development

```bash
cd app

# Install dependencies
npm install

# Run development server
npm run dev

# Access at http://localhost:3000
```

### Docker Build (Local)

```bash
cd app

# Build image
docker build -t aws-devops-app .

# Run container
docker run -p 3000:3000 aws-devops-app

# Access at http://localhost:3000
```

---

## CI/CD Pipeline

### Pipeline Stages

1. **Source** - Pulls code from GitHub on push
2. **Build** - CodeBuild builds Docker image and pushes to ECR
3. **Deploy** - Updates ECS service with new image

### Build Process

```yaml
# buildspec.yml
phases:
  pre_build:
    - Login to ECR
    - Set image tag from commit hash
  build:
    - Build Docker image
    - Tag with commit hash and 'latest'
  post_build:
    - Push images to ECR
    - Generate imagedefinitions.json
```

### Triggering Deployments

```bash
# Make code changes
git add .
git commit -m "Update feature"
git push origin main

# Pipeline automatically triggers!
```

- Pipeline Screenshot

  ![pipeline](screenshots/6.png)

### Monitoring & Logging

#### CloudWatch Log Groups
- Go to CloudWatch Console:
   - `/ecs/aws-devops` - Application logs
   - `/aws/codebuild/aws-devops` - Build logs




**⭐ Star this repo if you find it helpful!**
