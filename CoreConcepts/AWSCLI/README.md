# AWS CLI: A Beginner's Guide

The **AWS Command Line Interface (CLI)** is a unified tool to manage your AWS services. With just one tool to download and configure, you can control multiple AWS services from the command line and automate them through scripts.

## **1. Installation and Configuration**

### **Installing AWS CLI**

- **For macOS:**
  ```bash
  brew install awscli
  ```
- **For Linux:**
  ```bash
  sudo apt-get update
  sudo apt-get install awscli -y
  ```
- **For Windows:**
  - Download the installer from the [AWS CLI Download Page](https://aws.amazon.com/cli/).
  - Run the installer.

### **Configuring AWS CLI**

After installing, you need to configure the CLI with your AWS credentials.

```bash
aws configure
```

- **AWS Access Key ID**: [Your Access Key ID]
- **AWS Secret Access Key**: [Your Secret Access Key]
- **Default region name**: [e.g., `us-east-1`]
- **Default output format**: `json` (other options: `text`, `table`)

These credentials can be obtained from the IAM console in your AWS account.

## **2. Common AWS CLI Commands**

### **2.1 EC2 (Elastic Compute Cloud)**

**1. Launch an EC2 Instance:**

```bash
aws ec2 run-instances --image-id ami-0abcdef1234567890 --count 1 --instance-type t2.micro --key-name MyKeyPair --security-group-ids sg-0123456789abcdef0 --subnet-id subnet-6e7f829e
```

- **--image-id**: AMI ID to use for the instance.
- **--count**: Number of instances to launch.
- **--instance-type**: Type of instance (e.g., `t2.micro`).
- **--key-name**: Name of the key pair.
- **--security-group-ids**: Security group IDs for the instance.
- **--subnet-id**: Subnet ID for the instance.

**2. Describe EC2 Instances:**

```bash
aws ec2 describe-instances
```

This command lists all the EC2 instances in your account.

**3. Stop/Start an EC2 Instance:**

- **Stop:**
  ```bash
  aws ec2 stop-instances --instance-ids i-1234567890abcdef0
  ```
- **Start:**
  ```bash
  aws ec2 start-instances --instance-ids i-1234567890abcdef0
  ```

**4. Terminate an EC2 Instance:**

```bash
aws ec2 terminate-instances --instance-ids i-1234567890abcdef0
```

This command permanently deletes an EC2 instance.

### **2.2 S3 (Simple Storage Service)**

**1. Create an S3 Bucket:**

```bash
aws s3api create-bucket --bucket my-bucket --region us-east-1
```

**2. List S3 Buckets:**

```bash
aws s3 ls
```

This command lists all the S3 buckets in your account.

**3. Upload a File to S3:**

```bash
aws s3 cp myfile.txt s3://my-bucket/
```

This command uploads a file (`myfile.txt`) to the specified S3 bucket.

**4. Download a File from S3:**

```bash
aws s3 cp s3://my-bucket/myfile.txt ./myfile.txt
```

This command downloads the file (`myfile.txt`) from the S3 bucket to your local machine.

**5. Delete an S3 Bucket:**

```bash
aws s3 rb s3://my-bucket --force
```

This command deletes the specified S3 bucket and all its contents.

### **2.3 IAM (Identity and Access Management)**

**1. Create a New IAM User:**

```bash
aws iam create-user --user-name MyNewUser
```

**2. Add User to Group:**

```bash
aws iam add-user-to-group --user-name MyNewUser --group-name MyGroup
```

**3. Create Access Key for User:**

```bash
aws iam create-access-key --user-name MyNewUser
```

This command generates an access key for the user, which is needed for programmatic access to AWS services.

**4. Attach a Policy to a User:**

```bash
aws iam attach-user-policy --user-name MyNewUser --policy-arn arn:aws:iam::aws:policy/AmazonS3FullAccess
```

This command attaches a managed policy to the user.

### **2.4 RDS (Relational Database Service)**

**1. Create a New RDS Instance:**

```bash
aws rds create-db-instance --db-instance-identifier mydbinstance --db-instance-class db.t2.micro --engine mysql --master-username admin --master-user-password password --allocated-storage 20
```

- **--db-instance-identifier**: Unique name for the DB instance.
- **--db-instance-class**: Type of instance (e.g., `db.t2.micro`).
- **--engine**: Database engine to use (e.g., `mysql`).
- **--master-username**: Master username for the database.
- **--master-user-password**: Master password for the database.
- **--allocated-storage**: Size of the database in GB.

**2. Describe RDS Instances:**

```bash
aws rds describe-db-instances
```

This command lists all the RDS instances in your account.

**3. Delete an RDS Instance:**

```bash
aws rds delete-db-instance --db-instance-identifier mydbinstance --skip-final-snapshot
```

This command deletes the specified RDS instance. The `--skip-final-snapshot` option is used to skip the creation of a final snapshot before deletion.

### **2.5 CloudFormation**

**1. Create a CloudFormation Stack:**

```bash
aws cloudformation create-stack --stack-name mystack --template-body file://template.json --parameters ParameterKey=KeyName,ParameterValue=MyKeyPair
```

- **--stack-name**: Name of the stack.
- **--template-body**: Path to the CloudFormation template file.
- **--parameters**: List of parameter key-value pairs for the stack.

**2. Describe a CloudFormation Stack:**

```bash
aws cloudformation describe-stacks --stack-name mystack
```

This command shows the details of the specified CloudFormation stack.

**3. Delete a CloudFormation Stack:**

```bash
aws cloudformation delete-stack --stack-name mystack
```

This command deletes the specified CloudFormation stack.

### **2.6 CodeDeploy**

**1. Create a CodeDeploy Application:**

```bash
aws deploy create-application --application-name MyApp --compute-platform Server
```

**2. Create a Deployment Group:**

```bash
aws deploy create-deployment-group --application-name MyApp --deployment-group-name MyDeploymentGroup --service-role-arn arn:aws:iam::123456789012:role/CodeDeployDemoRole --deployment-config-name CodeDeployDefault.AllAtOnce --ec2-tag-filters Key=Name,Value=MyEC2Instance,Type=KEY_AND_VALUE
```

**3. Deploy an Application:**

```bash
aws deploy create-deployment --application-name MyApp --deployment-config-name CodeDeployDefault.AllAtOnce --deployment-group-name MyDeploymentGroup --s3-location bucket=mybucket,key=MyApp.zip,bundleType=zip
```

## **3. Useful Tips**

- **Use IAM Roles**: Instead of hardcoding your AWS credentials in scripts, use IAM roles attached to your EC2 instances or AWS services to securely access resources.
  
- **Automation with Scripts**: Combine AWS CLI commands into shell scripts for repetitive tasks like launching environments, creating backups, or deploying applications.

- **AWS CLI Profiles**: If you manage multiple AWS accounts, you can use profiles to switch between them.
  ```bash
  aws configure --profile myprofile
  ```
  Then, use it like this:
  ```bash
  aws s3 ls --profile myprofile
  ```

- **Help Command**: If you're unsure about a command or its options, you can always use the `help` command.
  ```bash
  aws ec2 help
  ```

The AWS CLI is a powerful tool that, once mastered, allows you to efficiently manage your AWS environment. Whether you're managing EC2 instances, deploying applications, or automating workflows, the CLI is an essential skill for any AWS user.

Citations:
[1] https://www.youtube.com/watch?v=9oYd5KQM8AQ
[2] https://www.youtube.com/watch?v=PWAnY-w1SGQ
[3] https://aws.amazon.com/cli/
[4] https://www.qa.com/resources/blog/how-to-use-aws-cli/
[5] https://itgix.com/blog/aws-cli-tutorial/
[6] https://www.freecodecamp.org/news/aws-cli-tutorial-install-configure-understand-resource-environment/
[7] https://www.geeksforgeeks.org/what-is-an-application-load-balancer/
[8] https://www.kerno.io/learn/how-to-deploy-an-application-load-balancer-alb