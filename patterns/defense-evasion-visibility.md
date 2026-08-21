# Defense Evasion & Visibility

## 21) Security tooling configuration toggles

Examples:

- exclusions added, then removed
- protection settings disabled, then restored
- sensor configuration changed briefly
- monitoring scope altered around a narrow time window

**Why it matters**  
The sequence can create a temporary gap while leaving the environment looking normal afterward.

**What to collect**
- configuration before/after
- actor/process
- affected hosts/users
- start/end time
- protected paths/processes
- actions during the gap
- change ticket

**Correlate with**
- suspicious execution
- file creation
- identity anomaly
- log/audit changes
- restoration shortly after activity

**Common benign explanations**
- troubleshooting
- false-positive testing
- software deployment
- vendor support
- approved exclusion change

**Related ATT&CK**  
T1685 — Disable or Modify Tools where the action actually disables, degrades, or tampers with defensive tooling.

---

## 22) Silence where activity used to exist

Examples:

- one host stops producing telemetry while peers remain healthy
- one user loses expected audit activity while related systems remain normal
- a previously regular sensor/check-in pattern stops without an obvious lifecycle event

**Why it matters**  
Selective silence can represent a blind spot, but **telemetry failure is the first alternate hypothesis to eliminate**.

**What to collect**
- last-seen timestamp
- ingestion health
- agent/service state
- host lifecycle state
- connector status
- network path
- licensing/configuration changes
- nearby telemetry from independent sources

**Correlate with**
- security-tool changes
- logging configuration changes
- process/service stops
- privileged activity immediately before silence
- network activity from the same host after telemetry stops

**Common benign explanations**
- host shutdown/sleep
- decommissioning
- agent failure
- ingestion outage
- routing/firewall change
- licensing change
- maintenance

**Related ATT&CK**  
T1685 — Disable or Modify Tools may be relevant if evidence shows deliberate impairment. Silence by itself is not proof of evasion.
