# Automated Infrastructure Deployment with Terraform and Kubernetes

## Introduction

In this guide, we will walk through the process of setting up a scalable and highly available infrastructure using Terraform and Kubernetes on AWS.
This step-by-step guide is designed for beginners, including elementary school students, who are interested in learning about cloud computing and automation.
By the end of this guide, you will have a functioning infrastructure on AWS and understand the basics of using Terraform and Kubernetes.

Objective: Design, implement, and automate a scalable and highly available infrastructure using Terraform and Kubernetes on AWS.

### Step 1: Install Terraform and AWS CLI

To start, we need to install Terraform and the AWS Command Line Interface (CLI) on our computer. These tools will help us automate the creation of cloud resources.

Open your terminal (the command line interface).

Install Terraform and AWS CLI by typing the following commands:

```
sudo yum install -y yum-utils
sudo yum-config-manager --add-repo https://rpm.releases.hashicorp.com/RHEL/hashicorp.repo
sudo yum -y install terraform
sudo yum install -y aws-cli
```

### Step 2: Configure AWS CLI

Next, we need to configure the AWS CLI with our AWS account credentials.

Create an IAM user in your AWS account with programmatic access. ![Learn how to create an IAM user.](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_users_create.html)

Get your Access Key and Secret Key from the IAM user.

Configure AWS CLI by typing the following command:

```
aws configure
```
Enter your AWS Access Key, Secret Key, Region, and Output format.


### Step 3: Create a Terraform Configuration File

Now, let's create a directory for our Terraform project and a configuration file to define our infrastructure.

Create a new directory and move into it:

```
mkdir terraform-project
cd terraform-project
```

Create a file named main.tf:

```
touch main.tf
```

### Step 4: Define AWS Provider and Create a VPC

Edit the **main.tf** file to include the AWS provider and define a Virtual Private Cloud (VPC).

Open main.tf in a text editor and add the following code:

```
provider "aws" {
  region = "us-east-1"
}

resource "aws_vpc" "main" {
  cidr_block = "10.0.0.0/16"
}

resource "aws_subnet" "subnet1" {
  vpc_id            = aws_vpc.main.id
  cidr_block        = "10.0.1.0/24"
  availability_zone = "us-east-1a"
}
```

### Step 5: Initialize and Apply Terraform Configuration

Let's initialize Terraform and apply the configuration to create the resources defined in the main.tf file.

Initialize Terraform:

```
terraform init
```

Apply the configuration:

```
terraform apply -auto-approve
```

### Step 6: Install kubectl and eksctl

Next, we need to install kubectl and eksctl, which are tools for managing Kubernetes clusters on AWS.

Install kubectl:

```
curl -o kubectl https://amazon-eks.s3.us-west-2.amazonaws.com/1.18.9/2020-11-02/bin/linux/amd64/kubectl
chmod +x ./kubectl
sudo mv ./kubectl /usr/local/bin/
```

Install eksctl:

Go to the eksctl ![releases page](https://github.com/eksctl-io/eksctl/releases) and find the latest version for your machine architecture.

Download and install eksctl:

```
curl --silent --location "https://github.com/eksctl-io/eksctl/releases/download/v0.183.0/eksctl_Linux_amd64.tar.gz" | tar xz -C /tmp
sudo mv /tmp/eksctl /usr/local/bin
```

### Step 7: Create an EKS Cluster

Now, we will create an EKS (Elastic Kubernetes Service) cluster using eksctl.

Create the cluster:

```
eksctl create cluster --name my-cluster --version 1.24 --region us-east-1 --nodegroup-name linux-nodes --node-type t2.medium --nodes 3 --nodes-min 1 --nodes-max 4 --managed
```

Note: If you encounter a version error, refer to the ![AWS EKS Kubernetes Versions](https://docs.aws.amazon.com/eks/latest/userguide/kubernetes-versions.html) for supported versions.


### Step 8: Verify the Cluster

After creating the cluster, we need to verify that it is running correctly.

Check the nodes in the cluster:

```
kubectl get nodes
```

### Step 9: Deploy a Sample Application

Finally, let's deploy a simple application to our Kubernetes cluster.

Create a deployment for an NGINX web server:

```
kubectl create deployment nginx --image=nginx
```
Expose the deployment to the internet:

```
kubectl expose deployment nginx --port 80 --type LoadBalancer
```

Check the services:

```
kubectl get svc
```

## Pushing Your Project to GitHub

Now that our project is up and running, let's push it to GitHub.

Initialize a Git repository in your project directory:

```
cd path/to/terraform-project
git init
```

Create a .gitignore file to exclude unnecessary files:

```
touch .gitignore
nano .gitignore
```

Edit **.gitignore** with the following content:

```
# Terraform files
*.tfstate
*.tfstate.*
*.tfvars
.terraform/
crash.log

# AWS CLI credentials
.aws/
```

Add and commit your files:

```
git add .
git commit -m "Initial commit of Terraform and Kubernetes project"
```

Create a GitHub repository and add it as a remote:

```
git remote add origin https://github.com/your-username/terraform-kubernetes-project.git
```

Push your project to GitHub:

```
git push -u origin main
```


## Stopping and Destroying the Project

When you're done with your project, it's important to clean up your resources to avoid unnecessary charges.

Delete the EKS cluster:

```
eksctl delete cluster --name my-cluster --region us-east-1

Destroy the Terraform-managed infrastructure:

Navigate to your Terraform project directory:

```
cd path/to/terraform-project
```

Run the destroy command:

```
terraform destroy -auto-approve
```

Clean up local files:

```
rm -rf .terraform
rm terraform.tfstate
rm terraform.tfstate.backup
```

## Conclusion
Congratulations! You have successfully set up a scalable and highly available infrastructure using Terraform and Kubernetes on AWS. You also learned how to deploy a sample application and push your project to GitHub. Remember to clean up your resources to avoid unnecessary charges. Keep experimenting and exploring the world of cloud computing and automation!
