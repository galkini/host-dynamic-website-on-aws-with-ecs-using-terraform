![Alt text](/Host_a_Dynamic_Website_on_AWS_with_ECS_with_Terraform.png)

![Alt text](/VPC_drawing.png)


# Hosting a Dynamic Website on AWS Using Terraform, Docker, ECS, and ECR

This project demonstrates how to host a dynamic website on AWS using Docker containers, ECS (Elastic Container Service), and ECR (Elastic Container Registry) with Terraform for infrastructure as code (IaC). The project encompasses creating various AWS resources to ensure high availability, scalability, and security. A reference diagram and configuration files are available in the private GitHub repository.

## Prerequisites

### Software and Tools

1. **Terraform**:
   - Install Terraform and update the system environment variable to access it from any path location.

2. **Git**:
   - Install and configure Git on your PC.

3. **GitHub**:
   - Create private GitHub repositories to store main configuration files and modules configuration files.
   - Generate an SSH key pair on your PC and add the public key to GitHub.
   - Clone the GitHub repositories to your PC and sync changes during the entire project implementation process.

4. **Visual Studio Code**:
   - Install Visual Studio Code and integrate it with Terraform.

5. **AWS CLI**:
   - Install the AWS CLI on your PC.

### AWS Services

1. **S3 Bucket**:
   - Create an S3 bucket in your AWS account to store the Terraform state.

2. **DynamoDB Table**:
   - Create a DynamoDB table to store the Terraform lock file.

3. **IAM User and Role**:
   - Create a Terraform user via IAM in your AWS account and authenticate Terraform with your AWS account.
   - Create an IAM role to grant permissions for ECS to execute tasks, including accessing the S3 bucket with the environment variables file.

## Infrastructure Setup Using Terraform

### Steps to Deploy

1. **VPC Configuration**:
   - Configure a Virtual Private Cloud (VPC) with public and private subnets spanning two availability zones.
   - Deploy an Internet Gateway to enable connectivity between the VPC instances and the wider internet.
   - Establish Security Groups to serve as a network firewall mechanism.
   - Use public subnets for infrastructure components like the NAT Gateway and Application Load Balancer.

2. **Application Load Balancer**:
   - Create an Application Load Balancer with a target group to distribute web traffic to the ECS Fargate tasks.

3. **Amazon RDS**:
   - Create an Amazon RDS for MySQL to store the website database.

4. **Bastion Host**:
   - Use a Bastion Host to set up an SSH tunnel to securely migrate SQL data into RDS.

5. **S3 Bucket for Environment Variables**:
   - Create an S3 bucket to store files containing environment variables for the container.

6. **ECS and Fargate**:
   - Create an Amazon ECS task and ECS service to containerize the web application on AWS Cloud and scale according to the given policy.
   - Use ECS Fargate to run the containerized application.

7. **Security**:
   - Secure application communications using AWS Certificate Manager.
   - Register the domain name and set up a DNS record using Route 53.

### Project Cleanup

1. **Destroy Infrastructure**:
   - After completing and testing the project, destroy the created infrastructure using the `terraform destroy` command.

## Example Terraform Configuration

Here's an example of some of the key Terraform configuration files you might use in this project:

### backend.tf

```hcl
terraform {
  backend "s3" {
    bucket = "<your-s3-bucket-name>"
    key    = "terraform/state"
    region = "us-east-1"
    dynamodb_table = "<your-dynamodb-table-name>"
  }
}
```

### providers.tf

```hcl
provider "aws" {
  region = "us-east-1"
}
```

### main.tf

```hcl
module "vpc" {
  source = "github.com/<your-repo>/vpc-module"
  # module variables
}

module "security_groups" {
  source = "github.com/<your-repo>/security-groups-module"
  # module variables
}

module "ecs" {
  source = "github.com/<your-repo>/ecs-module"
  # module variables
}
```

### variables.tf

```hcl
variable "region" {
  description = "The AWS region to deploy resources"
  default     = "us-east-1"
}

variable "vpc_cidr" {
  description = "CIDR block for the VPC"
  default     = "10.0.0.0/16"
}

# Add more variables as needed
```

## Repository Structure

Your GitHub repository should be organized to separate main configuration files and module configurations.

### Example Structure

```
root
├── main-configuration
│   ├── backend.tf
│   ├── main.tf
│   ├── providers.tf
│   ├── variables.tf
│   └── outputs.tf
└── modules
    ├── vpc
    │   ├── main.tf
    │   ├── variables.tf
    │   ├── outputs.tf
    ├── security-groups
    │   ├── main.tf
    │   ├── variables.tf
    │   ├── outputs.tf
    └── ecs
        ├── main.tf
        ├── variables.tf
        ├── outputs.tf
```

## Final Steps

- Ensure all configuration files are properly synced with your GitHub repository.
- Use `terraform init`, `terraform plan`, and `terraform apply` commands in your root module directory to deploy your infrastructure.

## Conclusion

By following these instructions, you will set up a dynamic website on AWS using Docker containers, ECS, and ECR managed by Terraform. This setup leverages various AWS services to provide a robust and scalable infrastructure, adhering to best DevOps practices.
