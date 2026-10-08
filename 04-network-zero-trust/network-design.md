# Network Security & Zero-Trust Design

## 1. Network Segmentation

Separate workloads based on environment and data sensitivity.

**Production** → Isolated from development and testing  
**Sensitive Workloads** → Dedicated private subnets with restricted access  
**Public Applications** → Only approved entry points exposed to the internet  
**Databases** → Private subnets, accessible only from authorised applications

Use native network controls:

**AWS** → VPC + Security Groups  
**Azure** → VNet + NSGs  
**GCP** → VPC + Firewall Rules

## 2. Cross-Cloud Connectivity

Use private connectivity where workloads need to communicate across clouds.

**AWS** → Transit Gateway  
**Azure** → Virtual WAN / Hub-and-Spoke  
**GCP** → Network Connectivity Center

Connect cloud networks through approved VPN or private interconnect services.

Avoid direct connectivity between every workload. Route cross-cloud traffic through controlled network hubs.

## 3. Internet Access

**Inbound Traffic**

Internet → WAF → Load Balancer → Application → Private Database

- Expose only approved public applications.
- Keep databases and internal services private.
- Allow only required ports and source networks.

**Outbound Traffic**

- Deny unnecessary internet access.
- Allow approved external APIs and services.
- Route required outbound traffic through controlled firewall or proxy services.
- Log outbound connections for investigation.

## 4. Common Zero-Trust Controls

Apply the same security principles across AWS, Azure and GCP.

**Default Deny** → Block traffic unless explicitly allowed  
**Least Privilege** → Allow only required ports, protocols and destinations  
**Segmentation** → Isolate workloads based on sensitivity  
**Encryption** → Use TLS for sensitive application communication  
**Monitoring** → Collect network flow and firewall logs

## 5. Multi-Cloud Consistency

Maintain one common network security standard across all three clouds.

Requirement | AWS | Azure | GCP 

Network isolation | VPC / Subnets | VNet / Subnets | VPC / Subnets
Traffic filtering | Security Groups | NSGs | Firewall Rules
Central connectivity | Transit Gateway | Virtual WAN | Network Connectivity Center
Private service access | PrivateLink | Private Link | Private Service Connect
Network monitoring | VPC Flow Logs | VNet Flow Logs | VPC Flow Logs

Use Terraform and common policy checks to apply and validate these controls consistently.
Cloud-specific implementations may differ, but the security rules should remain the same.
