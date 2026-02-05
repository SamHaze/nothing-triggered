# Before It’s Obvious

Signals and context that tend to exist **before** incidents become obvious.

Not exploits.  
Not payloads.  
Not tools for breaking things.

This is about **context**.

The goal isn’t completeness. It’s usefulness.

> A note on language: “precursors” and “indicators” show up in incident-handling guidance, but this list stays practical—things defenders actually trip over in the *gray area* before escalation. 

---

## How to use this

This repo is designed for:

- building triage checklists (“what should I verify next?”)
- creating lightweight detections / “watch” rules
- improving incident narratives (“what did we miss early?”)
- threat hunting prompts (“what shows up *before* persistence?”)

**Rule of thumb:** if it’s loud, it doesn’t belong here.

---

## Inclusion criteria

Something makes the list if:

- it regularly appears **before** escalation, not after
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
- [Data Access & Exfil “Shape”](#data-access--exfil-shape)
- [Defense Evasion & Visibility](#defense-evasion--visibility)
- [Contributing](#contributing)
- [References](#references)
- [License](#license)

---

## Identity & Access

### 1) Valid auth, wrong *story*
- Sign-in is **successful** but the surrounding context is new:
  - new device + familiar ASN
  - familiar device + new geo
  - same user agent, new token issuer / auth method
- Why it matters: early account takeover often looks “legit” because it is—valid creds, tokens, and auth flows (MITRE ATT&CK: Valid Accounts).

**What to collect**
- device ID / registration state
- auth method & conditional access result
- token claims (where available): `amr`, `appid`, `tid`, `upn`, `ipaddr`
- historical baselines: last-seen device/IP/geo

---

### 2) MFA succeeded… but the *enrollment story* changed
- MFA succeeds, but there’s recent:
  - new authenticator device
  - “additional security info” updates
  - changes to sign-in methods
- Why it matters: attackers frequently stabilize access by changing the recovery surface *before* doing anything loud.

---

### 3) Privilege use that matches role but not timing
- Admin action aligns with job function, but happens:
  - at unusual hours
  - from unusual device
  - in short bursts (5–10 minutes) across many objects
- Why it matters: “looks allowed” is exactly why it survives triage.

---

### 4) Break-glass “just to test”
- Break-glass / emergency accounts touched without a documented exercise
- Why it matters: these accounts are commonly targeted because controls are intentionally looser.

---

### 5) Consent / app access that doesn’t match how work gets done
- New OAuth app consent or permissions granted that are “technically reasonable” but rare in your org:
  - `Mail.Read`, `offline_access`, `Files.Read.All`, etc.
- Why it matters: token-based persistence can bypass password resets.

Separately: device-code and other “legit flow” abuse patterns are a real phenomenon defenders are seeing in the wild—often relying on social engineering users into authorizing access without stealing a password.

---

## Cloud Control Plane

### 6) API calls that are valid but rare
- Examples:
  - listing all secrets/keys
  - enumerating role assignments
  - reading audit settings
- Why it matters: most cloud compromises start with **inventory + permissions**.

**What to collect**
- caller identity (user/service principal/workload identity)
- user agent (CLI/SDK), IP, tenant/subscription
- “first seen” for the principal doing this action

---

### 7) Resource creation followed by immediate deletion
- Short-lived:
  - storage accounts
  - serverless functions
  - snapshots
  - forwarding rules
- Why it matters: attackers probe capability, exfil quickly, then remove evidence.

---

### 8) Policy edits that reduce *visibility* (not just access)
- Examples:
  - narrowing audit scope
  - excluding high-signal events
  - changing retention
- Why it matters: visibility impairment is a common defensive evasion pattern.

---

### 9) “Temporary” logging changes
- Logs disabled briefly, then restored
- Diagnostic settings modified and put back
- Why it matters: short visibility gaps are enough for key actions.

---

## Endpoint & Execution Context

### 10) Legit binary, abnormal parentage
- Examples:
  - Office/Browser → script host
  - remote mgmt tooling → shell with odd arguments
- Why it matters: attackers love living-off-the-land, and the *parent chain* often tells the truth first.

---

### 11) One-time script execution with no persistence
- A single run of:
  - `powershell`, `wscript`, `python`, `bash`
  - encoded / download cradle patterns
- Why it matters: not every intrusion needs persistence to cause damage.

---

### 12) New scheduled task / service created… then never triggers again
- Why it matters: “failed persistence” attempts are early tells.

---

### 13) Tools from allowed-but-uncommon paths
- Examples:
  - user profile temp dirs
  - programdata with random folder names
- Why it matters: policy allows it; humans don’t usually use it.

---

## Network & Egress

### 14) First-time destinations with clean reputation
- New domains/IPs that aren’t flagged and aren’t noisy
- Why it matters: early-stage infra is often low-reputation only *after* it’s used.

---

### 15) Short-lived TLS sessions with no follow-on traffic
- “connect, exchange small data, disappear”
- Why it matters: token exchange / staging / beacons can look like nothing.

---

### 16) DNS resolves once and never again
- Single A/AAAA lookup, no repeated access
- Why it matters: can indicate staging, validation, or mis-typed but malicious domains.

---

## Email & Collaboration

### 17) Mailbox rules that “make sense”
- Rules that:
  - move security alerts
  - hide finance threads
  - auto-forward “for convenience”
- Why it matters: this is often *pre-exfil hygiene*.

---

### 18) Internal phishing rehearsal artifacts
- A suspicious link opened but no payload
- A fake “document share” prompt
- Why it matters: reconnaissance on who will click works before the real attempt.

---

## Data Access & Exfil “Shape”

### 19) Unusual breadth, normal depth
- Many objects touched once:
  - many SharePoint sites
  - many mail folders
  - many repos
- Why it matters: attackers often map the surface before pulling volume.

---

### 20) “Small exfil” before big exfil
- A few test downloads, then quiet
- Why it matters: staging runs.

---

## Defense Evasion & Visibility

### 21) Security tooling configuration toggles
- Examples:
  - exclusions added then removed
  - tamper settings changed
- Why it matters: impairment patterns often appear before destructive actions.
---

### 22) Alerts stop… for a specific host/user
- Not “no alerts at all”—just one entity goes dark
- Why it matters: selective blindness is a smell.

---

## Contributing

PRs are welcome if they add **signal quality**, not volume.

Please include:
- **what it looked like**
- **why it mattered in hindsight**
- **what made it dismissible at the time**
- **what data source(s)** can confirm it

No payloads, exploit steps, or offensive how-tos.

---

## References

- NIST Computer Security Incident Handling Guide (precursors / indicators in incident handling) 
- MITRE ATT&CK: Valid Accounts — legitimate access abuse
- MITRE ATT&CK: Impair Defenses — visibility and control reduction
- Reporting on Proofpoint research: OAuth device-code flow abuse leading to M365 takeover

---

## License

MIT
