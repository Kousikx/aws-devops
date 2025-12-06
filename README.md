# AWS DevOps CI/CD Pipeline

[![AWS](https://img.shields.io/badge/AWS-ECS%20%7C%20ECR%20%7C%20CodePipeline-orange?logo=amazon-aws)](https://aws.amazon.com/)
[![Terraform](https://img.shields.io/badge/Terraform-1.0+-purple?logo=terraform)](https://www.terraform.io/)
[![Node.js](https://img.shields.io/badge/Node.js-18+-green?logo=node.js)](https://nodejs.org/)
[![Docker](https://img.shields.io/badge/Docker-Multi--stage-blue?logo=docker)](https://www.docker.com/)
[![License](https://img.shields.io/badge/License-MIT-yellow.svg)](LICENSE)

A **production-ready, fully automated CI/CD pipeline** built on AWS using Terraform, featuring containerized deployment with ECS Fargate, automated builds with CodePipeline, and a modern responsive web interface.

![application](screenshots/1.png)\

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

### Live Demo
- **Application URL**: Provided after `terraform apply` completes
- **CodePipeline Console**: `https://console.aws.amazon.com/codesuite/codepipeline/pipelines/aws-devops-pipeline/view`

---

## Architecture

![architecture](screenshots/11.png)

### Key Components:
- **VPC**: Isolated network with public/private subnets across 2 AZs
- **ECS Fargate**: Serverless container orchestration
- **Application Load Balancer**: Traffic distribution with health checks
- **ECR**: Private Docker image registry
- **CodePipeline**: Automated CI/CD workflow
- **CodeBuild**: Docker image building
- **CloudWatch**: Centralized logging and monitoring

---

## Features

### Infrastructure
- ✅ **Multi-AZ Deployment** - High availability across availability zones
- ✅ **Auto-scaling** - Scales from 2-4 tasks based on CPU/Memory
- ✅ **Zero-downtime Deployments** - Rolling updates with circuit breaker
- ✅ **Infrastructure as Code** - 100% Terraform-managed
- ✅ **Automated Backups** - S3 artifact storage with lifecycle policies

### CI/CD
- ✅ **Automated Builds** - Triggered on every GitHub push
- ✅ **Docker Multi-stage Builds** - Optimized image size
- ✅ **Vulnerability Scanning** - ECR image scanning on push
- ✅ **Deployment Circuit Breaker** - Automatic rollback on failures
- ✅ **Build Caching** - Faster builds with npm cache

### Application
- ✅ **Modern UI** - Responsive design with dark theme
- ✅ **Mobile-friendly** - Hamburger menu for mobile devices
- ✅ **Health Checks** - Application and container-level monitoring
- ✅ **Graceful Shutdown** - Proper signal handling
- ✅ **Security Headers** - Helmet.js for HTTP security

### Security
- ✅ **Least Privilege IAM** - Minimal required permissions
- ✅ **Private Subnets** - Containers run in private network
- ✅ **Security Groups** - Strict ingress/egress rules
- ✅ **Non-root Containers** - Enhanced container security
- ✅ **Secrets Management** - GitHub token via Terraform variables

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

```
Outputs:

alb_url = "http://aws-devops-alb-xxxxx.us-east-1.elb.amazonaws.com/"
codepipeline_url = "https://console.aws.amazon.com/codesuite/codepipeline/..."
ecr_repository_url = "xxxxx.dkr.ecr.us-east-1.amazonaws.com/aws-devops-app"
```

Visit the `alb_url` to see your application! 🎉

- Application Screenshot

  ![application](screenshots/1.png)

  ![application](screenshots/2.png)

  ![application](screenshots/3.png)

  ![application](screenshots/4.png)

  ![application](screenshots/5.png)

---

## Project Structure

```
aws-devops/
├── app/                          # Node.js Application
│   ├── src/
│   │   └── server.js            # Express server
│   ├── public/
│   │   ├── index.html           # Frontend UI
│   │   ├── css/style.css        # Responsive styles
│   │   └── js/app.js            # Client-side JavaScript
│   ├── Dockerfile               # Multi-stage Docker build
│   ├── buildspec.yml            # CodeBuild configuration
│   ├── package.json             # Node.js dependencies
│   └── .dockerignore            # Docker build exclusions
│
├── terraform/                    # Infrastructure as Code
│   ├── main.tf                  # Provider & data sources
│   ├── variables.tf             # Input variables
│   ├── outputs.tf               # Output values
│   ├── vpc.tf                   # VPC, subnets, NAT, IGW
│   ├── ecr.tf                   # Container registry
│   ├── ecs.tf                   # ECS cluster, service, ALB
│   ├── iam.tf                   # IAM roles & policies
│   ├── codebuild.tf             # Build project
│   ├── codepipeline.tf          # CI/CD pipeline
│   ├── terraform.tfvars.example # Example variables
│   └── .gitignore               # Terraform exclusions
│
├── README.md                     # This file
├── LICENSE                       # MIT License
└── .gitignore                    # Git exclusions
```

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

---

## Contributing

Contributions are welcome! Please follow these steps:

1. Fork the repository
2. Create a feature branch (`git checkout -b feature/amazing-feature`)
3. Commit your changes (`git commit -m 'Add amazing feature'`)
4. Push to the branch (`git push origin feature/amazing-feature`)
5. Open a Pull Request

---

## License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

---

## Acknowledgments

- AWS for comprehensive cloud services
- HashiCorp for Terraform
- The open-source community

---

**Built with ❤️ for DevOps by [Amitabh](https://github.com/Amitabh-DevOps)**

**⭐ Star this repo if you find it helpful!**
