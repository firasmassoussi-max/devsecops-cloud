# 01 - Project Overview

## Purpose

The goal of this project was to build a small but realistic DevSecOps workflow around AWS infrastructure.

Instead of configuring cloud resources manually, the infrastructure is described with Terraform. The Terraform code is stored in Git, pushed to GitHub, and checked automatically by GitHub Actions.

Security is integrated into the workflow through Checkov.

## Main learning goals

The project was designed to practice:

- Infrastructure as Code
- AWS resource configuration
- secure-by-default cloud settings
- Git version control
- GitHub repository management
- CI automation
- static security scanning
- troubleshooting failed pipelines
- documenting security decisions

## High-level workflow

~~~text
Local development
      |
      v
Terraform code
      |
      v
terraform fmt / validate
      |
      v
Git commit
      |
      v
GitHub push
      |
      v
GitHub Actions
      |
      +--> Terraform checks
      +--> Checkov security scan
      |
      v
Pass or fail
      |
      +--> Fix findings and push again
~~~

## Final state

The final project contains:

- an AWS provider configuration
- an S3 bucket
- S3 public-access blocking
- S3 versioning
- KMS encryption
- KMS key rotation
- S3 bucket key support
- lifecycle cleanup for old object versions
- cleanup for incomplete multipart uploads
- a GitHub Actions workflow
- Checkov security scanning
- a complete documentation set

## Scope

This is a portfolio and learning project.

The CI pipeline validates and scans the infrastructure configuration. It does not automatically run terraform apply.

Some production-grade controls are documented as out of scope in version 1.0, including:

- S3 access logging
- S3 event notifications
- cross-region replication
- a custom explicit KMS key policy

These are documented exceptions rather than silently ignored findings.
