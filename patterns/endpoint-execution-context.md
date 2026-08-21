# Endpoint & Execution Context

## 10) Legitimate binary, abnormal parentage

Examples:

- Office application or browser → script host
- remote-management tooling → shell with unusual arguments
- signed utility → unexpected child process
- trusted parent → executable from a user-writable location

**Why it matters**  
Signed or trusted processes are not inherently suspicious. Process lineage, command line, user context, and destination often reveal intent earlier than file reputation does.

**What to collect**
- parent and child process names
- full command line
- signer/hash
- user/session
- working directory
- file path
- network connections
- file creation/download events

**Correlate with**
- first-seen destination
- recent document/browser activity
- script execution
- credential access or discovery
- identity anomaly for the same user

**Common benign explanations**
- enterprise automation
- software deployment
- browser helper processes
- RMM administration
- developer tooling

**Related ATT&CK**  
T1218 — System Binary Proxy Execution when trusted binaries are actually abused for proxy execution; T1059 — Command and Scripting Interpreter for script/shell execution. Abnormal parentage alone is a detection feature, not a technique.

**Reference hunts**  
- [KQL](../queries/kql/abnormal-parentage.kql)
- [SPL](../queries/spl/abnormal-parentage.spl)

---

## 11) One-time script execution with no persistence

A single execution of:

- `powershell` / `pwsh`
- `wscript` / `cscript`
- `python`
- `bash`
- another interpreter

becomes more interesting when combined with:

- encoded/obfuscated content
- download behavior
- unusual parent process
- first-seen destination
- user-writable execution path
- rare execution for the host/user

**Why it matters**  
Persistence is not required for discovery, collection, credential access, or one-time execution. The absence of persistence should not end the investigation.

**What to collect**
- interpreter and command line
- parent/ancestor chain
- script path/content metadata where available
- network activity
- file writes
- user/session
- prevalence across hosts

**Correlate with**
- browser/Office parentage
- rare destination
- download or file creation
- identity anomaly
- follow-on administrative activity

**Common benign explanations**
- admin scripts
- software installation
- development work
- login scripts
- automation

**Related ATT&CK**  
T1059 — Command and Scripting Interpreter and relevant sub-techniques, such as T1059.001 PowerShell.

---

## 12) Scheduled task or service created, then never used

A task or service is created but:

- never executes
- fails immediately
- is deleted shortly afterward
- remains dormant with no business owner

**Why it matters**  
Failed, abandoned, or test persistence attempts can leave an artifact even when the intended follow-on execution never occurs.

**What to collect**
- task/service definition
- creator process and user
- creation/deletion time
- configured executable/arguments
- execution result
- remote-creation context

**Correlate with**
- shell/script execution
- remote administration
- credential use
- file creation
- task/service deletion

**Common benign explanations**
- deployment failures
- software installers
- administrator testing
- scheduled-maintenance tooling

**Related ATT&CK**  
T1053 — Scheduled Task/Job for scheduled tasks. Services may instead relate to Create or Modify System Process depending on the artifact.

---

## 13) Execution from allowed but uncommon paths

Examples:

- user profile temp directories
- randomly named subdirectories beneath `ProgramData`
- first-seen executable path for a normally stable application
- user-writable paths used by an uncommon parent process

**Why it matters**  
The path is not suspicious simply because it is writable. The useful signal is **rarity relative to the process, host role, signer, and peer group**.

**What to collect**
- full image path
- signer/hash
- parent process
- prevalence across fleet
- first/last seen
- creating process
- network activity

**Correlate with**
- abnormal parentage
- newly downloaded files
- rare destination
- script interpreter activity
- unsigned or low-prevalence binaries

**Common benign explanations**
- software updaters
- self-extracting installers
- collaboration tools
- development workflows
- enterprise deployment packages

**Related ATT&CK**  
No single ATT&CK technique follows from path rarity alone. Map the observed execution behavior only when supporting evidence exists.
