
# Kubernetes Demo Guide

## Overview
This demo will take you the hands-on demo on Deployments in Kubernetes.

## Prerequisites
- kubectl – A command line tool for working with Kubernetes clusters. For more information, see Installing or updating kubectl.
- eksctl – A command line tool for working with EKS clusters that automates many individual tasks. For more information, see Installing or updating.
- AWS CLI – 
A command line tool for working with AWS services, including Amazon EKS. For more information, see Installing, updating, and uninstalling the AWS CLI in the AWS Command Line Interface User Guide. After installing the AWS CLI, we recommend that you also configure it. For more information, see Quick configuration with aws configure in the AWS Command Line Interface User Guide.


check version:
kubectl version --version
eksctl version
aws --version

aws configure
AWS Access Key ID: *****
AWS Secret Access Key: *****
Default region name (e.g. us-east-1)
Default output format (e.g. json)


## Demo Structure

### Part 1: Understanding Containers
1. Docker basics
2. What are containers?
3. Container and images.
 
### Part 2: Introduction to Kubernete.
1. Why do we need Kubernetes?
2. Kubernetes architecture overview
3. Pods, Services, and Deployments


### Part 3: Hands-on Kubernetes deployment
1. Install cluster and create fargate profile
2. Deploy application inside pod
3. Create ingress resource and Integrate Identity provider and IAMOIDC provider
4. Create policy and role
5. Install helm, add and update.
6. Verify that the deployments are running
7. Final run to get a address.

## Getting Started

Follow the command in order:
1. eksctl create cluster --name demo-cluster --region us-east-1                           ###To installing the cluster

2. 	aws eks update -kubeconfig --name demo-cluster --region us-east-1               ###To download the kubeconfig file

3.   --cluster demo-cluster `
  --region us-east-1 `                            
  --name alb-sample-app `
  --namespace game-2048
                   
             OR 
    eksctl create fargateprofile --cluster demo-cluster --region us-east-1 --name alb-sample-app --namespace game-2048            ### To create a farget profile

    4. kubectl apply -f https://raw.githubusercontent.com/kubernetes-sigs/aws-load-balancer-controller/v2.5.4/docs/examples/2048/2048_full.yaml                             ### Take application(From official EKS Documentation)

    5. kubectl get ingress -n game-2048        ###  Create Ingress 
    kubectl get svc -n game-2048 

    6. eksctl utils associate-iam-oidc-provider --cluster demo-cluster –approve                ### Integrate Identity provider and IAMOIDC provider   

    7. curl https://raw.githubusercontent.com/kubernetes-sigs/aws-load-balancer-controller/v2.11.0/docs/install/iam_policy.json             ###  prerequisites for deploy ingress controller
To deploy the ingress controller in our case, we need to deploy ALB controller.

    8. aws --% iam create-policy --policy-name AWSLoadBalancerControllerIAMPolicy --policy-document file://iam_policy.json  
    ###  Create iam_policy  

    9. eksctl create iamserviceaccount --cluster=demo-cluster --namespace=kube-system --name=aws-load-balancer-controller --role-name AmazonEKSLoadBalancerControllerRole --attach-policy-arn=arn:aws:iam:: 180088310920:policy/AWSLoadBalancerControllerIAMPolicy --approve 
    ###  Create iam_role

    10.  https://github.com/helm/helm/releases     
    ###  Install helm

11. helm repo add eks https://aws.github.io/eks-charts
    ###  Add helm repo

12.  helm repo update
    ###  Update the repo

13.  helm install aws-load-balancer-controller eks/aws-load-balancer-controller `
  -n kube-system `
  --set clusterName=demo-cluster `
  --set serviceAccount.create=false `
  --set serviceAccount.name=aws-load-balancer-controller `
  --set region=us-east-1 `
  --set vpcId=vpc-0029c0a540319292b 
    ###  Install

  14. kubectl get deployment -n kube-system aws-load-balancer-controller   
    ###  Verify that the deployments are running

  15. kubectl get ingress -n game-2048 
    ###  Final run to get a address   
 


