# 02 - Environment Setup

## Development environment

The project was developed from a Windows machine using a Linux/WSL terminal environment.

The working directory used during development was:

~~~text
~/projects/devsecops-cloud
~~~

## Main tools

- Windows
- WSL / Linux terminal
- Git
- Terraform
- GitHub
- AWS provider for Terraform
- GitHub Actions
- Checkov in CI

## Terraform initialization

Terraform was initialized with:

~~~bash
terraform init
~~~

This downloaded the required provider plugins and created the provider lock file:

~~~text
.terraform.lock.hcl
~~~

The lock file is committed to Git so that provider selections are reproducible.

## Provider configuration

The project uses the HashiCorp AWS provider:

~~~hcl
terraform {
  required_providers {
    aws = {
      source  = "hashicorp/aws"
      version = "~> 6.0"
    }
  }
}
~~~

The configured AWS region is Frankfurt:

~~~hcl
provider "aws" {
  region = "eu-central-1"
}
~~~

## Local validation commands

The main local Terraform commands used were:

~~~bash
terraform fmt
terraform validate
~~~

The successful validation output was:

~~~text
Success! The configuration is valid.
~~~

## Git identity setup

Before the first commit, Git required a local identity.

The repository-specific identity was configured with commands in this form:

~~~bash
git config user.name "Firas Massoussi"
git config user.email "<your-email@example.com>"
~~~

The real email address is intentionally not documented here.

## Secret handling

Credentials are not stored in this documentation.

Examples of values that must never be committed or pasted into documentation:

- GitHub Personal Access Tokens
- AWS access keys
- AWS secret access keys
- passwords
- private keys

Use placeholders such as:

~~~text
<YOUR_GITHUB_TOKEN>
<YOUR_AWS_CREDENTIALS>
~~~
