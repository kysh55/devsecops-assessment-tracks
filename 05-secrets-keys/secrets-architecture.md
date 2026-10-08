# Secrets Architecture

## Approach

Application secrets should not be stored in Git, Kubernetes manifests or
long-lived CI/CD variables.

For the current AWS/EKS environment, I would use:

**AWS Secrets Manager → External Secrets Operator → Kubernetes Secret → Pod**

Applications continue consuming normal Kubernetes Secrets, while the actual
secret values are managed outside the cluster.


## Options Considered

### Option 1 – AWS Secrets Manager + External Secrets Operator

**Pros**
- Fits the current AWS/EKS environment
- Secrets are managed outside Kubernetes
- Supports rotation and AWS IAM access control
- Simple for application teams

**Cons**
- AWS-specific backend
- Another cloud secret manager is required when moving to Azure/GCP

### Option 2 – HashiCorp Vault

**Pros**
- Cloud-independent
- Central secrets platform across AWS, Azure and GCP
- Supports dynamic and short-lived secrets

**Cons**
- Additional infrastructure to operate
- More complex availability, backup and upgrade requirements
- Higher operational overhead


## Decision

I would use **AWS Secrets Manager + External Secrets Operator** for the current
EKS environment.

Vault provides stronger multi-cloud centralisation, but I would not introduce
that operational complexity unless there is a clear requirement for one
central secrets platform across multiple clouds.


## Access

Workloads should use their own identity to retrieve only the secrets they need.

**Pod → Workload Identity/IAM Role → Secrets Manager**

Avoid shared AWS credentials and long-lived access keys.



## Secret Rotation

Secrets should be rotated:

- automatically where supported
- immediately after suspected exposure
- when application/service ownership changes
- according to organisation security policy

Applications should be tested to ensure rotated secrets can be picked up
without unnecessary downtime.


## Key Hierarchy

**Cloud KMS Key → Encrypts Secrets Manager secrets → Application consumes secret**

Access to encryption keys and access to secret values should be controlled
separately using least privilege.



## Multi-Cloud

Keep the application consumption pattern consistent:

**Application → Kubernetes Secret**

The backend can change by cloud:

**AWS** → AWS Secrets Manager  
**Azure** → Azure Key Vault  
**GCP** → Google Secret Manager

External Secrets keeps this change mostly at the platform layer rather than
requiring application changes.

