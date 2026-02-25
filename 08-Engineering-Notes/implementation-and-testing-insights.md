# Implementation & Testing Insights

Project: Shopping List App (MERN)  
Author: Joshua Pearson  
Scope: Engineering Lessons from Implementation and Test Stabilisation  
Status: Portfolio Artifact  

---

## 1. Purpose

This document captures key engineering and testing lessons learned during the development and stabilisation of the Shopping List App.

The focus is on:

- Architectural decisions that affected testability
- Environment and database lifecycle management
- CI stability and determinism
- UX behaviour under failure conditions
- Cross-layer debugging and defect resolution

These insights reflect practical QA + development integration rather than theoretical test planning.

---

# 2. Backend Architecture & Database Lifecycle

## 2.1 Import-Time Database Coupling

Issue:  

Backend integration tests were intermittently failing due to implicit database access at module import time and unclear DB lifecycle ownership.

### Root Causes

- `getDB()` accessed during route import
- `connectDB()` returned before DB was fully initialised
- Multiple concurrent connection attempts
- Environment variables loaded too late in Jest lifecycle
- Development and test configurations insufficiently isolated

### Impact

- Non-deterministic test failures
- CI instability
- Hook timeouts
- Reduced confidence in regression results

### Resolution Principles

- No database access at import time
- Explicit, single DB connection per test run
- Dedicated `config.test.env`
- Centralised test setup/teardown
- Clear separation of development, test, and E2E environments

### Insight

Database lifecycle management is a core architectural concern.  
Implicit side effects create hidden coupling and unstable automation.

Deterministic tests require explicit infrastructure control.

---

# 3. E2E Environment Orchestration & Race Conditions

Issue:  

Playwright tests intermittently failed due to race conditions between:

- MongoDB container readiness
- Backend startup
- Frontend availability
- CI triggering test execution prematurely

### Root Causes

- Backend began listening before DB connection was guaranteed
- No explicit readiness checks for MongoDB container
- E2E DB configuration misalignment
- CI triggered tests before environment stabilised

### Impact

- HTTP 500 errors during authentication
- `ECONNREFUSED` errors
- False-negative CI failures
- Masked real regressions

### Resolution Principles

- Backend must connect to DB before listening
- Explicit readiness verification for Mongo container
- Unified E2E DB configuration
- CI orchestration mirrors local behaviour
- Health checks before Playwright execution

### Insight

Infrastructure timing issues can mimic functional defects.  
CI reliability is a quality attribute, not an afterthought.

---

# 4. Failure-Mode UX Gaps

## 4.1 Raw Network Error Exposure

Issue:  

When the backend was unavailable, the UI displayed raw browser network errors.

### Insight

- Functional correctness was intact.
- Failure-mode UX was neglected.
- Error-handling logic must translate technical failures into user-safe messages.

Quality includes controlled degradation.

---

## 4.2 Silent Session Invalidation

Issue:  

When a JWT was removed or invalidated, the user was logged out without explanation.

### Insight

- Security behaviour worked correctly.
- UX expectation was not met.
- Session lifecycle requires consistent messaging.

Security logic alone is insufficient; user perception matters.

---

# 5. Test Architecture Lessons

## 5.1 Separation of Concerns Improves Testability

Separating:

- `app.js` (Express app export)
- `server.js` (HTTP server start)

greatly simplified:

- Integration testing
- E2E orchestration
- Lifecycle control

Testability improves when runtime startup logic is decoupled from application definition.

---

## 5.2 Layer Responsibility Clarity

Each layer has a defined role:

- Backend tests → business logic, security, DB integrity
- Frontend tests → UI behaviour and state transitions
- E2E tests → integrated user journeys
- Exploratory testing → failure modes and UX behaviour

Avoiding duplication reduces flakiness and maintenance cost.

---

## 5.3 Determinism Over Speed

Key stabilisation principles:

- DB reset before execution
- Unique users in E2E
- Browser storage cleared between tests
- Environment isolation per context
- No shared persistent fixtures

Stable CI > fast CI.

---

# 6. CI as a Quality Gate

CI instability exposed architectural weaknesses.

Key improvements:

- Explicit environment readiness
- Clear separation of test contexts
- Removal of hidden side effects
- Stable, repeatable pipelines

Lesson:

CI reliability reflects architecture quality.

---

# 7. Security-Related Functional Testing Insights

Cross-user data isolation was validated at:

- Backend level (integration tests)
- E2E level (system perspective)

This dual-layer validation protects:

- Access control
- Multi-user data integrity
- Demo confidence

High-risk areas deserve multi-layer protection.

---

# 8. Regression & Defect Handling Discipline

Defect process followed:

1. Reproduce manually
2. Confirm with automated test
3. Identify root cause
4. Apply architectural fix (not patch)
5. Add regression safeguard
6. Verify in CI

Regression protection is added where defects reveal coverage gaps.

---

# 9. Broader Engineering Lessons

- Import-time side effects undermine stability.
- Environment coupling introduces hidden complexity.
- Infrastructure readiness must be explicit.
- UX behaviour under failure is part of quality.
- Small E2E suites are more sustainable than broad fragile ones.
- Test architecture reflects system architecture quality.

---

# 10. Summary

This project reinforced that:

- Testability is an architectural property.
- Deterministic automation requires environment discipline.
- CI stability is a quality requirement.
- Security validation must occur at multiple layers.
- Failure-mode UX deserves structured attention.

The experience reflects integration of QA thinking with development architecture, rather than treating testing as a separate afterthought.