# AWS ECS (Elastic Container Service)

## Introduction

Amazon Elastic Container Service (ECS) is a fully managed container orchestration service that makes it easy to run, stop, and manage Docker containers on a cluster. ECS eliminates the need to install and operate your own container orchestration software, manage and scale a cluster of virtual machines, or schedule containers on those VMs.

This guide will walk you through the basics of ECS, including key concepts, how to set up an ECS cluster, and how to deploy and manage applications.

## Table of Contents

1. [Key Concepts](#key-concepts)
2. [Prerequisites](#prerequisites)
3. [Setting Up ECS](#setting-up-ecs)
4. [Creating a Task Definition](#creating-a-task-definition)
5. [Running a Task](#running-a-task)
6. [Creating a Service](#creating-a-service)
7. [Monitoring and Scaling](#monitoring-and-scaling)
8. [Cleaning Up](#cleaning-up)
9. [Best Practices](#best-practices)
10. [Conclusion](#conclusion)

---

## Key Concepts

### 1. Cluster
A logical grouping of EC2 instances or Fargate tasks where you can run your containerized applications. Each ECS cluster can contain multiple services and tasks.

### 2. Task Definition
A blueprint for your application, describing one or more containers to run, including container images, CPU/memory requirements, networking, and IAM roles.

### 3. Task
An instantiation of a Task Definition that runs on an ECS cluster. A task can contain multiple containers.

### 4. Service
Manages and maintains a specified number of tasks simultaneously in a cluster. If a task fails, the service scheduler launches another instance of the task.

### 5. Container Instance
An EC2 instance running the ECS container agent. It's part of an ECS cluster and can host multiple tasks.

### 6. Fargate
A serverless compute engine for containers that removes the need to manage servers. With Fargate, you only need to define the resources required for your tasks.

## Prerequisites

- **AWS Account**: An active AWS account.
- **AWS CLI**: Installed and configured with your credentials.
- **Docker**: Installed on your local machine.
- **IAM Role**: Create an IAM role with the required permissions to manage ECS and related services.

## Setting Up ECS

### Step 1: Create an ECS Cluster

1. Log in to the [AWS Management Console](https://aws.amazon.com/console/).
2. Navigate to the ECS service.
3. Click on "Clusters" and then "Create Cluster."
4. Choose a cluster template:
   - **Networking only** (Fargate): For serverless containers.
   - **EC2 Linux + Networking**: If you want to manage EC2 instances.
5. Configure the cluster settings and click "Create."

### Step 2: Set Up Networking (VPC, Subnets)

If you don't already have a VPC, you'll need to create one. ECS requires a VPC with subnets for the tasks to run in.

1. Navigate to the VPC service in AWS.
2. Create a new VPC with at least two public subnets.
3. Ensure your VPC has an Internet Gateway attached.

## Creating a Task Definition

1. Go to the ECS Console.
2. Click on "Task Definitions" and "Create new Task Definition."
3. Choose either **Fargate** or **EC2** as the launch type.
4. Define the task and container configurations:
   - **Container Name**: A unique name for your container.
   - **Container Image**: The Docker image to use (e.g., `nginx:latest`).
   - **Memory and CPU**: Allocate resources for the container.
   - **Port Mappings**: Define the ports to be exposed.
5. Configure other settings like environment variables, volumes, and IAM roles.
6. Click "Create."

## Running a Task

1. Navigate to your ECS cluster.
2. Click on "Run new task."
3. Select the task definition and cluster you created.
4. Choose the launch type (Fargate or EC2).
5. Specify the number of tasks to run.
6. Click "Run Task."

## Creating a Service

To ensure your application is always running, create a service:

1. Go to the ECS cluster where you want to create the service.
2. Click on "Create" under the "Services" tab.
3. Select your task definition and the number of desired tasks.
4. Configure the service, such as load balancing and auto-scaling (optional).
5. Click "Create Service."

## Monitoring and Scaling

### Monitoring

- **CloudWatch**: ECS integrates with CloudWatch to monitor metrics like CPU/memory utilization and log container output.
- **ECS Console**: Use the ECS Console to view task and service status, logs, and events.

### Scaling

- **Manual Scaling**: Adjust the number of tasks in a service directly from the ECS Console.
- **Auto Scaling**: Set up auto-scaling policies to scale your tasks based on CloudWatch metrics.

## Cleaning Up

To avoid unnecessary costs, make sure to clean up resources after testing:

1. **Delete Tasks and Services**: Stop running tasks and delete services from the ECS Console.
2. **Delete Task Definitions**: Deregister or delete any unused task definitions.
3. **Delete ECS Cluster**: If no longer needed, delete the ECS cluster.
4. **Clean Up Networking**: Delete any VPC, subnets, and security groups you created.

## Best Practices

- **Use Fargate** for simpler and serverless container management.
- **Use IAM roles** for task execution to grant least-privilege access.
- **Monitor Costs**: Regularly review ECS-related costs in the AWS Billing dashboard.
- **Version Control Task Definitions**: Increment task definition revisions to keep track of changes.

## Conclusion

AWS ECS is a powerful service for managing containerized applications at scale. By understanding the key concepts and following the steps outlined in this guide, you'll be able to set up and manage an ECS cluster, deploy applications, and monitor their performance.

For more detailed information, refer to the [official AWS ECS documentation](https://docs.aws.amazon.com/AmazonECS/latest/developerguide/Welcome.html).