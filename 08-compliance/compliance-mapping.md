# Compliance Mapping

Finding | Control Domain | Control Objective | Compliance Evidence | Exception Evidence

F01 – Service account bound to `cluster-admin` | Access Control | Enforce least privilege | RBAC manifests, Role/RoleBinding, access review | Approved exception, business justification, risk owner, expiry date
F02 – Privileged container enabled | Container Security | Restrict privileged workloads | Deployment manifest, securityContext, admission-policy result | Approved workload exception, justification, compensating controls
F03 – Pod uses `hostPID` | Container Security | Maintain workload-to-host isolation | Deployment manifest showing `hostPID` disabled | Approved requirement, risk owner, review/expiry date
F04 – Host root filesystem mounted | Host Security | Restrict unnecessary host access | Deployment manifest, hostPath configuration, admission-policy result | Business justification, restricted mount details, approved exception
F05 – Privilege escalation allowed | Container Security | Prevent unnecessary privilege escalation | Deployment manifest showing `allowPrivilegeEscalation: false` | Approved exception with justification and compensating controls

## Evidence Collection

Evidence would be gathered from:

- Git-controlled Kubernetes manifests
- CI/CD security scan results
- Kubernetes RBAC configuration
- Admission-control results
- Approved exception/risk records