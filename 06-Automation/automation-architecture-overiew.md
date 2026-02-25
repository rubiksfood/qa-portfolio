# Automation Architecture Overview

Project: Shopping List App (MERN)  
Scope: Test Automation Architecture  
Status: Portfolio Artifact  

---

## 1. Purpose

This document outlines the automation architecture implemented for the Shopping List App.

It describes:

- Test pyramid implementation  
- Environment isolation strategy  
- Database separation model  
- CI integration approach  
- Tool selection rationale  

The objective is to demonstrate structured, maintainable, and deterministic automation design.

---

## 2. Test Pyramid Implementation

Automation follows a pragmatic test pyramid:

### Backend Layer (Broad Coverage)

- Integration-style API tests  
- JWT middleware validation  
- CRUD behaviour and data isolation  
- Negative-path and error handling  

This layer provides fast, stable regression protection for core business logic and security-related functionality.

Primary tools:
- Jest  
- Supertest  

---

### Frontend Layer (Focused Integration)

- Component and page-level behaviour  
- Auth state transitions  
- Protected route handling  
- Error and loading states  

API responses are mocked to ensure deterministic behaviour and avoid backend coupling.

Primary tools:
- Vitest  
- React Testing Library  
- MSW  

---

### End-to-End Layer (Small, High-Value Set)

- Authentication lifecycle  
- Protected route enforcement  
- Core CRUD workflows  
- Cross-user data isolation  

E2E tests validate system integration across frontend, backend, and database while remaining intentionally limited to reduce flakiness.

Primary tool:
- Playwright  

---

## 3. Environment Isolation

Environment isolation is treated as a core quality requirement.

Principles:

- Test environments are separated from development configuration  
- No automated test interacts with production data  
- Explicit environment variables control test execution context  
- Backend does not accept requests before DB readiness  
- Browser state is cleared between E2E tests  

Isolation prevents cross-test contamination and increases CI reliability.

---

## 4. Database Separation Strategy

Dedicated databases are used per test context:

- Backend test database  
- E2E test database  

Key controls:

- Database cleanup before each backend test  
- Dedicated E2E reset process before execution  
- Safeguards to prevent accidental deletion of non-test databases  
- No shared persistent fixtures across tests  

This ensures:

- Deterministic outcomes  
- Elimination of hidden state dependencies  
- Safe parallel development and testing  

Database lifecycle management is considered part of the automation architecture, not an afterthought.

---

## 5. CI Integration

Automation is integrated into CI as a quality gate.

Pipeline structure:

1. Backend tests (fast feedback)  
2. Frontend tests (fast feedback)  
3. E2E tests (system-level validation)  

Characteristics:

- Tests execute on push and pull requests  
- Failures block merge  
- E2E failures produce artifacts (reports, traces, screenshots)  
- CI mirrors local execution as closely as possible  

CI reliability is treated as a measurable quality attribute.

---

## 6. Tool Selection Rationale

Tool choices were made based on ecosystem alignment, stability, and suitability for project size.

### Jest + Supertest
- Native fit for Node.js + Express  
- Mature ecosystem  
- Clear HTTP simulation  
- Strong support for async flows  

### Vitest + React Testing Library + MSW
- Fast execution within Vite environment  
- Encourages behaviour-focused testing  
- MSW enables realistic API mocking without tight backend coupling  

### Playwright
- Reliable modern browser automation  
- Strong debugging artifacts (trace viewer, video, screenshots)  
- Good CI compatibility  
- Clear test isolation controls  

Tools were selected to prioritise:

- Deterministic execution  
- Low flakiness  
- Maintainability  
- Clear failure diagnostics  

---

## 7. Design Principles

The automation architecture follows these principles:

- Risk-based prioritisation  
- Layer separation to avoid redundancy  
- Deterministic test execution  
- Infrastructure stability as a quality requirement  
- CI as mandatory regression gate  

The architecture is intentionally proportional to the system size while remaining scalable for future extension.