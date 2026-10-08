# Compensating Controls – COTS Container Image

## Scenario

A vendor-provided COTS container image cannot currently be rebuilt or modified.

The image will be treated as an approved exception rather than weakening
security controls for the whole cluster.

## Compensating Controls

Control | Implementation

Image scanning | Scan the COTS image with Trivy before deployment and review Critical/High vulnerabilities.
Network isolation | Place the workload in a restricted namespace with default-deny NetworkPolicy and allow only required traffic.
Restricted access | Use a dedicated service account with minimum required RBAC permissions.
Runtime monitoring | Monitor the workload using Falco for unexpected processes, privilege escalation and suspicious runtime activity.
Image integrity | Pin the approved image by digest so only the reviewed version can be deployed.

## Residual Risk

The underlying image cannot be directly remediated by our team. Vulnerabilities
inside the vendor image may therefore remain until the vendor provides an
updated version.

**Residual Risk Owner:** Application / Business Owner

## Time-Bound Exception

The exception should have:

- documented business justification
- named risk owner
- approved image version/digest
- expiry/review date
- vendor remediation or upgrade plan

At expiry, the exception must be reviewed and either closed, renewed with
approval, or replaced with a remediated vendor image.