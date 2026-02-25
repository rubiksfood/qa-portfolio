# Shopping List App – Backend API Test Design

Author: Joshua Pearson  
Test Level: Backend System / API Integration  
Automation Alignment: Jest + Supertest  
Status: Portfolio Artifact  

---

## 1. Purpose

This document defines the backend API test design for the Shopping List App.  
It combines test strategy, scope definition, and structured test cases into a single consolidated artifact.

The objective is to ensure reliable regression coverage of:

- Authentication behaviour
- Authorization and data isolation
- CRUD operations
- Error handling and negative paths

---

## 2. System Under Test

Backend: Node.js (ESM) + Express  
Database: MongoDB (dedicated test database: `shopping-list-test`)  
Authentication: JWT-based  

### In-Scope Components

- `/auth/register`
- `/auth/login`
- `/auth/me`
- `/shopItem` (GET, POST)
- `/shopItem/:id` (GET, PATCH, DELETE)
- JWT authentication middleware
- Per-user data isolation

Out of Scope:

- Frontend/UI behaviour
- Browser-based E2E flows
- Performance testing
- Penetration/security testing

---

## 3. Test Objectives

- Verify correct API behaviour for valid requests
- Validate error handling for invalid inputs
- Confirm authorization enforcement
- Prevent cross-user data leakage
- Maintain deterministic automated regression

---

## 4. Test Approach

### 4.1 Test Levels

- Middleware-level validation (JWT handling)
- Route-level integration testing (Express + MongoDB)
- Backend treated as standalone system

### 4.2 Test Types

- Functional testing
- Security-related functional testing
- Negative testing
- Error handling validation

### 4.3 Design Techniques (ISTQB-aligned)

- Equivalence Partitioning (EP)
- Decision Table Testing (DT)
- State Transition Testing (ST)
- Error Guessing (EG)
- Boundary Value Analysis (BVA – planned where validation rules exist)

---

## 5. Test Environment

- Jest test runner
- Supertest for HTTP simulation
- Dedicated MongoDB test database
- Environment variables via `config.test.env`
- Database cleared between tests (`beforeEach`)
- Tokens generated via `/auth/login` or `jwt.sign()` where appropriate

Execution:

```
cd server
npm test
```

---

## 6. Entry and Exit Criteria

### Entry Criteria

- Backend routes implemented
- Test database accessible
- Test runner configured

### Exit Criteria

- All implemented test cases pass
- No open High severity backend defects
- Tests execute reliably in local and CI environments

---

# 7. Test Conditions and Cases

---

## 7.1 Authentication – `/auth`

### Registration

| TC ID            | Objective                              | Expected Result   | Technique | Automation Ref        |
|------------------|----------------------------------------|-------------------|-----------|-----------------------|
| API-AUTH-REG-01  | Valid registration                     | `201 Created`     | EP        | `auth.routes.test.js` |
| API-AUTH-REG-02  | Reject missing fields                  | `400 Bad Request` | EP, EG    | `auth.routes.test.js` |
| API-AUTH-REG-03  | Reject duplicate email                 | `409 Conflict`    | DT, EG    | `auth.routes.test.js` |
| API-AUTH-REG-04* | Password boundary validation (planned) | 4xx below min     | BVA       | Planned               |

### Login

| TC ID           | Objective      | Expected Result    | Technique | Automation Ref        |
|-----------------|----------------|--------------------|-----------|-----------------------|
| API-AUTH-LOG-01 | Valid login    | `200 OK` + token   | EP, DT    | `auth.routes.test.js` |
| API-AUTH-LOG-02 | Wrong password | `401 Unauthorized` | DT, EG    | `auth.routes.test.js` |
| API-AUTH-LOG-03 | Unknown email  | `401 Unauthorized` | EP, EG    | `auth.routes.test.js` |
| API-AUTH-LOG-04 | Missing fields | `401 Unauthorized` | EP, EG    | `auth.routes.test.js` |

### `/auth/me`

| TC ID          | Objective             | Expected Result    | Technique | Automation Ref        |
|----------------|-----------------------|--------------------|-----------|-----------------------|
| API-AUTH-ME-01 | Valid token           | `200 OK` + user    | EP, ST    | `auth.routes.test.js` |
| API-AUTH-ME-02 | Missing/invalid token | `401 Unauthorized` | DT, EG    | `auth.routes.test.js` |

---

## 7.2 Authorization – JWT Middleware

| TC ID           | Objective                          | Expected Result    | Technique | Automation Ref            |
|-----------------|------------------------------------|--------------------|-----------|---------------------------|
| API-AUTHZ-MW-01 | Missing header                     | `401 Unauthorized` | EP, EG    | `auth.middleware.test.js` |
| API-AUTHZ-MW-02 | Wrong scheme                       | `401 Unauthorized` | DT        | `auth.middleware.test.js` |
| API-AUTHZ-MW-03 | Invalid token                      | `401 Unauthorized` | EG        | `auth.middleware.test.js` |
| API-AUTHZ-MW-04 | Valid token populates user context | `200 OK`           | ST        | `auth.middleware.test.js` |

---

## 7.3 CRUD – `/shopItem`

### List & Create

| TC ID            | Objective                        | Expected Result    | Technique | Automation Ref            |
|------------------|----------------------------------|--------------------|-----------|---------------------------|
| API-ITEM-LIST-01 | Empty list                       | `200 OK`, `[]`     | EP        | `shopItem.routes.test.js` |
| API-ITEM-LIST-02 | Only own items returned          | No cross-user data | DT, SEC   | `shopItem.routes.test.js` |
| API-ITEM-CR-01   | Valid create                     | `201 Created`      | EP        | `shopItem.routes.test.js` |
| API-ITEM-CR-02*  | Missing required field (planned) | `4xx`              | EP, EG    | Planned                   |

### Get by ID

| TC ID           | Objective                 | Expected Result | Technique | Automation Ref            |
|-----------------|---------------------------|-----------------|-----------|---------------------------|
| API-ITEM-GET-01 | Get own item              | `200 OK`        | EP        | `shopItem.routes.test.js` |
| API-ITEM-GET-02 | Non-existent item         | `404 Not Found` | EG        | `shopItem.routes.test.js` |
| API-ITEM-GET-03 | Cross-user access blocked | `404 Not Found` | SEC       | `shopItem.routes.test.js` |

### Update

| TC ID           | Objective                 | Expected Result | Technique | Automation Ref            |
|-----------------|---------------------------|-----------------|-----------|---------------------------|
| API-ITEM-UPD-01 | Update own item           | `200 OK`        | ST        | `shopItem.routes.test.js` |
| API-ITEM-UPD-02 | Non-existent item         | `404 Not Found` | EG        | `shopItem.routes.test.js` |
| API-ITEM-UPD-03 | Cross-user update blocked | `404 Not Found` | SEC       | `shopItem.routes.test.js` |

### Delete

| TC ID           | Objective                 | Expected Result | Technique | Automation Ref            |
|-----------------|---------------------------|-----------------|-----------|---------------------------|
| API-ITEM-DEL-01 | Delete own item           | `200 OK`        | ST        | `shopItem.routes.test.js` |
| API-ITEM-DEL-02 | Non-existent item         | `404 Not Found` | EG        | `shopItem.routes.test.js` |
| API-ITEM-DEL-03 | Cross-user delete blocked | `404 Not Found` | SEC       | `shopItem.routes.test.js` |

---

## 8. Risks and Mitigations

| Risk                      | Impact                    | Mitigation                     |
|---------------------------|---------------------------|--------------------------------|
| DB lifecycle issues       | Flaky CI                  | Deterministic cleanup per test |
| Token expiry issues       | Intermittent failures     | Controlled JWT configuration   |
| Cross-test data pollution | False positives/negatives | Standardised test setup        |

---

## 9. Traceability

- TC IDs (API-*) map to automated Jest + Supertest suites:  
  e.g. API-AUTH-LOG-02 → maps to test "should reject login with wrong password"
- Security-related conditions explicitly validate data isolation.
- Planned cases reflect future validation rule enhancements.

This document serves as a consolidated backend API test design artifact for portfolio review.