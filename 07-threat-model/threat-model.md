# Threat Model

## Simple Data Flow

Developer → GitLab CI/CD → Container Registry → EKS → Application

External User → Ingress / Load Balancer → Application → Backend Services

## STRIDE Threat Analysis

ID | STRIDE | Threat | Likelihood | Impact | Control | Residual Risk

T01 | Spoofing | Attacker uses compromised CI credentials to access EKS | Medium | Critical | OIDC, short-lived credentials | Low
T02 | Tampering | Container image is modified or replaced before deployment | Medium | High | Image signing and verification | Low
T03 | Repudiation | Kubernetes administrative actions cannot be traced | Medium | High | Kubernetes audit logging | Low
T04 | Information Disclosure | Secrets stored in manifests are exposed | High | High | External secret management | Low
T05 | Denial of Service | Workload consumes excessive CPU or memory | Medium | Medium | Resource requests and limits | Low
T06 | Elevation of Privilege | CI service account with `cluster-admin` is compromised | High | Critical | Least-privilege RBAC | Low
T07 | Elevation of Privilege | Privileged container gains access to the underlying node | High | Critical | Restricted security context | Low
T08 | Information Disclosure | Compromised workload communicates with unrelated workloads | Medium | High | Default-deny NetworkPolicy | Low