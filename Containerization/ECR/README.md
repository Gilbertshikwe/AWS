# AWS ECR (Elastic Container Registry)

## Introduction

Amazon Elastic Container Registry (ECR) is a fully managed Docker container registry that allows you to store, manage, and deploy Docker container images. ECR integrates with Amazon ECS, EKS, and AWS Lambda, simplifying the process of managing and deploying containerized applications.

This guide will teach you the basics of using ECR, including important concepts, common commands, and the steps to push and pull Docker images using ECR.

## Table of Contents

1. [Key Concepts](#key-concepts)
2. [Prerequisites](#prerequisites)
3. [Setting Up ECR](#setting-up-ecr)
4. [Building and Tagging Docker Images](#building-and-tagging-docker-images)
5. [Pushing Images to ECR](#pushing-images-to-ecr)
6. [Pulling Images from ECR](#pulling-images-from-ecr)
7. [Cleaning Up](#cleaning-up)
8. [Best Practices](#best-practices)
9. [Conclusion](#conclusion)

---

## Key Concepts

### 1. Repository
An ECR repository is where your Docker images are stored. Each repository can contain multiple versions (tags) of a Docker image.

### 2. Image
A Docker image is a lightweight, standalone, and executable software package that includes everything needed to run a piece of software, including the code, a runtime, libraries, environment variables, and configuration files.

### 3. Tag
A tag is a label applied to a Docker image within a repository. Tags help you manage and identify different versions of an image.

### 4. Authentication
Before interacting with an ECR repository, you must authenticate Docker to ECR using AWS credentials.

## Prerequisites

Before using ECR, ensure you have the following:

- **AWS Account**: An active AWS account.
- **AWS CLI**: Installed and configured with your credentials.
- **Docker**: Installed and running on your local machine.

## Setting Up ECR

### Step 1: Create an ECR Repository

Create an ECR repository to store your Docker images. You can create a repository using the AWS CLI:

```bash
aws ecr create-repository --repository-name my-repository --region us-west-2
```

This command will output details about the newly created repository, including its URI (Uniform Resource Identifier).

### Step 2: Authenticate Docker to ECR

ECR requires Docker to authenticate before pushing or pulling images. Use the following command to authenticate Docker to your ECR registry:

```bash
aws ecr get-login-password --region us-west-2 | docker login --username AWS --password-stdin <aws_account_id>.dkr.ecr.us-west-2.amazonaws.com
```

Replace `<aws_account_id>` with your AWS account ID. You should see a login success message.

## Building and Tagging Docker Images

### Step 1: Build a Docker Image

Build your Docker image using a `Dockerfile` in your project directory:

```bash
docker build -t my-image .
```

### Step 2: Tag the Docker Image

Tag the image with the ECR repository URI to prepare it for pushing:

```bash
docker tag my-image:latest <aws_account_id>.dkr.ecr.us-west-2.amazonaws.com/my-repository:latest
```

Replace `<aws_account_id>` with your AWS account ID and `my-repository` with your repository name.

## Pushing Images to ECR

### Step 1: Push the Docker Image

Push the tagged image to your ECR repository:

```bash
docker push <aws_account_id>.dkr.ecr.us-west-2.amazonaws.com/my-repository:latest
```

This command uploads the Docker image to the specified ECR repository. You can verify the image is uploaded by checking the repository in the AWS Management Console.

## Pulling Images from ECR

### Step 1: Authenticate Docker (If not already authenticated)

If you haven't already authenticated Docker, repeat the authentication step:

```bash
aws ecr get-login-password --region us-west-2 | docker login --username AWS --password-stdin <aws_account_id>.dkr.ecr.us-west-2.amazonaws.com
```

### Step 2: Pull the Docker Image

Pull the image from your ECR repository:

```bash
docker pull <aws_account_id>.dkr.ecr.us-west-2.amazonaws.com/my-repository:latest
```

After pulling, you can run the image locally or deploy it using services like ECS or EKS.

## Cleaning Up

### Step 1: Delete ECR Images

To delete an image from an ECR repository:

```bash
aws ecr batch-delete-image --repository-name my-repository --image-ids imageTag=latest
```

This command deletes the image with the specified tag.

### Step 2: Delete the ECR Repository

To delete an entire ECR repository:

```bash
aws ecr delete-repository --repository-name my-repository --region us-west-2 --force
```

The `--force` flag deletes the repository even if it contains images.

## Best Practices

- **Use Tags Wisely**: Implement a tagging strategy (e.g., `latest`, `v1.0.0`, `staging`, `production`) to manage different versions of your images.
- **Clean Up Old Images**: Regularly clean up old images to avoid unnecessary costs.
- **Enable Image Scanning**: ECR can automatically scan images for vulnerabilities. Enable image scanning when creating a repository.
- **Set Repository Policies**: Use repository policies to control access to your ECR repositories.

## Conclusion

AWS ECR simplifies the process of managing Docker images, especially when working with AWS services like ECS and EKS. By following this guide, you can create repositories, push and pull images, and implement best practices for efficient image management.

For more detailed information, refer to the [official AWS ECR documentation](https://docs.aws.amazon.com/AmazonECR/latest/userguide/what-is-ecr.html).

