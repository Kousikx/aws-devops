# AWS DevOps CI/CD Pipeline Project

A complete AWS DevOps project demonstrating CI/CD pipeline implementation using AWS CodePipeline, CodeBuild, ECR, and ECS - all provisioned with Terraform.

## 🚀 Project Overview

This project showcases a production-ready CI/CD pipeline that:
- **Source**: Pulls code from GitHub
- **Build**: Builds Docker images using AWS CodeBuild
- **Deploy**: Deploys containerized application to AWS ECS Fargate

## 📁 Project Structure

```
AWS-DevOps/
├── app/                    # Node.js Express Application
│   ├── src/               # Application source code
│   ├── public/            # Static assets (CSS, JS, images)
│   ├── Dockerfile         # Container configuration
│   ├── package.json       # Node.js dependencies
│   └── buildspec.yml      # CodeBuild build specification
├── terraform/             # Infrastructure as Code
│   ├── main.tf           # Main Terraform configuration
│   ├── variables.tf      # Input variables
│   ├── outputs.tf        # Output values
│   ├── vpc.tf            # VPC and networking
│   ├── ecr.tf            # Elastic Container Registry
│   ├── ecs.tf            # ECS Cluster and Service
│   ├── codepipeline.tf   # CodePipeline configuration
│   ├── codebuild.tf      # CodeBuild project
│   └── iam.tf            # IAM roles and policies
└── README.md             # This file
```

## 🛠️ Technologies Used

### Infrastructure
- **Terraform** - Infrastructure as Code
- **AWS CodePipeline** - CI/CD orchestration
- **AWS CodeBuild** - Build automation
- **AWS ECR** - Container registry
- **AWS ECS Fargate** - Container orchestration
- **AWS VPC** - Networking

### Application
- **Node.js** - Runtime environment
- **Express.js** - Web framework
- **Docker** - Containerization
- **HTML/CSS/JavaScript** - Frontend

## 📋 Prerequisites

1. **AWS Account** with appropriate permissions
2. **Terraform** (v1.0+) installed
3. **AWS CLI** configured with credentials
4. **GitHub Personal Access Token** (for CodePipeline source)
5. **Node.js** (v18+) for local development

## 🚀 Quick Start

### 1. Clone the Repository

```bash
git clone https://github.com/Amitabh-DevOps/aws-devops.git
cd aws-devops
```

### 2. Configure AWS Credentials

```bash
aws configure
# Enter your AWS Access Key ID, Secret Access Key, and region (us-east-1)
```

### 3. Set Up Terraform Variables

Create a `terraform.tfvars` file in the `terraform/` directory:

```hcl
aws_region          = "us-east-1"
project_name        = "aws-devops"
github_repo         = "Amitabh-DevOps/aws-devops"
github_branch       = "main"
github_token        = "your-github-personal-access-token" # with `repo` and `admin:repo_hook` permissions
```

### 4. Deploy Infrastructure

```bash
cd terraform
terraform init
terraform plan
terraform apply
```

### 5. Trigger the Pipeline

Once Terraform completes, the pipeline will automatically trigger on the next push to the `main` branch:

```bash
git add .
git commit -m "Trigger pipeline"
git push origin main
```

## 🏗️ Infrastructure Components

### VPC & Networking
- VPC with public and private subnets across 2 AZs
- Internet Gateway and NAT Gateways
- Route tables and security groups

### ECR (Elastic Container Registry)
- Private Docker image repository
- Lifecycle policies for image management

### ECS (Elastic Container Service)
- Fargate launch type (serverless containers)
- Application Load Balancer
- Auto-scaling configuration
- CloudWatch logging

### CodePipeline
- **Source Stage**: GitHub integration
- **Build Stage**: CodeBuild for Docker image creation
- **Deploy Stage**: ECS deployment

### CodeBuild
- Docker image building
- Automated testing
- Push to ECR

## 🌐 Application Features

The deployed web application is a modern DevOps dashboard that showcases:
- Real-time CI/CD pipeline visualization
- AWS service integration status
- Modern glassmorphism UI design
- Responsive layout
- Animated components

## 📊 Pipeline Stages

1. **Source**: Detects changes in GitHub repository
2. **Build**: 
   - Pulls source code
   - Builds Docker image
   - Runs tests
   - Pushes image to ECR
3. **Deploy**:
   - Updates ECS task definition
   - Deploys new container version
   - Performs health checks

## 🔒 Security Best Practices

- IAM roles with least privilege access
- Secrets stored in AWS Secrets Manager
- Private subnets for ECS tasks
- Security groups with minimal required access
- ECR image scanning enabled

## 📝 Useful Commands

### Terraform
```bash
terraform init          # Initialize Terraform
terraform plan          # Preview changes
terraform apply         # Apply changes
terraform destroy       # Destroy infrastructure
terraform output        # View outputs
```

### AWS CLI
```bash
# View pipeline status
aws codepipeline get-pipeline-state --name <pipeline-name>

# View ECS service
aws ecs describe-services --cluster <cluster-name> --services <service-name>

# View logs
aws logs tail /ecs/<task-family> --follow
```

### Local Development
```bash
cd app
npm install            # Install dependencies
npm start              # Run locally on port 3000
npm test               # Run tests
docker build -t app .  # Build Docker image
docker run -p 3000:3000 app  # Run container
```

## 🎯 Accessing the Application

After successful deployment, get the Application Load Balancer URL:

```bash
cd terraform
terraform output alb_url
```

Visit the URL in your browser to see the deployed application.

## 🔄 Making Changes

1. Make code changes in the `app/` directory
2. Commit and push to GitHub:
   ```bash
   git add .
   git commit -m "Your changes"
   git push origin main
   ```
3. Pipeline automatically triggers and deploys changes

## 📈 Monitoring

- **CloudWatch Logs**: Application and container logs
- **CodePipeline Console**: Pipeline execution history
- **ECS Console**: Service health and task status
- **CloudWatch Metrics**: CPU, memory, and custom metrics

## 🧹 Cleanup

To avoid AWS charges, destroy all resources:

```bash
cd terraform
terraform destroy
```

**Note**: Confirm the destruction when prompted.

## 🤝 Contributing

1. Fork the repository
2. Create a feature branch
3. Commit your changes
4. Push to the branch
5. Create a Pull Request

## 📄 License

This project is licensed under the MIT License.

## 👨‍💻 Author

**Amitabh**
- GitHub: [@Amitabh-DevOps](https://github.com/Amitabh-DevOps)

## 🙏 Acknowledgments

- AWS Documentation
- Terraform Registry
- DevOps Community

---

**Happy DevOps! 🚀**
