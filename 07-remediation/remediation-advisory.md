# Remediation Advisory

The following three findings are prioritised based on the potential impact on cloud resources and sensitive data.

## F01 – RDS Database Publicly Accessible

**Risk:**  
The database is exposed outside the approved network, increasing the risk of unauthorised access.

**Remediation:**
1. Set `publicly_accessible = false` in Terraform.
2. Place the database in private subnets.
3. Restrict the database security group to approved application sources.
4. Run Terraform plan, review the changes and apply.

**Evidence:**
- Updated Terraform configuration.
- Terraform plan and apply results.
- AWS RDS configuration showing public access disabled.
- Security group rules confirming restricted access.

## F02 – S3 Public Access Block Missing

**Risk:**  
The S3 bucket could become publicly accessible through an incorrect bucket policy or ACL.

**Remediation:**
1. Configure `aws_s3_bucket_public_access_block`.
2. Enable all four public access block settings.
3. Review the bucket policy and remove unnecessary public permissions.
4. Run Terraform plan and apply.

**Evidence:**
- Updated Terraform configuration.
- AWS S3 Public Access Block settings.
- Checkov scan result confirming the issue is resolved.

## F03 – RDS Encryption at Rest Not Enabled

**Risk:**  
Database storage does not have the required encryption protection.

**Remediation:**
1. Create an encrypted database snapshot.
2. Restore the snapshot to a new encrypted RDS instance using AWS KMS.
3. Test database connectivity and application functionality.
4. Migrate to the encrypted instance through an approved change window.

**Evidence:**
- RDS configuration showing encryption enabled.
- KMS key configuration.
- Migration and application validation results.
- Updated Terraform configuration.