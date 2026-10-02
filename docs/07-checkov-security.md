# 07 - Checkov Security Scanning

## Why Checkov was added

Terraform can be syntactically valid while still defining insecure cloud infrastructure.

Checkov adds static security analysis for Infrastructure as Code.

## Command used in CI

~~~bash
checkov -d . --framework terraform
~~~

## First security scan

The first CI security scan reported:

~~~text
Passed checks: 11
Failed checks: 5
Skipped checks: 0
~~~

The important findings included:

- CKV2_AWS_62 - S3 event notifications
- CKV_AWS_145 - KMS encryption
- CKV_AWS_18 - S3 access logging
- CKV_AWS_144 - cross-region replication
- CKV2_AWS_61 - lifecycle configuration

## Remediation: KMS encryption

The project added a dedicated AWS KMS key and configured S3 to use aws:kms encryption.

After this change, the KMS encryption check passed.

## Remediation: lifecycle configuration

An S3 lifecycle resource was added.

It deletes non-current versions after 90 days.

This resolved the lifecycle configuration finding.

## Second security scan

After the first round of improvements, Checkov identified additional details that still required attention.

The remaining findings included:

- CKV_AWS_300 - multipart uploads did not have an abort period
- CKV2_AWS_62 - event notifications
- CKV_AWS_18 - access logging
- CKV2_AWS_64 - explicit KMS key policy
- CKV_AWS_144 - cross-region replication

This demonstrated an important security lesson: fixing one layer can reveal additional requirements during the next scan.

## Remediation: incomplete multipart uploads

The lifecycle configuration was extended with:

~~~hcl
abort_incomplete_multipart_upload {
  days_after_initiation = 7
}
~~~

This resolved CKV_AWS_300.

## Documented exceptions

Some controls were considered outside the scope of version 1.0.

They were documented directly on the relevant Terraform resources with Checkov skip comments.

For the S3 bucket:

~~~hcl
#checkov:skip=CKV2_AWS_62:Event notifications are out of scope for this demo
#checkov:skip=CKV_AWS_18:Access logging is out of scope for this demo
#checkov:skip=CKV_AWS_144:Cross-region replication is out of scope for this demo
~~~

For the KMS key:

~~~hcl
#checkov:skip=CKV2_AWS_64:Explicit KMS key policy is out of scope for this demo
~~~

## Why documented exceptions matter

A security scanner should not simply be disabled because it reports findings.

A better practice is to:

1. understand the finding
2. fix it when appropriate
3. document the risk and reason when a control is intentionally out of scope
4. keep the rest of the scanner active

## Final result

The third CI run completed successfully.

The project therefore reached a state where:

- Terraform formatting passed
- Terraform initialization passed
- Terraform validation passed
- the Checkov scan passed with explicit documented exceptions
