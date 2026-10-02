# 04 - S3 Security

## Security objective

The S3 configuration was built to reduce common cloud-storage risks such as accidental public exposure, unencrypted data, loss of previous versions, and uncontrolled storage growth.

## Public access protection

The bucket is associated with an S3 public access block:

~~~hcl
resource "aws_s3_bucket_public_access_block" "demo" {
  bucket = aws_s3_bucket.demo.id

  block_public_acls       = true
  block_public_policy     = true
  ignore_public_acls      = true
  restrict_public_buckets = true
}
~~~

### block_public_acls

Prevents new public ACLs from being used to expose the bucket.

### block_public_policy

Prevents bucket policies that would make the bucket public.

### ignore_public_acls

Causes public ACLs to be ignored.

### restrict_public_buckets

Adds additional restrictions when a bucket policy could allow public access.

## Versioning

Versioning is enabled:

~~~hcl
resource "aws_s3_bucket_versioning" "demo" {
  bucket = aws_s3_bucket.demo.id

  versioning_configuration {
    status = "Enabled"
  }
}
~~~

Benefits include:

- recovery from accidental overwrites
- recovery from accidental deletion
- retention of previous object versions
- additional resilience against destructive changes

## KMS encryption

A dedicated KMS key is created:

~~~hcl
resource "aws_kms_key" "s3" {
  description             = "KMS key for DevSecOps S3 bucket"
  deletion_window_in_days = 7
  enable_key_rotation     = true
}
~~~

Key rotation is enabled.

The S3 bucket uses AWS KMS for server-side encryption:

~~~hcl
apply_server_side_encryption_by_default {
  kms_master_key_id = aws_kms_key.s3.arn
  sse_algorithm     = "aws:kms"
}
~~~

## S3 bucket key

The encryption configuration enables:

~~~hcl
bucket_key_enabled = true
~~~

S3 Bucket Keys can reduce direct KMS request volume for S3 encryption operations.

## Lifecycle management

The project keeps previous versions for 90 days:

~~~hcl
noncurrent_version_expiration {
  noncurrent_days = 90
}
~~~

This provides a balance between recovery capability and long-term storage growth.

## Incomplete multipart uploads

Incomplete multipart uploads are removed after 7 days:

~~~hcl
abort_incomplete_multipart_upload {
  days_after_initiation = 7
}
~~~

This prevents abandoned upload parts from remaining indefinitely.

## Documented exceptions

The following checks are intentionally skipped in version 1.0:

- CKV2_AWS_62 - S3 event notifications
- CKV_AWS_18 - S3 access logging
- CKV_AWS_144 - cross-region replication
- CKV2_AWS_64 - explicit KMS key policy

These controls are not claimed to be unnecessary in production. They are simply outside the scope of this focused learning project.

A real production design should evaluate them according to:

- business requirements
- compliance requirements
- incident-response requirements
- availability objectives
- data sensitivity
- operational cost
