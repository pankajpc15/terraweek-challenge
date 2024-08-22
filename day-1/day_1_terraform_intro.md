# TerraWeek Day 1: Introduction to Terraform and Terraform Basics

## What is Terraform and How Can It Help You Manage Infrastructure as Code?

Terraform is an open-source tool created by HashiCorp that allows you to manage your infrastructure using code. Think of it like writing a recipe for your infrastructure. Instead of manually creating and managing servers, databases, and other resources, you write code to define your infrastructure. Terraform then reads this code and automatically sets up and manages these resources for you.

### Real-Life Example:

Imagine you're setting up a new kitchen in your house. Instead of buying appliances, cabinets, and utensils piece by piece and figuring out where they should go, you design the entire kitchen layout on paper first. Once you have the design, you can follow it to set up the kitchen quickly and efficiently. Terraform works the same way, but for IT infrastructure. You write a blueprint (code), and Terraform builds it for you.

## Why Do We Need Terraform and How Does It Simplify Infrastructure Provisioning?

Terraform simplifies infrastructure provisioning in several ways:

- **Consistency:** With Terraform, you define your infrastructure in code. This means you can recreate the same environment multiple times with consistent configurations, reducing human error.

- **Automation:** Terraform automates the process of setting up and managing infrastructure. This reduces the need for manual intervention and speeds up deployments.

- **Version Control:** Since Terraform configurations are written in code, you can use version control systems like Git to track changes. This makes it easy to roll back to previous configurations if something goes wrong.

- **Scalability:** Terraform can manage complex environments, from a single server to large-scale deployments with hundreds of servers, databases, and other resources.

### Real-Life Example:

Think of Terraform like a smart home system. Once you set up the system, it automatically adjusts the lighting, temperature, and security settings based on your preferences. You don't have to manually change each setting every time. Terraform does the same for your infrastructure by automatically managing and adjusting resources based on your code.

## How Can You Install Terraform and Set Up the Environment for AWS, Azure, or GCP?

### Installing Terraform on Ubuntu

Follow these steps to install Terraform on Ubuntu:

1. **Download the HashiCorp GPG Key:**

    ```bash
    wget -O- https://apt.releases.hashicorp.com/gpg | sudo gpg --dearmor -o /usr/share/keyrings/hashicorp-archive-keyring.gpg
    ```

2. **Add the HashiCorp APT Repository:**

    ```bash
    echo "deb [signed-by=/usr/share/keyrings/hashicorp-archive-keyring.gpg] https://apt.releases.hashicorp.com $(lsb_release -cs) main" | sudo tee /etc/apt/sources.list.d/hashicorp.list
    ```

3. **Update and Install Terraform:**

    ```bash
    sudo apt update
    sudo apt install terraform
    ```

### Set Up AWS Environment

1. **Install the AWS CLI:**

    ```bash
    sudo apt-get install awscli -y
    ```

2. **Configure the AWS CLI:**

    ```bash
    aws configure
    ```

   - **Note:** You'll be prompted to enter your AWS Access Key, Secret Access Key, region, and output format.

### Set Up Azure Environment

1. **Install the Azure CLI:**

    ```bash
    curl -sL https://aka.ms/InstallAzureCLIDeb | sudo bash
    ```

2. **Log in to your Azure account:**

    ```bash
    az login
    ```

3. **Set your subscription (Optional):**

    ```bash
    az account set --subscription "subscription_id"
    ```

4. **Create a Service Principal for Terraform:**

    ```bash
    az ad sp create-for-rbac --role="contributor" --scopes="/subscriptions/subscription_id"
    ```

   - **Note:** Note down the `appId`, `password`, and `tenant`.

5. **Set Up Terraform Configuration:**

    ```bash
    mkdir terraform-azure-setup
    cd terraform-azure-setup
    ```

### Set Up GCP Environment

1. **Install the Google Cloud SDK:**

    ```bash
    sudo apt-get install apt-transport-https ca-certificates gnupg
    echo "deb https://packages.cloud.google.com/apt cloud-sdk main" | sudo tee -a /etc/apt/sources.list.d/google-cloud-sdk.list
    curl https://packages.cloud.google.com/apt/doc/apt-key.gpg | sudo apt-key add -
    sudo apt-get update && sudo apt-get install google-cloud-sdk
    ```

2. **Authenticate with GCP:**

    ```bash
    gcloud init
    ```

   - Follow the prompts to log in and set your default project.

3. **Create a Service Account for Terraform:**

    ```bash
    gcloud iam service-accounts create terraform --display-name "Terraform Service Account"
    ```

4. **Grant the necessary roles to the Service Account:**

    ```bash
    gcloud projects add-iam-policy-binding YOUR_PROJECT_ID \
      --member="serviceAccount:terraform@YOUR_PROJECT_ID.iam.gserviceaccount.com" \
      --role="roles/editor"
    ```

5. **Generate a Key file for the Service Account:**

    ```bash
    gcloud iam service-accounts keys create ~/terraform-key.json \
      --iam-account terraform@YOUR_PROJECT_ID.iam.gserviceaccount.com
    ```

## Example Terraform Configuration for AWS

Here’s a sample Terraform configuration to create an S3 bucket on AWS:

```hcl
provider "aws" {
  region = "us-east-1"
}

resource "aws_s3_bucket" "example" {
  bucket = "example-bucket"
  tags = {
    Name        = "example-bucket"
    Environment = "Dev"
  }
}
