# 03 - Terraform Infrastructure

## Overview

The entire AWS infrastructure for version 1.0 is defined in main.tf.

The configuration is intentionally small enough to understand end to end while still demonstrating real security controls.

## Complete Terraform configuration

~~~hcl
terraform {
  required_providers {
    aws = {
      source  = "hashicorp/aws"
      version = "~> 6.0"
    }
  }
}

provider "aws" {
  region = "eu-central-1"
}

resource "aws_s3_bucket" "demo" {
  #checkov:skip=CKV2_AWS_62:Event notifications are out of scope for this demo
  #checkov:skip=CKV_AWS_18:Access logging is out of scope for this demo
  #checkov:skip=CKV_AWS_144:Cross-region replication is out of scope for this demo

  bucket_prefix = "devsecops-demo-"
}

resource "aws_s3_bucket_public_access_block" "demo" {
  bucket = aws_s3_bucket.demo.id

  block_public_acls       = true
  block_public_policy     = true
  ignore_public_acls      = true
  restrict_public_buckets = true
}

resource "aws_s3_bucket_versioning" "demo" {
  bucket = aws_s3_bucket.demo.id

  versioning_configuration {
    status = "Enabled"
  }
}

resource "aws_kms_key" "s3" {
  #checkov:skip=CKV2_AWS_64:Explicit KMS key policy is out of scope for this demo

  description             = "KMS key for DevSecOps S3 bucket"
  deletion_window_in_days = 7
  enable_key_rotation     = true
}

resource "aws_s3_bucket_server_side_encryption_configuration" "demo" {
  bucket = aws_s3_bucket.demo.id

  rule {
    apply_server_side_encryption_by_default {
      kms_master_key_id = aws_kms_key.s3.arn
      sse_algorithm     = "aws:kms"
    }

    bucket_key_enabled = true
  }
}

resource "aws_s3_bucket_lifecycle_configuration" "demo" {
  bucket = aws_s3_bucket.demo.id

  depends_on = [
    aws_s3_bucket_versioning.demo
  ]

  rule {
    id     = "cleanup-old-versions"
    status = "Enabled"

    filter {}

    noncurrent_version_expiration {
      noncurrent_days = 90
    }

    abort_incomplete_multipart_upload {
      days_after_initiation = 7
    }
  }
}
~~~

## Terraform block

The terraform block declares the required AWS provider and restricts the provider to the 6.x version family.

This improves reproducibility and avoids silently moving to an incompatible future major version.

## AWS provider

The provider block configures Terraform to work with AWS in eu-central-1.

## S3 bucket

The S3 bucket uses a prefix:

~~~text
devsecops-demo-
~~~

AWS can then generate a globally unique final bucket name.

## Resource relationships

Several resources reference the S3 bucket ID:

- public access block
- versioning configuration
- encryption configuration
- lifecycle configuration

This creates dependencies inside the Terraform resource graph.

The lifecycle configuration also explicitly depends on the versioning resource because its rule manages non-current object versions.

## Why the infrastructure is separated into resources

Modern versions of the AWS Terraform provider commonly model S3 functionality with separate resources.

This keeps concerns separate:

- bucket creation
- public access protection
- versioning
- encryption
- lifecycle management

That separation also makes the security intent easier to review.
