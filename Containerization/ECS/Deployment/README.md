# AWS ECS with a React-Frontend and Flask-Backend Application

## Introduction

This README will guide you through deploying a simple web application using AWS Elastic Container Service (ECS). The application consists of a React frontend, a Flask backend, and an SQLite database. We will containerize these components using Docker, push the images to Amazon Elastic Container Registry (ECR), and deploy the application using ECS. 

### Prerequisites

1. **AWS Account**: Sign up at [aws.amazon.com](https://aws.amazon.com/).
2. **AWS CLI**: Installed and configured with your AWS credentials. Instructions can be found [here](https://docs.aws.amazon.com/cli/latest/userguide/install-cliv2.html).
3. **Docker**: Installed on your local machine. Instructions can be found [here](https://docs.docker.com/get-docker/).
4. **Basic Knowledge**: Familiarity with React, Flask, Docker, and basic AWS services.

### Application Structure

1. **React**: A frontend application served on port 3000.
2. **Flask**: A backend API served on port 5000, connected to an SQLite database.
3. **SQLite**: A lightweight, file-based database for data storage.

### Steps Overview

1. **Prepare Your Application**:
   - Set up the React frontend.
   - Set up the Flask backend.
   - Set up the SQLite database.
   - Dockerize both frontend and backend.
2. **Set Up AWS Resources**:
   - Create an ECR repository for your Docker images.
   - Push your Docker images to ECR.
   - Create an ECS Cluster and Task Definitions.
   - Deploy your application on ECS using Fargate.
3. **Access and Test**:
   - Access the application.
   - Verify functionality.

---

## Step 1: Prepare Your Application

### 1.1. React Frontend

Create a simple React application. Below is an example using `create-react-app`.

```bash
npx create-react-app react-frontend
cd react-frontend
```

Modify `src/App.js` to communicate with the Flask backend:

```javascript
import React, { useState, useEffect } from 'react';

function App() {
  const [data, setData] = useState(null);

  useEffect(() => {
    fetch('/api/data')
      .then(response => response.json())
      .then(data => setData(data.message));
  }, []);

  return (
    <div>
      <h1>React Frontend</h1>
      <p>{data ? data : 'Loading...'}</p>
    </div>
  );
}

export default App;
```

### 1.2. Flask Backend

Create a simple Flask application.

1. **Directory Structure:**

   ```plaintext
   flask-backend/
   ├── app.py
   ├── requirements.txt
   └── data.db
   ```

2. **app.py:**

   ```python
   from flask import Flask, jsonify, g
   import sqlite3

   app = Flask(__name__)
   DATABASE = 'data.db'

   def get_db():
       db = getattr(g, '_database', None)
       if db is None:
           db = g._database = sqlite3.connect(DATABASE)
       return db

   @app.teardown_appcontext
   def close_connection(exception):
       db = getattr(g, '_database', None)
       if db is not None:
           db.close()

   @app.route('/api/data', methods=['GET'])
   def get_data():
       cur = get_db().cursor()
       cur.execute("SELECT message FROM data LIMIT 1")
       message = cur.fetchone()[0]
       return jsonify({"message": message})

   if __name__ == '__main__':
       app.run(host='0.0.0.0', port=5000)
   ```

3. **requirements.txt:**

   ```plaintext
   Flask==2.0.2
   ```

4. **SQLite Database Setup:**

   ```bash
   sqlite3 data.db "CREATE TABLE data (id INTEGER PRIMARY KEY, message TEXT);"
   sqlite3 data.db "INSERT INTO data (message) VALUES ('Hello from Flask!');"
   ```

### 1.3. Dockerization

#### Dockerfile for React

Create a `Dockerfile` in the `react-frontend` directory:

```Dockerfile
# Build stage
FROM node:16 AS build
WORKDIR /app
COPY package.json ./
RUN npm install
COPY . ./
RUN npm run build

# Production stage
FROM nginx:alpine
COPY --from=build /app/build /usr/share/nginx/html
EXPOSE 80
CMD ["nginx", "-g", "daemon off;"]
```

#### Dockerfile for Flask

Create a `Dockerfile` in the `flask-backend` directory:

```Dockerfile
FROM python:3.9-slim
WORKDIR /app
COPY . /app
RUN pip install --no-cache-dir -r requirements.txt
EXPOSE 5000
CMD ["python", "app.py"]
```

### 1.4. Build and Test Locally

1. **Build Docker Images:**

   ```bash
   # React
   docker build -t react-frontend ./react-frontend
   # Flask
   docker build -t flask-backend ./flask-backend
   ```

2. **Run Docker Containers:**

   ```bash
   docker run -d -p 3000:80 react-frontend
   docker run -d -p 5000:5000 flask-backend
   ```

3. **Test Locally:**
   - Access `http://localhost:3000` for the React frontend.
   - Verify that the React app can communicate with the Flask backend.

---

## Step 2: Set Up AWS Resources

### 2.1. Create ECR Repositories

1. **Create Repositories:**

   ```bash
   aws ecr create-repository --repository-name react-frontend
   aws ecr create-repository --repository-name flask-backend
   ```

2. **Authenticate Docker with ECR:**

   ```bash
   aws ecr get-login-password --region <your-region> | docker login --username AWS --password-stdin <account-id>.dkr.ecr.<region>.amazonaws.com
   ```

3. **Tag and Push Images:**

   ```bash
   # Tagging
   docker tag react-frontend:latest <account-id>.dkr.ecr.<region>.amazonaws.com/react-frontend:latest
   docker tag flask-backend:latest <account-id>.dkr.ecr.<region>.amazonaws.com/flask-backend:latest

   # Pushing
   docker push <account-id>.dkr.ecr.<region>.amazonaws.com/react-frontend:latest
   docker push <account-id>.dkr.ecr.<region>.amazonaws.com/flask-backend:latest
   ```

### 2.2. Create an ECS Cluster

1. **Create ECS Cluster:**

   ```bash
   aws ecs create-cluster --cluster-name react-flask-cluster
   ```

2. **Create Task Definitions:**

   Create two task definitions, one for React and one for Flask.

   - **React Task Definition:**

     ```json
     {
       "family": "react-frontend",
       "networkMode": "awsvpc",
       "containerDefinitions": [
         {
           "name": "react-frontend",
           "image": "<account-id>.dkr.ecr.<region>.amazonaws.com/react-frontend:latest",
           "essential": true,
           "portMappings": [
             {
               "containerPort": 80,
               "hostPort": 80
             }
           ]
         }
       ],
       "requiresCompatibilities": [
         "FARGATE"
       ],
       "cpu": "256",
       "memory": "512",
       "executionRoleArn": "arn:aws:iam::<account-id>:role/ecsTaskExecutionRole"
     }
     ```

   - **Flask Task Definition:**

     ```json
     {
       "family": "flask-backend",
       "networkMode": "awsvpc",
       "containerDefinitions": [
         {
           "name": "flask-backend",
           "image": "<account-id>.dkr.ecr.<region>.amazonaws.com/flask-backend:latest",
           "essential": true,
           "portMappings": [
             {
               "containerPort": 5000,
               "hostPort": 5000
             }
           ]
         }
       ],
       "requiresCompatibilities": [
         "FARGATE"
       ],
       "cpu": "256",
       "memory": "512",
       "executionRoleArn": "arn:aws:iam::<account-id>:role/ecsTaskExecutionRole"
     }
     ```

   - **Register Task Definitions:**

     ```bash
     aws ecs register-task-definition --cli-input-json file://react-task-definition.json
     aws ecs register-task-definition --cli-input-json file://flask-task-definition.json
     ```

3. **Create ECS Services:**

   - **React Service:**

     ```bash
     aws ecs create-service \
       --cluster react-flask-cluster \
       --service-name react-frontend-service \
       --task-definition react-frontend \
       --desired-count 1 \
       --launch-type FARGATE \
       --network-configuration "awsvpcConfiguration={subnets=[<your-subnet-ids>],securityGroups=[<your-security-group-id>],assignPublicIp=ENABLED}"
     ```

   - **Flask Service:**

     ```

bash
     aws ecs create-service \
       --cluster react-flask-cluster \
       --service-name flask-backend-service \
       --task-definition flask-backend \
       --desired-count 1 \
       --launch-type FARGATE \
       --network-configuration "awsvpcConfiguration={subnets=[<your-subnet-ids>],securityGroups=[<your-security-group-id>],assignPublicIp=ENABLED}"
     ```

---

## Step 3: Access and Test

1. **Obtain Public IPs:**

   - Use the ECS Console or CLI to get the public IPs of the running tasks.

2. **Access the Application:**

   - Navigate to the public IP of the React service in your browser. The frontend should load and successfully fetch data from the Flask backend.

3. **Test Application Functionality:**

   - Verify that the Flask API returns the expected data, confirming successful integration between the frontend and backend.

---

## Conclusion

You've successfully deployed a React-Frontend and Flask-Backend application with an SQLite database on AWS ECS using Fargate. ECS manages the infrastructure, allowing you to focus on your application logic.

For further enhancements:
- Consider using an RDS instance instead of SQLite for a more robust database solution.
- Implement CI/CD pipelines with AWS CodePipeline for automated deployments.
- Use Amazon Route 53 for DNS management and route traffic to your ECS services.

### References
- [AWS ECS Documentation](https://docs.aws.amazon.com/AmazonECS/latest/developerguide/Welcome.html)
- [Docker Documentation](https://docs.docker.com/)
- [React Documentation](https://reactjs.org/)
- [Flask Documentation](https://flask.palletsprojects.com/)