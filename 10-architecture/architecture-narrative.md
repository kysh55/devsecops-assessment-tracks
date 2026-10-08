# Architecture Narrative – Multi-Cloud Platform Security

## 1. Architecture Overview

The proposed architecture provides a common security approach across AWS, Azure and GCP.
AWS is the current primary environment, with Azure and GCP planned for onboarding.
The design uses Terraform, GitLab CI/CD and cloud-native security services to maintain consistent controls without adding unnecessary complexity.

## 2. High-Level Architecture (HLD)

**Developer → GitLab CI/CD → Security Checks → Approval → Cloud Deployment**

The architecture includes:

- **Identity:** Corporate SSO and MFA for users; OIDC federation for CI/CD.
- **Pipeline Security:** Gitleaks, Checkov, OPA/Conftest and Trivy.
- **Cloud Environments:** Separate production and non-production environments with controlled access.
- **Network:** Public entry points where required, with application and database resources isolated in private networks.
- **Data Protection:** Cloud-native KMS, secrets management and encryption.
- **Monitoring:** Cloud audit logs and security findings forwarded to a central SIEM.

The same security principles apply across all three cloud providers, using their respective native services.

## 3. Low-Level Architecture (LLD) – AWS Application and Database

The LLD focuses on one representative AWS application.

**Application Flow:**

User → HTTPS (443) → Application → Database (Private Network)

**Deployment Flow:**

Developer → GitLab → Security Scans → Approval → OIDC IAM Role → Terraform → AWS

Component | Design

Network | Application and RDS deployed in separate private subnets
Application Access | HTTPS on port 443 through an approved public entry point
Database Access | Only the application security group can connect to the required database port
CI/CD Identity | GitLab OIDC with a scoped AWS IAM deployment role
Secrets | AWS Secrets Manager; application retrieves secrets using its assigned identity
Encryption | AWS KMS for supported storage and RDS encryption; TLS for communication
Monitoring | CloudTrail, VPC Flow Logs, database logs and central SIEM
Deployment | Terraform plan review and approval before production apply

## 4. Security Boundaries

- **User Boundary:** External users access only the approved application entry point.
- **Network Boundary:** Databases remain private and are not directly exposed to the internet.
- **Identity Boundary:** Human access and CI/CD access use separate identities and permissions.
- **Environment Boundary:** Production and non-production access are separated.
- **Cloud Boundary:** AWS, Azure and GCP maintain separate cloud identities and security controls under common governance.

## 5. Key Design Decisions

Decision | Reason
Cloud-native security services | Easier integration and lower operational complexity |
OIDC instead of static CI credentials | Reduces long-lived credential exposure |
Private database access | Reduces external attack exposure |
Common Terraform security policies | Supports consistent controls across 100 workloads |
Central SIEM | Provides a common view of security incidents |
Separate production approval | Prevents unauthorised production changes |

## 6. Alternative Considered

**Alternative:** Use a single centralised security and key-management platform for all three clouds.

**Why not selected:** It introduces additional integration, availability and operational dependencies.

**Preferred Approach:** Use cloud-native identity, encryption and security services, with common policies and central monitoring.

This keeps the design practical while allowing each cloud to use its supported security capabilities.

## 7. Operational Considerations

- Reuse approved Terraform modules and GitLab pipeline templates.
- Apply security checks before deployment.
- Review IAM permissions and network rules regularly.
- Maintain audit logs and security findings centrally.
- Document security exceptions with an owner and expiry date.
- Test backup and recovery procedures against agreed RTO/RPO targets.

## 8. Conclusion

The proposed architecture follows a simple principle:

**Common Security Standards + Cloud-Native Controls + Automated Validation + Central Monitoring**

This approach supports secure multi-cloud onboarding, reduces manual effort and provides a maintainable security model as the environment grows.