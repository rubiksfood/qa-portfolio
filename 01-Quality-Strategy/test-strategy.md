# Test Strategy - Shopping List App

### Purpose

This document defines the project-level test strategy for the Shopping List App.  
It translates the quality philosophy into practical test scope and prioritisation.

---

## System Overview

Full-stack MERN application with:

- JWT-based authentication  
- Protected routes  
- Per-user data isolation  
- CRUD functionality  

Primary risks relate to authentication, authorization, and data integrity.

---

## Scope

### In Scope

- Registration and login workflows  
- JWT validation and protected route enforcement  
- CRUD operations and persistence  
- Cross-user data isolation  
- Error handling and session behaviour  

### Out of Scope (Current Iteration)

- Performance testing  
- Accessibility audits  
- Full browser matrix  
- Deep penetration testing  

Scope remains proportional to system size.

---

## Test Levels

### Backend (API Integration)

Validates:

- Route correctness  
- JWT middleware behaviour  
- CRUD integrity  
- Access control and isolation  

Primary tools: Jest + Supertest  

---

### Frontend (Component / Integration)

Validates:

- Authentication state transitions  
- Protected route behaviour  
- UI state updates  
- Error and loading states  

Primary tools: Vitest + React Testing Library + MSW  

---

### End-to-End (System)

Validates:

- Login/logout lifecycle  
- Protected route enforcement  
- Item creation and deletion  
- Cross-user isolation  

Primary tool: Playwright  

E2E scope is intentionally limited to high-value journeys.

---

## Regression Model

- Backend and frontend provide fast feedback.  
- E2E confirms integration integrity.  
- High-risk areas receive multi-layer validation.  
- All automated suites execute in CI as quality gates.