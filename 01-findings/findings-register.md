# Kubernetes Security Findings Register

## Assessment Scope

This review assesses the Kubernetes Goat repository as the primary reference 

Findings are prioritised based on:
- potential impact,
- exploitability,
- blast radius,
- exposure of privileged resources,
- and likelihood of the weakness being abused.

The purpose of this register is to identify the highest-risk security weaknesses
and provide practical remediation without unnecessarily redesigning the entire
platform.

## Findings Summary

ID: F01
Severity: Critical
Finding: Service account bound to `cluster-admin`
Exact Location: `scenarios/insecure-rbac/setup.yaml`
Control Domain: RBAC / Access Control
Risk: Compromise of the service account can provide unrestricted administrative access across the Kubernetes cluster.
Remediation: Remove the `cluster-admin` ClusterRoleBinding. Create a least-privilege Role/RoleBinding containing only the resources and verbs required by the workload.

ID: F02
Severity: Critical
Finding: Privileged container enabled
Exact Location: `scenarios/system-monitor/deployment.yaml`
Control Domain: Container Security
Risk: `privileged: true` significantly weakens container isolation and can provide extensive host-level capabilities.
Remediation: Remove privileged mode. Use the minimum required Linux capabilities and enforce the restriction through admission policy.

ID: F03
Severity: High 
Finding: Pod shares host PID namespace
Exact Location: `scenarios/system-monitor/deployment.yaml` 
Control Domain: Container Isolation
Risk: `hostPID: true` exposes host process information to the container and increases the host attack surface.
Remediation: Disable `hostPID` unless there is a documented operational requirement. Enforce the restriction through admission policy.

ID: F04
Severity: Critical
Finding: Host root filesystem mounted into container
Exact Location: `scenarios/system-monitor/deployment.yaml`
Control Domain: Host Security
Risk: A `hostPath` mount exposing `/` provides access to highly sensitive node filesystem content.
Remediation: Remove the host root mount and use purpose-specific Kubernetes volumes. Restrict dangerous `hostPath` mounts using admission policy.

ID: F05
Severity: High
Finding: Privilege escalation explicitly allowed
Exact Location: `scenarios/system-monitor/deployment.yaml`
Control Domain: Container Security
Risk: `allowPrivilegeEscalation: true` permits processes to gain privileges beyond those of their parent process.
Remediation: Set `allowPrivilegeEscalation: false` and apply a restricted container security context where application compatibility permits.

ID: F06
Severity: High
Finding: Secret material stored directly in Kubernetes manifest
Exact Location: `scenarios/system-monitor/deployment.yaml`
Control Domain: Secrets Management
Risk: Secret values committed to source control may remain accessible through repository history even after the current file is changed.
Remediation: Remove secret values from Git, rotate exposed credentials and retrieve secrets from an approved external secret-management service.

ID: F07
Severity: High
Finding: RBAC rule uses wildcard resource access
Exact Location: `scenarios/hunger-check/deployment.yaml`
Control Domain: RBAC / Access Control
Risk: The RBAC configuration grants access to `resources: ["*"]`, which is broader than a typical application requires.
Remediation: Explicitly enumerate only the Kubernetes resources and verbs required by the application and scope permissions to the namespace where possible.

ID: F08
Severity: High
Finding: Application credentials committed as Kubernetes Secrets
Exact Location: `scenarios/hunger-check/deployment.yaml`
Control Domain: Secrets Management
Risk: Credentials are included directly in deployment configuration, increasing exposure through Git and CI/CD systems.
Remediation: Move credentials to an external secrets manager, rotate existing credentials and restrict secret retrieval through workload identity.

ID: F09
Severity: Medium
Finding: CPU and memory resource controls are not enforced
Exact Location: `scenarios/hunger-check/deployment.yaml`
Control Domain: Availability / Resource Governance
Risk: Resource controls are absent/commented, allowing uncontrolled consumption of node resources.
Remediation: Configure realistic CPU/memory requests and limits based on measured usage. Add namespace `LimitRange` or `ResourceQuota` where appropriate.

ID: F10
Severity: Medium 
Finding: Internal service exposed using `NodePort`
Exact Location: `scenarios/internal-proxy/deployment.yaml`
Control Domain: Network Security
Risk: `NodePort` exposes the service through Kubernetes node addresses and increases the reachable attack surface.
Remediation: Use `ClusterIP` for internal-only services and expose external services through approved ingress/load-balancer controls.