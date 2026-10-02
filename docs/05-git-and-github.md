# 05 - Git and GitHub

## Purpose

Git was used to track every meaningful infrastructure change.

GitHub was used as the remote repository and as the platform for continuous integration.

## Initial repository workflow

Typical commands used during setup were:

~~~bash
git init
git status
git add .
git commit -m "Add S3 configuration with public access protection and versioning"
~~~

## GitHub remote

The local repository was connected to GitHub with:

~~~bash
git remote add origin https://github.com/firasmassoussi-max/devsecops-cloud.git
~~~

The configured remote was checked with:

~~~bash
git remote -v
~~~

## First push

The main branch was pushed and configured to track the GitHub branch:

~~~bash
git push -u origin main
~~~

After the upstream branch was configured, later pushes only required:

~~~bash
git push
~~~

## Important Git concepts used

### Working tree

The files currently present and edited locally.

### Staging area

Files selected for the next commit with git add.

### Commit

A saved version of the project with a message describing the change.

### Remote

The GitHub repository is stored locally under the remote name origin.

### Branch

The project uses main as the primary branch.

### Push

Uploads local commits to GitHub.

## Commit examples from the project

Examples of meaningful commits created during the project include:

~~~text
Add S3 configuration with public access protection and versioning
Add project README
Add Terraform CI security pipeline
Improve S3 security with KMS and lifecycle
Fix remaining Terraform security checks
~~~

## Authentication

GitHub HTTPS authentication used a Fine-grained Personal Access Token.

The token required repository access and permissions appropriate for the operations being performed.

When the GitHub Actions workflow file was added, the token also required permission to modify workflow files.

## Token security

Tokens must never be stored in:

- Terraform code
- README files
- Git commits
- screenshots
- chat messages
- shell history where avoidable

If a token is exposed, the correct response is to revoke it and create a new one.

The actual token value is intentionally not documented anywhere in this repository.

## Final Git status

Before marking version 1.0, the repository was checked with:

~~~bash
git status
~~~

The clean state was:

~~~text
On branch main
Your branch is up to date with 'origin/main'.

nothing to commit, working tree clean
~~~
