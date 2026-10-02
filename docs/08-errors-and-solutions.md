# 08 - Errors and Solutions

## Overview

Several errors occurred during the project.

They are documented here because troubleshooting is an important part of DevSecOps work.

## 1. Git identity was not configured

### Problem

The first commit could not be created because Git did not know the author's identity.

### Solution

Repository-specific identity values were configured:

~~~bash
git config user.name "Firas Massoussi"
git config user.email "<your-email@example.com>"
~~~

### Lesson

Git needs an author identity before creating commits.

## 2. No GitHub remote configured

### Symptom

Running:

~~~bash
git remote -v
~~~

initially returned no remote.

### Solution

The GitHub repository was created and connected:

~~~bash
git remote add origin https://github.com/firasmassoussi-max/devsecops-cloud.git
~~~

### Lesson

A local Git repository and a GitHub repository are separate until a remote is configured.

## 3. GitHub HTTPS authentication failed

### Error

~~~text
remote: Invalid username or token.
fatal: Authentication failed
~~~

### Cause

GitHub HTTPS Git operations require token-based authentication rather than a normal account password.

### Solution

A Fine-grained Personal Access Token was created with access to the repository.

### Lesson

Authentication failures can be caused by an invalid token, an expired token, incorrect repository access, or insufficient permissions.

## 4. Workflow file push was rejected

### Problem

The repository token could push normal files but could not push changes under:

~~~text
.github/workflows/
~~~

### Cause

The Fine-grained PAT did not have permission to modify workflow files.

### Solution

Repository permissions were updated to include the required workflow write capability while retaining repository content access.

### Lesson

Use least privilege, but make sure the token has the specific permission required for the Git operation.

## 5. Checkov was not installed locally

### Error

~~~text
checkov: command not found
~~~

### Cause

Checkov was installed only inside the GitHub Actions runner.

### Decision

Local installation was not required for the version 1.0 workflow because GitHub Actions performs the authoritative CI security scan.

### Lesson

CI tools can be installed inside an isolated runner even when they are not installed on the developer workstation.

## 6. First CI security scan failed

### Result

~~~text
Passed checks: 11
Failed checks: 5
~~~

### Solution

The Terraform configuration was improved with:

- KMS encryption
- KMS key rotation
- lifecycle management

The pipeline was run again.

## 7. Second CI security scan still failed

### Findings

The second scan identified:

- missing multipart upload cleanup
- event notification requirement
- access logging requirement
- explicit KMS policy requirement
- cross-region replication requirement

### Solution

Multipart upload cleanup was implemented.

The remaining controls were explicitly documented as out of scope for this learning project.

## 8. Final CI run passed

After the changes, the third Terraform CI run completed successfully.

### Lesson

A failed CI pipeline is not only an error. It is feedback.

The development cycle became:

~~~text
Fail
  |
  v
Read logs
  |
  v
Understand finding
  |
  v
Change code
  |
  v
Validate
  |
  v
Commit and push
  |
  v
Run CI again
~~~

## 9. Credential exposure lesson

During development, a GitHub token was accidentally shared outside its intended authentication context.

The correct security response to any exposed token is:

1. revoke the exposed token
2. generate a replacement token
3. limit the new token to the required repository and permissions
4. never commit or document the token value

No real token value is included in this repository.

This is an important practical lesson in secret hygiene and credential rotation.
