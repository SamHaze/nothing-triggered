# Nothing Triggered

Cases where access, activity, or change **succeeded** — and the surrounding context is what made the activity worth investigating.

No meaningful alert had fired yet.  
No access control necessarily failed.  
The activity was accepted by the system.

That acceptance is what this repository examines.

If you are looking only for events that should have been blocked, this repo will disappoint you. These are actions that can be **technically valid, successfully authorized, or individually explainable**.

This is about **context**.

The goal is not completeness. It is usefulness.

> **Language note:** Terms such as *precursors* and *indicators* appear in incident-response and threat-detection guidance. This project stays deliberately practical: it focuses on patterns defenders can miss in the gray space **before the first meaningful signal**.

> **Important:** Nothing here is a verdict by itself. Each pattern is a **hunt hypothesis seed**, not a standalone detection. The value comes from correlation, baselining, and context.

---

## Why this exists

Most detection guidance is naturally centered on events that are already suspicious enough to alert.

**Nothing Triggered** focuses on the quieter period before that point:

- authentication succeeds
- an API call is authorized
- a process is signed and allowed
- a configuration change is legitimate in isolation
- a network destination has no known malicious reputation
- telemetry disappears without an obvious error

In many investigations, the decisive context exists **before** the alert, impact, lateral movement, or later discovery.

The project asks a simple question:

> **What would have changed the analyst's decision if it had been visible in context at the time?**

---

## What makes the list

A pattern belongs here when most of the following are true:

- it can occur **before a meaningful alert exists**
- it is technically valid, successfully authorized, or otherwise accepted by the system
- it is easy to dismiss in isolation
- it becomes more significant when correlated with surrounding behavior
- it appears across multiple environments rather than as a single anecdote
- it would have changed an investigative decision, not merely confirmed a detection after the fact

This list is intentionally biased toward **contextual signals that are individually explainable but become meaningful when correlated across identity, endpoint, cloud, network, email, and data-access telemetry**.

---

## How to use this repository

Treat each entry as a starting point for a hunt, not a rule to deploy unchanged.

A useful workflow is:

1. **Establish the baseline.** What is normal for this user, host, service principal, process, destination, or business workflow?
2. **Find the contextual break.** What changed: device, timing, parent process, permission, destination, logging state, access breadth, or sequence?
3. **Correlate across telemetry.** Weak signals become useful when identity, endpoint, cloud, email, and network evidence agree.
4. **Enumerate benign explanations.** A hunt that cannot explain normal behavior will create volume rather than signal.
5. **Decide what would change action.** The point is not to label every anomaly malicious; it is to identify the evidence that should change triage or investigation.

---

## Repository layout

```text
nothing-triggered/
├── README.md
├── CONTRIBUTING.md
├── REFERENCES.md
├── LICENSE-CODE
├── LICENSE-DOCS.md
├── patterns/
│   ├── identity-access.md
│   ├── cloud-control-plane.md
│   ├── endpoint-execution-context.md
│   ├── network-egress.md
│   ├── email-collaboration.md
│   ├── data-access-exfil-shape.md
│   └── defense-evasion-visibility.md
├── queries/
│   ├── kql/
│   │   ├── valid-auth-wrong-story.kql
│   │   ├── mfa-enrollment-change.kql
│   │   ├── abnormal-parentage.kql
│   │   ├── mailbox-forwarding-change.kql
│   │   └── temporary-cloud-logging-change.kql
│   └── spl/
│       ├── abnormal-parentage.spl
│       ├── mailbox-forwarding-change.spl
│       └── first-time-destination.spl
└── examples/
    └── synthetic-data/
        └── README.md
```

---

## Pattern index

### Identity & Access
1. **Valid auth, wrong story** — successful authentication with a contextual break  
2. **MFA succeeded, but the enrollment story changed** — recent authentication-method changes before successful access  
3. **Privilege use that matches role, not timing** — legitimate privilege used in an anomalous pattern  
4. **Emergency access without matching context** — break-glass use outside a documented exercise or incident  
5. **Consent or app access that does not match how work is done** — unusual OAuth/app access that persists outside the normal user workflow  

[Read Identity & Access](patterns/identity-access.md)

### Cloud Control Plane
6. **API activity that is valid but uncommon** — discovery-like API behavior by an unusual principal  
7. **Resource creation followed by immediate deletion** — short-lived cloud resources with little business context  
8. **Policy edits that reduce visibility** — changes that narrow audit coverage or retention  
9. **Temporary logging changes** — visibility disabled or modified, then restored  

[Read Cloud Control Plane](patterns/cloud-control-plane.md)

### Endpoint & Execution Context
10. **Legitimate binary, abnormal parentage** — trusted process with an unusual parent/child chain  
11. **One-time script execution with no persistence** — transient scripting activity where persistence is not required  
12. **Scheduled task or service created, then never used** — abandoned or failed persistence artifacts  
13. **Execution from allowed but uncommon paths** — permitted execution from paths that are unusual for the observed workload  

[Read Endpoint & Execution Context](patterns/endpoint-execution-context.md)

### Network & Egress
14. **First-time destination with no known malicious reputation** — new destination that becomes interesting only with context  
15. **Short-lived TLS session with contextual anomalies** — brief encrypted communication correlated with rare process/destination behavior  
16. **One-time DNS resolution with execution or connection context** — rare resolution tied to a process, connection, or identity anomaly  

[Read Network & Egress](patterns/network-egress.md)

### Email & Collaboration
17. **Mailbox rules that make sense individually** — plausible rules that alter security visibility or message flow  
18. **Low-impact social-engineering probes before credential capture** — weak interaction signals that may precede a more consequential attempt  

[Read Email & Collaboration](patterns/email-collaboration.md)

### Data Access & Exfil Shape
19. **Unusual breadth, normal depth** — shallow access across many objects or locations  
20. **Low-volume transfer preceding larger access** — small export or retrieval activity that tests capability before scale  

[Read Data Access & Exfil Shape](patterns/data-access-exfil-shape.md)

### Defense Evasion & Visibility
21. **Security tooling configuration toggles** — exclusions or protection changes that are later reverted  
22. **Silence where activity used to exist** — selective telemetry loss that differs from the rest of the environment  

[Read Defense Evasion & Visibility](patterns/defense-evasion-visibility.md)

---

## Reference hunts

These are **reference hunts**, not production-ready detections. Field names and tables vary by connector, product, data model, and ingestion design. Validate every query against your own schema and normal behavior.

### KQL
- [Valid auth, wrong story](queries/kql/valid-auth-wrong-story.kql)
- [MFA enrollment/security-info change followed by sign-in](queries/kql/mfa-enrollment-change.kql)
- [Legitimate binary, abnormal parentage](queries/kql/abnormal-parentage.kql)
- [Mailbox forwarding/rule changes](queries/kql/mailbox-forwarding-change.kql)
- [Temporary cloud logging change](queries/kql/temporary-cloud-logging-change.kql)

### SPL
- [Legitimate binary, abnormal parentage](queries/spl/abnormal-parentage.spl)
- [Mailbox forwarding/rule changes](queries/spl/mailbox-forwarding-change.spl)
- [First-time destination](queries/spl/first-time-destination.spl)

---

## ATT&CK usage

ATT&CK mappings in this repository are deliberately conservative.

A pattern may be **related to** an ATT&CK behavior without being sufficient evidence that the technique occurred. Where the relationship is indirect, the entry says *Related ATT&CK* rather than presenting the mapping as a detection verdict.

ATT&CK evolves. Verify mappings against the live ATT&CK site before turning them into durable engineering metadata.

---

## Scope and limitations

This project intentionally avoids:

- payloads
- exploit instructions
- offensive tradecraft walkthroughs
- claims that an anomaly is malicious by itself
- universal thresholds
- vendor-specific assumptions presented as general truth

The examples are defensive and hypothesis-driven. A useful hunt must be tuned to the organization, telemetry quality, administrative practices, and business context.

---

## Contributing

Contributions are welcome when they improve **signal quality, not volume**.

See [CONTRIBUTING.md](CONTRIBUTING.md).

---

## References

Primary references and supporting guidance are maintained in [REFERENCES.md](REFERENCES.md).

---

## License

- Code and query examples: MIT — see `LICENSE-CODE`
- Documentation and written content: CC BY 4.0 — see `LICENSE-DOCS.md`
