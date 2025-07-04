![image](https://github.com/user-attachments/assets/522f539e-142b-4a75-962f-c1aacc0ebd16)


# Terraform Infra Divergence Management POC

|**Author**        | **created on**       | **Version** |**Last edited on**| **Review Level**   | **Reviewer**      |
|---------------|------------|---------|--------|--------|----------------------|
| Anitha Annem  | Jul 04  | v1.0|  Jul 05    | Pre-Reviewer   | Priyanshu            |
| Anitha Annem  |  |  |   | L0             | Khushi Malhothra    |
| Anitha Annem  |     |      |         | L1             | Mukul Joshi       |
| Anitha Annem  |     |      |         | L2             | piyush Upadhyay      |

# Introduction
This POC demonstrates how to detect and manage infrastructure drift using Terraform, with clear and auditable outputs.

For more information related refer this documentation [Terraform Infra Divergence Management Understanding](https://github.com/Cloud-NInja-snaatak/Documentation/tree/Anitha-SCRUM-573/vcs_design/gitops/gitops/infra-divergence/doc)

# Prerequisites
- AWS account with necessary permissions (e.g., to create S3 buckets).

- AWS CLI configured locally (with aws configure).

- Terraform installed (v1.0 or above recommended).

- Basic understanding of Terraform workflow (init, plan, apply).


# Steps
## Create a new working directory
```
mkdir terraform-drift-poc
cd terraform-drift-poc
```
##  Create main.tf file
```
terraform {
  required_providers {
    aws = {
      source  = "hashicorp/aws"
      version = ">= 4.0.0"
    }
    random = {
      source  = "hashicorp/random"
      version = ">= 3.0.0"
    }
  }
}

provider "aws" {
  region = "us-east-2"
}

# Generate a random suffix
resource "random_pet" "suffix" {
  length = 2
}

# Use generated name for bucket
resource "aws_s3_bucket" "example" {
  bucket = "terraform-drift-demo-bucket-${random_pet.suffix.id}"
  acl    = "private"
}
```
## Create variables.tf file
```
variable "bucket_name" {
  description = "Name of the S3 bucket"
  type        = string
}
```
## Create outputs.tf file
```
output "bucket_name" {
  value = aws_s3_bucket.example.bucket
}
```

## Initialize and deploy
```
terraform init
terraform apply -var="terraform-drift-demo-bucket-stunning-corgi"
```
![image](https://github.com/user-attachments/assets/465d9f49-9267-41dc-b1d7-6971af09310f)

![image](https://github.com/user-attachments/assets/8a41e51a-4bd8-4437-86fc-77700476dc10)

## Simulate a drift
 Go to AWS Console → S3 → Find terraform-drift-demo-bucket-stunning-corgi.
  Add or remove a bucket tag.
  Adding the tag
  ![image](https://github.com/user-attachments/assets/4c7ee1b9-abc8-431c-9130-b3d04760cbbf)
  
## Run terraform plan again
![image](https://github.com/user-attachments/assets/331cb415-4e65-446b-b3ff-936c203bb572)

## Manage (fix) drift
![image](https://github.com/user-attachments/assets/74db99bc-eab4-4b02-881b-7832f5351328)

Removes the manually added tag, bringing infrastructure back in sync with Terraform code.

![image](https://github.com/user-attachments/assets/88058bd9-d99a-46eb-961d-12e36de860a9)

# Conclusion

This proof of concept (POC) demonstrated how Terraform can be effectively used to:

- **Detect infrastructure drift** using `terraform plan`, which identifies any changes that have occurred outside of Terraform’s control.
- **Manage and reconcile drift** using `terraform apply`, which brings the actual infrastructure back in line with the desired configuration defined in code.
- **Provide clear outputs**, ensuring transparency, maintaining infrastructure integrity, and avoiding unexpected configuration drift.

By automating drift detection and remediation, teams can confidently maintain consistent, reliable infrastructure in a reproducible way.

# Contact Information 
| Name       | Email Address                |
|------------|------------------------------|
| Anitha     |anitha.annem.snaatak@mygurukulam.co|

# References
| **Link** | **Description** |
|------------------------------------------------------|------------------|
| [Manage resource drift](https://developer.hashicorp.com/terraform/tutorials/state/resource-drift)| Documentation on Manage resource drift    |





