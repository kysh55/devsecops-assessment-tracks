# Remediation Advisory

The following three findings are selected for remediation.

## F01 – Service Account Bound to `cluster-admin`

**Risk:**  
The service account has unrestricted access across the Kubernetes cluster. If compromised, it could allow full cluster control.

**Remediation:**  
Remove the `cluster-admin` ClusterRoleBinding and replace it with a namespace-scoped Role and RoleBinding containing only the permissions required by the service account.

**Evidence:**  
Updated RBAC manifest showing removal of `cluster-admin` and the new restricted Role/RoleBinding.


## F02 – Privileged Container Enabled

**Risk:**  
The container runs with `privileged: true`, significantly reducing isolation between the container and the underlying node.

**Remediation:**  
Remove `privileged: true` and allow only the specific permissions or capabilities required by the workload.

**Evidence:**  
Updated deployment manifest showing the container running without privileged mode.


## F03 – Pod Uses Host PID Namespace

**Risk:**  
`hostPID: true` allows the pod to see processes running on the Kubernetes node, increasing the impact if the container is compromised.

**Remediation:**  
Remove `hostPID: true` unless there is a confirmed operational requirement. If required, document and control it as an approved exception.

**Evidence:**  
Updated deployment manifest showing `hostPID` removed or disabled.