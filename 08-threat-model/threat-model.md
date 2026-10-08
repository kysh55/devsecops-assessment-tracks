# Threat Model – AWS Database Application

## 1. Scope

This threat model covers the AWS database-backed application reviewed in TerraGoat, focusing on application access, IAM, networking, storage and database security.

## 2. Data Flow Diagram

External User → Public Entry Point → Application (EC2) → RDS Database

GitLab CI/CD → AWS IAM Role → Terraform → AWS Resources

Application → S3 Storage

## 3. STRIDE Threat Analysis

| ID | STRIDE | Threat | Likelihood | Impact | Security Control |

T01 | Spoofing | Stolen CI credentials used to access AWS | High | Critical | OIDC federation and short-lived credentials
T02 | Tampering | Unauthorised changes to Terraform infrastructure | Medium | High | Protected branches, code review and Terraform plan approval
T03 | Repudiation | Cloud resource changes cannot be traced | Medium | High | CloudTrail and central audit logging
T04 | Information Disclosure | Publicly accessible RDS database exposes sensitive data | High | Critical | Private subnets and restricted security groups
T05 | Information Disclosure | S3 bucket becomes publicly accessible | Medium | High | S3 Block Public Access and restricted bucket policies
T06 | Denial of Service | Application or database becomes unavailable due to excessive traffic | Medium | High | Rate limiting, monitoring and appropriate capacity controls
T07 | Elevation of Privilege | CI identity with excessive IAM permissions gains wider AWS access | High | Critical | Least-privilege IAM roles
T08 | Information Disclosure | Unencrypted database storage exposes sensitive information | Medium | High | RDS encryption using AWS KMS

## 4. Residual Risk

**T04 – Database Exposure:** Risk is reduced after disabling public access and restricting database connectivity. Misconfigured security groups or compromised application identities may still allow unauthorised access.

**Residual Risk:** • Ensure that any risk left unmitigated is formally documented, quantified, and accepted by business and product stakeholders, not just the engineering team.
Use real time monitoring, logging, and APM tools in production to detect when a residual risk escalates into an active threat or failure.

## 5. Validation

- Review Terraform security configurations.
- Run Checkov scans.
- Verify IAM permissions and network restrictions.
- Confirm encryption and audit logging are enabled.
- Retest identified weaknesses after remediation.