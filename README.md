# DevSecOps Assessment – Track 2: Multi-Cloud Platform Security

## 1. Overview

This assessment focuses on improving security, compliance and operational resilience across AWS, Azure and GCP.

The scenario covers approximately 100 workloads, mainly deployed on AWS using Terraform and GitLab CI/CD, with Azure and GCP onboarding planned.

**Key focus areas:**
- Identify and prioritise cloud security risks.
- Secure CI/CD pipelines and cloud identities.
- Improve network security, encryption and monitoring.
- Define remediation, compliance and disaster recovery approaches.

**Reference Repository:** [TerraGoat](https://github.com/bridgecrewio/terragoat) – An intentionally vulnerable Terraform project used for security assessment.

## 2. Approach

1. **Assess** – Review Terraform configurations and identify security gaps.
2. **Prioritise** – Classify findings based on severity and business impact.
3. **Secure** – Recommend practical controls for IAM, networking, CI/CD and data protection.
4. **Remediate** – Provide fixes, compensating controls and validation evidence.
5. **Operate** – Define monitoring, incident response, compliance and recovery processes.

The approach focuses on proven cloud-native security practices, automation and solutions that are simple to maintain.

## 3. Repository Navigation

Folder | Contents 

01-findings - Security findings and severity methodology
02-pipeline-supply-chain - Secure GitLab CI/CD pipeline design
03-identity-governance - Cross-cloud IAM and least-privilege access
04-network-zero-trust - Network segmentation and connectivity
05-data-protection - Encryption and key management
06-detection-ir - Security monitoring and incident response
07-remediation - Remediation advisory and compensating controls
08-threat-model - STRIDE threat modelling
09-compliance - Compliance mapping and policy-as-code 
10-architecture - High-level and low-level architecture diagrams
11-resilience-dr Backup and disaster recovery plan
12-presentation - Assessment presentation

## 4. Key Design Principles

**Least Privilege** – Provide only the permissions required.
**Shift-Left Security** – Detect issues before deployment.
**Zero Trust** – Verify access and restrict network communication.
**Central Visibility** – Collect security findings and logs across clouds.
**Automation First** – Use Terraform and CI/CD for consistent controls.
**Risk-Based Decisions** – Prioritise critical issues and document exceptions.
