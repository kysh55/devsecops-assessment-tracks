# Monitoring & Incident Response Plan

## 1. Monitoring Approach

Use cloud-native security tools for detection and send important alerts to one central SIEM for investigation.

**AWS** → CloudTrail + AWS Config + Security Hub  
**Azure** → Activity Logs + Azure Policy + Defender for Cloud  
**GCP** → Cloud Audit Logs + Security Command Center  
**Central SIEM** → Collect, correlate and investigate security alerts

## 2. Continuous Security Monitoring (CSPM)

Monitor cloud configurations across AWS, Azure and GCP.

Focus on:

- Public storage and database exposure
- Excessive IAM permissions
- Disabled encryption
- Open firewall rules
- Missing audit logs and backup controls

**CSPM Findings → Severity Review → Remediation Backlog → Fix → Rescan**

Use common security policies across clouds while keeping provider-specific checks where required.

## 3. Central Logging & Alert Management

Collect:

**Identity Logs** → Login failures, privileged access and role changes  
**Cloud Audit Logs** → Resource creation, deletion and configuration changes  
**Network Logs** → Suspicious inbound and outbound connections  
**Database Logs** → Unauthorised or unusual database access  
**Security Alerts** → CSPM findings and threat detections

To reduce alert fatigue:

- **Critical/High** → Immediate investigation
- **Medium** → Review during regular security operations
- **Low** → Track for reporting and trends
- Group duplicate alerts and tune noisy rules.

## 4. Incident Scenario – Compromised CI Service Account

**Scenario:** An attacker compromises the CI service account with wildcard permissions and uses it to access a database.

### Step 1 – Detect

- SIEM identifies unusual CI identity activity.
- Cloud audit logs show unexpected database access.
- Security team confirms the activity is unauthorised.

### Step 2 – Contain

- Disable the compromised CI identity and revoke active access where supported.
- Block unauthorised database access.
- Pause affected deployments.
- Preserve logs and evidence before making further changes.

### Step 3 – Eradicate

- Identify how the CI identity was compromised.
- Remove wildcard IAM permissions.
- Rotate exposed credentials and secrets.
- Fix the security gap that allowed the compromise.

### Step 4 – Recover

- Restore the approved least-privilege CI identity.
- Validate database integrity and check for unauthorised changes.
- Resume deployments after security verification.
- Continue monitoring for suspicious activity.

## 5. Evidence Collection

Preserve:

- GitLab pipeline and authentication logs
- AWS CloudTrail / Azure / GCP audit logs
- Database access and query logs, where enabled
- IAM role and permission changes
- Relevant network logs
- Incident timeline and actions taken

## 6. ITSO Communication

### During the Incident

Provide short updates covering:

- What happened and when it was detected
- Affected systems and possible business impact
- Containment actions completed
- Current investigation status
- Next actions and decisions required

### Post-Incident Report

Include:

- Confirmed root cause
- Incident timeline
- Systems and data affected
- Actions taken to contain and recover
- Any remaining risks
- Preventive actions, owners and target dates

## 7. Recovery Validation

Before closing the incident:

- Confirm the compromised identity can no longer access resources.
- Verify database integrity and application functionality.
- Confirm the replacement CI identity has only required permissions.
- Verify security monitoring and alerts are working.
- Obtain incident closure approval through the established process.