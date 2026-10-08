# Pipeline Design – Multi-Cloud Security

## 1. Pipeline Flow

Developer → GitLab → Security Checks → Build → Dependency Scan → SBOM → Approval → Terraform Deploy

## 2. Security Controls

**Gitleaks** → Detect secrets committed to source code  
**Checkov** → Scan Terraform misconfigurations across AWS, Azure and GCP  
**OPA/Conftest** → Enforce common security policies across clouds  
**Trivy** → Scan application dependencies and generate SBOM  
**GitLab Protected Environment** → Require security approval before production deployment

## 3. Hard Fail vs Soft Fail

**Hard Fail** → Exposed credentials, public databases, excessive IAM permissions and Critical security violations.

**Soft Fail** → Medium/Low findings that do not create immediate security exposure.

Exceptions require approval, a risk owner and a remediation date.

## 4. Production Approval

Production deployment is allowed only after:

- Required security checks pass.
- Critical findings are resolved or formally approved as exceptions.
- An authorised reviewer approves the protected environment.

## 5. Scaling Across 100 Workloads

Use the same pipeline template across all projects.

**High-risk workloads** → Full security review and approval.  
**Medium-risk workloads** → Automated checks with review of significant findings.  
**Low-risk workloads** → Automated checks with periodic sampling.

This keeps security checks consistent without delaying every deployment.

## 6. SBOM and Remediation

Generate an SBOM for application dependencies and retain it with the pipeline artifacts.

**Trivy Findings → Severity Review → Remediation Backlog → Fix → Rescan**

Track unresolved findings until they are fixed or formally accepted.

## 7. Multi-Cloud Support

The same security checks apply across AWS, Azure and GCP.

**AWS** → Terraform + Checkov + AWS deployment identity  
**Azure** → Terraform + Checkov + Azure deployment identity  
**GCP** → Terraform + Checkov + GCP deployment identity

Use OIDC-based short-lived credentials instead of storing permanent cloud access keys.
Cloud-specific Terraform policies can differ, but the security requirements and approval process remain consistent.
