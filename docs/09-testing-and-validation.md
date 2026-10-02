# 09 - Testing and Validation

## Local Terraform formatting

Terraform formatting was applied with:

~~~bash
terraform fmt
~~~

The CI pipeline independently checks formatting with:

~~~bash
terraform fmt -check -recursive
~~~

## Local Terraform validation

The configuration was validated locally with:

~~~bash
terraform validate
~~~

Successful result:

~~~text
Success! The configuration is valid.
~~~

## Git status verification

Git status was used repeatedly to understand the current repository state:

~~~bash
git status
~~~

Before the release, the repository reached:

~~~text
On branch main
Your branch is up to date with 'origin/main'.

nothing to commit, working tree clean
~~~

## GitHub Actions validation

The final CI pipeline verifies:

| Check | Purpose |
| --- | --- |
| Checkout | Loads repository content |
| Terraform setup | Installs Terraform |
| Format check | Enforces consistent formatting |
| Terraform init | Initializes provider configuration |
| Terraform validate | Checks Terraform validity |
| Install Checkov | Installs security scanner |
| Checkov scan | Tests Infrastructure as Code security |

## CI development history

### Run 1

Status: failed

Reason: Checkov detected five security findings.

Outcome: Terraform security was improved.

### Run 2

Status: failed

Reason: additional security requirements remained, including multipart upload cleanup and several controls outside the project scope.

Outcome: multipart cleanup was added and scope exceptions were documented.

### Run 3

Status: successful

Outcome:

~~~text
Terraform Format Check   PASS
Terraform Init           PASS
Terraform Validate       PASS
Checkov Security Scan    PASS
~~~

## What was not tested

Version 1.0 does not automatically run:

~~~bash
terraform apply
~~~

Therefore the CI pipeline validates the configuration but does not prove that an AWS deployment was executed successfully.

This distinction is intentionally documented so that the project does not claim more than it actually demonstrates.

## Suggested production-level tests for a future version

- terraform plan in pull requests
- deployment to a dedicated test AWS account
- post-deployment verification
- policy-as-code gates
- drift detection
- infrastructure integration tests
- branch protection requiring successful CI
