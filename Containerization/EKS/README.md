# AWS EKS (Elastic Kubernetes Service)

## Introduction

Amazon Elastic Kubernetes Service (EKS) is a fully managed Kubernetes service that makes it easy to run Kubernetes on AWS without needing to install, operate, or maintain your own Kubernetes control plane or nodes. EKS handles the management and scaling of your Kubernetes clusters, allowing you to focus on deploying and managing applications.

This guide will walk you through the basics of EKS, including key concepts, how to set up an EKS cluster, and how to deploy and manage applications on it.

## Table of Contents

1. [Key Concepts](#key-concepts)
2. [Prerequisites](#prerequisites)
3. [Setting Up EKS](#setting-up-eks)
4. [Deploying Applications](#deploying-applications)
5. [Managing and Scaling](#managing-and-scaling)
6. [Monitoring and Logging](#monitoring-and-logging)
7. [Cleaning Up](#cleaning-up)
8. [Best Practices](#best-practices)
9. [Conclusion](#conclusion)

---

## Key Concepts

### 1. Cluster
An EKS cluster is a managed Kubernetes control plane that handles API requests, scheduling, and managing containerized applications.

### 2. Node Group
A set of Amazon EC2 instances that Kubernetes uses to run your application pods. EKS allows you to manage these nodes with managed node groups or self-managed nodes.

### 3. Pod
The smallest deployable unit in Kubernetes, a pod encapsulates one or more containers with shared storage/network and a specification for how to run them.

### 4. Service
A Kubernetes abstraction that defines a logical set of pods and a policy by which to access them. Services are used to expose your application to external or internal consumers.

### 5. Deployment
A Kubernetes object that manages the deployment and scaling of a set of pods, ensuring the desired number of replicas are running at all times.

### 6. Helm
A package manager for Kubernetes that helps you define, install, and upgrade even the most complex Kubernetes applications.

## Prerequisites

- **AWS Account**: An active AWS account.
- **AWS CLI**: Installed and configured with your credentials.
- **kubectl**: The Kubernetes command-line tool for interacting with the cluster.
- **eksctl**: A CLI tool for creating and managing EKS clusters.
- **IAM Role**: Create an IAM role with the necessary permissions to manage EKS and related services.

## Setting Up EKS

### Step 1: Install Required Tools

Ensure that `aws-cli`, `kubectl`, and `eksctl` are installed on your local machine.

```bash
# Install AWS CLI
pip install awscli --upgrade --user

# Install kubectl
curl -o kubectl https://amazon-eks.s3.us-west-2.amazonaws.com/latest/2023-06-01/bin/linux/amd64/kubectl
chmod +x ./kubectl
sudo mv ./kubectl /usr/local/bin/

# Install eksctl
curl --silent --location "https://github.com/weaveworks/eksctl/releases/latest/download/eksctl_$(uname -s)_amd64.tar.gz" | tar xz -C /tmp
sudo mv /tmp/eksctl /usr/local/bin
```

### Step 2: Create an EKS Cluster

Use `eksctl` to create an EKS cluster:

```bash
eksctl create cluster \
  --name my-cluster \
  --region us-west-2 \
  --nodegroup-name my-nodes \
  --node-type t3.medium \
  --nodes 3 \
  --nodes-min 1 \
  --nodes-max 4 \
  --managed
```

This command creates a cluster with three managed EC2 instances as nodes. Adjust the node type and number based on your needs.

### Step 3: Configure kubectl

After the cluster is created, configure `kubectl` to interact with your new cluster:

```bash
aws eks --region us-west-2 update-kubeconfig --name my-cluster
```

Verify the configuration by running:

```bash
kubectl get nodes
```

You should see the list of nodes in your cluster.

## Deploying Applications

### Step 1: Create a Kubernetes Deployment

Create a deployment that runs your application. For example, deploy an NGINX web server:

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: nginx-deployment
spec:
  replicas: 2
  selector:
    matchLabels:
      app: nginx
  template:
    metadata:
      labels:
        app: nginx
    spec:
      containers:
      - name: nginx
        image: nginx:latest
        ports:
        - containerPort: 80
```

Save this to a file named `nginx-deployment.yaml` and apply it:

```bash
kubectl apply -f nginx-deployment.yaml
```

### Step 2: Expose the Deployment

Create a Kubernetes service to expose the NGINX deployment:

```bash
kubectl expose deployment nginx-deployment --type=LoadBalancer --name=nginx-service
```

This will create a LoadBalancer service that automatically provisions an ELB (Elastic Load Balancer) in AWS to expose your application.

### Step 3: Verify Deployment

Check the status of your deployment and service:

```bash
kubectl get deployments
kubectl get services
```

Once the LoadBalancer is provisioned, you'll see an external IP address that you can use to access your application.

## Managing and Scaling

### Scaling Your Application

You can scale the number of pods in your deployment:

```bash
kubectl scale deployment nginx-deployment --replicas=5
```

### Rolling Updates

Kubernetes supports rolling updates to update your application without downtime. For example, update the image in your deployment:

```bash
kubectl set image deployment/nginx-deployment nginx=nginx:1.19.0
```

Kubernetes will gradually replace the old pods with new ones running the updated image.

## Monitoring and Logging

### Monitoring

EKS integrates with CloudWatch for monitoring. You can collect and visualize metrics from your Kubernetes cluster using CloudWatch Container Insights.

- **Enable Container Insights**: Enable Container Insights via the EKS console or CLI for detailed monitoring.

### Logging

To view logs from your pods:

```bash
kubectl logs <pod-name>
```

For more extensive logging, consider deploying a logging stack like EFK (Elasticsearch, Fluentd, Kibana) or use CloudWatch Logs.

## Cleaning Up

To avoid incurring unnecessary costs, clean up resources:

### Delete the EKS Cluster

```bash
eksctl delete cluster --name my-cluster
```

This command deletes the cluster, associated node groups, and all related resources.

### Delete Associated AWS Resources

Manually delete any remaining resources in your AWS account, such as VPCs, subnets, and security groups created by EKS.

## Best Practices

- **Use Managed Node Groups** for easier management and automatic upgrades of worker nodes.
- **Enable Auto Scaling** to automatically scale your node groups based on demand.
- **Secure Your Cluster**: Implement IAM roles for service accounts (IRSA) to secure access to AWS services.
- **Monitor Costs**: Regularly review your AWS billing dashboard to monitor EKS-related costs.
- **Backup Configurations**: Use tools like `kubectx` and `kubens` to manage and backup your cluster configurations.

## Conclusion

AWS EKS simplifies the management of Kubernetes at scale, enabling you to deploy and manage containerized applications without worrying about the underlying infrastructure. By following this guide, you can set up and manage an EKS cluster, deploy applications, and monitor your Kubernetes environment.

For more detailed information, refer to the [official AWS EKS documentation](https://docs.aws.amazon.com/eks/latest/userguide/what-is-eks.html).