# Network & Egress

## 14) First-time destination with no known malicious reputation

A host or user communicates with a domain/IP that:

- has no local historical association
- is new to the host or peer group
- has no known malicious reputation at the time

**Why it matters**  
"Not known malicious" is not the same as benign. New infrastructure may have little reputation history. The destination becomes useful only when the process, identity, prevalence, timing, or data flow is also unusual.

**What to collect**
- source host/user
- initiating process where available
- destination domain/IP
- DNS history
- TLS SNI/certificate metadata
- destination prevalence
- bytes/packets
- first/last seen

**Correlate with**
- unusual parent process
- script execution
- recent identity anomaly
- file creation
- rare TLS/client fingerprint
- low organizational prevalence

**Common benign explanations**
- SaaS/CDN infrastructure
- software updates
- newly deployed business services
- advertising/analytics endpoints
- mobile/cloud infrastructure churn

**Related ATT&CK**  
Do not map "first seen" by itself to an ATT&CK technique. Map the actual protocol, C2, or transfer behavior only when additional evidence supports it.

**Reference hunt**  
[SPL: first-time destination](../queries/spl/first-time-destination.spl)

---

## 15) Short-lived TLS sessions with contextual anomalies

A brief TLS connection is more interesting when it is also:

- first-seen for the host
- initiated by an unusual process
- rare across peers
- associated with unusual byte ratios
- repeated periodically
- preceded by an identity or execution anomaly

**Why it matters**  
Short TLS sessions are extremely common. Their value comes from **contextual rarity**, not duration alone.

**What to collect**
- source/destination
- initiating process
- SNI/certificate metadata
- TLS/client fingerprint where available
- bytes sent/received
- session count and periodicity
- destination prevalence

**Correlate with**
- first-seen domain
- rare process
- script execution
- token/authentication anomaly
- subsequent larger transfer

**Common benign explanations**
- API calls
- health checks
- telemetry
- certificate validation
- browser/background services
- SaaS polling

**Related ATT&CK**  
No technique should be inferred from session duration alone.

---

## 16) One-time DNS resolution with execution or connection context

A single A/AAAA lookup becomes more useful when paired with:

- first-seen process behavior
- immediate outbound connection
- low organizational prevalence
- rare domain age/reputation context
- unusual parent process
- subsequent authentication or data-access anomaly

**Why it matters**  
One-off DNS is normal in enterprise environments. The signal is the **relationship** between the resolution and other rare behavior.

**What to collect**
- query name/type
- source host/user
- process where available
- answer/IP
- connection that followed
- organizational prevalence
- first/last seen

**Correlate with**
- process creation
- TLS/HTTP session
- download behavior
- first-seen destination
- identity anomaly

**Common benign explanations**
- CDNs
- tracking/analytics
- browser resources
- update infrastructure
- randomized SaaS endpoints

**Related ATT&CK**  
Do not map one-time DNS resolution to ATT&CK without evidence of the behavior it supported.
