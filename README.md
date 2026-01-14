
A **production-ready, fully automated CI/CD pipeline** built on AWS using Terraform, featuring containerized deployment with ECS Fargate, automated builds with CodePipeline, and a modern responsive web interface.


## Overview

This project demonstrates a **complete AWS DevOps pipeline** that automatically builds, tests, and deploys a Node.js application to ECS Fargate whenever code is pushed to GitHub. Everything is provisioned using Infrastructure as Code (Terraform) following AWS best practices.



### AWS Account Requirements
- Active AWS account with admin access
- AWS credentials configured (`aws configure`)
- GitHub Personal Access Token with `repo` and `admin:repo_hook` permissions

---

## Quick Start

### 1. Clone the Repository

```bash
git clone https://github.com/Kousikx/aws-devops.git
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

### 6. Access Your Application
http://aws-devops-alb-373892596.us-east-1.elb.amazonaws.com/

<img width="975" height="613" alt="image" src="https://github.com/user-attachments/assets/514c8390-a783-4d1c-9d23-214171e16d6a" />

<img width="1000" height="689" alt="image" src="https://github.com/user-attachments/assets/d1727fbb-159e-4257-adbc-47bb85b7efe4" />

