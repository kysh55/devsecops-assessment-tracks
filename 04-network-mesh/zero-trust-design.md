# Zero-Trust Network Design

## Approach

The current environment does not enforce NetworkPolicy. My first priority would
be to control which workloads can communicate with each other rather than
introducing a service mesh immediately.

The approach is:

**Default Deny → Allow Required Traffic → Control Ingress/Egress → Monitor**


## Network Policy

Each application namespace should start with a default-deny policy for both
ingress and egress.

Only required communication should then be allowed.

Example:

**Ingress Controller → Application** → Allow  
**Application → Required Backend/API** → Allow  
**Application → DNS** → Allow  
**Application → Approved External Service** → Allow  
**Application → Other Namespaces** → Deny unless required

This will reduce the blast radius during the security incidents.



## Ingress and Egress

External traffic should enter through the approved Load Balancer / Ingress
Controller rather than exposing application services directly.

Internal services should normally use `ClusterIP`.

`NodePort` should only be used where there is a specific requirement.

Egress should also be restricted so that a compromised workload cannot freely
connect to arbitrary internal or external destinations.



## Service Mesh Decision

I would **not introduce a service mesh in Phase 1**.

A service mesh can provide:

- service-to-service mTLS
- workload identity
- detailed traffic policies
- additional observability

However, it also introduces:

- additional operational complexity
- latency and resource overhead
- certificate and policy management
- another platform component to upgrade and support

For the current risks, Kubernetes NetworkPolicy combined with controlled
ingress/egress provides a simpler and sufficient first step.

A service mesh can be reconsidered later if there is a clear requirement for
service-to-service mTLS or more advanced east-west authorization.



## Multi-Cloud Consideration

The security principle remains the same across EKS, AKS and GKE:

**Default Deny → Explicit Allow**

The actual NetworkPolicy implementation depends on the CNI/networking solution
used by each managed Kubernetes platform.

Therefore, NetworkPolicy behaviour should be validated when moving workloads
between cloud providers rather than assuming identical enforcement.

