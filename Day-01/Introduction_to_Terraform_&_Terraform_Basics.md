# Day-01 - Terra-Week-Challenge
## Introduction to Terraform and Terraform Basics

### Introduction
As part of the Terra-Week-Challenge, Day-01 is dedicated to understanding what Terraform is and how it can revolutionize the way we manage and provision infrastructure. In this post, we'll explore why Terraform is a powerful tool for DevOps and Cloud Engineers, walk through the installation process, and dive into some key Terraform terminologies with examples.

### What is Terraform
Terraform, developed by HashiCorp, is an open-source Infrastructure as Code (IaC) tool that enables you to define and manage your cloud infrastructure using a high-level configuration language called HashiCorp Configuration Language (HCL) or JSON. Terraform allows you to describe your infrastructure in a file that can be versioned, reused, and shared, making your infrastructure configurations consistent and scalable.

### How Terraform Can Help You Manage Infrastructure as Code?
Consistency: Terraform ensures that your infrastructure is consistent across different environments, eliminating configuration drift.
Automation: Automate the provisioning and management of your infrastructure with simple, reusable scripts.
Scalability: Easily manage and scale complex infrastructure by codifying it into simple configuration files.
Version Control: Use version control systems like Git to manage and track changes to your infrastructure code.

### Why Do We Need Terraform?
#### Simplifying Infrastructure Provisioning
In traditional IT environments, provisioning infrastructure manually is time-consuming and error-prone. With Terraform, you can automate this process, reducing the risk of human error and making it easier to manage complex infrastructures.

#### Cross-Platform Compatibility
Terraform is platform-agnostic, meaning it can manage infrastructure across multiple cloud providers like AWS, Azure, GCP, and even on-premises solutions. This flexibility allows you to use a single tool to manage all your infrastructure needs, regardless of where they are hosted.

#### Reproducibility
Terraform configurations can be reused to reproduce environments with minimal effort. This is particularly useful in CI/CD pipelines where you need to deploy identical environments for testing, staging, and production.

### How to Install Terraform and Set Up the Environment?
#### Step 1: Install Terraform
To get started with Terraform, you'll first need to install it on your local machine. Here’s how to do it on different operating systems:

- Windows
1. Download the [Terraform binary](https://developer.hashicorp.com/terraform/install#windows) for Windows.

2. Unzip the downloaded file and move the terraform.exe to a directory included in your system's PATH.
   
- macOS
1. Run the following command to install Terraform using Homebrew:
   
   ```
      brew tap hashicorp/tap
      brew install hashicorp/tap/terraform

3. To update to the latest version of Terraform, first update Homebrew:

   ```
      brew update

4. Then, run the upgrade command to download and use the latest Terraform version:

   ```
      brew upgrade hashicorp/tap/terraform   

5. Verify the installation:

   ```
       terraform -v
       terraform -help

- Ubuntu/Debian
1. Ensure that your system is up to date and you have installed the gnupg, software-properties-common, and curl packages installed. You will use these packages to verify HashiCorp's GPG signature and install HashiCorp's Debian package repository:

   ```
      sudo apt-get update && sudo apt-get install -y gnupg software-properties-common

2. Install the HashiCorp GPG key:

   ```
      wget -O- https://apt.releases.hashicorp.com/gpg | \
      gpg --dearmor | \
      sudo tee /usr/share/keyrings/hashicorp-archive-keyring.gpg > /dev/null

3. Verify the key's fingerprint:

   ```
      gpg --no-default-keyring \
      --keyring /usr/share/keyrings/hashicorp-archive-keyring.gpg \
      --fingerprint

4. Add the official HashiCorp repository to your system. The lsb_release -cs command finds the distribution release codename for your current system, such as buster, groovy, or sid:

   ```
      echo "deb [signed-by=/usr/share/keyrings/hashicorp-archive-keyring.gpg] \
      https://apt.releases.hashicorp.com $(lsb_release -cs) main" | \
      sudo tee /etc/apt/sources.list.d/hashicorp.list

5. Download the package information from HashiCorp & install the Terraform:

   ```
      sudo apt update
      sudo apt-get install terraform
   ```

6. Verify the installation

   ```
      terraform -v
      terraform -help
  
#### Step 2: Set Up AWS Environment
To manage AWS infrastructure using Terraform, you'll need to configure your AWS credentials:

1. Install the AWS CLI:

   ```
      sudo apt-get install awscli -y

3. Configure the AWS CLI:
   ```
      aws configure

Note:- 
You'll be prompted to enter your AWS Access Key, Secret Access Key, region, and output format.

#### Step 3: Set Up Azure Environment

1. Install the Azure CLI:

   ```
      curl -sL https://aka.ms/InstallAzureCLIDeb | sudo bash

2. Log in to your Azure account:

   ```
      az login
   
3. Set your subscription (Optional):

   ```
      az account set --subscription "subscription_id"
   
4. Create a Service Principal for Terraform:

   ```
      az ad sp create-for-rbac --role="contributor" --scopes="/subscriptions/subscription_id"

Note down the sppId, password, and tenant.

5. Set Up Terraform Configuration:
- Create a directory for your Terraform files:

  ```
     mkdir terraform-azure-setup
     cd terrform-azure-setup
     
- Create a main.tf file with the following content:
  ```
     provider "azurerm"{
         features = {}

         tenant_id = "your_tenant_id"
         subscription_id = "your_subscription_id"
         client_id = "your_app_id"
         client_secret = "your password"
     }

     resource "azurerm_resource_group" "azure_resource"{
         name = "azure-resources"
         location = "East US"
     }

6. Initialize the Terraform Environment:

   ```
      terraform init
   ```

7. Validate the Terraform Configuration:
    
   ```
      terraform validate
   ```

8. Apply the Terraform Configuration:

   ```
      terraform apply
   ```

9. Destroy the Terraform Configuration:

   ```
      terraform destroy
   ```
    
#### Step 4: Set Up GCP Environment

1. Install the Google Cloud SDK:
   ```
      sudo apt-get install apt-transport-https ca-certificates gnupg
      echo "deb https://packages.cloud.google.com/apt cloud-sdk main" | sudo tee -a /etc/apt/sources.list.d/google-cloud-sdk.list
      curl https://packages.cloud.google.com/apt/doc/apt-key.gpg | sudo apt-key add -
      sudo apt-get update && sudo apt-get install google-cloud-sdk

2. Authenticate with GCP:
   - Initialize the Google CLoud SDK and logged into it.
     
   ```
      gcloud init
   ```
    
   - Follow the prompts to log in and set your default project.
     
3. Create a Service Account for Terraform:

   ```
      gcloud iam service-accounts create terraform
      --display-name "Terrform Service Account"
   ```
   
4. Grant the neccessary roles to the Service Account:

   ```
      gcloud projects add-iam-policy-binding YOUR_PROJECT_ID
      --member="serviceAccount:terraform@YOUR_PROJECT_ID.iam.gserviceaccount.com"
      --role="roles/editor"
   ```
   
5. Generate a Key file for the Service Account:

   ```
      gcloud iam service-accounts keys create ~/ terraform-key.json
      --iam-account terraform@YOUR_PROJECT_ID.iam.gserviceaccount.com
   ```
   
6. Set Up Terraform Configuration:
   - Create a directory for your Terraform files:
   ```
      mkdir terraform-gcp-setup
      cd terrform-gcp-setup
   ```

   - Create a main.tf file with the following content:
   ```
      provider ""google" {
         credentails = file("<PATH_TO_KEY_FILE>")
         project = "YOUR_PROJECT_ID"
         region = "us-central1"
      }

      resource "google_compute_instance" "gcp_resource"{
         name = "terraform-instance"
         machine_type = "f1-micro"
         zone = "us-central1-a"

         boot_disk {
            initialize_params {
               image = "debian-cloud/debian-9"
            }
         }

         network_interface {
            netwrok = "default"
            access_config {}
         }
      }
   ```
Note:- Replace <PATH_TO_KEY_FILE> with the path to your Service Account key file (e.g. ~/terraform-key.json).

7. Initialize the Terraform Environment:
    
    ```
       terraform init
    
7. Validate the Terraform Configuration:
    
    ```
       terraform validate
    
8. Apply the Terraform Configuration:
    
    ```
       terraform apply
    
9.  Destroy the Terraform Configuration:

    ```
       terraform destroy
    ```

### Top important Terraform terminologies
#### 1. Providers
Providers are plugins that allow Terraform to interact with APIs of cloud providers, SaaS providers, or other services (e.g., AWS, Azure, GCP).

   ```
      provider "aws" {
        region = "us-west-2"
      }
   ```

#### 2. Resources
Resources are the fundamental building blocks of Terraform. They represent the infrastructure components you want to manage, such as virtual machines, storage, or networking.

   ```
      resource "aws_instance" "web_server" {
           ami           = "ami-0c55b159cbfafe1f0"
           instance_type = "t2.micro"
      }
   ```

#### 3. Modules
Modules are containers for multiple resources that are used together. They help in organizing and reusing code across projects.

   ```
      module "vpc" {
           source = "terraform-aws-modules/vpc/aws"
           version = "2.0.0"
  
           name = "my-vpc"
           cidr = "10.0.0.0/16"
      }

   ```

#### 4. State
Terraform state is a file that tracks the current state of your infrastructure. Terraform uses this state to map real-world resources to your configuration and to determine what changes need to be applied.

   ```
      terraform {
        backend "s3" {
             bucket = "my-terraform-state"
             key    = "terraform.tfstate"
             region = "us-west-2"
        }
      }
   ```

#### 5. Variables
Variables allow you to parameterize your Terraform configurations, making them more flexible and reusable.

   ```
      variable "instance_type" {
           description = "Type of the instance"
           default     = "t2.micro"
      }

      resource "aws_instance" "web_server" {
           ami           = "ami-0c55b159cbfafe1f0"
           instance_type = var.instance_type
      }
   ```

#### 6. Outputs
Outputs are used to extract information from your Terraform configuration and display it after a Terraform apply operation. This information can be useful for debugging or sharing data between modules.

   ```
      output "instance_ip" {
           value = aws_instance.web_server.public_ip
      }
   ```

#### 7. Data Sources
Data sources allow Terraform to fetch information defined outside of Terraform, such as existing resources in your cloud provider. This can be useful when you want to use data from resources not managed by Terraform.

   ```
      data "aws_ami" "latest" {
           most_recent = true
           owners      = ["amazon"]

           filter {
                name   = "name"
                values = ["amzn2-ami-hvm-*"]
           }
      }

      resource "aws_instance" "web_server" {
           ami           = data.aws_ami.latest.id
            instance_type = "t2.micro"
      }

   ```

#### 8. Terraform init
'terraform init' initializes a working directory containing Terraform configuration files. It sets up the backend and installs any necessary plugins (providers).

   ```
      terraform init
   ```

#### 9. Terraform validate
'terraform validate' is a command that checks whether your Terraform configuration files are syntactically valid and internally consistent. It verifies that the configuration is correct in terms of syntax and structure, ensuring that all required attributes and references are properly defined. However, it does not check if the infrastructure changes will succeed; it only validates the configuration itself.

Usage:
Before applying changes, it’s good practice to run terraform validate to catch any errors in your Terraform configuration files.

   ```
      terraform validate
   ```

#### 10. Terraform plan
A 'terraform plan' is a command that shows the changes that will be made by Terraform when you run terraform apply. It gives you a preview of what will happen.

   ```
      terraform plan
   ```

#### 11. Terraform apply
 'terraform apply' is the command that actually applies the changes required to reach the desired state of the configuration. It makes the changes described in the Terraform plan.

    ```
      terraform apply

