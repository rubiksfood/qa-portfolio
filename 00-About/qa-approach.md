# QA Approach

## Overview

My approach to quality assurance is risk-based, layered, and infrastructure-aware.

Quality is not limited to finding visible defects. It includes ensuring that systems are deterministic, 
test environments are stable, and CI results are trustworthy. My work reflects a focus on reliability, 
clear defect communication, and traceable decision-making.  

---

## 1. Risk-Based Testing

Testing effort is prioritised based on impact and likelihood.

High-risk areas typically include:

- Authentication and authorisation
- Database lifecycle and persistence
- State management
- CI reliability and test determinism
- User-facing error handling

Rather than attempting exhaustive coverage, I prioritise:

- Core workflows
- Failure modes
- Edge conditions that affect stability or user trust

Smoke and regression checklists are derived from identified risk areas and maintained explicitly.

---

## 2. Layered Test Strategy

Testing is structured across layers:

### Unit Tests
- Validate isolated logic.
- Fast feedback for core functions.
- Guard against regressions in business logic.

### Integration Tests
- Validate backend behaviour with real database connections.
- Focus on lifecycle correctness and data integrity.
- Ensure explicit, concurrency-safe DB management.

### End-to-End (E2E) Tests
- Validate full-stack workflows.
- Confirm authentication flows and user journeys.
- Ensure system behaviour remains stable in CI.

### Manual & Exploratory Testing
- Target UX gaps and failure scenarios.
- Identify issues not easily caught through automation.
- Validate error messaging and graceful degradation.

Each layer serves a different purpose. Automated tests are not a replacement for structured manual validation.

---

## 3. Deterministic Test Environments

Reliable test results are a core quality attribute.

Key principles:

- Backend must not accept requests before DB readiness.
- Database lifecycle must be explicit and concurrency-safe.
- Test environments must be isolated from development configuration.
- CI must mirror local behaviour as closely as possible.
- Readiness checks must be explicit rather than timing-based.

Infrastructure instability is treated as a defect, not an inconvenience.

---

## 4. Error Handling & Failure Modes

Quality includes behaviour under failure conditions.

Testing includes:

- Network outages
- Invalid sessions
- Expired or missing tokens
- Backend unavailability
- Invalid input scenarios

The goal is graceful degradation:
- Clear user-facing messaging
- No exposure of raw technical errors
- Predictable and recoverable behaviour

---

## 5. Defect Reporting

Defects are documented with:

- Clear reproduction steps
- Expected vs actual results
- Severity justification
- Root cause analysis (when available)
- Verification and regression safeguards

Severity is impact-driven and avoids inflation.  
Not all defects are user-facing, but infrastructure instability is treated as high impact due to its effect on release confidence.

---

## 6. Continuous Improvement

Quality artifacts evolve alongside the system.

Improvements typically include:

- Adding regression safeguards after stabilisation work
- Aligning configuration across environments
- Strengthening readiness guarantees
- Refining checklists based on discovered defects

Each resolved issue informs future test design and environment structure.

---

## 7. Alignment

This approach aligns with:

- Risk-based testing principles
- Structured defect lifecycle management
- Layered test strategy models
- Deterministic CI practices

The goal is not only to detect defects, but to build systems that are stable, predictable, and maintainable.
