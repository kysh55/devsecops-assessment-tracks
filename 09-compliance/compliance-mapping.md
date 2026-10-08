# Compliance Mapping – Multi-Cloud Security

## 1. Compliance Mapping

The following five findings are mapped to common security control domains. Actual government compliance clauses will be confirmed against the programme's approved standards.

| Finding | Control Domain | Control Objective | Compliance Evidence | Exception Evidence |

F01 – Publicly accessible RDS database | Network Security | Restrict database access to approved networks | Terraform configuration, RDS settings, security group rules | Approved risk acceptance, business justification, owner and expiry date |
F02 – S3 public access block missing | Data Protection / Access Control | Prevent unauthorised access to stored data | S3 public access settings, bucket policy, Checkov results | Approved exception, access restrictions and risk owner |
F03 – RDS encryption not enabled | Encryption / Data Protection | Protect sensitive data at rest | RDS encryption settings, KMS configuration | Approved exception, compensating controls and remediation timeline |
F04 – SSH open to the internet | Network Security | Restrict administrative access to approved sources | Security group rules, Terraform configuration, scan results | Approved access requirement, compensating controls and expiry date |
F05 – Hardcoded secrets in EC2 user data | Secrets Management | Prevent credentials from being exposed in source code | Updated Terraform, secret scan results, credential rotation records | Documented risk acceptance, temporary controls and remediation date |

## 2. Evidence Collection

Evidence should be collected from:

- **Terraform / GitLab** → Approved configuration and change history
- **Checkov / Gitleaks** → Security scan results
- **Cloud Console / API** → Actual deployed resource settings
- **Cloud Audit Logs** → Configuration changes and access activity
- **Risk Register** → Approved exceptions, owners and expiry dates

## 3. Common Policy Across AWS, Azure and GCP

Use **OPA/Conftest** to maintain common security rules across all three cloud providers.
OPA eliminates the need to rewrite compliance rules for each individual cloud vendor and 
eliminates the operational issues of maintaining each cloud.

Common Policy | AWS | Azure | GCP |

No public database access --> RDS | Azure SQL | Cloud SQL |
Restrict public storage --> S3 | Blob Storage | Cloud Storage |
Encryption at rest --> AWS KMS | Azure Key Vault / Managed Keys | Cloud KMS |
Restrict administrative access --> Security Groups | NSGs | VPC Firewall Rules |
No hardcoded secrets --> Secrets Manager | Key Vault | Secret Manager |

**Common Policy → Cloud-Specific Terraform Checks → CI/CD Validation → Compliance Evidence**

The same security requirements apply across clouds, while the actual Terraform resource checks are adjusted for each provider.

## 4. Exception Handling

Any control that cannot be implemented before go-live must have:

- Business justification
- Compensating controls
- Named risk owner
- Formal approval
- Remediation target and expiry date