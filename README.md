# AWS-S3-Creation-Terraform
Create an S3 Bucket using Terraform
## Introduction

The goal is to create an S3 bucket using Terraform that EC2 can access using IAM roles, not credentials.
## Module Structure
We shall follow the following structure.

```
aws-data-engineering/module-04-s3-terraform/
├── main.tf
├── variables.tf
├── outputs.tf
└── README.md

```
In your VS, navigate to the folder.

```
cd aws-data-engineering
cd module-04-s3-terraform
```
## Hands On
### Data Lake Structure
Our data lake solution will follow this structure

```
S3
├── raw/        → source data (immutable)
├── processed/  → cleaned/transformed data
└── logs/       → pipeline & access logs

```

### Step 1: Create a folder
On your terminal, navigate to _aws-data-engineering_ and add the _module-04-s3-terraform_ subfolder where we will run the S3 terraform

```
cd aws-data-engineering
mkdir -p module-04-s3-terraform
cd module-04-s3-terraform
```

### Step 2: Create variables.tf

In VS Studio, open the created folder, create the _variables.tf_ in the subfolder and add the following.

```
variable "aws_region" {
  type    = string
  default = "us-east-1"
}

variable "project_name" {
  type    = string
  default = "aws-data-engineering"
}

```

### Step 3: Create main.tf
Still within the subfolder, create a new file named _main.tf_. This is going to carry the terraform and provider.

```
terraform {
  required_version = ">= 1.5.0"

  required_providers {
    aws = {
      source  = "hashicorp/aws"
      version = "~> 5.0"
    }
  }
}

provider "aws" {
  region = var.aws_region
}

```
### Step 4: Create Raw Data Bucket
This is where raw data is stored. Under _main.tf_, add the following

```
resource "aws_s3_bucket" "raw_bucket" {
  bucket = "${var.aws-data-engineering-kevins}-raw-data"

  tags = {
    Layer   = "raw"
    Project = var.aws-data-engineering-kevins
  }
}

```







