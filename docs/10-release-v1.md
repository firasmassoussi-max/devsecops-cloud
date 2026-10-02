# 10 - Release v1.0

## Release objective

Version 1.0 marks the first stable technical baseline of the DevSecOps Cloud Project.

Before tagging the release, the repository was synchronized and the working tree was clean.

## Final repository check

~~~bash
git status
~~~

Expected clean state:

~~~text
On branch main
Your branch is up to date with 'origin/main'.

nothing to commit, working tree clean
~~~

## Version tag

The project was tagged with:

~~~bash
git tag -a v1.0 -m "DevSecOps Cloud Project v1.0"
~~~

The tag was pushed to GitHub with:

~~~bash
git push origin v1.0
~~~

Successful result:

~~~text
[new tag] v1.0 -> v1.0
~~~

## What v1.0 contains

The v1.0 technical baseline includes:

- Terraform AWS provider configuration
- S3 bucket definition
- S3 public access protection
- S3 versioning
- KMS encryption
- KMS key rotation
- S3 bucket key support
- lifecycle rules
- incomplete multipart upload cleanup
- Git version control
- GitHub repository
- GitHub Actions CI
- Terraform validation
- Checkov security scanning
- documented security exceptions

## Documentation note

The detailed docs directory and expanded README were added after the original technical v1.0 tag.

Therefore the v1.0 tag marks the completed technical implementation, while the main branch contains the latest portfolio documentation.

A future documentation or feature release can use a new semantic version tag instead of moving the existing v1.0 tag.
