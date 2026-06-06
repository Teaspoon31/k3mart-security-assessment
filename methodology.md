---

### File 2: `methodology.md`
*This document breaks down your exact step-by-step methodology to prove to technical interviewers that you follow a standard engineering workflow.*

```markdown
# Technical Assessment Methodology

This security assessment followed an industry-standard, structured approach mimicking real-world external threat actors. The workflow is split into four distinct, sequential phases.

---

## Phase 1: Reconnaissance & Infrastructure Mapping
The objective of this phase was to identify the perimeter architecture, discover edge entry points, and map the hosting network infrastructure.

### 1. Network Layer Enumeration
Using active packet manipulation, a comprehensive port scan was executed against the root domain to intercept responding services.
```bash
# Executing full 65,535 TCP port scanning with service mapping at a safe execution rate
nmap -sV -sC -p- --min-rate 1000 k3mart.store
