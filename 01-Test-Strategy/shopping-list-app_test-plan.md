# Master Test Plan – Shopping List App (MERN)
Author: Joshua Pearson  
Last updated: 2026-01-11  
Document type: ISTQB-aligned Master Test Plan (project-level)

---

## 1. Purpose and Objectives

### 1.1 Purpose
This document defines the overall test approach for the Shopping List App across:
- Backend (API + security-related functional testing)
- Frontend (UI + state + routing with mocked backend)
- End-to-End (E2E) system validation (Playwright)

It consolidates the existing layer-specific test strategy and specifications into a single plan suitable for QA portfolio review and project governance.  

### 1.2 High-level objectives
- Provide high confidence in correctness and regression protection using a test pyramid approach.  
- Validate authentication, access control, and user-specific data isolation (primary risk area).  
- Validate core user workflows: login/logout, protected route handling, and shopping item CRUD with persistent storage.  
- Ensure automated tests run reliably in CI and produce actionable failure evidence (reports/artifacts).  

---

## 2. Test Items (What is being tested)

### 2.1 Application under test
Shopping List App (full-stack MERN):
- Frontend: React (Vite), Tailwind, React Router, AuthContext, custom hook `useShopItems`  
- Backend: Node.js (ESM), Express routes, JWT auth middleware, MongoDB persistence  
- E2E: Playwright system tests, Docker-backed E2E database and reset strategy  

### 2.2 Key features and behaviours (baseline scope)
- Secure authentication: register/login, JWT session, protected API routes and protected pages  
- Shopping list management: create, read, update, delete items; toggle `isChecked`; list grouping (active vs checked); modal add/edit  
- Per-user isolation: User A cannot access User B’s items (backend + E2E)  

Planned feature (explicitly out of current functional scope unless implemented):
- Shared lists / shared household  

---

## 3. Test Basis (Sources of requirements / test conditions)

Test conditions and cases are derived from:
- Project README and usage guide (expected user behaviour and workflows)  
- Testing strategy and scope boundaries  
- Backend test process/specification (routes, conditions, test environment)  
- Frontend test process/specification (components, routing, context/hook behaviours)  
- E2E test specification (critical user journeys, system-level risks, evidence)  
- Development notes describing historical defects and risk areas (data integrity, patch behaviour, `_id` update risk)  

---

## 4. Test Strategy (ISTQB-aligned)

### 4.1 Overall approach
A pragmatic test pyramid:
- Broad automated coverage at backend and frontend layers
- Small, stable E2E layer validating critical user journeys
- Risk-based selection to maximise confidence while controlling flakiness and maintenance cost  

### 4.2 Test levels
1) **Backend**
- Unit: JWT middleware behaviour
- Integration/System (backend-only): Express routes + DB + JWT with realistic request/response flows (Jest + Supertest)  

2) **Frontend**
- Unit/Integration/System (frontend-only): React components/pages + AuthContext + routing, with API mocked via MSW (Vitest + RTL + MSW)  

3) **End-to-End (System)**
- Playwright tests in a real browser against running frontend/backend and a dedicated E2E database (Docker-managed), reset before execution  

### 4.3 Test types
- Functional testing (core workflows and CRUD correctness)  
- Security-related functional testing:
  - authentication/authorization behaviour
  - access control, data isolation, protected route behaviour  
- Negative and error-handling testing:
  - missing/invalid inputs, malformed headers/tokens, invalid IDs
  - API error handling, 401 handling in UI where implemented  
- State-based testing:
  - auth state transitions (logged out → logged in → logged out)
  - item `isChecked` transitions and list grouping behaviour  
- Regression testing:
  - automated across all layers, executed via CI pipelines  

### 4.4 Test design techniques (ISTQB CTFL-aligned)
- Equivalence Partitioning (valid/invalid credentials, inputs, headers)  
- Boundary Value Analysis (planned for stronger password/item validation when enforced)  
- Decision Table Testing (login outcomes based on credential combinations; auth states)  
- State Transition Testing (auth lifecycle, `isChecked` toggling, navigation changes)  
- Error Guessing (malformed JWT, invalid ObjectId, network failures, cross-user access)  

### 4.5 Coverage boundaries (explicitly intentional)
**Covered**
- Auth + route protection
- CRUD for items, toggle behaviour, per-user isolation
- Error handling and invalid access attempts
- CI-driven regression safety net  

**Intentionally not covered (current iteration)**
- Performance/load testing
- Visual regression / pixel-perfect checks
- Exhaustive cross-browser testing
- Accessibility testing (not yet implemented; listed as future enhancement)  
- Deep E2E negative testing (handled at backend/frontend levels)  

---

## 5. Test Environment and Test Data

### 5.1 Environments
**Local development/exploration**
- App runs via `npm run dev` (frontend + backend concurrently)  

**Backend automated tests**
- Node.js (ESM), Jest, Supertest
- Dedicated MongoDB test database: `shopping_list_test`
- Env config via `config.test.env`; `NODE_ENV=test` ensures test DB usage  

**Frontend automated tests**
- Vitest + RTL in JSDOM
- API mocked via MSW; routing via MemoryRouter; AuthProvider wrapper  

**E2E automated tests**
- Playwright in real browser (Chromium default)
- Docker required for E2E DB workflow
- Dedicated E2E DB (e.g., `shopping-list-test-e2e`) and reset before execution
- Env config via `e2e/config.e2e.env`  

### 5.2 Test data strategy
- Backend: DB cleared before each test; users/items created dynamically; avoids persistent fixtures  
- Frontend: no persistent data; MSW handlers provide realistic success/error/401/empty list responses; handlers reset per test  
- E2E: unique test users; browser storage cleared between tests; DB reset and safeguards to avoid deleting non-test DBs  

---

## 6. Test Deliverables (Testware)

### 6.1 Deliverables produced by this project
- Master test plan (this document)
- Layer strategies/specs (already present in project repo):
  - `TESTING_STRATEGY.md`  
  - `TEST_PROCESS_BACKEND.md` / `TESTING_BACKEND.md`  
  - `TEST_PROCESS_FRONTEND.md` / `TESTING_FRONTEND.md`  
  - `TESTING_E2E.md`  
- Automated test suites (code) and CI workflows running on push/PR  

### 6.2 Evidence and reporting
- Backend/frontend: test runner output (Jest/Vitest), stack traces on failure  
- E2E: Playwright HTML report, screenshots, video/trace artifacts on failure (enabled in CI)  

---

## 7. Entry Criteria and Exit Criteria

### 7.1 Entry criteria (start testing)
- Core features implemented: auth, CRUD, toggle, protected routes  
- Test environments configured and runnable locally (backend/frontend/E2E)
- Test databases accessible and isolated (test + E2E DBs)  

### 7.2 Exit criteria (testing considered complete for this iteration)
- All planned automated suites pass locally and in CI
- No open high-severity defects in:
  - auth/access control/data isolation
  - core CRUD workflows
- E2E suite remains small, stable, and green (regression safety net)
- Test documentation kept current and traceable to tests  

---

## 8. Test Activities (ISTQB lifecycle)

### 8.1 Planning
- Define scope by layer, set risk priorities, and define CI as quality gate  
- Agree evidence standards (reports/screens/logs) for failures  

### 8.2 Analysis and design
- Identify test conditions from:
  - route behaviour (backend)
  - UI behaviours/state transitions (frontend)
  - critical journeys (E2E)  
- Design tests using EP/DT/ST/EG and introduce BVA when validation rules exist  

### 8.3 Implementation
- Build testware per layer:
  - backend Jest + Supertest with real test DB and cleanup
  - frontend Vitest + RTL with MSW handlers and providers
  - E2E Playwright with Docker DB reset and setup/teardown  

### 8.4 Execution
- Run locally during development and in CI on push/PR
- Triage failures as:
  - product defect vs test defect vs environment defect  

### 8.5 Completion
- Confirm exit criteria, document gaps/future enhancements (token expiry, accessibility smoke checks, multi-browser E2E)  

---

## 9. Defect Management and Retest/Regression

### 9.1 Defect logging
- Defects will be documented in GitHub Issues with:
  - reproducible steps
  - expected vs actual
  - link to test case/spec and affected layer  

### 9.2 Verification and regression
- Fixes will be verified by re-running relevant suites
- A regression test will be added when a defect indicates a missing or weak safety net
- For E2E, prefer adding coverage at lower layers unless the risk is truly end-to-end  

---

## 10. Risks, Assumptions, and Mitigations

### 10.1 Key risks (project-level)
| Risk                                                                                | Impact                 | Mitigation                                                                 |
|-------------------------------------------------------------------------------------|------------------------|----------------------------------------------------------------------------|
| Flaky tests due to DB state/env issues                                              | Reduced trust in CI    | Strict DB reset/cleanup and isolation (test DBs), health checks (E2E)      |
| Incorrect API mocking on frontend                                                   | False confidence       | Prefer MSW with realistic responses; reset handlers per test               |
| Security regressions (auth/access control/data leakage)                             | High severity          | Multi-layer coverage: backend + E2E data isolation; protected route tests  |
| Schema/data integrity issues (e.g., `isChecked` persistence, PATCH merge behaviour) | User-visible defects   | Include integration/system tests around persistence and updates; regression tests for known defect classes |
| ESM/Jest configuration friction                                                     | Test suite instability | Document configuration and keep minimal Jest/Node flags                    |

### 10.2 Assumptions
- Authentication and CRUD features are implemented as described in README and test specs  
- E2E execution uses dedicated, non-production database with safeguards enabled  

---

## 11. Responsibilities and Roles (small project)
- Test planning/design/implementation: Joshua Pearson
- CI maintenance: Joshua Pearson
- Defect triage and fixes: Joshua Pearson
(For a team setting, these would typically be shared across QA/Dev and product roles.)

---

## 12. References
- Project overview and usage guide: `README.md`  
- Strategy: `TESTING_STRATEGY.md`  
- Backend: `TEST_PROCESS_BACKEND.md`, `TESTING_BACKEND.md`  
- Frontend: `TEST_PROCESS_FRONTEND.md`, `TESTING_FRONTEND.md`  
- E2E: `TESTING_E2E.md`  
- Dev notes / historical issues: `DEV_NOTES.md`  
