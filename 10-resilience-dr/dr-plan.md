# Disaster Recovery Plan

## Recovery Targets

Component | RTO | RPO |

EKS platform / cluster configuration | 4 hours | 1 hour
Application workloads | 4 hours | 1 hour
Stateful application data | 4 hours | 1 hour

## Backup

**Control Plane / etcd**
- EKS control plane and etcd are managed by AWS.
- Kubernetes configuration should be maintained in Git so the required cluster state can be recreated.

**Stateful Workloads**
- Back up persistent application data and volumes.
- Backups should be stored separately from the cluster and tested regularly.

## Recovery

1. Rebuild the EKS infrastructure using the approved Terraform configuration.
2. Restore Kubernetes configuration from Git.
3. Restore required secrets from the external secrets store.
4. Deploy trusted container images from the registry.
5. Restore persistent application data from backup.
6. Validate application health and connectivity before restoring traffic.

## Git-Based Recovery

Git should remain the source of truth for:

- Terraform configuration
- Kubernetes manifests
- RBAC
- NetworkPolicies
- Security policies

This allows the environment to be rebuilt consistently rather than manually recreating configuration.

## Potential Data Loss

Data created after the most recent successful backup may be lost.
With an **RPO of 1 hour**, up to one hour of stateful application data could be lost during a disaster.
Logs or runtime data stored only inside failed pods or nodes may also be lost.