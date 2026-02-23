# Severity and Priority Model

## Overview

This repository uses a structured defect classification model to ensure consistent, impact-driven, and transparent decision-making.

Severity and Priority are treated as separate dimensions:

- **Severity** reflects the impact of a defect on the system or user.
- **Priority** reflects the urgency with which the defect should be addressed.

This distinction supports rational trade-offs and avoids emotional or inflated classifications.

---

## Severity Levels

Severity is assessed based on:

- User impact
- Data integrity risk
- Security exposure
- System stability
- CI reliability
- Release confidence

### Critical

**Definition:**  
System is unusable, data integrity is compromised, or there is a security vulnerability.

**Examples:**
- Data corruption or loss
- Authorisation bypass
- Application crash on startup
- Production credentials exposed

Critical defects require immediate attention.

---

### High

**Definition:**  
Core functionality is broken or system stability is compromised.

**Examples:**
- Authentication fails for valid users
- Database lifecycle instability causing non-deterministic behaviour
- CI pipelines unreliable due to test race conditions
- Persistent 500 errors in core workflows

High severity may not always be user-visible but significantly impacts release confidence and engineering productivity.

---

### Medium

**Definition:**  
Functionality works but behaviour is degraded, confusing, or inconsistent.

**Examples:**
- Poor error handling in failure scenarios
- Session invalidation without user feedback
- Raw network errors exposed in UI
- Incorrect but recoverable edge-case behaviour

Medium severity issues affect user experience or quality perception but do not threaten system integrity.

---

### Low

**Definition:**  
Minor defects with limited impact.

**Examples:**
- Cosmetic UI issues
- Minor text inconsistencies
- Non-blocking validation message inconsistencies

Low severity defects do not materially affect system behaviour.

---

## Priority Levels

Priority is determined by business context, release timing, and risk exposure.

Severity influences priority, but the two are not identical.

### P1 – Immediate

- Blocks release or deployment
- Affects CI stability or release confidence
- High impact with high likelihood

### P2 – High

- Should be addressed in the current development cycle
- Impacts user trust or workflow clarity
- No immediate release block, but undesirable to defer

### P3 – Normal

- Can be scheduled into upcoming sprint/backlog
- Limited impact or clear workaround exists

### P4 – Low

- Improvement-level issue
- Safe to defer without operational risk

---

## Severity vs Priority Examples

A defect can be:

- **High Severity / Medium Priority**  
  Example: Infrastructure instability in a non-production branch.

- **Medium Severity / High Priority**  
  Example: Confusing logout behaviour before a demo.

Classification considers context, not just technical impact.

---

## Guiding Principles

- Severity is impact-based, not emotion-based.
- User-facing defects are not automatically High severity.
- Infrastructure instability can be High severity even if not user-visible.
- Priority may change over time; severity typically does not.
- Clear justification is required for High or Critical classifications.

This model ensures consistent reasoning across defect reports and aligns with risk-based testing principles.