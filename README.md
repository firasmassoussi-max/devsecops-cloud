# DevSecOps Cloud Project

[![Terraform CI](https://github.com/firasmassoussi-max/devsecops-cloud/actions/workflows/terraform-ci.yml/badge.svg)](https://github.com/firasmassoussi-max/devsecops-cloud/actions/workflows/terraform-ci.yml)

A documented DevSecOps learning and portfolio project built with Terraform, AWS, GitHub Actions, and Checkov.

The project demonstrates how cloud infrastructure can be defined as code, version controlled, validated automatically, and scanned for security issues before changes are accepted.

## Project status

- Version: v1.0 technical baseline
- CI status: passing
- Infrastructure deployment: not automated in CI
- AWS region configured in Terraform: eu-central-1
- Primary cloud resource: Amazon S3
- Security tooling: Checkov

## What this project demonstrates

- Infrastructure as Code with Terraform
- Secure Amazon S3 configuration
- AWS KMS encryption
- S3 versioning
- S3 public-access protection
- Lifecycle management
- Git and GitHub version control
- GitHub Actions continuous integration
- Terraform formatting and validation
- Checkov security scanning
- Security findings, remediation, and documented exceptions
- Release tagging with Git

## Architecture

~~~text
Developer
   |
   v
Terraform configuration
   |
   v
Git repository
   |
   v
GitHub
   |
   v
GitHub Actions
   |
   +--> Terraform format check
   +--> Terraform initialization
   +--> Terraform validation
   +--> Checkov security scan
   |
   v
Validated DevSecOps project
~~~

## Repository structure

~~~text
devsecops-cloud/
├── .github/
│   └── workflows/
│       └── terraform-ci.yml
├── docs/
│   ├── 01-project-overview.md
│   ├── 02-environment-setup.md
│   ├── 03-terraform-infrastructure.md
│   ├── 04-s3-security.md
│   ├── 05-git-and-github.md
│   ├── 06-github-actions-ci.md
│   ├── 07-checkov-security.md
│   ├── 08-errors-and-solutions.md
│   ├── 09-testing-and-validation.md
│   ├── 10-release-v1.md
│   └── 11-future-improvements.md
├── .gitignore
├── .terraform.lock.hcl
├── main.tf
└── README.md
~~~

## Documentation

The project is documented step by step:

1. [Project overview](docs/01-project-overview.md)
2. [Environment setup](docs/02-environment-setup.md)
3. [Terraform infrastructure](docs/03-terraform-infrastructure.md)
4. [S3 security](docs/04-s3-security.md)
5. [Git and GitHub](docs/05-git-and-github.md)
6. [GitHub Actions CI](docs/06-github-actions-ci.md)
7. [Checkov security scanning](docs/07-checkov-security.md)
8. [Errors and solutions](docs/08-errors-and-solutions.md)
9. [Testing and validation](docs/09-testing-and-validation.md)
10. [Release v1.0](docs/10-release-v1.md)
11. [Future improvements](docs/11-future-improvements.md)

## Important note about deployment

Version 1.0 validates and security-scans the Terraform configuration, but the GitHub Actions workflow does not run terraform apply.

That means this repository demonstrates the Infrastructure as Code and DevSecOps workflow without automatically deploying cloud resources from CI.

A future version can add AWS authentication through GitHub OIDC, terraform plan, approval gates, and controlled deployment.

## Security note

No credentials, Personal Access Tokens, or AWS access keys should ever be committed to this repository.

Secrets must be stored only in appropriate secret-management systems or short-lived identity mechanisms.

## Author

**Firas Massoussi**

Cybersecurity / IT student focused on cloud security, DevSecOps, infrastructure security, and automation.
