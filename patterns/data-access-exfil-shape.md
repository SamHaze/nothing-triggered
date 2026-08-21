# Data Access & Exfil Shape

## 19) Unusual breadth, normal depth

Examples:

- many objects touched once
- shallow access across many folders/sites
- broad enumeration with little content read from each location
- many resources queried without a corresponding business workflow

**Why it matters**  
Discovery and collection can have a different **shape** from normal user work. Breadth may increase before volume does.

**What to collect**
- object/resource IDs
- count of unique locations
- operation type
- bytes/items retrieved
- user/app identity
- time window
- peer-group baseline

**Correlate with**
- cloud-service discovery
- unusual app consent
- new device/IP
- later bulk download/export
- privilege changes

**Common benign explanations**
- search/indexing
- backup
- DLP/eDiscovery
- inventory tooling
- migrations
- compliance scans

**Related ATT&CK**  
Map the specific discovery or collection behavior only when the data source and operation support it; access breadth alone is an analytic feature.

---

## 20) Low-volume transfer preceding larger access

Examples:

- small cloud-file export followed by a quiet interval
- low-volume API retrieval before broader collection
- brief outbound transfer from an endpoint before later higher-volume activity

**Why it matters**  
A small transfer may test access, permissions, routing, or data value. The signal is strongest when the same identity, host, application, or destination later performs larger related activity.

**What to collect**
- direction of transfer
- object/file type
- bytes/items
- source/destination
- identity/application
- time between small and later transfer
- permission changes

**Correlate with**
- prior discovery
- first-seen destination
- unusual identity context
- archive creation
- later volume increase

**Common benign explanations**
- preview/download testing
- sync clients
- API pagination
- user spot-checking files
- backup verification

**Related ATT&CK**  
The specific exfiltration technique depends on the channel. Do not label a small download or export as exfiltration without evidence of unauthorized transfer.
