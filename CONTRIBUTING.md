# Contributing

PRs are welcome when they add **signal quality, not volume**.

A proposed pattern should explain:

- **what it looked like at the time**
- **why it mattered in hindsight**
- **what made it dismissible initially**
- **what telemetry was required**
- **what correlated behavior increased confidence**
- **common benign explanations**
- **which data source(s) confirmed or disproved the hypothesis**
- **whether the ATT&CK relationship is direct or merely related**

For query contributions, include:

- the expected data source/table/index
- assumptions about field names
- a short tuning note
- at least one likely benign explanation
- no organization-specific secrets, tenant IDs, internal domains, or client data

Do not submit:

- payloads
- exploit steps
- credential theft instructions
- offensive how-tos
- rules that depend only on an IOC or reputation verdict
- thresholds presented as universally safe defaults

The objective is a repository another defender can use to reason more clearly, not a catalog of suspicious-looking strings.
