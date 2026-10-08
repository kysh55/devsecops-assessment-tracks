# Shift-Left and Software Supply Chain Security Design

## 1. Objective

The current environment relies on CI/CD for workload deployment, and there are potential 
gaps in Kubernetes RBAC setup and the absence of cluster-wide admission controls.

The objective is to introduce security controls at two levels:

1. **Shift-left controls** - identify insecure code, Kubernetes configuration,
   secrets and container vulnerabilities before deployment.
2. **Cluster enforcement** - prevent non-compliant or untrusted workloads from
   being admitted even if the CI/CD pipeline is bypassed.

The design uses a small number of controls which serve the puspose at each build stage, 
and does not require a redesign on the pipelines. 

## Security Controls

**Gitleaks** → Detect secrets committed to source  
**Checkov** → Scan Terraform and Kubernetes misconfigurations  
**Conftest/OPA** → Enforce custom policies such as blocking `cluster-admin`  
**Docker/BuildKit** → Build the application container image  
**Trivy** → Scan image vulnerabilities + generate SBOM  
**Cosign** → Sign and verify trusted container images  
**GitLab OIDC** → Obtain short-lived AWS credentials without stored access keys  
**RBAC** → Restrict what the CI/CD identity can perform inside EKS  
**GitLab Protected Environment** → Require approval for production deployment  
**Kyverno** → Final cluster-side policy and signed-image enforcement

## 3. Proposed Pipeline

Code → Gitleaks/Checkov/Conftest → Build → Trivy/SBOM → Cosign
     → Approval → GitLab OIDC → EKS RBAC → Kyverno → Workload

```
Developer
    |
    v
Merge Request
    |
    +--> Secret Scan
    |
    +--> IaC / Kubernetes Policy Check
    |
    v
Build Container
    |
    +--> Container Vulnerability Scan
    |
    +--> Generate SBOM
    |
    v
Push Immutable Image
    |
    +--> Sign Image
    |
    v
Protected Environment Approval
    |
    v
Deploy to Kubernetes
    |
    v
Admission Policy
    |
    +--> Policy compliant?
    +--> Trusted/signed image?
    |
    v
EKS
```