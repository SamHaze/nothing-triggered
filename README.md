# Nothing Triggered

Cases where access, activity, and change were **successful** —
and that success is exactly what made the intrusion possible.

No alerts fired.  
No policies failed.  
No controls broke.

Everything worked as designed.

That design is what this repository examines.

If you’re looking for things that should have been blocked, this repo will disappoint you.  
These are things that were **approved**.

This is about **context**.

The goal isn’t completeness. It’s usefulness.

> **Language note:** Terms like “precursors” and “indicators” appear in incident-handling guidance.  
> This list stays practical — things defenders actually trip over in the *gray area* before escalation.

---

## Why this exists

Most detection guidance focuses on what breaks.

This repository documents what **passes** —  
and why that’s often worse.

---

## What survived triage

Something makes the list if:

- it appears **before** escalation, not after
- it’s easy to dismiss in isolation
- it becomes obvious only in hindsight
- it shows up across multiple environments (not a one-off)
- it changes the **decision**, not just the detection

This is biased. That’s intentional.

If you disagree, you’re probably right somewhere else.

---

## Index

- [Identity & Access](#identity--access)
- [Cloud Control Plane](#cloud-control-plane)
- [Endpoint & Execution Context](#endpoint--execution-context)
- [Network & Egress](#network--egress)
- [Email & Collaboration](#email--collaboration)
- [Data Access & Exfil Shape](#data-access--exfil-shape)
- [Defense Evasion & Visibility](#defense-evasion--visibility)
- [Contributing](#contributing)
- [References](#references)
- [License](#license)

---

## Identity & Access

### 1) Valid auth, wrong *story*
- Sign-in succeeds, but surrounding context is new:
  - new device + familiar ASN
  - familiar device + new geo
  - same user agent, new token issuer or auth method

**Why it matters**  
Early account takeover often looks legitimate because it is — valid credentials, tokens, and auth flows.

**What to collect**
- device ID / registration state
- auth method and conditional access result
- token claims (where available): `amr`, `appid`, `tid`, `upn`, `ipaddr`
- historical baselines (last-seen device, IP, geo)

---

### 2) MFA succeeded, but the *enrollment story* changed
- MFA passes, but there’s recent:
  - new authenticator device
  - security info updates
  - sign-in method changes

**Why it matters**  
Attackers often stabilize access by changing the recovery surface *before* doing anything loud.

---

### 3) Privilege use that matches role, not timing
- Admin action aligns with job function, but occurs:
  - at unusual hours
  - from an unfamiliar device
  - in short bursts across many objects

**Why it matters**  
“Looks allowed” is exactly why it survives triage.

---

### 4) Break-glass “just to test”
- Emergency accounts touched without a documented exercise

**Why it matters**  
These accounts are targeted precisely because controls are intentionally looser.

---

### 5) Consent or app access that doesn’t match how work gets done
- New OAuth consent that is technically reasonable but rare:
  - `Mail.Read`, `offline_access`, `Files.Read.All`, etc.

**Why it matters**  
Token-based persistence bypasses password resets.

*Note:* Legitimate OAuth and device-code flows are routinely abused via social engineering without stealing passwords.

---

## Cloud Control Plane

### 6) API calls that are valid but rare
Examples:
- listing secrets or keys
- enumerating role assignments
- reading audit or diagnostic settings

**Why it matters**  
Most cloud compromises start with inventory and permissions.

**What to collect**
- caller identity (user, service principal, workload identity)
- user agent (CLI or SDK), IP, tenant or subscription
- first-seen activity for the principal

---

### 7) Resource creation followed by immediate deletion
- Short-lived storage, functions, snapshots, forwarding rules

**Why it matters**  
Attackers probe capability, exfil quickly, then remove evidence.

---

### 8) Policy edits that reduce *visibility*
- Narrowed audit scope
- Excluded high-signal events
- Retention changes

**Why it matters**  
Visibility impairment often precedes destructive actions.

---

### 9) “Temporary” logging changes
- Logs disabled briefly, then restored
- Diagnostic settings modified and reverted

**Why it matters**  
Short visibility gaps are enough for key actions.

---

## Endpoint & Execution Context

### 10) Legitimate binary, abnormal parentage
Examples:
- Office or browser → script host
- remote management tooling → shell with odd arguments

**Why it matters**  
Living-off-the-land relies on trust in binaries; the parent chain often tells the truth first.

---

### 11) One-time script execution with no persistence
- Single execution of `powershell`, `wscript`, `python`, `bash`
- Encoded or download-cradle patterns

**Why it matters**  
Not every intrusion needs persistence to cause damage.

---

### 12) Scheduled task or service created, then never used
**Why it matters**  
Failed persistence attempts are early signals.

---

### 13) Tools executed from allowed but uncommon paths
Examples:
- user profile temp directories
- `ProgramData` with randomly named folders

**Why it matters**  
Policy allows it; humans rarely do it.

---

## Network & Egress

### 14) First-time destinations with clean reputation
- New domains or IPs that are quiet and unflagged

**Why it matters**  
Early-stage infrastructure often looks benign until it’s reused.

---

### 15) Short-lived TLS sessions with no follow-on traffic
- connect → exchange small data → disappear

**Why it matters**  
Token exchange, staging, and beacons often look like nothing.

---

### 16) DNS resolves once and never again
- Single A or AAAA lookup

**Why it matters**  
Common during staging, validation, or early infrastructure testing.

---

## Email & Collaboration

### 17) Mailbox rules that “make sense”
- rules that move security alerts
- hide finance threads
- auto-forward “for convenience”

**Why it matters**  
This is often pre-exfil hygiene.

---

### 18) Internal phishing rehearsal artifacts
- suspicious links opened without payload
- fake document-share prompts

**Why it matters**  
Reconnaissance on who will click comes before real attempts.

---

## Data Access & Exfil Shape

### 19) Unusual breadth, normal depth
- many objects touched once
- many sites, folders, or repos accessed shallowly

**Why it matters**  
Attackers often map the surface before pulling volume.

---

### 20) Small exfil before big exfil
- brief test downloads followed by quiet

**Why it matters**  
Staging runs.

---

## Defense Evasion & Visibility

### 21) Security tooling configuration toggles
- exclusions added then removed
- tamper or protection settings changed

**Why it matters**  
Impairment patterns often precede escalation.

---

### 22) Alerts stop for a specific entity
- one host or user goes quiet
- the rest of the environment does not

**Why it matters**  
Selective blindness is a smell.

---

## Contributing

PRs are welcome if they add **signal quality**, not volume.

Please include:
- what it looked like
- why it mattered in hindsight
- what made it dismissible at the time
- what data source(s) confirmed it

No payloads, exploit steps, or offensive how-tos.

---

## References

- NIST Computer Security Incident Handling Guide
- MITRE ATT&CK: Valid Accounts  
- MITRE ATT&CK: Impair Defenses  
- Reporting on OAuth device-code flow abuse leading to M365 takeover

---

## License

MIT
