# Risk Matrix – Shopping List App
Author: Joshua Pearson  
Document type: QA Risk Assessment (ISTQB-aligned)  
Scope: Shopping List App (Full-stack MERN)

---

## 1. Purpose

This document identifies and evaluates key product and project risks for the Shopping List App. It is used to prioritise testing effort and inform the test strategy described in the Master Test Plan.

Risk assessment follows ISTQB principles by considering:
- Likelihood of failure
- Impact of failure
- Appropriate test mitigation strategies

---

## 2. Risk Evaluation Scale

### Likelihood
- **Low** – Unlikely to occur; stable or well-covered area
- **Medium** – Possible; moderate complexity or change frequency
- **High** – Likely; complex, error-prone, or historically risky

### Impact
- **Low** – Minor inconvenience or cosmetic issue
- **Medium** – Loss of functionality or degraded user experience
- **High** – Data loss, security issue, or core functionality broken

---

## 3. Product Risk Matrix

| ID | Risk Area           | Description                                                 | Likelihood | Impact | Risk Level | Test Mitigation                                                  |
|----|---------------------|-------------------------------------------------------------|------------|--------|------------|------------------------------------------------------------------|
| R1 | Authentication & Authorization | Incorrect handling of login, JWT validation, or protected routes allowing unauthorised access | Medium | High | **High** | Multi-layer testing: backend auth tests, frontend protected route tests, E2E auth journeys |
| R2 | User Data Isolation | One user accessing or modifying another user’s shopping items | Low–Medium | High | **High** | Backend integration tests enforcing user ownership; E2E cross-user scenarios |
| R3 | CRUD Persistence | Items not correctly created, updated, deleted, or persisted in the database | Medium | Medium | **Medium** | Backend integration tests + frontend state tests + E2E smoke coverage |
| R4 | Partial Updates (PATCH behaviour) | Incorrect merge or overwrite of item fields during updates | Medium | Medium | **Medium** | Backend tests targeting update logic; regression tests for known defect classes |
| R5 | Frontend State Management | UI showing stale, duplicated, or inconsistent item state | Medium | Medium | **Medium** | Frontend integration tests with mocked API responses; state transition testing |
| R6 | Error Handling & Invalid Input | App crashes or behaves unpredictably on invalid input or API errors | Medium | Medium | **Medium** | Negative testing at backend and frontend levels; MSW error simulations |
| R7 | E2E Test Flakiness | Unstable E2E tests reducing confidence in CI results | Medium | Medium | **Medium** | Keep E2E suite small; strict DB reset; failure artefacts (screenshots, traces) |
| R8 | Test Environment Configuration | Tests accidentally running against wrong database or environment | Low | High | **Medium** | Environment safeguards, explicit test DB names, fail-fast checks |
| R9 | CI Regression Gaps | Regressions introduced without being detected by automated tests | Low–Medium | Medium | **Medium** | CI execution on push/PR; regression-focused test selection |
| R10 | Accessibility Gaps | UI not usable via keyboard or screen readers | Medium | Low–Medium | **Low–Medium** | Exploratory testing notes; listed as future enhancement (not fully automated) |

---

## 4. Project / Process Risks

| ID | Risk Area             | Description                                                  | Likelihood | Impact | Risk Level     | Mitigation                                               |
|----|-----------------------|--------------------------------------------------------------|------------|--------|----------------|----------------------------------------------------------|
| P1 | Over-reliance on E2E  | Too many E2E tests increasing maintenance cost and flakiness | Medium     | Medium | **Medium**     | Test pyramid enforcement; prefer lower-level coverage     |
| P2 | Mock Drift (Frontend) | Frontend mocks diverging from real backend behaviour         | Medium     | Medium | **Medium**     | Use realistic MSW handlers; verify with backend/E2E tests |
| P3 | Documentation Drift   | Test documentation becoming outdated as features evolve      | Low–Medium | Medium | **Low–Medium** | Keep documentation close to code; update during changes   |

---

## 5. Risk-Based Testing Priorities

Based on the above assessment, testing effort is prioritised as follows:

### Highest priority
- Authentication and authorization
- User data isolation
- Core CRUD persistence

### Medium priority
- State management
- Error handling
- Update logic (PATCH)
- CI stability

### Lower priority (current iteration)
- Accessibility
- Non-critical UI polish
- Performance/load characteristics

---

## 6. Residual Risk and Acceptance

Some risks are accepted for the current iteration due to scope and resource constraints:
- Accessibility testing is limited to exploratory checks
- Performance testing is not included
- Visual regression testing is not implemented

These risks are documented and may be addressed in future iterations.

---

## 7. Relationship to Test Plan

This risk matrix directly informs:
- Test scope selection
- Test level emphasis (backend vs frontend vs E2E)
- Regression focus in CI
- Entry/exit criteria in the Master Test Plan

Changes to application scope or architecture should trigger a review of this risk assessment.