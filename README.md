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

In VS Studio, open the created folder, create the _variables.tf_ in the subfolder, and add the following.

```
variable "aws_region" {
  type    = string
  default = "us-east-1"
}

variable "project_name" {
  type    = string
  default = "aws-data-engineering-kevins"
}

```
The project name _aws-data-engineering-kevins_ is going to be our prefix for the S3 bucket

### Step 3: Create main.tf
Still within the subfolder, create a new file named _main.tf_. This will include the Terraform and provider.

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
  bucket = "${var.project_name}-raw-data"

  tags = {
    Layer   = "raw"
    Project = var.project_name
  }
}

```
### Step 5: Enable Versioning.
This will help in data recovery and audit.
In _main.tf_, add the following

```
resource "aws_s3_bucket_versioning" "raw_versioning" {
  bucket = aws_s3_bucket.raw_bucket.id

  versioning_configuration {
    status = "Enabled"
  }
}

```
### Step 6: Enable Server-side Encryption

In _main.tf_, add the following

```
resource "aws_s3_bucket_server_side_encryption_configuration" "raw_encryption" {
  bucket = aws_s3_bucket.raw_bucket.id

  rule {
    apply_server_side_encryption_by_default {
      sse_algorithm = "AES256"
    }
  }
}

```

### Step 7: Block public Access
This is going to prevent data leakage.
In _main.tf_, add the following

```
resource "aws_s3_bucket_public_access_block" "raw_block_public" {
  bucket = aws_s3_bucket.raw_bucket.id

  block_public_acls       = true
  block_public_policy     = true
  ignore_public_acls      = true
  restrict_public_buckets = true
}

```
### Step 8: Create Processed Data Bucket
Add the following

```
resource "aws_s3_bucket" "processed_bucket" {
  bucket = "${var.project_name}-processed-data"

  tags = {
    Layer   = "processed"
    Project = var.project_name
  }
}

```
For this, we shall repeat the same process for versioning, encryption, and public as below

```
#Enable Versioning on Processed bucket
resource "aws_s3_bucket_versioning" "processed_versioning" {
  bucket = aws_s3_bucket.processed_bucket.id

  versioning_configuration {
    status = "Enabled"
  }
}
#Enable Server-side Encryption on processed bucket
resource "aws_s3_bucket_server_side_encryption_configuration" "processed_encryption" {
  bucket = aws_s3_bucket.processed_bucket.id

  rule {
    apply_server_side_encryption_by_default {
      sse_algorithm = "AES256"
    }
  }
}
#Block public access on processed bucket
resource "aws_s3_bucket_public_access_block" "processed_block_public" {
  bucket = aws_s3_bucket.processed_bucket.id

  block_public_acls       = true
  block_public_policy     = true
  ignore_public_acls      = true
  restrict_public_buckets = true
}


```
### Step 9: Create Logs Bucket

Add the following

```
resource "aws_s3_bucket" "logs_bucket" {
  bucket = "${var.project_name}-logs"

  tags = {
    Layer   = "logs"
    Project = var.project_name
  }
}

```




