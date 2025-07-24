# CI/CD Pipeline with Jenkins, AWS ECR, EKS, and APISIX

This README provides detailed steps for building a CI/CD pipeline using Jenkins, integrating with AWS Elastic Container Registry (ECR) for image storage, deploying containers to Amazon Elastic Kubernetes Service (EKS), and exposing the service using the APISIX API Gateway.

## Prerequisites

1. **Jenkins** installed on your machine or server.
2. **AWS Account** with permissions to create resources in ECR, EKS, and manage IAM roles.
3. **Docker** installed on your Jenkins machine.
4. **kubectl** installed for Kubernetes management.
5. **Helm** installed for installing APISIX on Kubernetes.

## Step 1: Install Jenkins

1. **Install Jenkins** on a server. Follow the [official Jenkins installation guide](https://www.jenkins.io/doc/book/installing/) for your OS (Ubuntu or other).
   
2. **Start Jenkins**:
   ```bash
   sudo systemctl start jenkins

3. Access Jenkins by going to http://localhost:8080 in your browser.
4. Unlock Jenkins by finding the Admin password:
sudo cat /var/lib/jenkins/secrets/initialAdminPassword
5.Install necessary plugins:
Docker plugin
Amazon ECR plugin
Kubernetes plugin
Git plugin


Step 2: Set Up AWS ECR (Elastic Container Registry)

1.Create an ECR repository:
Go to the AWS console and navigate to Elastic Container Registry (ECR).
Click Create repository.
Name your repository (e.g., my-app-repository) and click Create.

2.Configure AWS CLI on Jenkins:

Install the AWS CLI on your Jenkins server if not already installed:
sudo apt install awscli

Configure AWS CLI:
aws configure

Step 3: Set Up Jenkins Pipeline for Docker Build and Push to AWS ECR
1.Create a Jenkins Pipeline Job:
In Jenkins, click New Item > Pipeline.
Name the job (e.g., MyApp-CI-CD).
Under Pipeline, select Pipeline script.

2.Create Jenkinsfile in Your Git Repository:
Create a Jenkinsfile in your repository with the following contents:
pipeline {
    agent any

    environment {
        // AWS configurations
        AWS_REGION = 'us-east-1'
        ECR_REPO = 'my-app-repository'
        IMAGE_NAME = 'my-app-image'
    }

    stages {
        stage('Checkout') {
            steps {
                // Checkout the code from Git
                git 'https://github.com/your-repo/your-app.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    // Build Docker image
                    sh "docker build -t ${ECR_REPO}:${BUILD_NUMBER} ."
                }
            }
        }

        stage('Login to AWS ECR') {
            steps {
                script {
                    // Log into ECR using AWS CLI
                    sh "aws ecr get-login-password --region ${AWS_REGION} | docker login --username AWS --password-stdin ${AWS_ACCOUNT_ID}.dkr.ecr.${AWS_REGION}.amazonaws.com"
                }
            }
        }

        stage('Push to ECR') {
            steps {
                script {
                    // Tag the Docker image with the ECR repository URL
                    sh "docker tag ${ECR_REPO}:${BUILD_NUMBER} ${AWS_ACCOUNT_ID}.dkr.ecr.${AWS_REGION}.amazonaws.com/${ECR_REPO}:${BUILD_NUMBER}"
                    // Push the image to ECR
                    sh "docker push ${AWS_ACCOUNT_ID}.dkr.ecr.${AWS_REGION}.amazonaws.com/${ECR_REPO}:${BUILD_NUMBER}"
                }
            }
        }
    }
}

3.Create AWS Credentials in Jenkins:
Go to Manage Jenkins > Manage Credentials > Global > Add Credentials.
Select AWS Credentials and add your Access Key ID and Secret Access Key.

4.Run the Pipeline:
Trigger the Jenkins job by pushing code to your Git repository.
Jenkins will build the Docker image and push it to ECR.

Step 4: Set Up Amazon EKS (Elastic Kubernetes Service)

1.Create an EKS Cluster:
Go to EKS in the AWS Console and click Create Cluster.
Select a name for your cluster, configure VPC and other settings, and click Create Cluster.
Wait for the cluster to be created.

2.Install kubectl (if not already installed):
sudo apt install kubectl
3.Install AWS CLI (if not already installed):
sudo apt install awscli

4.Configure kubectl for EKS:
aws eks --region <region> update-kubeconfig --name my-app-cluster

Step 5: Deploy the Docker Image to EKS
1.Create Deployment YAML File (deployment.yaml):
apiVersion: apps/v1
kind: Deployment
metadata:
  name: my-app
spec:
  replicas: 2
  selector:
    matchLabels:
      app: my-app
  template:
    metadata:
      labels:
        app: my-app
    spec:
      containers:
      - name: my-app-container
        image: <aws_account_id>.dkr.ecr.<region>.amazonaws.com/my-app-repository:<build_number>
        ports:
        - containerPort: 80

2.Apply Deployment to EKS:
kubectl apply -f deployment.yaml

Step 6: Install and Configure APISIX API Gateway

1.Install Helm:
Install Helm to manage Kubernetes applications:
curl https://raw.githubusercontent.com/helm/helm/master/scripts/get-helm-3 | bash

2.Install APISIX:
Add APISIX's Helm repository:
helm repo add apisix https://charts.apisix.apache.org
helm repo update

Install APISIX in the apisix namespace:
kubectl create namespace apisix
helm install apisix apisix/apisix --namespace apisix

Step 7: Expose Your Application Using APISIX
1.Create Service for Application (service.yaml):
apiVersion: v1
kind: Service
metadata:
  name: my-app-service
spec:
  selector:
    app: my-app
  ports:
    - protocol: TCP
      port: 80
      targetPort: 80

2.Apply the Service:
kubectl apply -f service.yaml

3.Create APISIX Route (apisix-route.yaml):
apiVersion: apisix.apache.org/v2
kind: ApisixRoute
metadata:
  name: my-app-route
spec:
  http:
    - name: my-app-route
      match:
        hosts:
          - "*"
        paths:
          - "/my-app"
      backends:
        - serviceName: my-app-service
          servicePort: 80

4.Apply the Route:
kubectl apply -f apisix-route.yaml

5.Test the Exposed API:
Now you can access your application via the APISIX Gateway.

