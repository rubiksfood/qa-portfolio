# BR-004 – Intermittent E2E Failures Due to Startup and DB Readiness Race Conditions

## Source
Original GitHub Issue: 
https://github.com/rubiksfood/shopping-list-app/issues/4  
Status: Resolved  

---

## Summary

Playwright end-to-end (E2E) tests intermittently failed during authentication and early test execution.
This was due to race conditions between application startup, database readiness, and CI orchestration.

Failures were non-deterministic and typically disappeared on rerun.

---

## Environment

- Application: Shopping List App (MERN stack)
- Test layer: Playwright E2E
- Execution context:
  - Local (Docker-based E2E database)
  - CI (GitHub Actions)
- Database: MongoDB (separate E2E instance)

---

## Preconditions

- E2E test environment started (Docker Mongo container + backend + frontend)
- Playwright tests triggered immediately after service startup

---

## Steps to Reproduce

1. Start E2E test environment (locally or via CI).
2. Immediately trigger Playwright test execution.
3. Observe test behaviour during authentication flow.

---

## Actual Result

Intermittent failures including:

- HTTP 500 errors during authentication
- `ECONNREFUSED` when connecting to MongoDB
- Early test failures that disappear when rerun
- CI passing or failing depending on timing

E2E suite behaved non-deterministically.

---

## Expected Result

- Backend should not accept requests until DB connection is fully established.
- MongoDB container readiness should be confirmed before tests run.
- E2E database configuration should be aligned across application and reset scripts.
- Playwright tests should execute reliably in both local and CI environments.

---

## Severity

High

Rationale:
- Undermines confidence in CI results
- Causes false negatives
- Increases debugging time
- Masks genuine regressions behind infrastructure instability

Although not user-facing, the defect directly impacts release confidence.

---

## Root Cause Analysis

The instability was caused by multiple readiness and configuration issues:

1. Backend began listening for requests before MongoDB connection was fully established.
2. MongoDB container was started without explicit readiness verification.
3. E2E database name/configuration was not fully aligned with reset scripts.
4. CI triggered Playwright before all services were guaranteed available.

The issue represented an environment orchestration race condition rather than a functional defect.

---

## Scope of Fix

The resolution included:

- Ensuring backend connects to DB before listening for requests.
- Unifying E2E DB configuration (`DB_NAME`) across application and reset scripts.
- Adding explicit readiness waits for:
  - MongoDB
  - Backend health endpoint
  - Frontend (where applicable)
- Stabilising CI workflow to mirror local startup behaviour.

---

## Verification

- Repeated local execution of E2E suite with no intermittent failures.
- Multiple CI runs confirming deterministic behaviour.
- No further HTTP 500 or `ECONNREFUSED` errors observed.
- Authentication flow stable under repeated execution.

---

## Regression Safeguards

- Explicit readiness checks implemented in startup sequence.
- CI workflow updated to prevent premature test execution.
- E2E DB lifecycle aligned with reset logic.

---

## Related

Informed by backend DB lifecycle work (Issue #3 – Backend integration tests unstable due to DB lifecycle & test environment coupling).

---

## Lessons Learned

- Deterministic test execution requires explicit readiness guarantees.
- Infrastructure timing issues can mimic functional defects.
- CI reliability is a critical quality attribute, even when not user-facing.