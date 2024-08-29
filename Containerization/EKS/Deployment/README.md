# Deploying a React-Flask App with PostgreSQL on AWS EKS

## Introduction

This guide provides step-by-step instructions to deploy a full-stack React-Flask application with a PostgreSQL database on AWS Elastic Kubernetes Service (EKS). The process includes containerizing the application, setting up an EKS cluster, deploying the application, and managing the PostgreSQL database.

## Table of Contents

1. [Prerequisites](#prerequisites)
2. [Architecture Overview](#architecture-overview)
3. [Setting Up the Environment](#setting-up-the-environment)
4. [Containerizing the Application](#containerizing-the-application)
5. [Setting Up EKS](#setting-up-eks)
6. [Deploying PostgreSQL on EKS](#deploying-postgresql-on-eks)
7. [Deploying the Flask API](#deploying-the-flask-api)
8. [Deploying the React Frontend](#deploying-the-react-frontend)
9. [Configuring Ingress and Load Balancing](#configuring-ingress-and-load-balancing)
10. [Monitoring and Scaling](#monitoring-and-scaling)
11. [Cleaning Up](#cleaning-up)
12. [Best Practices](#best-practices)
13. [Conclusion](#conclusion)

---

## Prerequisites

Before starting, ensure you have the following:

- **AWS Account**: An active AWS account.
- **AWS CLI**: Installed and configured with your credentials.
- **kubectl**: Installed for managing Kubernetes clusters.
- **eksctl**: Installed for creating and managing EKS clusters.
- **Docker**: Installed for containerizing the application.
- **IAM Role**: With necessary permissions to manage EKS and related services.
- **kubectx and kubens**: (Optional) Installed for easier Kubernetes context and namespace management.

## Architecture Overview

This deployment will involve the following components:

1. **React Frontend**: A single-page application built with React.
2. **Flask API**: A backend API built with Flask.
3. **PostgreSQL Database**: A relational database for storing application data.
4. **AWS EKS**: A managed Kubernetes service for orchestrating containerized applications.

## Setting Up the Environment

### Step 1: Configure AWS CLI and IAM Role

Ensure your AWS CLI is configured:

```bash
aws configure
```

Set up an IAM role with permissions to manage EKS:

```bash
eksctl create iamidentitymapping --cluster my-cluster --arn arn:aws:iam::123456789012:role/EKSRole --group system:masters --username admin
```

### Step 2: Install Necessary Tools

Install `kubectl`, `eksctl`, and Docker if not already installed:

```bash
# Example for installing kubectl
curl -o kubectl https://amazon-eks.s3.us-west-2.amazonaws.com/latest/2023-06-01/bin/linux/amd64/kubectl
chmod +x ./kubectl
sudo mv ./kubectl /usr/local/bin/

# Install eksctl
curl --silent --location "https://github.com/weaveworks/eksctl/releases/latest/download/eksctl_$(uname -s)_amd64.tar.gz" | tar xz -C /tmp
sudo mv /tmp/eksctl /usr/local/bin
```

## Containerizing the Application

### Step 1: Dockerize Flask API

Create a `Dockerfile` for the Flask API:

```dockerfile
# Dockerfile for Flask API
FROM python:3.9-slim

WORKDIR /app

COPY requirements.txt requirements.txt
RUN pip install -r requirements.txt

COPY . .

CMD ["flask", "run", "--host=0.0.0.0"]
```

### Step 2: Dockerize React Frontend

Create a `Dockerfile` for the React app:

```dockerfile
# Dockerfile for React App
FROM node:14-alpine as build

WORKDIR /app
COPY package.json ./
RUN npm install
COPY . ./
RUN npm run build

FROM nginx:alpine
COPY --from=build /app/build /usr/share/nginx/html
```

### Step 3: Build and Push Docker Images

Build and push your Docker images to a registry like Amazon ECR:

```bash
# Build images
docker build -t my-flask-api ./backend
docker build -t my-react-app ./frontend

# Tag images
docker tag my-flask-api:latest <aws_account_id>.dkr.ecr.<region>.amazonaws.com/my-flask-api:latest
docker tag my-react-app:latest <aws_account_id>.dkr.ecr.<region>.amazonaws.com/my-react-app:latest

# Push images to ECR
docker push <aws_account_id>.dkr.ecr.<region>.amazonaws.com/my-flask-api:latest
docker push <aws_account_id>.dkr.ecr.<region>.amazonaws.com/my-react-app:latest
```

## Setting Up EKS

### Step 1: Create an EKS Cluster

Use `eksctl` to create an EKS cluster:

```bash
eksctl create cluster \
  --name my-cluster \
  --region us-west-2 \
  --nodegroup-name standard-workers \
  --node-type t3.medium \
  --nodes 3 \
  --nodes-min 1 \
  --nodes-max 4 \
  --managed
```

### Step 2: Configure kubectl

Configure `kubectl` to use your new cluster:

```bash
aws eks --region us-west-2 update-kubeconfig --name my-cluster
```

Verify the connection:

```bash
kubectl get nodes
```

## Deploying PostgreSQL on EKS

### Step 1: Create a PostgreSQL Deployment

Create a Kubernetes YAML file for PostgreSQL:

```yaml
apiVersion: v1
kind: PersistentVolumeClaim
metadata:
  name: postgres-pvc
spec:
  accessModes:
    - ReadWriteOnce
  resources:
    requests:
      storage: 5Gi

---
apiVersion: v1
kind: Service
metadata:
  name: postgres
spec:
  type: ClusterIP
  ports:
    - port: 5432
  selector:
    app: postgres

---
apiVersion: apps/v1
kind: Deployment
metadata:
  name: postgres
spec:
  replicas: 1
  selector:
    matchLabels:
      app: postgres
  template:
    metadata:
      labels:
        app: postgres
    spec:
      containers:
        - name: postgres
          image: postgres:13
          ports:
            - containerPort: 5432
          env:
            - name: POSTGRES_DB
              value: mydb
            - name: POSTGRES_USER
              value: user
            - name: POSTGRES_PASSWORD
              value: password
          volumeMounts:
            - mountPath: /var/lib/postgresql/data
              name: postgres-storage
      volumes:
        - name: postgres-storage
          persistentVolumeClaim:
            claimName: postgres-pvc
```

Deploy PostgreSQL:

```bash
kubectl apply -f postgres-deployment.yaml
```

### Step 2: Verify the Deployment

Check if PostgreSQL is running:

```bash
kubectl get pods
kubectl get svc postgres
```

## Deploying the Flask API

### Step 1: Create a Deployment for Flask API

Create a Kubernetes YAML file for the Flask API:

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: flask-api
spec:
  replicas: 2
  selector:
    matchLabels:
      app: flask-api
  template:
    metadata:
      labels:
        app: flask-api
    spec:
      containers:
        - name: flask-api
          image: <aws_account_id>.dkr.ecr.<region>.amazonaws.com/my-flask-api:latest
          ports:
            - containerPort: 5000
          env:
            - name: DATABASE_URL
              value: "postgresql://user:password@postgres:5432/mydb"

---
apiVersion: v1
kind: Service
metadata:
  name: flask-api
spec:
  type: ClusterIP
  ports:
    - port: 5000
  selector:
    app: flask-api
```

Deploy the Flask API:

```bash
kubectl apply -f flask-api-deployment.yaml
```

### Step 2: Verify the Deployment

Check if the Flask API is running:

```bash
kubectl get pods
kubectl get svc flask-api
```

## Deploying the React Frontend

### Step 1: Create a Deployment for React

Create a Kubernetes YAML file for the React app:

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: react-app
spec:
  replicas: 2
  selector:
    matchLabels:
      app: react-app
  template:
    metadata:
      labels:
        app: react-app
    spec:
      containers:
        - name: react-app
          image: <aws_account_id>.dkr.ecr.<region>.amazonaws.com/my-react-app:latest
          ports:
            - containerPort: 80

---
apiVersion: v1
kind: Service
metadata:
  name: react-app
spec:
  type: LoadBalancer
  ports:
    - port: 80
  selector:
    app: react-app
```

Deploy the React app:

```bash
kubectl apply -f react-app-deployment.yaml
```

### Step 2: Verify the Deployment

Check if the React app is running:

```bash
kubectl get pods


kubectl get svc react-app
```

## Configuring Ingress and Load Balancing

### Step 1: Install an Ingress Controller

Install an NGINX Ingress Controller for routing:

```bash
kubectl apply -f https://raw.githubusercontent.com/kubernetes/ingress-nginx/main/deploy/static/provider/aws/deploy.yaml
```

### Step 2: Create an Ingress Resource

Define an ingress resource to route traffic:

```yaml
apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  name: app-ingress
  annotations:
    nginx.ingress.kubernetes.io/rewrite-target: /
spec:
  rules:
  - host: my-app.example.com
    http:
      paths:
      - path: /
        pathType: Prefix
        backend:
          service:
            name: react-app
            port:
              number: 80
      - path: /api
        pathType: Prefix
        backend:
          service:
            name: flask-api
            port:
              number: 5000
```

Deploy the ingress:

```bash
kubectl apply -f ingress.yaml
```

### Step 3: Verify Ingress

Check the ingress status:

```bash
kubectl get ingress
```

Configure your DNS to point to the external IP of the ingress.

## Monitoring and Scaling

### Step 1: Enable Monitoring with CloudWatch

Enable Container Insights for monitoring:

```bash
aws eks update-cluster-config \
    --name my-cluster \
    --region us-west-2 \
    --logging '{"clusterLogging":[{"types":["api","audit","authenticator"],"enabled":true}]}'
```

### Step 2: Autoscaling

Enable the Kubernetes Cluster Autoscaler to scale nodes based on demand:

```bash
kubectl apply -f https://github.com/kubernetes/autoscaler/releases/download/cluster-autoscaler-chart-1.0.1/cluster-autoscaler-chart.yaml
```

## Cleaning Up

### Step 1: Delete the EKS Cluster

To avoid incurring charges, delete the EKS cluster when you're done:

```bash
eksctl delete cluster --name my-cluster
```

### Step 2: Delete Associated Resources

Manually delete any leftover resources such as VPCs, security groups, or load balancers.

## Best Practices

- **Use Helm for Managing Deployments**: Helm simplifies deploying and managing Kubernetes applications.
- **Secure Connections**: Use SSL/TLS for securing connections between components.
- **Enable Auto Scaling**: Automatically scale your application based on demand.
- **Monitor Costs**: Regularly review your AWS billing to monitor costs associated with EKS and other resources.

## Conclusion

Deploying a React-Flask application with a PostgreSQL database on AWS EKS involves several steps, from setting up the EKS cluster to containerizing the application and managing deployments. Following this guide will help you set up and manage your application effectively in a scalable and secure manner.

For more detailed information, refer to the [official AWS EKS documentation](https://docs.aws.amazon.com/eks/latest/userguide/what-is-eks.html).