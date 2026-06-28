# Lab 14 - Amazon EKS

## Objective

Understand how a learning workflow changes when moving from a local cluster to Amazon Elastic Kubernetes Service (EKS). This lab focuses on cluster creation, authentication, node groups, basic workload deployment, verification, and cleanup planning.

## Theory

Amazon EKS is a managed Kubernetes service where AWS operates the control plane, but the user still owns major parts of the environment such as worker nodes or Fargate configuration, VPC design, IAM integration, add-ons, cost control, and workload security. Compared with a local lab cluster, EKS adds cloud networking, cloud identity, billing, and regional availability concerns. The main learning goal is to understand which responsibilities move to AWS and which remain yours.

## Prerequisites

- An AWS account with permission to create EKS, IAM, EC2, and VPC resources
- AWS CLI installed and configured
- `kubectl`
- `eksctl` for a beginner-friendly provisioning path
- Basic understanding of IAM, VPCs, and Kubernetes fundamentals
- Awareness that EKS creates billable resources

## Lab Steps

1. Confirm AWS access and region configuration:
   - `aws sts get-caller-identity`
   - `aws configure list`
2. Create an EKS cluster with a simple `eksctl` example:
   - `eksctl create cluster --name k8s-indepth-eks --region us-east-1 --nodes 2`
3. Update local kubeconfig and verify cluster access:
   - `aws eks update-kubeconfig --name k8s-indepth-eks --region us-east-1`
   - `kubectl get nodes`
4. Deploy a basic workload to confirm scheduling and networking:
   - `kubectl create deployment web --image=nginx:1.25`
   - `kubectl expose deployment web --port=80 --target-port=80 --type=LoadBalancer`
5. Inspect the cluster and supporting resources:
   - `kubectl get pods -A`
   - `eksctl get nodegroup --cluster k8s-indepth-eks --region us-east-1`
6. Record cleanup steps so the lab does not continue incurring cost:
   - `eksctl delete cluster --name k8s-indepth-eks --region us-east-1`

## Verification

- `aws eks list-clusters`
- `kubectl get nodes`
- `kubectl get pods -A`
- `kubectl get svc`

## What I Learned

Expected outcomes after completing this lab:

- I can explain what EKS manages and what I still need to manage myself.
- I understand the additional networking, IAM, and cost considerations of a cloud-managed cluster.
- I can connect `kubectl` to EKS and verify that a basic workload is running.

## Interview Questions

1. What parts of Kubernetes does Amazon EKS manage for you?
2. What responsibilities remain with the user when running EKS?
3. How does IAM relate to Kubernetes access in EKS?
4. Why is VPC design important for an EKS cluster?
5. What should you verify before leaving an EKS lab environment running?

## Common Mistakes

- Creating an EKS cluster without understanding the cost impact
- Assuming a managed control plane means the rest of the platform is fully managed
- Forgetting to update kubeconfig after cluster creation
- Leaving node groups, load balancers, or entire clusters running after the lab

## References

- Amazon EKS Documentation: https://docs.aws.amazon.com/eks/
- Create an Amazon EKS Cluster: https://docs.aws.amazon.com/eks/latest/userguide/create-cluster.html
- `eksctl` Documentation: https://eksctl.io/
- AWS IAM Documentation: https://docs.aws.amazon.com/iam/
