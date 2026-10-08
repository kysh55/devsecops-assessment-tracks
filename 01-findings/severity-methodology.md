# Severity Methodology


Findings are prioritised using a practical risk-based approach based on:

- **Impact** – potential effect on confidentiality, integrity, availability, or platform control.
- **Exploitability / Likelihood** – how realistically the weakness could be abused.
- **Blast Radius** – whether compromise is limited to a workload/namespace or can extend to the Kubernetes node or cluster.
- **Privilege Level** – level of access an attacker could obtain after exploitation.
- **Exposure** – whether the vulnerable component is externally reachable or accessible through common application or CI/CD paths.

The objective is to prioritise remediation effort toward weaknesses that create the most significant attack paths rather than treating every configuration issue equally.

## Severity Levels


**Critical** :  A weakness that could reasonably lead to cluster-wide administrative access, Kubernetes node compromise, or control of the container runtime. Immediate remediation or explicit risk acceptance is required. | `cluster-admin` access, unrestricted ClusterRole, privileged container with host access, container runtime socket exposure 
**High** : A significant security weakness that can expose sensitive resources, increase privilege, or substantially increase the impact of a compromised workload, but normally requires additional conditions to achieve full cluster compromise. | Excessive RBAC permissions, committed secrets, `hostPID`, `hostNetwork`, privilege escalation 
**Medium** : A security weakness that increases attack surface, reduces resilience, or weakens security/reproducibility but does not normally provide direct cluster or node compromise by itself. | Missing resource limits, unnecessary `NodePort`, mutable image tags 
**Low** : A defence-in-depth or hardening improvement with limited immediate security impact. It should be addressed as part of normal platform security improvement. | Minor configuration hardening or security hygiene issues 

## Prioritisation Approach

Remediation priority is determined by both severity and the role of the affected component.

For example:

- A CI/CD identity with `cluster-admin` is treated as **Critical** because compromise of the delivery pipeline could provide control over the entire cluster.
- A container runtime socket exposed to an application workload is **Critical** because it can create a direct path from workload compromise to node/runtime control.
- A mutable image tag is generally **Medium** because it weakens provenance and deployment reproducibility but does not independently provide cluster compromise.

## Context and Exceptions

Severity is not assigned purely from the presence of a Kubernetes configuration flag.

Some security or operational workloads may legitimately require elevated permissions such as `hostPID`, `hostNetwork`, `hostPath`, or runtime visibility.

Where such access is technically required, the configuration should be treated as a **controlled exception** rather than automatically removed.

The exception should include:

- documented business/technical justification,
- minimum required privileges,
- workload and namespace isolation,
- restricted service-account permissions,
- runtime monitoring,
- named risk owner,
- and periodic review or expiry.

This approach avoids weakening the platform-wide security baseline while still supporting workloads with legitimate privileged requirements.