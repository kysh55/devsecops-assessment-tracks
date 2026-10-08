# Severity Methodology

Findings are prioritised based on the potential impact, likelihood of exploitation, exposure and number of resources affected.

Severity  ---  Remediation Priority  ---- Criteria

Critical --> Immediate --> Could lead to major data exposure, administrative compromise or widespread cloud access
High --> High priority --> Significant security weakness affecting sensitive data, access or workload protection
Medium --> Planned remediation --> Increases security risk but normally requires additional conditions to cause major impact
Low --> Regular improvement cycle --> Minor security weakness or general hardening improvement 

## Assessment Factors

**Impact** → What happens if the weakness is exploited?  
**Exposure** → Is the resource accessible from the internet?  
**Permissions** → Can the weakness provide privileged access?  
**Blast Radius** → How many workloads or accounts could be affected?  
**Likelihood** → How easily could the weakness be exploited?

## Exceptions

Findings that cannot be fixed immediately should have a documented justification, compensating controls, named risk owner and review date.
Severity should be reviewed against the actual workload configuration and business impact before final approval.