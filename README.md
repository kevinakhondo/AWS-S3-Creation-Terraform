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
