# Compensating Controls – COTS Encryption Gap

## 1. Scenario

The COTS financial reconciliation application cannot currently support encryption at rest due to a vendor driver limitation.

Until the vendor provides a supported fix, the application requires additional security controls and formal risk acceptance.

## 2. Compensating Controls

| Control | Implementation |

Network isolation | Keep the application and database in private subnets. Allow only approved connections.
Access restriction | Use dedicated service identities, least-privilege IAM and MFA for administrative access.
Encryption in transit | Enforce TLS for supported application and database connections.
Monitoring | Enable database access logging and monitor suspicious activity through the central SIEM.
Backup protection | Encrypt backups where supported, restrict access and test recovery.

These controls reduce exposure but do not replace encryption at rest.

## 3. Residual Risk

**Recommended Rating:** High, pending validation of the compensating controls.

Sensitive data remains unencrypted at rest, so the risk cannot be considered fully resolved.

**Risk Owner:** Application / Business Owner  
**Approval:** Authorised security and governance stakeholders

## 4. Time-Bound Remediation

| Timeline | Action |
0–30 days | Implement network restrictions, access controls and monitoring.
30–60 days | Validate controls, test backup protection and obtain formal risk acceptance.
60–90 days | Review the vendor's encryption fix or supported upgrade options.
At exception expiry | Implement the approved fix or seek renewed risk approval with justification.

## 5. Exception Governance

This exception applies only to the identified COTS application.

- Record the affected application and encryption limitation.
- Assign a named risk owner and expiry date.
- Document the approved compensating controls.
- Review the exception before expiry.
- Require separate assessment and approval for any other workload requesting the same exception.

The exception must not be treated as a general exemption from encryption requirements across the other workloads.