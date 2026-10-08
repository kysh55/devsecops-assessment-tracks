# Disaster Recovery Plan – Multi-Cloud Security

## 1. Recovery Targets

For the representative AWS database application, the proposed recovery targets are:

**RTO** → Restore application service within 4 hours.  
**RPO** → Accept a maximum of 1 hour of data loss.

These are proposed targets and must be confirmed with the business owner.
To achieve near-zero RTO and RPO across multi-cloud environments, we must implement an Active-Active Cross-Cloud Application Architecture 
combined with a Distributed Database Mirroring Strategy and there will be challenges in the setup.

## 2. Backup Approach

Resource --> Backup Approach --> Protection 

**Database** --> Automated database backups and point-in-time recovery  || KMS encryption, restricted access
**Storage** --> Versioning, backup policies and approved replication || Encryption and least-privilege access
**Compute** --> Rebuild servers using Terraform and approved application images || No dependency on manual server configuration
**Terraform State** --> Versioned remote state with recovery protection || Encryption, restricted IAM and access logging

Backups should be stored separately from the affected workload, protected against accidental deletion and tested regularly.

## 3. Recovery Architecture

To support the proposed RTO/RPO:

- **Database** → Automated backups and point-in-time recovery.
- **Application** → Redeploy using Terraform and GitLab CI/CD.
- **Availability** → Use Multi-AZ deployment for critical AWS workloads.
- **Backup** → Maintain protected recovery copies based on workload criticality.
- **Validation** → Perform regular restore testing against the recovery targets.

## 4. Multi-Cloud Recovery

Use the same backup, recovery and access-control standards across cloud providers.

**AWS** → AWS Backup / RDS backups  
**Azure** → Azure Backup / Azure SQL backups  
**GCP** → Backup and DR / Cloud SQL backups

For the current environment, recovery within the same cloud provider is preferred.

Cross-cloud failover is not recommended as the default because it requires additional application compatibility, data replication, networking and operational support.

## 5. Full Account / Subscription / Project Recovery (Worst-case disaster recovery event)

If an entire cloud environment is lost:

1. Recreate the cloud environment and required access using approved recovery procedures.
2. Restore infrastructure using Terraform from Git.
3. Restore IAM, networking, encryption and monitoring configurations.
4. Redeploy applications using GitLab CI/CD.
5. Restore databases and storage from protected backups.
6. Validate application functionality, security controls and data integrity before reopening access.

## 6. Git Recovery vs Data Loss

**Recoverable from Git:**
- Terraform infrastructure configuration
- Network and IAM configuration
- Security policies
- CI/CD pipeline definitions
- Application deployment configuration

**Not recoverable from Git alone:**
- Database records and application data
- Uploaded files
- Secrets and encryption keys
- Runtime logs and temporary data
- Terraform state, unless separately backed up

These require separate backup and recovery arrangements.

## 7. DR Testing

- Test database and storage restoration regularly.
- Validate application recovery against RTO/RPO targets.
- Confirm backup encryption and recovery permissions.
- Record test results, issues and corrective actions.