# Cross-Cloud IAM Design

## 1. Identity Approach

Use one central corporate identity provider, such as Microsoft Entra ID, for employee access across AWS, Azure and GCP.

**Human Access** → Corporate SSO + MFA → Cloud IAM Roles  
**CI/CD Access** → GitLab OIDC → Short-lived Cloud Credentials

Avoid permanent access keys and separate user accounts in each cloud.

## 2. Workload Identity Federation

Cloud | Authentication | Access Control

AWS | GitLab OIDC → AWS STS | IAM Role
Azure | GitLab OIDC → Entra Workload Identity Federation | Azure RBAC
GCP | GitLab OIDC → Workload Identity Federation | GCP IAM

Each pipeline gets temporary access only to the cloud resources required for deployment.

## 3. Human Access

**Read-Only** → Developers and auditors who need visibility  
**Deployer** → Approved deployment activities  
**Administrator** → Restricted platform administration

Access is assigned through central identity groups and mapped to cloud-specific roles.

- MFA required for human access.
- No shared administrator accounts.
- Access reviewed periodically.
- Remove access when no longer required.

## 4. Privileged and Emergency Access

**Normal Admin Access** → Request → Approval → Time-limited elevation → Audit
Use just-in-time privileged access instead of permanent administrator permissions.

**Break-Glass Access:**
- Maintain dedicated emergency accounts with strong authentication.
- Restrict and monitor their use.
- Alert security when activated.
- Review all actions after the incident.

## 5. CI Service Account Remediation

**Current Risk:** The CI identity has wildcard permissions, allowing more access than required.

**Replacement – Terraform Deployment Role**

For an AWS S3 deployment example:

**Allow:**
- `s3:ListBucket` on the approved bucket
- `s3:GetObject`, `s3:PutObject` on the approved object path
- Required read access to the approved Terraform state location

**Deny by omission:**
- Unrelated S3 buckets
- IAM administrator permissions
- Database access
- Unnecessary resource deletion

Apply the same least-privilege principle using Azure RBAC and GCP IAM.
Permissions must be adjusted to the actual Terraform resources being deployed.

## 6. Audit and Governance

**Central SSO** → Human authentication records  
**Cloud Audit Logs** → AWS CloudTrail, Azure Activity Logs, GCP Cloud Audit Logs  
**Access Reviews** → Validate permissions and remove unused access

Keep approval records and privileged-access logs available for security review.

## 7. Multi-Cloud Maintenance

Maintain one common access model:

**Central Identity → Cloud-Specific Roles → Least Privilege → Audit**

Use reusable Terraform modules for role assignments and separate deployment identities for each environment.
This keeps access management consistent across clouds without forcing all three providers to use the same IAM implementation.