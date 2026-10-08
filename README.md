# DevSecOps Assessment – Track 1: Kubernetes & Container Security

## 1. Overview

This assessment focuses on identifying and improving security risks in Kubernetes environments, from container images and CI/CD pipelines to runtime security and disaster recovery.

**Key focus areas:**
- Identify and prioritise Kubernetes security findings.
- Secure container images and CI/CD pipelines.
- Improve RBAC, network security and secrets management.
- Define remediation and compensating controls.
- Establish runtime monitoring, compliance and disaster recovery.

**Reference Repository:** [Kubernetes Goat](https://github.com/madhuakula/kubernetes-goat) – An intentionally vulnerable Kubernetes environment used for security assessment.

## 2. Approach

1. **Assess** – Review Kubernetes configurations and identify security gaps.
2. **Prioritise** – Classify findings based on severity, exposure and impact.
3. **Secure** – Recommend practical controls for RBAC, networking, secrets and container security.
4. **Remediate** – Provide fixes, compensating controls and validation evidence.
5. **Operate** – Define runtime monitoring, incident response, compliance and disaster recovery.

The approach follows proven Kubernetes security practices, focusing on simple, practical and maintainable solutions.

## 3. Repository Navigation

Folder | Contents |

01-findings -  Kubernetes security findings and severity methodology 
02-pipeline-supply-chain - Secure CI/CD pipeline and container image scanning 
03-identity-governance - Kubernetes RBAC and service account security 
04-network-zero-trust -  Network policies and workload isolation 
05-secrets-management - Secure secrets storage and access
06-detection-ir - Runtime monitoring and incident response
07-remediation - Security fixes and compensating controls
08-threat-model - STRIDE threat modelling
09-compliance - Security compliance mapping
10-architecture - High-level and low-level architecture diagrams
11-resilience-dr - Kubernetes backup and disaster recovery

## 4. Key Design Principles

**Least Privilege** – Restrict RBAC and service account permissions.
**Shift-Left Security** – Scan images and configurations before deployment.
**Zero Trust** – Apply network isolation and controlled workload access.
**Secure Secrets** – Avoid hardcoded credentials and use managed secret storage.
**Runtime Visibility** – Detect suspicious container and Kubernetes activity.
**Automation First** – Apply consistent security controls through CI/CD and Infrastructure as Code.
