#AWS ECR Deployment Pipeline

This repository contains a GitHub Actions workflow to automate the process of building, tagging, and pushing Docker images to an Amazon Elastic Container Registry (ECR). The workflow also updates the ECS service with the new image, ensuring a streamlined CI/CD pipeline.

#Prerequisites

AWS Setup:

An AWS account with permissions for ECR, ECS, and IAM.

A pre-existing ECR repository.

ECS cluster and task definitions configured.

#GitHub Secrets:

**Ensure the following secrets are added to your GitHub repository:**

AWS_ACCESS_KEY_ID: Your AWS access key ID.

AWS_SECRET_ACCESS_KEY: Your AWS secret access key.

AWS_REGION: AWS region (e.g., us-east-1).

ECR_REPOSITORY: Name of the ECR repository.

TASK_DEFINITION: Name of the ECS task definition.

ECS_SERVICE: Name of the ECS service.

ECS_CLUSTER: Name of the ECS cluster.

CONTAINER_NAME: Name of the container in the ECS task definition.

GITHUB_TOKEN: Personal access token for accessing private GitHub repositories (if required).

GITHUB_USERNAME: GitHub username for the repository.

#Workflow Overview

The pipeline is triggered on every push to the main branch and performs the following steps:

Checkout Code:

Pulls the latest code from the repository.

**Configure AWS Credentials:**
Authenticates to AWS using the provided secrets.

**Login to ECR:**
Logs into the specified Amazon ECR repository.

**Build and Push Docker Image:**
Generates a unique image tag based on the commit hash and timestamp.

**Builds the Docker image and pushes it to ECR.**

**Update ECS Task Definition:**
Retrieves the current task definition.

**Updates the container image with the newly pushed Docker image.**

**Registers the updated task definition with ECS.**

**Update ECS Service:**

**Deploys the updated task definition to the specified ECS service.**

Folder Structure

.
├── .github/
│   └── workflows/
│       └── deploy.yml  # The GitHub Actions workflow file
├── Dockerfile           # Dockerfile for building the application image
└── README.md            # Documentation

How to Use

Clone this repository and add the required secrets to your GitHub repository.

Modify the workflow file (deploy.yml) if necessary to match your environment.

Commit and push your changes to the main branch. The workflow will automatically execute.

#Key Environment Variables

AWS_REGION: Specifies the AWS region.

ECR_REPOSITORY: ECR repository name where the Docker image will be pushed.

TASK_DEFINITION: Name of the ECS task definition to update.

ECS_SERVICE: ECS service to update with the new task definition.

ECS_CLUSTER: ECS cluster where the service is running.

#Notes

The Dockerfile must exist in the root of your repository and be correctly configured to build your application.

The pipeline relies on the AWS CLI and jq for JSON parsing and ECS updates.

#Troubleshooting

#Pipeline Fails at AWS Steps:

Ensure AWS credentials and permissions are correctly configured in GitHub Secrets.

Check the task definition, service, and cluster names for typos.

#Image Not Updating in ECS:

Verify that the CONTAINER_NAME matches the container name in the ECS task definition.

Ensure the updated task definition is successfully registered and deployed.

#Docker Build Errors:

Check the Dockerfile for issues.

Ensure all required build arguments are correctly passed.

#Resources

[GitHub Actions Documentation]
(https://docs.github.com/en/actions)

[AWS CLI Documentation]
(https://docs.aws.amazon.com/cli/latest/userguide/cli-chap-welcome.html)

[Amazon ECS Documentation]
(https://docs.aws.amazon.com/AmazonECS/latest/developerguide/Welcome.html)

