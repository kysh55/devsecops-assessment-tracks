# Encryption & Key Management

## 1. Approach

Use each cloud provider's native Key Management Service (KMS) rather than building a separate central key management platform.

**AWS** → AWS KMS  
**Azure** → Azure Key Vault  
**GCP** → Cloud KMS

Apply the same encryption standards across all three clouds.

## 2. Encryption Controls

**Data at Rest** → Encrypt databases, storage and backups using approved encryption keys.

**Data in Transit** → Use TLS 1.2 or higher for application, database and service communication.

**Secrets** → Store credentials in approved secrets managers, not Terraform code or GitLab variables.

## 3. Key Management

**Recommended:** Per-cloud native KMS with separate keys for sensitive workloads and environments.

**Centralised Key Management** → Consistent oversight, but introduces additional dependencies and potentially a larger impact if central access is compromised.

**Per-Cloud KMS** → Easier integration, simpler operations and better separation between cloud environments.

Key controls:
- Restrict key access using least-privilege IAM.
- Separate key administrators from application users.
- Enable key rotation where supported.
- Log key usage and administrative changes.
- Protect keys from accidental deletion.

## 4. BYOK / HYOK

**BYOK (Bring Your Own Key)** → Consider for highly sensitive workloads requiring customer-controlled key material.

**HYOK (Hold Your Own Key)** → Consider only where regulations or data ownership requirements justify keeping keys outside the cloud provider.

For normal workloads, cloud-native KMS is sufficient. BYOK/HYOK should not be introduced without a clear requirement.

## 5. Data Classification

Classification | Examples | Protection 

Public | Published information | Standard security controls 
Internal | Operational information | Encryption and restricted access
Confidential | Financial and personal data | Encryption, dedicated access controls and audit logging
Restricted | Highly sensitive government data | Stronger key isolation, access approval and monitoring

## 6. COTS Encryption Gap

The COTS financial reconciliation tool cannot support encryption at rest due to a vendor driver limitation.

Until the vendor provides a supported fix:

- Isolate the application and database within a restricted private network.
- Allow access only from approved application identities.
- Encrypt supported storage layers, backups and network connections.
- Enable access logging and monitor unusual activity.
- Record the remaining risk with a named owner and approved exception.

Storage-level encryption must be tested for compatibility. It should not be claimed as resolving the encryption gap until verified.

## 7. Data Residency & Sovereignty

For government workloads:

- Keep data and backups within approved geographic regions.
- Confirm whether cross-region replication is permitted.
- Restrict cross-border data transfers.
- Confirm key storage and administration meet applicable government requirements.
- Validate the requirements against the actual programme security standards before deployment.