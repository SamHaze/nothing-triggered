# Identity & Access

## 1) Valid auth, wrong *story*

A sign-in succeeds, but the surrounding context has changed:

- new device + familiar ASN
- familiar device + new geography
- known user agent + unusual authentication method
- known account + first-seen IP, application, or device registration state

**Why it matters**  
Valid-account abuse can look ordinary because the authentication itself is legitimate. The investigative value comes from inconsistency with the account's established story.

**What to collect**
- user/account identifier
- device ID, trust, compliance, and registration state
- authentication method and Conditional Access outcome
- application/client ID
- IP, ASN, geography, and network type
- token claims where available (`amr`, `appid`, `tid`, `upn`, `ipaddr`)
- historical last-seen and peer-group baselines

**Correlate with**
- recent MFA/security-info changes
- new OAuth grants
- unusual mailbox or file access
- privileged actions after sign-in
- endpoint activity from the same user/device

**Common benign explanations**
- device replacement
- travel
- VPN/proxy changes
- new corporate egress
- mobile carrier changes
- application migration

**Related ATT&CK**  
T1078 — Valid Accounts; T1078.004 — Cloud Accounts.

**Reference hunt**  
[KQL: valid auth, wrong story](../queries/kql/valid-auth-wrong-story.kql)

---

## 2) MFA succeeded, but the *enrollment story* changed

MFA passes, but there is recent:

- authenticator/device enrollment
- security-info modification
- authentication-method change
- device registration associated with the account

**Why it matters**  
An attacker who already has enough access to change the recovery or authentication surface may be able to stabilize access before performing noisier actions.

**What to collect**
- authentication-method audit events
- device-registration events
- actor that initiated the change
- target account
- sign-ins before and after the change
- source IP/device for both the change and subsequent access

**Correlate with**
- successful sign-in shortly after the change
- first-seen device or geography
- privileged activity
- new app consent
- password/session-reset events

**Common benign explanations**
- new phone
- help-desk recovery
- planned MFA migration
- passwordless rollout
- lost-device replacement

**Related ATT&CK**  
T1098 — Account Manipulation; T1098.005 — Device Registration when a device is enrolled for access.

**Reference hunt**  
[KQL: security-info change followed by sign-in](../queries/kql/mfa-enrollment-change.kql)

---

## 3) Privilege use that matches role, not timing

Administrative actions align with the user's job function, but occur:

- at unusual hours
- from an unfamiliar device or network
- in short bursts across many objects
- immediately after an identity or session anomaly

**Why it matters**  
The permission may be legitimate while the **timing and sequence** are not. "Allowed" is precisely why this behavior can blend into normal administration.

**What to collect**
- privileged identity
- role and entitlement state
- target objects
- source device/IP
- timestamps and action sequence
- elevation/activation history where applicable

**Correlate with**
- unusual sign-in context
- recent role changes
- cloud/API enumeration
- mailbox/file access
- security-control changes

**Common benign explanations**
- after-hours maintenance
- incident response
- automation
- approved bulk administration
- time-zone differences

**Related ATT&CK**  
T1078 — Valid Accounts. The timing anomaly itself is not a technique.

---

## 4) Emergency access without matching context

An emergency or break-glass account is used, but there is no corresponding:

- documented exercise
- lockout scenario
- incident
- approved administrative reason

**Why it matters**  
Emergency accounts deliberately have a different operational purpose from normal administrator accounts. Their use outside a documented emergency or test is high-value context even when authentication succeeds.

**What to collect**
- every sign-in and audit event for the account
- source IP/device
- authentication method
- target resources
- administrative actions
- incident/change ticket context

**Correlate with**
- outage or identity-provider incident
- documented emergency-access test
- role/permission changes
- logging or security-tool changes

**Common benign explanations**
- scheduled break-glass test
- recovery from tenant lockout
- documented emergency administration

**Related ATT&CK**  
T1078.004 — Valid Accounts: Cloud Accounts.

**Operational note**  
Emergency access should normally be explicitly monitored; the interesting condition is **use without matching context**, not the absence of an alert as a design goal.

---

## 5) Consent or app access that does not match how work is done

OAuth/app access is technically valid but unusual for the user or organization:

- rare delegated permissions
- new consent to broad data access
- first-seen application
- application access that persists outside the normal interactive workflow
- device-code authentication where it is uncommon

Examples of permissions that deserve context rather than automatic condemnation:

- `Mail.Read`
- `offline_access`
- `Files.Read.All`

**Why it matters**  
OAuth grants and application sessions can preserve application access independently of the user's normal interactive sign-in pattern. Remediation may require revoking the grant and associated sessions/tokens rather than relying only on a password or MFA change.

**What to collect**
- app/client ID and publisher state
- consent type and granted permissions
- granting user/admin
- service-principal creation history
- sign-ins by the application
- device-code/authentication-flow indicators
- downstream mailbox/file/API activity

**Correlate with**
- consent followed by data access
- new app + rare permissions
- device-code sign-in + first-seen IP
- mailbox-rule creation
- abnormal download/export behavior

**Common benign explanations**
- newly approved SaaS integration
- developer testing
- migration tooling
- sanctioned automation

**Related ATT&CK**  
T1098 — Account Manipulation may be relevant when access or credentials are modified to preserve access. Do not force-map every OAuth event to ATT&CK.

**Sources**  
See Microsoft consent-phishing, illicit-consent, and authentication-flow guidance in `REFERENCES.md`.
