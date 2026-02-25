# Test Design – Shopping List App

Author: Joshua Pearson  
Scope: System-Level Test Design Across All Test Layers  
Status: Portfolio Artifact  

---

## 1. Purpose

This document defines the test design approach for the Shopping List App.  
It translates system behaviour and risk areas into structured test conditions and coverage across backend, frontend, and end-to-end layers.

The objective is to:

- Ensure risk-based, layered coverage
- Protect authentication and data isolation
- Maintain deterministic regression protection
- Align automation with structured QA design principles

---

## 2. System Under Test

Full-stack MERN application consisting of:

- React frontend (routing, context, hooks)
- Express backend (REST API)
- MongoDB persistence
- JWT-based authentication
- Playwright-based E2E validation

Core capabilities:

- Secure user registration and login
- Protected routes (frontend + backend)
- CRUD operations for shopping items
- Per-user data isolation

---

## 3. Test Design Approach

Testing follows a layered pyramid model:

- Backend integration tests validate business logic and security rules
- Frontend tests validate UI behaviour and state transitions
- E2E tests validate integrated system workflows
- Manual exploratory testing validates failure-mode behaviour

Each test level has a defined responsibility to avoid redundancy.

---

## 4. Risk-Based Design Model

High-risk areas receive multi-layer validation.

| Risk Area                  | Mitigation Layer(s) |
|----------------------------|---------------------|
| Authentication failure     | Backend + E2E       |
| Cross-user data leakage    | Backend + E2E       |
| CRUD integrity             | Backend + Frontend  |
| UI state desynchronisation | Frontend            |
| Backend failure masking    | Frontend + E2E      |
| DB lifecycle instability   | Backend             |

Lower-risk concerns (styling, minor UI variations) are intentionally excluded.

---

## 5. Backend Test Design

### 5.1 Test Objectives

- Validate API correctness
- Validate JWT authentication & authorization
- Prevent cross-user data access
- Ensure correct error handling
- Guarantee deterministic DB behaviour

### 5.2 Identified Test Conditions

#### Authentication
- Valid registration
- Duplicate email rejection
- Valid login
- Invalid login (wrong password / missing fields)
- `/auth/me` with valid token
- `/auth/me` without token

#### Authorization (Middleware)
- Missing Authorization header
- Invalid JWT format
- Valid token populates user context

#### CRUD (Shop Items)
- List only own items
- Create valid item
- Reject malformed input
- Update own item
- Reject update of non-owned item
- Delete own item
- Reject delete of non-owned item

### 5.3 Design Techniques Applied

- Equivalence Partitioning (valid vs invalid input)
- Decision Table Testing (credential combinations)
- State Transition Testing (JWT lifecycle)
- Error Guessing (malformed headers, invalid ObjectIds)

---

## 6. Frontend Test Design

### 6.1 Test Objectives

- Validate UI rendering and state transitions
- Validate authentication flows
- Confirm protected route behaviour
- Detect UI regressions independent of backend
- Validate failure-mode UX

### 6.2 Identified Test Conditions

#### Authentication UI
- Valid login → navigate to protected route
- Invalid login → error message displayed
- Logout clears authentication state
- Unauthorized redirect to login

#### Component Behaviour
- Navbar reflects auth state
- Shopping list renders items correctly
- Toggle state updates correctly
- Delete removes item from UI
- Form validation prevents invalid submission

#### Custom Hook Behaviour
- Fetch items on mount
- Handle loading state
- Handle API error gracefully

### 6.3 Design Techniques Applied

- Equivalence Partitioning (valid/invalid forms)
- State Transition Testing (auth state changes)
- Error Guessing (network failure, 401 responses)
- Use Case Testing (login → manage list → logout)

---

## 7. End-to-End (E2E) Test Design

### 7.1 Objectives

- Validate full-stack integration
- Confirm real user journeys work in browser
- Protect high-value demo and release flows
- Verify system-level data isolation

### 7.2 Covered Test Conditions

#### Routing & Authentication
- Unauthenticated user redirected to login
- Login grants access to protected route
- Logout clears session

#### Core Shopping Flow
- Add item
- Delete item

#### Security Behaviour
- User A cannot see User B’s items

### 7.3 Design Techniques Applied

- Use Case Testing (UC)
- State Transition Testing (ST)
- Decision Table Testing (auth vs unauthenticated)
- Security-focused functional validation

E2E scope is intentionally small to control flakiness and maintenance cost.

---

## 8. Regression Design Model

Regression coverage is distributed:

- Backend: Core logic and security rules
- Frontend: UI behaviour and state handling
- E2E: Critical integrated flows

Smoke validation focuses on:

- Authentication
- CRUD
- Persistence
- Protected routing

Automation is executed in CI to enforce regression gates.

---

## 9. Traceability

Traceability is maintained through:

- Test condition IDs (where applicable)
- Mapping to automated test files
- Explicit defect linkage in GitHub Issues
- Risk mapping in strategy documentation

High-risk areas are validated at multiple layers.

---

## 10. Boundaries & Deliberate Exclusions

Intentionally not covered:

- Performance/load testing
- Accessibility audits
- Visual regression
- Full browser matrix testing
- Deep penetration testing

The test design is proportional to system complexity and aligned with portfolio scope.

---

## 11. Summary

This test design demonstrates:

- Structured, risk-based QA thinking
- Clear separation of test layers
- Security-aware validation
- Deterministic automation practices
- CI-integrated regression protection

The design reflects a QA-first engineering mindset while remaining scalable and maintainable.