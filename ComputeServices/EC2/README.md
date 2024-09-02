
# **Amazon EC2 Overview**

Amazon EC2 is like renting a virtual computer in the cloud. Imagine you have a business, and instead of buying physical servers, you can "rent" virtual servers from Amazon. These servers, called **instances**, are fully customizable, allowing you to choose the amount of processing power, memory, and storage that you need. The best part? You only pay for the computing power you actually use, just like paying for utilities like electricity or water.

## **Key Concepts in EC2**

### **Instances**

**Instances** are your virtual servers in the cloud. Think of them as the workhorses running your applications, websites, or services. When you start an instance, you choose an **Amazon Machine Image (AMI)**, which is like choosing a blueprint for your server. This blueprint includes the operating system and any software you need.

#### **Instance Types**

Different tasks require different types of workhorses. For example:

- **General Purpose (e.g., `t2.micro`, `m5.large`)**: Imagine you run a small online shop that doesn’t get a ton of traffic. A **General Purpose** instance is like a reliable, all-around car—it handles daily tasks like browsing the web, processing orders, and running your website efficiently without being overkill.

- **Compute Optimized (e.g., `c5.large`)**: Suppose you run a video encoding service that processes thousands of videos simultaneously. You need lots of computing power, like a sports car for speed. **Compute Optimized** instances give you the horsepower to handle these heavy computational tasks quickly.

- **Memory Optimized (e.g., `r5.large`)**: Let’s say you’re running a database for a large social media site that needs to store and process huge amounts of user data in real-time. **Memory Optimized** instances are like big trucks with lots of storage space, perfect for handling large datasets efficiently.

- **Storage Optimized (e.g., `i3.large`)**: If you’re managing a big data analytics service where quick access to large amounts of data is critical, **Storage Optimized** instances are like high-speed trains—they deliver data rapidly, making them ideal for tasks that require high I/O performance.

- **Accelerated Computing (e.g., `p3.large`)**: For machine learning models that require immense processing power, **Accelerated Computing** instances are like supercomputers with GPUs, giving your applications the muscle they need for intensive tasks like graphics processing or scientific computations.

### **Amazon Machine Images (AMIs)**
# Why Do You Need an Amazon Machine Image (AMI)? - A Simple Explanation

## Introduction

**AMIs** are the blueprints for your instances. Imagine you’re opening a new restaurant. An AMI is like the recipe book and layout plan—you decide on the ingredients (software) and the kitchen setup (operating system). When you're working with cloud services like Amazon Web Services (AWS), you often need to launch multiple virtual servers (EC2 instances) that all have the same setup. This setup includes the operating system, installed software, and specific configurations. Instead of manually configuring each server from scratch every time, you can use an Amazon Machine Image (AMI).

An AMI is essentially a template that allows you to quickly and consistently create new servers with the exact same setup.

There are three types:
- **Public AMIs**: Like a community recipe shared by chefs around the world, these are provided by AWS or the community.
- **Private AMIs**: Think of these as your secret family recipe that only you or your organization uses.
- **AWS Marketplace AMIs**: These are like premium recipes you can buy from a professional chef, tailored for specific needs.

## Why Do You Need an AMI?

### Example Scenario: Deploying a Web Application

Let's say you’re running a web application for your business. This application runs on a server that needs:
- **Ubuntu Linux** as the operating system.
- **Apache** as the web server.
- **MySQL** as the database.
- **PHP** for server-side scripting.
- Your specific web application files.

### The Traditional Approach (Without an AMI)

1. **Manual Setup**: Every time you need to launch a new server (EC2 instance), you start from a basic OS installation. You manually install Apache, MySQL, PHP, and upload your application files.
2. **Time-Consuming**: This process takes time and requires you to repeat the same steps every time.
3. **Prone to Errors**: Each manual setup increases the risk of missing a step or misconfiguring something, leading to inconsistencies across servers.

### The AMI Approach

1. **Create an AMI**: After manually setting up the first server with everything you need (Ubuntu, Apache, MySQL, PHP, and your application), you create an AMI from this instance.
2. **Launch Instances from AMI**: Now, whenever you need a new server, you can simply launch it from this AMI. The new server will have the exact same setup as the original one.
3. **Save Time and Effort**: Using an AMI eliminates the need to repeat the setup process. You can launch multiple servers within minutes, all configured identically.
4. **Consistency**: Every server launched from the AMI will be identical, ensuring consistent performance and behavior across all your servers.

### Why Is It Needed?

- **Scalability**: When your web application grows, you may need to handle more traffic by adding more servers. With an AMI, scaling up is easy and quick.
- **Disaster Recovery**: If a server fails, you can quickly spin up a new one from your AMI, reducing downtime.
- **Version Control**: You can create different AMIs for different versions of your application, making it easy to roll back to a previous version if needed.

An AMI is crucial for efficiently managing and scaling your infrastructure on AWS. It saves time, reduces errors, and ensures that all your servers are consistently configured. By creating an AMI from a well-configured server, you can easily and quickly deploy identical servers whenever you need them, making your operations smoother and more reliable.

### **Elastic Block Store (EBS)**

EBS is like a hard drive for your instance, but in the cloud. It stores all your important data, and just like a USB drive, you can attach and detach it from different instances as needed. The best part? Your data stays safe even if you turn off your instance.

#### **Types of EBS Volumes**

- **General Purpose SSD (gp3)**: Think of this as your everyday, reliable SSD drive—great for most tasks where you need a balance between performance and cost.
- **Provisioned IOPS SSD (io1, io2)**: Imagine you’re running a high-frequency trading platform where milliseconds matter. This is your high-performance SSD, delivering speed where you need it most.
- **Magnetic (st1, sc1)**: If you need to store large amounts of archived data that you don’t access often, these are like your old-school, but reliable, hard drives.

### **Security Groups**

**Security Groups** are your instance’s bodyguards. They control who can and cannot access your instance. Imagine you’re throwing a private event at your restaurant; the security group is like the bouncer at the door who checks the guest list. By default, no one can enter unless you specifically allow them, ensuring your data is safe.

### **Key Pairs**

**Key Pairs** are like the keys to your virtual house. When you launch an instance, you get a pair of keys (public and private). You keep the private key secret and use it to unlock your instance when you need to connect to it securely via SSH (for Linux) or RDP (for Windows).

### **Elastic IP Addresses**

**Elastic IP Addresses** are like having a permanent business address. Even if you move your restaurant (or change the server hosting your application), your customers (users) can always find you at the same address. You can quickly reassign your Elastic IP to another instance if something goes wrong with the current one.

## **Launching an EC2 Instance**

Let’s say you’re opening a new online shop. Here’s how you’d "launch" your virtual store in the cloud:

1. **Login to AWS Management Console**: Enter the AWS dashboard, like unlocking the door to your new store.
2. **Choose an AMI**: Select the blueprint for your store. Is it a Linux-based shop or a Windows-based one? Pick the AMI that suits your needs.
3. **Choose an Instance Type**: Decide how big or powerful your store needs to be. Is it a small shop (`t2.micro`) or a large warehouse (`m5.large`)?
4. **Configure Instance Details**: Set up the neighborhood (network), decide if you want to expand automatically (auto-scaling), monitor your shop (CloudWatch), and assign roles (IAM) to your employees (applications).
5. **Add Storage**: Choose how much shelf space (EBS) you need for your products (data).
6. **Add Tags**: Label your shop for easy identification. For example, “Summer Sale” or “New Product Launch.”
7. **Configure Security Group**: Set up the bouncer at the door (firewall rules) to ensure only the right customers can enter.
8. **Review and Launch**: Double-check everything and then open your shop (launch the instance).
9. **Connect to Your Instance**: Once your shop is open, walk inside using your private key (connect using SSH or RDP).

## **Pricing Models**

Amazon EC2 offers flexible pricing options, much like different rental agreements:

- **On-Demand Instances**: Ideal for pop-up shops or seasonal businesses. Pay for what you use by the hour or second, with no long-term commitment. Perfect for when you expect spikes in traffic, like Black Friday.
  
- **Reserved Instances**: Imagine signing a lease for a year or three. You get a discount for committing to a longer term. Best for steady, predictable traffic—like a popular café that sees consistent daily customers.

- **Spot Instances**: These are like bidding for last-minute event spaces. You can get a great deal if the space (computing capacity) isn’t already booked, but you might need to move out quickly if someone else offers more. Perfect for flexible projects, like rendering video during off-peak hours.

- **Savings Plans**: This is like a subscription service where you commit to using a certain amount of compute power over a year or three. You get the best rates for steady, ongoing usage, like a chain of coffee shops planning to open new branches over time.



## Scaling with EC2

# Auto Scaling and Elastic Load Balancing (ELB) in Amazon EC2

## 1. Auto Scaling

Auto Scaling in Amazon EC2 helps you automatically adjust the number of instances in your environment based on demand. This ensures you have the right amount of computing power at all times without manually adding or removing instances.

### Real-Life Example: An E-commerce Website

Imagine you run an e-commerce website, and your traffic varies significantly throughout the day. During peak hours, like lunchtime or evenings, you might have thousands of users shopping simultaneously, leading to high demand on your servers. Conversely, during late-night hours, traffic might be minimal.

#### Without Auto Scaling:
- **Fixed Instances**: If you manually launch a fixed number of EC2 instances to handle peak traffic, those instances will be underutilized during off-peak hours, leading to unnecessary costs.
- **Insufficient Instances**: Conversely, if you only launch a few instances to save on costs, your website might become slow or unresponsive during peak traffic, leading to a poor user experience and potentially lost sales.

#### With Auto Scaling:
- **Scaling Policy**: You define a scaling policy based on metrics such as CPU utilization or the number of active sessions.
- **Automatic Adjustment**: As traffic increases and CPU utilization goes up, Auto Scaling automatically launches additional EC2 instances to handle the load. As traffic decreases, Auto Scaling terminates the extra instances, reducing your costs.

#### Example Scenario:
- **Morning**: 3 EC2 instances running, handling regular traffic.
- **Afternoon (Lunchtime Rush)**: Traffic spikes, and CPU utilization reaches 80%. Auto Scaling launches 5 additional instances to maintain performance.
- **Evening (After Work)**: Traffic is at its peak, with 10 instances running.
- **Night**: Traffic drops significantly; CPU utilization falls below 20%. Auto Scaling reduces the number of instances to 2, cutting costs.

By using Auto Scaling, you ensure that your website remains responsive and can handle fluctuations in traffic without manual intervention. You only pay for the resources you actually use, optimizing both performance and cost.

## 2. Elastic Load Balancing (ELB)

Elastic Load Balancing (ELB) distributes incoming network traffic (or "load") across multiple EC2 instances. This improves fault tolerance and availability by ensuring that no single instance is overwhelmed and that your application remains accessible even if one or more instances fail.

### Real-Life Example: A Mobile Gaming Application

Imagine you're running a mobile gaming application that has thousands of players connected at any given time. To ensure a smooth gaming experience, you host the application on multiple EC2 instances.

#### Without Elastic Load Balancing:
- **Overloaded Instance**: If all traffic is directed to a single EC2 instance, that instance might become overloaded, leading to slow performance or crashes.
- **Single Point of Failure**: If the instance fails, your entire application goes down, leaving all players disconnected and frustrated.

#### With Elastic Load Balancing:
- **Traffic Distribution**: An ELB is placed in front of your EC2 instances. It acts as a traffic cop, distributing incoming traffic evenly across all available instances.
- **Failover Handling**: If one instance becomes overloaded or fails, the ELB automatically reroutes traffic to the remaining healthy instances.

#### Example Scenario:
- **Normal Operation**: You have 4 EC2 instances running. The ELB distributes traffic evenly across these instances, ensuring no single instance is overwhelmed.
- **Traffic Spike**: A new game update is released, leading to a sudden spike in players. The ELB continues to distribute traffic evenly, preventing any instance from becoming a bottleneck.
- **Instance Failure**: One of the instances fails due to a hardware issue. The ELB detects the failure and immediately routes all traffic to the remaining 3 instances, keeping the game online.

By using Elastic Load Balancing, you improve the availability and reliability of your application, ensuring that users have a seamless experience even during high traffic or hardware failures.

## Combining Auto Scaling and ELB

Auto Scaling and ELB are often used together to create a highly scalable and resilient architecture. Here's how they work in tandem:

### Real-Life Example: An Online Video Streaming Service

Imagine you're running an online video streaming service, similar to Netflix. During peak times, like weekends or new releases, the demand for streaming increases dramatically.

#### Scenario:
- **Normal Operation**: Your service is running with 5 EC2 instances behind an ELB.
- **Peak Hours**: As more users start streaming videos, the CPU utilization on your instances increases. Auto Scaling triggers and adds 5 more instances to handle the increased load. The ELB automatically starts distributing traffic to the new instances.
- **Instance Failure**: One of the instances fails during this high-demand period. The ELB stops sending traffic to the failed instance and distributes it to the remaining healthy instances.
- **Off-Peak Hours**: Late at night, traffic decreases. Auto Scaling reduces the number of instances back to 5, and the ELB adjusts accordingly.

By using both Auto Scaling and ELB, your video streaming service can efficiently handle varying traffic levels, maintain high availability, and minimize costs by only running the necessary number of instances.

## Conclusion

Auto Scaling ensures that your application has the right number of instances to handle current traffic, while Elastic Load Balancing evenly distributes incoming traffic across those instances to prevent overload and ensure availability. Together, they help you build a scalable, cost-effective, and resilient architecture that can adapt to changing demands and maintain a seamless user experience.

## Monitoring and Management

- **Amazon CloudWatch**: Monitors your EC2 instances and provides metrics like CPU utilization, disk reads/writes, network traffic, etc. You can set alarms and take automated actions when thresholds are breached.
- **AWS Systems Manager**: Provides operational data from multiple AWS services and allows you to automate tasks across your AWS resources.

## EC2 Best Practices

### Security
- Always use key pairs for secure access.
- Use Security Groups to restrict access to instances.
- Regularly patch your instances to keep them secure.
- Implement IAM roles for permissions instead of embedding credentials in instances.

### Cost Management
- Choose the right instance type for your workload.
- Use Reserved Instances or Savings Plans for predictable workloads.
- Monitor and review your instances and usage regularly.
- Use Auto Scaling to optimize resource usage.

### Availability and Fault Tolerance
- Distribute instances across multiple Availability Zones.
- Use Elastic Load Balancing to distribute traffic.
- Implement backups using EBS snapshots or AMIs.

## Advanced Topics (Optional)

- **Elastic File System (EFS)**: Provides scalable file storage that can be shared across multiple EC2 instances.
- **EC2 Instance Store**: Temporary storage located on disks that are physically attached to the host instance.
- **Elastic Network Interfaces (ENIs)**: Allows you to attach multiple network interfaces to your instances.
- **EC2 Placement Groups**: Allows you to influence the placement of your instances to meet specific networking or performance requirements.

## Summary

Amazon EC2 is a powerful and flexible service that enables you to run virtual servers in the cloud. By understanding the key components like instances, AMIs, EBS, and security groups, you can effectively launch and manage your applications in the cloud. Additionally, leveraging the various pricing models, scaling options, and best practices will help you optimize performance and costs.

This README with real-life examples should help you better understand how Amazon EC2 works and how it can be applied to various scenarios. Whether you're launching a new app, managing a website, or running a large-scale enterprise system, EC2 offers the flexibility and scalability you need to succeed in the cloud.

You can explore further by experimenting with launching instances, configuring security groups, and using CloudWatch for monitoring.


# Deploying a Flask Application on Amazon EC2 with Amazon RDS

This guide provides step-by-step instructions for deploying a Flask application on an Amazon EC2 instance, connecting it to an Amazon RDS MySQL database, and configuring Nginx to serve the application.

## Prerequisites

- **AWS Account**: Sign up for an AWS account if you don’t have one.
- **Basic Knowledge**: Familiarity with Flask, MySQL, and command-line operations.
- **Flask Application**: Ensure you have a Flask application ready (e.g., `app.py`).

## Steps

### Step 1: Set Up an RDS MySQL Database

1. **Sign in** to the [AWS Management Console](https://aws.amazon.com/console/).
2. **Navigate** to the RDS Dashboard.
3. **Create a New Database**:
   - Click **Create database**.
   - Choose **MySQL** as the database engine.
   - Select **Free tier** if eligible.
   - **Configure Database Settings**:
     - **DB Instance Identifier**: `flaskapp-db`
     - **Master Username**: `flaskuser`
     - **Master Password**: `flaskpassword`
   - **Connectivity**:
     - Ensure the instance is in the same VPC as your EC2 instance.
     - Enable **Public accessibility** to connect from your EC2 instance.
   - Click **Create database**.
   - **Note down the RDS endpoint**; you'll need this to connect your Flask app.

### Step 2: Launch an EC2 Instance

1. **Navigate** to the [EC2 Dashboard](https://aws.amazon.com/ec2/).
2. **Launch an Instance**:
   - Click **Launch Instance**.
   - **Choose an AMI**: Select **Ubuntu Server** or your preferred Linux distribution.
   - **Choose an Instance Type**: `t2.micro` is suitable for the free tier.
   - **Configure Instance Details**:
     - Ensure the instance is in the same VPC as your RDS instance.
   - **Add Storage**: The default 8 GB is sufficient.
   - **Configure Security Group**:
     - Allow **HTTP (port 80)** and **SSH (port 22)**.
     - Add a rule to allow **MySQL/Aurora (port 3306)** from your RDS instance’s security group.
   - Click **Review and Launch** and then **Launch** the instance.
   - **Download the key pair** to connect via SSH.

### Step 3: SSH into the EC2 Instance

1. **Connect** to your EC2 instance using SSH:
   ```bash
   ssh -i /path/to/your-key.pem ubuntu@your-ec2-public-ip
   ```

### Prerequisites

1. **Python**: Ensure you have Python installed on your system.
2. **MySQL**: Install and set up MySQL.
3. **Pip**: Install `pip`, Python's package manager, to install the necessary Python packages.

### Step 1: Set Up a Virtual Environment

First, let's create a virtual environment for your project to keep dependencies isolated.

```bash
mkdir flask_mysql_app
cd flask_mysql_app
python3 -m venv venv
source venv/bin/activate  # On Windows use `venv\Scripts\activate`
```

### Step 2: Install Required Packages

Install Flask and the necessary MySQL connector libraries.

```bash
pip install Flask Flask-MySQLdb
```

### Step 3: Set Up the MySQL Database

Log in to your MySQL server and create a database.

```sql
CREATE DATABASE flask_app_db;
CREATE USER 'flaskuser'@'localhost' IDENTIFIED BY 'flaskpassword';
GRANT ALL PRIVILEGES ON flask_app_db.* TO 'flaskuser'@'localhost';
FLUSH PRIVILEGES;
```

### Step 4: Create the Flask App

Create a file named `app.py` and add the following code:

```python
from flask import Flask, render_template, request, redirect
from flask_mysqldb import MySQL
import MySQLdb.cursors

app = Flask(__name__)

# Configure MySQL
app.config['MYSQL_HOST'] = 'localhost'
app.config['MYSQL_USER'] = 'flaskuser'
app.config['MYSQL_PASSWORD'] = 'flaskpassword'
app.config['MYSQL_DB'] = 'flask_app_db'

# Initialize MySQL
mysql = MySQL(app)

@app.route('/')
def index():
    cur = mysql.connection.cursor(MySQLdb.cursors.DictCursor)
    cur.execute("SELECT * FROM users")
    users = cur.fetchall()
    return render_template('index.html', users=users)

@app.route('/add', methods=['POST'])
def add_user():
    name = request.form['name']
    cur = mysql.connection.cursor()
    cur.execute("INSERT INTO users(name) VALUES (%s)", [name])
    mysql.connection.commit()
    return redirect('/')

if __name__ == '__main__':
    app.run(debug=True)
```

### Step 5: Create a Database Table

Before running the app, create a `users` table in your MySQL database.

```sql
USE flask_app_db;

CREATE TABLE users (
    id INT AUTO_INCREMENT PRIMARY KEY,
    name VARCHAR(100) NOT NULL
);
```

### Step 6: Create a Template

Create a `templates` directory and add a file named `index.html` inside it:

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>User List</title>
</head>
<body>
    <h1>Users</h1>
    <form method="POST" action="/add">
        <input type="text" name="name" placeholder="Enter a name">
        <button type="submit">Add User</button>
    </form>
    <ul>
        {% for user in users %}
            <li>{{ user.name }}</li>
        {% endfor %}
    </ul>
</body>
</html>
```

### Step 7: Run the Flask App

Finally, run your Flask app:

```bash
python app.py
```

Visit `http://127.0.0.1:5000/` in your browser. You should see a form to add users and a list of users from the database.

### Step 8: Set Up the Environment on EC2

If you're deploying on an EC2 instance, follow these additional steps:

1. **Update the Instance** and install necessary packages:
   ```bash
   sudo apt-get update
   sudo apt-get install python3-pip python3-dev libmysqlclient-dev nginx git
   ```

2. **Install and Set Up `virtualenv`**:
   ```bash
   pip3 install virtualenv
   ```

3. **Clone or Transfer Your Flask Application**:
   ```bash
   git clone https://github.com/your-repo/flask_mysql_app.git
   cd flask_mysql_app
   ```

4. **Set Up a Virtual Environment** and install dependencies:
   ```bash
   virtualenv venv
   source venv/bin/activate
   pip install -r requirements.txt
   ```

5. **Update `app.py`** to connect to the RDS database:
   ```python
   from flask import Flask, render_template, request, redirect
   from flask_mysqldb import MySQL
   import MySQLdb.cursors

   app = Flask(__name__)

   # Configure MySQL
   app.config['MYSQL_HOST'] = '<your-rds-endpoint>'
   app.config['MYSQL_USER'] = 'flaskuser'
   app.config['MYSQL_PASSWORD'] = 'flaskpassword'
   app.config['MYSQL_DB'] = 'flask_app_db'

   # Initialize MySQL
   mysql = MySQL(app)

   @app.route('/')
   def index():
       cur = mysql.connection.cursor(MySQLdb.cursors.DictCursor)
       cur.execute("SELECT * FROM users")
       users = cur.fetchall()
       return render_template('index.html', users=users)

   @app.route('/add', methods=['POST'])
   def add_user():
       name = request.form['name']
       cur = mysql.connection.cursor()
       cur.execute("INSERT INTO users(name) VALUES (%s)", [name])
       mysql.connection.commit()
       return redirect('/')

   if __name__ == '__main__':
       app.run(debug=True)
   ```

6. **Run the Flask App** to test:
   ```bash
   python app.py
   ```

### Step 6: Create and Configure the MySQL Database Table

1. **Log in** to your MySQL database:
   ```bash
   mysql -h <your-rds-endpoint> -u flaskuser -p
   ```
2. **Create the Database Table**:
   ```sql
   USE flask_app_db;

   CREATE TABLE users (
       id INT AUTO_INCREMENT PRIMARY KEY,
       name VARCHAR(100) NOT NULL
   );
   ```

### Summary

You've successfully deployed a Flask application on an EC2 instance with an RDS MySQL database. The app should be accessible via the public IP of your EC2 instance and will interact with the RDS database to store and retrieve user data.

## Prerequisites for Cloning and Setting Up the Flask Backend

Before cloning the repository and setting up your Flask backend, make sure you have your SSH key properly configured on your Ubuntu machine. Follow these steps to ensure that your SSH key is set up and added to your GitHub account.

### 1. Check Existing SSH Key

Verify that you have an SSH key pair by running:

```bash
ls -al ~/.ssh
```

You should see files like `id_ed25519` and `id_ed25519.pub`. If these files are not present, you need to generate a new SSH key.

### 2. Generate a New SSH Key (If Needed)

If you don’t have an SSH key pair, generate one using:

```bash
ssh-keygen -t ed25519 -C "your_email@example.com"
```

Follow the prompts to save the key (default location is `~/.ssh/id_ed25519`) and optionally set a passphrase.

### 3. Add Your SSH Key to the SSH Agent

Start the SSH agent and add your SSH key:

```bash
eval "$(ssh-agent -s)"
ssh-add ~/.ssh/id_ed25519
```

### 4. Add Your SSH Key to GitHub

1. **Copy Your Public Key:**

   ```bash
   cat ~/.ssh/id_ed25519.pub
   ```

   Copy the output of this command.

2. **Add the Key to GitHub:**

   - Go to [GitHub SSH and GPG keys settings](https://github.com/settings/keys).
   - Click on "New SSH key."
   - Paste your public key into the "Key" field and provide a title.
   - Click "Add SSH key."

### 5. Test Your SSH Connection

Ensure that your SSH connection to GitHub is working:

```bash
ssh -T git@github.com
```

You should see a message like:

```
Hi `username`! You've successfully authenticated, but GitHub does not provide shell access.
```

## Other Options 
### Option 1: Using a Personal Access Token (PAT)

1. **Generate a Personal Access Token**

   - Go to [GitHub Settings](https://github.com/settings/tokens).
   - Click on **"Generate new token"**.
   - Provide a name for the token, set expiration, and select the scopes (permissions) you need. For basic operations, you can select `repo`.
   - Click **"Generate token"**.
   - Copy the token. You won’t be able to see it again after you navigate away.

2. **Use the Personal Access Token**

   When prompted for a password during the `git clone` command, use the personal access token instead of your GitHub password.

   ```bash
   git clone https://github.com/Gilbertshikwe/EC2-flask-backend.git
   ```

   Enter your GitHub username and use the personal access token as the password.

### Option 2: Using SSH Keys

1. **Generate SSH Keys (if you don’t have one)**

   Generate a new SSH key pair if you don't already have one:

   ```bash
   ssh-keygen -t rsa -b 4096 -C "your_email@example.com"
   ```

   Press Enter to accept the default file location and optionally provide a passphrase.

2. **Add Your SSH Key to GitHub**

   - Copy your SSH public key to your clipboard:

     ```bash
     cat ~/.ssh/id_rsa.pub
     ```

     Copy the output of this command.

   - Go to [GitHub SSH and GPG keys settings](https://github.com/settings/keys).
   - Click on **"New SSH key"**.
   - Paste your key into the "Key" field and give it a title.
   - Click **"Add SSH key"**.

3. **Clone the Repository Using SSH**

   Change the URL to use SSH:

   ```bash
   git clone git@github.com:Gilbertshikwe/EC2-flask-backend.git
   ```

   This should not require a password if your SSH key is correctly set up.

### Summary

- **Personal Access Token**: Use a PAT as the password for HTTPS operations.
- **SSH Keys**: Configure SSH keys and use the SSH URL for cloning.

Both methods provide secure ways to authenticate with GitHub, so choose the one that best fits your workflow.

# Deploying Flask Application with uWSGI on EC2

This guide outlines the steps for deploying a Flask application using uWSGI instead of Gunicorn on an Amazon EC2 instance. It also covers how to set up Nginx as a reverse proxy.

## Prerequisites

- An Amazon EC2 instance running Ubuntu.
- A Flask application ready for deployment.
- Basic knowledge of AWS, Linux, and command-line operations.

## Setup Instructions

### Step 1: Create the WSGI Entry Point

1. **SSH into your EC2 instance**:

   ```bash
   ssh -i /path/to/your-key.pem ubuntu@your-ec2-public-ip
   ```

2. **Navigate to your project directory**:

   ```bash
   cd /home/ubuntu/EC2_Flask
   ```

3. **Create the WSGI entry point**:

   Create a file named `wsgi.py` in your project directory:

   ```bash
   nano ~/EC2_Flask/wsgi.py
   ```

   Add the following content to `wsgi.py`:

   ```python
   from app import app

   if __name__ == "__main__":
       app.run()
   ```

   Save and close the file.

### Step 2: Install and Configure uWSGI

1. **Install uWSGI**:

   Make sure you are in your virtual environment and install uWSGI:

   ```bash
   source venv/bin/activate
   pip install uwsgi
   ```

2. **Test uWSGI**:

   Test that uWSGI can serve your application by running:

   ```bash
   uwsgi --socket 0.0.0.0:5000 --protocol=http -w wsgi:app
   ```

   Visit `http://your_server_ip:5000` in your web browser to check if your application is running. Press `CTRL + C` to stop uWSGI.

3. **Create a uWSGI configuration file**:

   Create a file named `myproject.ini` in your project directory:

   ```bash
   nano ~/EC2_Flask/myproject.ini
   ```

   Add the following content to `myproject.ini`:

   ```ini
   [uwsgi]
   module = wsgi:app

   master = true
   processes = 5

   socket = /home/ubuntu/EC2_Flask/myproject.sock
   chmod-socket = 660
   vacuum = true

   die-on-term = true
   ```

   Save and close the file.

### Step 3: Create a systemd Unit File

1. **Create a systemd service file**:

   Create a file named `myproject.service` in `/etc/systemd/system/`:

   ```bash
   sudo nano /etc/systemd/system/myproject.service
   ```

   Add the following content to `myproject.service`:

   ```ini
   [Unit]
   Description=uWSGI instance to serve myproject
   After=network.target

   [Service]
   User=ubuntu
   Group=www-data
   WorkingDirectory=/home/ubuntu/EC2_Flask
   Environment="PATH=/home/ubuntu/EC2_Flask/venv/bin"
   ExecStart=/home/ubuntu/EC2_Flask/venv/bin/uwsgi --ini myproject.ini

   [Install]
   WantedBy=multi-user.target
   ```

   Save and close the file.

2. **Start and enable the uWSGI service**:

   ```bash
   sudo systemctl start myproject
   sudo systemctl enable myproject
   ```

   Check the status of the service:

   ```bash
   sudo systemctl status myproject
   ```

### Step 4: Configure Nginx

1. **Create an Nginx configuration file**:

   Create a file named `myproject` in `/etc/nginx/sites-available/`:

   ```bash
   sudo nano /etc/nginx/sites-available/myproject
   ```

   Add the following content to `myproject`:

   ```nginx
   server {
       listen 80;
       server_name your_domain www.your_domain;

       location / {
           include uwsgi_params;
           uwsgi_pass unix:/home/ubuntu/EC2_Flask/myproject.sock;
       }
   }
   ```

   Save and close the file.

2. **Enable the Nginx configuration**:

   Link the file to the `sites-enabled` directory:

   ```bash
   sudo ln -s /etc/nginx/sites-available/myproject /etc/nginx/sites-enabled
   ```

3. **Test the Nginx configuration**:

   ```bash
   sudo nginx -t
   ```

4. **Restart Nginx**:

   ```bash
   sudo systemctl restart nginx
   ```

5. **Adjust the firewall rules**:

   Remove the rule allowing port 5000 and allow Nginx:

   ```bash
   sudo ufw delete allow 5000
   sudo ufw allow 'Nginx Full'
   ```

### Step 5: Verify Your Setup

Visit `http://your_domain` in your web browser. You should see your Flask application running.

## Troubleshooting

- **uWSGI Service Issues**: Check the logs in `/var/log/syslog` or use `journalctl -u myproject`.
- **Nginx Issues**: Check Nginx logs in `/var/log/nginx/`.

---

This README provides a comprehensive guide for setting up your Flask application with uWSGI and Nginx on an EC2 instance. Adjust paths and usernames as needed for your specific setup.

### 8. (Optional) Secure with SSL/TLS

#### 8.1 Install Certbot and Obtain SSL Certificate
```bash
sudo apt install certbot python3-certbot-nginx -y
sudo certbot --nginx -d your_domain
```
### 9. Deleting the EC2 Instance After Use

Once you are done with your project or testing, it's important to terminate the EC2 instance to avoid unnecessary charges. 

1. **Navigate to the EC2 Dashboard:**
   - Go to the EC2 dashboard in the AWS Management Console.

2. **Select the Instance:**
   - In the **"Instances"** section, locate the instance you want to terminate.
   - Check the box next to the instance ID.

3. **Terminate the Instance:**
   - Click on the **"Instance State"** dropdown menu.
   - Select **"Terminate Instance"** from the options.

4. **Confirm Termination:**
   - Confirm the action when prompted. The instance will be terminated, and all associated resources will be released.

By terminating the instance, you prevent any further billing for that instance, helping manage costs effectively.

## Conclusion
You have successfully deployed a React and Flask application on an AWS EC2 instance! This setup allows your React frontend to interact with your Flask backend through API requests, all served from a single server instance.

This `README.md` provides clear instructions on how to set up and deploy a React and Flask application on an AWS EC2 instance, covering the essential steps from server setup to running the application.
