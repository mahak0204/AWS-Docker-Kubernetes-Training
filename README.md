# AWS-Docker-Kubernetes-Training
# Dockerization

1. **Dockerfile**:
   ```Dockerfile
   FROM node:14
   WORKDIR /usr/src/app
   COPY package*.json ./
   RUN npm install
   COPY . .
   EXPOSE 80
   CMD ["npm", "start"]
   ```
![image](https://github.com/user-attachments/assets/9d4fb970-3bd3-4d3c-ace1-9d4a844a0048)

![image](https://github.com/user-attachments/assets/841148da-ae2b-4101-90af-cc21135968d8)

![image](https://github.com/user-attachments/assets/d484b564-8904-40a1-9b9f-fb86f8ac6f1f)


2. **Build Image**:
   ```bash
   docker build -t my-simple-app .
   ```

   ![image](https://github.com/user-attachments/assets/696121b3-ab3e-48d6-a5a4-b7a16f9a6134)

   ![image](https://github.com/user-attachments/assets/ec0e967b-84bf-40c8-b22b-43dca55192fa)

   ![image](https://github.com/user-attachments/assets/3da71a32-2a34-4c4a-b501-a7270748b168)



4. **Run the Docker Container:**
   ```bash
   docker run --name my-container -d my-simple-app
   ```

   ![image](https://github.com/user-attachments/assets/ef5aaf3e-b760-4adc-b374-bf4ff4d0c4ee)

   

## My Lambda Function and SDK with CodePipeline

## Table of Contents

- [Setup Development Environment](#setup-development-environment)
- [Write Lambda Function](#write-lambda-function)
- [Package Lambda Function](#package-lambda-function)
- [Source Code Repository](#source-code-repository)
- [AWS CLI and IAM Configuration](#aws-cli-and-iam-configuration)
- [Deploy Lambda Function](#deploy-lambda-function)
- [CI/CD Pipeline](#cicd-pipeline)
- [Final Checks](#final-checks)

## Setup Development Environment

1. **Install Python:** Ensure Python is installed. Check with:
   ```bash
   python --version
   ```

2. **Install Git:** Install Git from [git-scm.com](https://git-scm.com/).

3. **Set Up Virtual Machine (VM):** Ensure your VM has internet access and necessary permissions. Install Python and Git on the VM.

4. **Create Project Directory:**
   ```bash
   mkdir my_lambda_project
   cd my_lambda_project
   ```

5. **Set Up Virtual Environment:**
   ```bash
   python -m venv venv
   source venv/bin/activate  # On Windows use: venv\Scripts\activate
   ```

## Write Lambda Function

1. **Create Python Script:** In your project directory, create `my_lambda_function.py`:
   ```python
   def lambda_handler(event, context):
       return {
           'statusCode': 200,
           'body': 'Hello from Lambda!'
       }
   ```

   ![image](https://github.com/user-attachments/assets/a3ce703d-539c-405d-a0da-4e35b0d76835)


2. **Create `requirements.txt`:** List dependencies:
   ```
   boto3==1.17.11
   ```
   ![image](https://github.com/user-attachments/assets/cf3d0b76-ef22-4d4b-a58b-269e19e7b8c7)


## Package Lambda Function

1. **Install Dependencies:**
   ```bash
   pip install -r requirements.txt -t ./package
   ```
![image](https://github.com/user-attachments/assets/26d5f905-dfd8-4c92-8fc0-098b6e4b9a1d)

2. **Copy Function Code:**
   ```bash
   cp my_lambda_function.py ./package
   ```
   ![image](https://github.com/user-attachments/assets/46547c08-6d22-4798-a24a-d48efce8d6a9)


3. **Create ZIP File:**
   ```bash
   cd package
   zip -r ../my_lambda_function.zip .
   cd ..
   ```

## Source Code Repository

1. **Create Git Repository:**
   ```bash
   git init
   ```

2. **Add and Commit Files:**
   ```bash
   git add .
   git commit -m "Initial commit"
   ```

3. **Push to Remote Repository:**
   ```bash
   git remote add origin <your-repo-url>
   git push -u origin main
   ```

## AWS CLI and IAM Configuration

1. **Install AWS CLI:** Download from [aws.amazon.com/cli](https://aws.amazon.com/cli).

2. **Configure AWS CLI:**
   ```bash
   aws configure
   ```

3. **Create IAM User:** Create a user with programmatic access and attach `AWSLambda_FullAccess`.

![image](https://github.com/user-attachments/assets/993d6afc-11e0-42e7-be19-cf41195ce679)


## Deploy Lambda Function

1. **Create Lambda Function:**
   ```bash
   aws lambda create-function --function-name my_lambda_function \
     --zip-file fileb://my_lambda_function.zip --handler my_lambda_function.lambda_handler \
     --runtime python3.x --role arn:aws:iam::your-account-id:role/your-lambda-execution-role
   ```
   ![image](https://github.com/user-attachments/assets/b8730bc6-4112-4b88-998e-91fd8736ab6b)


2. **Update Function for Changes:**
   ```bash
   aws lambda update-function-code --function-name my_lambda_function --zip-file fileb://my_lambda_function.zip
   ```
   ![image](https://github.com/user-attachments/assets/f71c1ac6-3343-473a-ae84-ba808c9f2704)

   ![image](https://github.com/user-attachments/assets/fbfbcc6d-680f-49eb-8b81-d7c9857b5182)


## CI/CD Pipeline

1. **Choose CI/CD Tool:** Use AWS CodePipeline or Jenkins.

2. **Configure Pipeline:** Define stages for source, build, and deploy.

3. **Deploy with Pipeline:** Ensure it pulls from the repository and deploys to AWS Lambda.

# Kubernetes with Minikube:

## Prerequisites

- **Minikube:** Ensure Minikube is installed on your system. You can download it from [Minikube's official site](https://minikube.sigs.k8s.io/docs/start/).
- **kubectl:** Install kubectl, the Kubernetes command-line tool, by following the instructions at [Kubernetes official site](https://kubernetes.io/docs/tasks/tools/).
- **Virtualization:** Verify that your virtual machine (VM) supports virtualization and is enabled in the BIOS. Minikube supports various drivers like VirtualBox, Hyper-V, and Docker.

## Start Minikube

1. **Initialize Minikube:**
   - Open a terminal on your VM and start Minikube with the appropriate driver:
     ```bash
     minikube start --driver=virtualbox
     ```
   - Replace `virtualbox` with the driver suitable for your setup.

2. **Check Minikube Status:**
   - Verify that Minikube is running correctly:
     ```bash
     minikube status
     ```

## Use kubectl to Manage Kubernetes

1. **Check Cluster Information:**
   - Ensure kubectl is configured to use your Minikube cluster:
     ```bash
     kubectl cluster-info
     ```

2. **Deploy a Simple Pod:**
   - Create a pod running an NGINX container:
     ```bash
     kubectl run my-nginx --image=nginx --port=80
     ```

3. **List Active Pods:**
   - Check the status of your pods:
     ```bash
     kubectl get pods
     ```

4. **Expose the Pod:**
   - Make the NGINX pod accessible from outside the cluster:
     ```bash
     kubectl expose pod my-nginx --type=NodePort
     ```

5. **Access the NGINX Pod:**
   - Retrieve the URL to access the NGINX service:
     ```bash
     minikube service my-nginx --url
     ```

6. **Delete the Pod:**
   - Remove the NGINX pod when finished:
     ```bash
     kubectl delete pod my-nginx
     ```

## Stop Minikube

1. **Stop Minikube:**
   - Stop the Minikube cluster when you're done:
     ```bash
     minikube stop
     ```

2. **Delete Minikube Cluster:**
   - Delete the Minikube cluster to free up resources:
     ```bash
     minikube delete
     ```

## Additional Resources

- **Kubernetes Dashboard:**
  - Access Minikube's built-in dashboard to visualize and manage resources:
    ```bash
    minikube dashboard
    ```

# My Simple EKS Application
## Prerequisites

- [AWS CLI](https://aws.amazon.com/cli/)
- [kubectl](https://kubernetes.io/docs/tasks/tools/install-kubectl/)
- [eksctl](https://eksctl.io/)
- [Docker](https://www.docker.com/products/docker-desktop)
- AWS account with necessary permissions

## Project Setup

1. **Clone the repository**:
   ```bash
   git clone https://github.com/your-username/my-simple-eks-app.git
   cd my-simple-eks-app
   ```

2. **Node.js Application**:
   - `app.js`: Basic Express server setup.
   - `package.json`: Includes Express dependency.

## Dockerization

1. **Dockerfile**:
   ```Dockerfile
   FROM node:14
   WORKDIR /usr/src/app
   COPY package*.json ./
   RUN npm install
   COPY . .
   EXPOSE 80
   CMD ["npm", "start"]
   ```

2. **Build Image**:
   ```bash
   docker build -t my-simple-app .
   ```

## Push to AWS ECR

1. **Create Repository**: In AWS ECR, named `my-simple-app`.
2. **Authenticate & Push**:
   ```bash
   aws ecr get-login-password --region your-region | docker login --username AWS --password-stdin your-account-id.dkr.ecr.your-region.amazonaws.com
   docker tag my-simple-app:latest your-account-id.dkr.ecr.your-region.amazonaws.com/my-simple-app:latest
   docker push your-account-id.dkr.ecr.your-region.amazonaws.com/my-simple-app:latest
   ```

   ![image](https://github.com/user-attachments/assets/93db9ca9-c52a-488b-8146-ea4a03bd507a)


## Deploy on EKS

1. **Create Cluster**:
   ```bash
   eksctl create cluster --name my-simple-cluster --region your-region --nodes 3
   ```

2. **Configure kubectl**:
   ```bash
   aws eks update-kubeconfig --region your-region --name my-simple-cluster
   ```

3. **Deploy Application**:
   - `deployment.yaml` for app deployment.
   - `service.yaml` to expose via LoadBalancer.

   ```bash
   kubectl apply -f deployment.yaml
   kubectl apply -f service.yaml
   ```

## Monitor and Access

- **Monitor**: `kubectl get pods` and `kubectl get services`
- **Access**: Use external IP from `kubectl get service my-simple-app-service`

# Terraform 
## Prerequisites

Ensure you have the following before starting:

- **Terraform**: Install from the [Terraform website](https://www.terraform.io/downloads.html).
- **AWS Account**: Sign up at [AWS](https://aws.amazon.com/).
- **AWS CLI**: Install and configure by following the [AWS CLI User Guide](https://docs.aws.amazon.com/cli/latest/userguide/cli-configure-quickstart.html).

## AWS Configuration

Configure your AWS credentials:

```bash
aws configure
# Enter your AWS Access Key, Secret Key, region, and output format
```

## Quick Start Guide

1. **Initialize Terraform**:

   ```bash
   terraform init
   ```

2. **Format and Validate Configuration**:

   ```bash
   terraform fmt
   terraform validate
   ```

3. **Plan the Deployment**:

   ```bash
   terraform plan
   ```
   ![image](https://github.com/user-attachments/assets/7f9e1988-aca6-4922-8be1-472547315e13)


4. **Apply the Configuration**:

   ```bash
   terraform apply
   ```
   ![image](https://github.com/user-attachments/assets/d22d1a20-6ae2-4bbc-8186-7c49407b13b8)
   ![image](https://github.com/user-attachments/assets/66dcbe91-972b-4c29-b11c-5fa3c2bead9f)



6. **Verify Deployment**: Check your AWS console to ensure resources are created.

## Destroying the Infrastructure

Clean up all resources:

```bash
terraform destroy
```
![image](https://github.com/user-attachments/assets/db5fccfb-72e3-40e5-a11c-c658bec94395)
![image](https://github.com/user-attachments/assets/1d85f57c-7793-4c69-aadd-e7d7ac3ac42c)

