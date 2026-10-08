# Runtime Detection & Incident Response Plan

## 1. Approach

CI/CD and admission controls reduce risk before deployment, but runtime
monitoring is required to detect suspicious behaviour after a container starts.

**Falco** → Detect suspicious container/runtime behaviour  
**Kubernetes Audit Logs** → Record Kubernetes API activity  
**Existing Central SIEM/Logging** → Centralise alerts, correlation and investigation  

No separate monitoring platform is introduced, as falcon events will be streamed to the moniotoring tool


## 2. Runtime Detection

Falco will focus on high-risk behaviour rather than generating alerts for every
container activity.

Priority detections:

- Shell execution inside sensitive containers
- Unexpected privilege escalation
- Access to container runtime sockets
- Sensitive host file access
- Unexpected process execution
- Suspicious Kubernetes API activity

### Alert Priority

**Critical/High** →  Active exploit, root privilege escalation  
**Medium** → Unexpected shell opened, unauthorized file write  
**Low** → Routine config changes, system diagnostics

Genric Rules should be tuned as per the workload requirement to reduce alert fatigue.



## 3. Container Escape Incident Response



**1. Detect**  
Falco identifies suspicious runtime behaviour indicating a breakout related to unexpected privilege escalation, namespaces modification, or unauthorized host file writes.

**2. Contain**  
Isolate the blast radius immediately:
- Isolate the affected network policies to block egress/ingress traffic.
- Cordon and taint the host node to prevent new pods from scheduling onto the compromised host.
- Terminate the affected pod if immediate active exploitation must be stopped.

**3. Revoke**  
Revoke affected Kubernetes Service Accounts, IAM cloud credentials linked to the node/pod, and rotate any secrets exposed during the breach window.

**4. Preserve Evidence**  
Before destroying the infrastructure, capture forensics data:
- Collect Falco alerts, Kubernetes audit trails, container logs, and pod manifests.
- Take a snapshot of the worker node's root block storage volume.

**5. Eradicate**  
- Delete the compromised workloads.
- Terminate and destroy the underlying worker node instance so it is completely recreated from a clean image.
- Scan the cluster for lingering unauthorized ClusterRoleBindings or malicious DaemonSets.
- Identify the root vulnerability


**6. Recover**  
Redeploy workloads strictly from trusted Infrastructure as Code (IaC) pipelines and cryptographic signed container images.

**7. Review**  
Conduct a post-mortem to update CI/CD scanning, tighten Kubernetes Admission Controller rules 



## 4. ITSO Communication

**What is hit:** prod-eks-cluster-01 / payment-gateway namespace.
**When & How:** Detected at xx:xx UTC via a Falco runtime alert 
**Blast Radius:** High risk. The attacker has successfully escaped the container boundary. Access to the underlying host worker node (node-aws-ec2-99) is confirmed.
**Containment:** In progress. The affected pod has been killed. The host node has been cordoned so no new applications run on it. Network isolation policies are active.
**Evidence:** Pod logs and Falco JSON logs are backed up.
**Recovery:** Application is running normally on other nodes, but capacity is reduced by 20%.




