# Automation Strategy

Project Reference: Shopping List App (MERN)  
Scope: Test Automation Approach  
Last Updated: 2026-02-25

---

## 1. Purpose

This document defines the automation strategy applied to the Shopping List application.  
The objective is to ensure reliable regression protection, risk-based coverage, and maintainable automated tests across test levels.

The approach follows a pyramid-aligned structure with emphasis on fast, stable feedback.

---

## 2. Test Level Strategy

### 2.1 Backend (API-Level)

**Tools:** Jest, Supertest  

Scope:
- Authentication endpoints (register, login, current user)
- JWT middleware validation
- CRUD operations for shopping items
- Access control and cross-user isolation
- Negative and error-path validation

Rationale:
Core business logic and security behaviour are validated at API level to ensure fast and deterministic feedback.

---

### 2.2 Frontend (Component / Integration)

**Tools:** Vitest, React Testing Library, MSW  

Scope:
- Login and registration flows
- Protected route redirects
- Authentication state transitions
- Shopping list rendering and interaction
- Error and loading states

Rationale:
UI behaviour is validated independently of backend services using controlled API mocks.  
Focus is on user-visible outcomes and state consistency.

---

### 2.3 End-to-End (System)

**Tool:** Playwright  

Scope:
- Login and logout lifecycle
- Protected route enforcement
- Item creation and deletion
- Cross-user data isolation (full-stack validation)

Rationale:
E2E coverage is limited to critical user journeys to minimise flakiness while validating integration across frontend, backend, and database.

---

## 3. Design Principles

- **Risk-based prioritisation:** authentication, access control, and core CRUD flows are primary automation targets.
- **Deterministic execution:** isolated test databases and controlled setup/teardown.
- **Layer separation:** avoid duplication across test levels.
- **Regression-first mindset:** automation protects high-impact functionality.

---

## 4. Regression Model

- Backend and frontend tests provide fast feedback.
- E2E tests validate system integration.
- All automated suites are executed via CI.
- Authentication, CRUD, and isolation failures are considered blocking defects.