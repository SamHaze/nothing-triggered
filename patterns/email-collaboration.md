# Email & Collaboration

## 17) Mailbox rules that make sense individually

Examples:

- moving security notifications
- hiding finance threads
- redirecting selected messages
- auto-forwarding "for convenience"

**Why it matters**  
Mailbox rules are legitimate features. In a compromised account, they can alter what the user sees, preserve access to message content, or support collection. The rule's timing and relationship to account activity matter more than its mere existence.

**What to collect**
- rule creator
- creation/modification time
- rule conditions/actions
- forwarding/redirect targets
- hidden-rule indicators where available
- account sign-ins
- mailbox audit events

**Correlate with**
- unusual sign-in
- OAuth consent
- new forwarding destination
- finance/security keywords
- subsequent collection or fraud activity

**Common benign explanations**
- legitimate inbox organization
- delegation
- workflow automation
- travel/vacation forwarding
- shared-mailbox administration

**Related ATT&CK**  
T1114.003 — Email Forwarding Rule when rules are used for email collection.

**Reference hunts**  
- [KQL](../queries/kql/mailbox-forwarding-change.kql)
- [SPL](../queries/spl/mailbox-forwarding-change.spl)

---

## 18) Low-impact social-engineering probes before credential capture

Examples:

- benign-looking document-share prompts that produce unusual interaction telemetry
- suspicious links with no delivered payload
- repeated low-impact lures against a narrow group
- interaction patterns that appear to test which recipients engage

**Why it matters**  
A weak or unsuccessful interaction can still provide reconnaissance value, but this pattern should be treated cautiously. The analyst should be able to explain **why the activity looks like probing rather than an ordinary failed campaign**.

**What to collect**
- message sender/domain
- recipient set
- click/open telemetry
- redirect chain
- authentication prompts
- campaign timing
- follow-on messages to the same recipients

**Correlate with**
- later credential-harvesting attempt
- repeated targeting of users who interacted
- new domain/infrastructure
- device-code or OAuth prompts
- account anomalies after interaction

**Common benign explanations**
- legitimate training simulations
- marketing links
- misconfigured document shares
- ordinary failed phishing
- security-awareness testing

**Related ATT&CK**  
Phishing techniques may become relevant when the behavior constitutes an actual phishing attempt. Do not infer reconnaissance solely from a click with no payload.
