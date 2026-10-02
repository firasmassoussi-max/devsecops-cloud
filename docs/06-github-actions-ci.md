# 06 - GitHub Actions CI

## Purpose

GitHub Actions provides continuous integration for the Terraform project.

Every push to main and every pull request targeting main triggers automated checks.

## Workflow file

The workflow is stored at:

~~~text
.github/workflows/terraform-ci.yml
~~~

## Complete workflow

~~~yaml
name: Terraform CI

on:
  push:
    branches:
      - main
  pull_request:
    branches:
      - main

permissions:
  contents: read

jobs:
  terraform:
    name: Terraform Checks
    runs-on: ubuntu-latest

    steps:
      - name: Checkout repository
        uses: actions/checkout@v4

      - name: Setup Terraform
        uses: hashicorp/setup-terraform@v3

      - name: Terraform Format Check
        run: terraform fmt -check -recursive

      - name: Terraform Init
        run: terraform init -backend=false -input=false

      - name: Terraform Validate
        run: terraform validate -no-color

      - name: Install Checkov
        run: pip install checkov

      - name: Terraform Security Scan
        run: checkov -d . --framework terraform
~~~

## Trigger

The workflow runs for:

- pushes to main
- pull requests targeting main

## Permissions

The workflow itself only requests read access to repository contents:

~~~yaml
permissions:
  contents: read
~~~

This follows the principle of keeping CI permissions limited.

## Pipeline stages

### 1. Checkout repository

~~~yaml
uses: actions/checkout@v4
~~~

Copies the repository into the GitHub-hosted runner.

### 2. Setup Terraform

~~~yaml
uses: hashicorp/setup-terraform@v3
~~~

Installs and configures Terraform in the CI environment.

### 3. Format check

~~~bash
terraform fmt -check -recursive
~~~

Fails the workflow if Terraform formatting does not match the expected format.

### 4. Initialization

~~~bash
terraform init -backend=false -input=false
~~~

Initializes Terraform for validation without configuring a remote backend.

### 5. Validation

~~~bash
terraform validate -no-color
~~~

Checks that the Terraform configuration is syntactically valid and internally consistent.

### 6. Install Checkov

~~~bash
pip install checkov
~~~

Installs the infrastructure security scanner in the GitHub runner.

### 7. Security scan

~~~bash
checkov -d . --framework terraform
~~~

Scans the Terraform configuration for known security misconfigurations and policy violations.

## Why this is DevSecOps

Security is not performed only after development.

Instead, every code change is automatically checked as part of the same CI workflow.

This creates a feedback loop:

~~~text
Code change
   |
   v
Push / Pull Request
   |
   v
Automated Terraform checks
   |
   v
Automated security scan
   |
   +--> Pass
   |
   +--> Fail -> fix -> push again
~~~
