# Cloud Control Plane

## 6) API activity that is valid but uncommon

Examples:

- listing secrets, keys, or credentials metadata
- enumerating role assignments
- listing compute, storage, snapshots, or databases
- reading audit or diagnostic settings
- broad enumeration across multiple cloud services

**Why it matters**  
Post-compromise cloud discovery often includes inventory, permissions, and service enumeration. The calls themselves may be valid; the unusual caller, sequence, scope, or timing makes them useful.

**What to collect**
- caller identity: user, service principal, or workload identity
- API/operation name
- user agent: portal, CLI, SDK, automation
- source IP/region
- tenant, subscription, project, or account
- target resources
- first-seen activity for the principal

**Correlate with**
- recent identity anomaly
- bursty multi-service enumeration
- snapshot or resource creation
- permission changes
- later data access

**Common benign explanations**
- inventory tooling
- CSPM/CNAPP products
- asset discovery
- Terraform/CI pipelines
- administrator troubleshooting

**Related ATT&CK**  
T1526 — Cloud Service Discovery; T1580 — Cloud Infrastructure Discovery.

---

## 7) Resource creation followed by immediate deletion

Examples:

- short-lived storage
- functions
- snapshots
- forwarding/relay resources
- temporary service principals or automation resources

**Why it matters**  
A create → use → delete sequence can represent testing, short-lived staging, or cleanup. The sequence matters more than creation or deletion alone.

**What to collect**
- resource ID/type
- creator identity
- creation and deletion timestamps
- configuration at creation
- actions performed during the resource lifetime
- related data access or network activity

**Correlate with**
- first-seen principal
- discovery API activity
- data retrieval
- unusual source IP
- audit/logging changes

**Common benign explanations**
- CI/CD
- autoscaling
- ephemeral test environments
- infrastructure-as-code rollbacks
- failed deployments

**Related ATT&CK**  
The relationship depends on what the resource was used for. Do not map create/delete activity to a technique without supporting behavior.

---

## 8) Policy edits that reduce *visibility*

Examples:

- audit scope narrowed
- high-signal events excluded
- retention shortened
- diagnostic destinations removed
- collection filters changed

**Why it matters**  
Visibility impairment can precede follow-on activity. The change may be an authorized administrative action, but it becomes significant when it creates a selective blind spot around later behavior.

**What to collect**
- policy/configuration before and after
- actor
- source IP/device
- affected telemetry
- change ticket
- duration of reduced visibility

**Correlate with**
- privileged sign-in anomalies
- activity during the blind window
- restoration of the setting afterward
- security-tool configuration changes

**Common benign explanations**
- cost optimization
- logging migration
- retention-policy change
- troubleshooting
- planned configuration rollout

**Related ATT&CK**  
T1685.002 — Disable or Modify Cloud Log where the change disables or degrades cloud logging.

---

## 9) Temporary logging changes

Examples:

- cloud diagnostic settings removed, then restored
- audit collection disabled briefly
- logging destination changed and reverted
- host audit configuration modified for a short window

**Why it matters**  
A short visibility gap can be enough to obscure activity. The strongest signal is often the sequence: **visibility change → consequential activity → restoration**.

**What to collect**
- exact logging configuration changes
- actor and source
- gap start/end
- telemetry expected during the gap
- target systems/resources
- change-management context

**Correlate with**
- privileged operations inside the gap
- data access
- process execution
- resource creation/deletion
- security-tool changes

**Common benign explanations**
- maintenance
- connector migration
- diagnostic troubleshooting
- log-pipeline outage
- approved cost controls

**Related ATT&CK**  
T1685.001 — Disable or Modify Windows Event Log; T1685.002 — Disable or Modify Cloud Log, depending on the platform.

**Reference hunt**  
[KQL: temporary cloud logging change](../queries/kql/temporary-cloud-logging-change.kql)
