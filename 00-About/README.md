# About

This folder defines the QA principles and classification model used across this portfolio.

It provides context for the test artifacts, bug reports, and traceability references found in the repository.

---

## Contents

### qa-approach.md
Describes the overall testing philosophy, including:

- Risk-based testing
- Layered test strategy (unit → integration → E2E → manual)
- Deterministic CI and environment stability
- Failure-mode and UX validation

This document explains *how* quality decisions are made.

---

### severity-and-priority-model.md
Defines the defect classification model used in bug reports.

- Severity = impact
- Priority = urgency

This ensures consistent reasoning across:
- Infrastructure defects
- CI instability
- UX issues
- Session and error-handling cases

---

## Relationship to the Repository

The documents in this folder support the artifacts in:

- `01-Test-Strategy/`
- `02-Test-Cases/`
- `03-Bug-Reports/`

They describe the framework behind the practical QA work demonstrated elsewhere in the portfolio.

---

## Scope

This portfolio demonstrates structured QA thinking applied to real project work.

It focuses on:

- Risk identification
- Clear defect communication
- Stability and determinism
- Traceable test coverage
- Practical, maintainable documentation

The intent is clarity and discipline rather than volume.