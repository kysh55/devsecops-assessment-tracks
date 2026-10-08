# Architecture Narrative – Kubernetes & Container Security

## 1. Architecture Overview

The proposed architecture focuses on securing Kubernetes workloads from code development to production deployment and runtime monitoring.

The design uses **GitLab CI/CD, container security scanning, AWS EKS and cloud-native security services** to provide a secure and maintainable platform.

**Reference Repository:** [Kubernetes Goat](https://github.com/madhuakula/kubernetes-goat)

## 2. High-Level Architecture (HLD)

**Developer → GitLab CI/CD → Security Scans → Approval → Container Registry → AWS EKS**

The architecture includes:

- **Pipeline Security:** Gitleaks for secrets scanning, Trivy for container vulnerabilities and Checkov for infrastructure and Kubernetes configuration checks.
- **Identity:** AWS IAM for cloud access and Kubernetes RBAC for cluster permissions.
- **Network:** Public access only through an approved ingress point; internal services are restricted using NetworkPolicies.
- **Secrets:** AWS Secrets Manager with External Secrets Operator (ESO).
- **Runtime Security:** Falco for suspicious container activity.
- **Monitoring:** Kubernetes audit logs, Falco alerts and cloud security logs forwarded to a central SIEM.

## 3. Low-Level Architecture (LLD) – EKS Application

The LLD focuses on one representative application deployed in AWS EKS.

**Application Flow:**

External User → HTTPS (443) → Ingress / Load Balancer → Kubernetes Service → Application Pod

**Deployment Flow:**

Developer → GitLab → Security Scans → Image Build → Container Registry → Approval → EKS Deployment

Component | Design 

Kubernetes | AWS EKS with application workloads in dedicated namespaces
Ingress | HTTPS on port 443 through an approved ingress controller
Application | Pods running with non-root users and restricted privileges
Access Control | Kubernetes RBAC with least-privilege roles and service accounts
Network Security | Default-deny NetworkPolicies with only required traffic allowed
Container Images | Scan images before deployment and use approved image versions
Secrets | AWS Secrets Manager → ESO → Kubernetes Secrets → Application Pod
Runtime Detection | Falco monitors suspicious container activity
Monitoring | Kubernetes audit logs, Falco alerts and AWS logs forwarded to SIEM

## 4. Security Boundaries

**External Boundary:** Users access applications only through the approved ingress.
**Cluster Boundary:** Kubernetes API access is restricted to authorised identities.
**Namespace Boundary:** Applications are isolated using namespaces, RBAC and NetworkPolicies.
**Workload Boundary:** Pods use restricted security contexts and dedicated service accounts.
**Secrets Boundary:** Secrets are managed externally and accessed only by approved workloads.
**Environment Boundary:** Production and non-production environments have separate access controls.

## 5. Key Design Decisions

Decision | Reason 

AWS EKS | Managed Kubernetes control plane reduces operational overhead
GitLab security checks | Detects security issues before production deployment
Least-privilege RBAC | Reduces unauthorised cluster access
Default-deny NetworkPolicies | Limits unnecessary communication between workloads
AWS Secrets Manager + ESO | Avoids storing application secrets directly in Git
Falco | Detects suspicious activity during runtime
Central SIEM | Provides one place to investigate security events

## 6. Alternative Considered

**Alternative:** Introduce a service mesh to enforce workload-to-workload security.

**Why not selected:** It adds operational complexity and additional components to manage.

**Preferred Approach:** Start with Kubernetes NetworkPolicies, restricted RBAC and controlled ingress.

A service mesh can be considered later if workload identity, mutual TLS or advanced traffic management becomes a confirmed requirement.

## 7. Operational Considerations

- Use reusable GitLab pipeline templates and Kubernetes deployment configurations.
- Scan container images and Kubernetes manifests before deployment.
- Review RBAC permissions and network rules regularly.
- Forward important runtime and audit events to the central SIEM.
- Document security exceptions with an owner and expiry date.
- Test application recovery against agreed RTO/RPO targets.

## 8. Conclusion

The proposed architecture follows a simple principle:
**Secure Pipeline + Least-Privilege Access + Workload Isolation + Runtime Detection + Central Monitoring**
This approach reduces Kubernetes security risks while keeping the platform practical, scalable and easy to maintain.

