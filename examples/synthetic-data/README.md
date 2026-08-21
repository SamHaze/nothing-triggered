# Synthetic data examples

This directory is reserved for small, sanitized datasets that demonstrate query behavior without exposing production or client data.

Good examples:

- fabricated Entra sign-ins with a baseline device followed by a first-seen device
- synthetic process trees showing Office/browser → script-host parentage
- mock mailbox-rule audit events
- synthetic cloud logging delete/write sequences
- generated network records containing a first-seen destination

Do not commit:

- client telemetry
- real tenant IDs
- internal domains
- employee identifiers
- production IP addresses
- secrets, tokens, or credentials

Synthetic examples should make it possible to explain *why a hunt matched* and *why a benign case did not*.
