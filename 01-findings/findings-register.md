# Findings Register – Multi-Cloud Security

## Assessment Scope

This review assesses the Terra Goat repository as the primary reference 

Findings are prioritised based on:
- potential impact,
- exploitability,
- blast radius,
- exposure of privileged resources,
- and likelihood of the weakness being abused.

The purpose of this register is to identify the highest-risk security weaknesses
and provide practical remediation without unnecessarily redesigning the entire
platform.

### F01
**Severity:** Critical  
**Finding:** RDS database is publicly accessible  
**Exact Location:** `terraform/aws/db-app.tf` → `aws_db_instance.default`  
**Control Domain:** Database / Network Security  
**Risk:** The database could be reached from outside the approved network, increasing the risk of unauthorised access.  
**Remediation:** Disable public accessibility and allow connections only from approved application networks.

### F02
**Severity:** High  
**Finding:** S3 bucket has no public access block  
**Exact Location:** `terraform/aws/s3.tf` → `aws_s3_bucket.financials`  
**Control Domain:** Storage / Access Control  
**Risk:** Without a public access block, the bucket could be exposed through an incorrect policy or ACL.  
**Remediation:** Enable S3 Block Public Access and restrict access to approved IAM identities.

### F03
**Severity:** High  
**Finding:** RDS encryption at rest is not configured  
**Exact Location:** `terraform/aws/db-app.tf` → `aws_db_instance.default`  
**Control Domain:** Data Protection  
**Risk:** Database storage does not have the required encryption configuration, increasing the impact of storage exposure.  
**Remediation:** Enable RDS encryption at rest using AWS KMS. Migrate to an encrypted instance where required.

### F04
**Severity:** High  
**Finding:** SSH access is open to the internet  
**Exact Location:** `terraform/aws/ec2.tf` → `aws_security_group.web-node`  
**Control Domain:** Network Security  
**Risk:** Allowing SSH from any IP exposes the server to unnecessary external access attempts.  
**Remediation:** Remove unrestricted port 22 access. Use approved administrative access such as AWS Systems Manager.

### F05
**Severity:** High  
**Finding:** EC2 user data contains hardcoded secrets  
**Exact Location:** `terraform/aws/ec2.tf` → `aws_instance.web_host`  
**Control Domain:** Secrets Management  
**Risk:** Credentials included in user data may be exposed to people or systems with access to the instance configuration.  
**Remediation:** Remove embedded secrets, rotate exposed credentials and retrieve secrets securely at runtime.

### F06
**Severity:** High  
**Finding:** EBS volume encryption is not configured  
**Exact Location:** `terraform/aws/ec2.tf` → `aws_ebs_volume.web_host_storage`  
**Control Domain:** Data Protection  
**Risk:** The volume does not meet the required encryption baseline for stored data.  
**Remediation:** Enable EBS encryption using AWS KMS and migrate existing data to an encrypted volume.

### F07
**Severity:** High  
**Finding:** EKS control-plane audit logging is incomplete  
**Exact Location:** `terraform/aws/eks.tf` → `aws_eks_cluster.eks_cluster`  
**Control Domain:** Logging / Monitoring  
**Risk:** Important Kubernetes API activity may not be recorded, making suspicious actions harder to investigate.  
**Remediation:** Enable the required EKS control-plane log types and configure central collection and retention.

### F08
**Severity:** High  
**Finding:** RDS automated backup policy is insufficient  
**Exact Location:** `terraform/aws/db-app.tf` → `aws_db_instance.default`  
**Control Domain:** Backup / Resilience  
**Risk:** Database failure or accidental deletion could lead to data loss and longer recovery time.  
**Remediation:** Configure automated backups, an appropriate retention period and regular restore testing.

### F09
**Severity:** Medium  
**Finding:** S3 access logging is not configured  
**Exact Location:** `terraform/aws/s3.tf` → `aws_s3_bucket.financials`  
**Control Domain:** Logging / Audit  
**Risk:** Access to financial files may be difficult to trace during an investigation.  
**Remediation:** Enable S3 access logging or equivalent approved object-access audit logging.

### F10
**Severity:** High  
**Finding:** Azure SQL auditing retention is insufficient  
**Exact Location:** `terraform/azure/sql.tf` → `azurerm_sql_server.example`  
**Control Domain:** Logging / Compliance  
**Risk:** Audit records may not be retained long enough for investigation or compliance review.  
**Remediation:** Configure SQL auditing with the required retention period and approved log storage.