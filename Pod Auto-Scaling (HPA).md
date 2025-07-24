# Horizontal Pod Autoscaler (HPA) Setup on AWS EKS

This README provides a step-by-step guide on how to configure **Horizontal Pod Autoscaler (HPA)** in an **EKS (Elastic Kubernetes Service)** cluster based on **CPU** or **memory utilization**.

## Prerequisites

- **AWS Account** with permission to create and manage EKS resources.
- **kubectl** and **AWS CLI** installed and configured.
- A running **EKS cluster**.

## Steps to Configure HPA

### Step 1: Set Up EKS Cluster

1. **Create an EKS Cluster** via the AWS Console or using the AWS CLI.
2. **Configure kubectl** to use your EKS cluster:
   ```bash
   aws eks --region <region> update-kubeconfig --name <cluster-name>


3.Verify the cluster access:
kubectl get nodes

Step 2: Install Metrics Server
  1. Deploy Metrics Server in the EKS cluster:
     kubectl apply -f https://github.com/kubernetes-sigs/metrics-server/releases/download/v0.6.1/components.yaml


2.Verify the Metrics Server is working:

kubectl get deployment metrics-server -n kube-system
kubectl top nodes
kubectl top pods

Step 3: Deploy a Sample Application
  1.Deploy an Nginx application:
  kubectl create deployment nginx --image=nginx

  2.Expose the Nginx deployment via a service:
  kubectl expose deployment nginx --port=80 --type=LoadBalancer

Step 4: Configure Horizontal Pod Autoscaler (HPA)
    1.Create an HPA to scale based on CPU utilization:
    kubectl autoscale deployment nginx --cpu-percent=50 --min=1 --max=5
    2.Verify HPA is created:
    kubectl get hpa
Step 5: Simulate Load to Trigger Autoscaling
    1.Simulate CPU load using a busybox pod:
    kubectl run -i --tty load-generator --image=busybox /bin/sh

Inside the busybox container, run:
while true; do wget -q -O- http://nginx; done


2.Verify HPA scales the pods:

kubectl get hpa
kubectl get pods

Step 6: Clean Up
1.Delete the Nginx deployment and service:
kubectl delete deployment nginx
kubectl delete service nginx


2.Delete the HPA:
Delete the HPA:
