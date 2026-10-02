# DevSecOps Cloud Project

A small DevSecOps project using Terraform and AWS.

## Features

- AWS S3 bucket managed with Terraform
- S3 versioning enabled
- Public access protection enabled
- Infrastructure as Code with Terraform
- Source code versioned with Git and GitHub

## Technologies

- Terraform
- AWS
- Git
- GitHub

## Security

The S3 bucket is configured to block public access.

Terraform state files and sensitive local files are excluded through `.gitignore`.

## Usage

```bash
terraform init
terraform fmt
terraform validate
terraform plan




```

## Author

Firas Massoussi
