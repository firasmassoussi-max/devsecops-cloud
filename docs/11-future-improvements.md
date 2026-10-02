# 11 - Future Improvements

## Purpose

Version 1.0 intentionally focuses on a small, understandable DevSecOps pipeline.

The following improvements would make the project closer to a production-grade cloud delivery workflow.

## 1. GitHub OIDC authentication to AWS

Replace long-lived AWS access keys with short-lived credentials issued through OpenID Connect.

Benefits:

- no permanent AWS secret stored in GitHub
- reduced credential exposure
- role-based access
- short-lived sessions

## 2. Terraform plan in CI

Add:

~~~bash
terraform plan
~~~

to pull-request validation.

A future workflow could publish the plan as a pull-request artifact or comment.

## 3. Controlled terraform apply

Deployment could be added with:

- protected environments
- manual approval
- dedicated deployment role
- least-privilege IAM permissions
- branch protection

Automatic apply should not be added without appropriate controls.

## 4. Remote Terraform state

Move local Terraform state to a protected remote backend.

Possible design:

- S3 backend
- encryption
- versioning
- restricted access
- state locking mechanism

## 5. Multiple environments

Separate infrastructure into:

~~~text
dev
staging
production
~~~

Each environment should use isolated state and appropriately scoped permissions.

## 6. S3 access logging

Implement the control currently documented as out of scope.

This would improve auditability of S3 access.

## 7. S3 event notifications

Add event-driven monitoring or processing for relevant S3 events.

Possible integrations:

- SNS
- SQS
- Lambda
- EventBridge

## 8. Cross-region replication

Add replication for workloads that require stronger disaster-recovery or regional-resilience characteristics.

## 9. Explicit KMS key policy

Replace the documented version 1.0 exception with a carefully scoped KMS key policy.

## 10. Branch protection

Require:

- pull requests
- successful CI
- review before merge
- restricted direct pushes to main

## 11. Additional security scanning

Possible tools:

- Trivy
- tfsec-compatible scanning where applicable
- secret scanning
- dependency review
- GitHub code scanning

## 12. Policy as Code

Add organizational security rules through policy engines.

Examples:

- Open Policy Agent
- Conftest
- Terraform policy frameworks

## 13. Monitoring and audit

Future AWS controls could include:

- CloudTrail
- AWS Config
- CloudWatch
- Security Hub
- GuardDuty

## Suggested v2.0 direction

A strong next milestone would be:

~~~text
Pull Request
    |
    v
Terraform fmt / validate
    |
    v
Checkov
    |
    v
Terraform plan
    |
    v
Review + approval
    |
    v
GitHub OIDC
    |
    v
Controlled AWS deployment
~~~

That would turn the current CI-focused project into a more complete DevSecOps delivery pipeline.
