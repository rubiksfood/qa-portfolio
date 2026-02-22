# BR-003 – Backend Integration Test Instability Due to Database Lifecycle Coupling

## Source
Original GitHub Issue: https://github.com/rubiksfood/shopping-list-app/issues/3  
Project: Shopping List App  
Status: Resolved  

---

## Summary

Backend Jest integration tests intermittently failed or hung due to ambiguous database lifecycle management, implicit DB access at import time, and environment configuration coupling between development and test contexts.

The instability resulted in non-deterministic behaviour, inconsistent CI runs, and difficult-to-diagnose timing-related failures.

---

## Environment

- Application: Shopping List App (Backend – Node.js, Express, MongoDB)
- Test framework: Jest (integration tests with real database)
- Execution context:
  - Local development
  - CI (GitHub Actions)
- Database contexts:
  - Development
  - Test
  - E2E

---

## Preconditions

- Backend integration test suite executed via Jest.
- Test environment variables not fully isolated from development configuration.
- Database lifecycle handled implicitly across modules.

---

## Steps to Reproduce

1. Execute backend integration test suite.
2. Observe intermittent failures such as:
   - "Database not initialised. Call connectDB() first."
   - Hook timeouts during beforeAll(connectDB).
3. Re-run test suite and observe different behaviour.
4. Run test suite multiple times locally and in CI.

---

## Actual Result

- Tests intermittently failed or hung.
- Database initialisation errors occurred unpredictably.
- Multiple concurrent connectDB() calls were triggered.
- Test behaviour differed depending on execution order or environment.
- CI runs occasionally timed out.

Test results were non-deterministic and unreliable.

---

## Expected Result

- Backend tests use a dedicated test environment configuration.
- Database connection is established once per test run.
- No module or route accesses the database at import time.
- Database lifecycle is explicit, isolated, and concurrency-safe.
- All integration tests pass consistently locally and in CI.

---

## Severity

High

Rationale:
- Undermines confidence in backend test results.
- Causes CI instability and delays.
- Makes defect diagnosis difficult.
- Introduces risk of environment cross-contamination.

Although not user-facing, this defect directly impacts release confidence and development productivity.

---

## Root Cause Analysis

Multiple contributing factors were identified:

1. getDB() was called at module import time in route files, causing implicit DB access.
2. connectDB() returned before the database was fully initialised.
3. No single, explicit database lifecycle was defined for backend tests.
4. Multiple concurrent connectDB() calls occurred during a single test run.
5. Test environment variables were not loaded early enough in Jest execution.
6. Test configuration was too tightly coupled to development runtime behaviour.

The issue represented architectural lifecycle coupling rather than a functional defect.

---

## Scope of Fix

The resolution included:

- Isolating backend test configuration from development and E2E environments.
- Introducing an explicit, concurrency-safe database lifecycle.
- Ensuring DB access occurs only inside request handlers.
- Preventing database access at module import time.
- Centralising database setup and teardown in Jest global setup.
- Using a dedicated test environment file (config.test.env).

Example of corrected lifecycle approach:

```
async function connectDB() {
  if (db) return db;
}
```

---

## Acceptance Criteria

- Backend tests use a dedicated test environment (config.test.env).
- Database connects once per test run and disconnects cleanly.
- No route or module accesses the DB at import time.
- All backend integration tests pass reliably locally and in CI.
- Test output is clean and readable.

---

## Verification

- Multiple repeated local test runs with no intermittent failures.
- Stable CI execution across repeated pipeline runs.
- No hook timeouts or "Database not initialised" errors observed.
- Confirmed DB connection lifecycle behaves consistently across test contexts.

---

## Regression Safeguards

- Explicit DB lifecycle control in test setup.
- Clear separation between development, test, and E2E configurations.
- Removal of import-time side effects.
- Centralised and documented test environment configuration.

---

## Related

- BR-004 – Intermittent E2E failures due to startup and DB readiness race conditions.
- Backend DB lifecycle improvements informed E2E stability work.

---

## Lessons Learned

- Database lifecycle must be explicit and deterministic in automated tests.
- Import-time side effects create hidden coupling and instability.
- Test environments must be isolated from development runtime configuration.
- CI reliability is a core quality attribute, even when not user-facing.