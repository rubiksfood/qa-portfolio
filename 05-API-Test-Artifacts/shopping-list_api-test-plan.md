# Shopping List App – Backend API Test Plan

Author: Joshua Pearson  
Test level: Backend system testing (API-level) with integration coverage (Express + MongoDB) 
Status: Portfolio artifact (aligned with implemented Jest + Supertest coverage)

---

## 1. Purpose

This test plan defines the approach, scope, environment, and exit criteria for backend API testing of the **Shopping List App**. It is designed to be aligned with a risk-based, testing approach and supports reliable automated regression.

---

## 2. System Under Test

**Backend:** Node.js (ESM) + Express API  
**Persistence:** MongoDB (dedicated test database)  
**Auth:** JWT-based authentication + authorization middleware

### In-scope routes/components
- `/auth/register`
- `/auth/login`
- `/auth/me`
- `/shopItem` (GET, POST)
- `/shopItem/:id` (GET, PATCH, DELETE)
- JWT authentication middleware
- Per-user data isolation and access control

---

## 3. Test Objectives

- Verify correct API behaviour for valid requests (happy paths).
- Verify robust handling of invalid requests (negative testing).
- Validate authorization and user data isolation (security-related functional testing).
- Ensure deterministic test execution (stable local + CI outcomes).
- Maintain a repeatable automated regression suite for backend changes.

---

## 4. Test Scope

### 4.1 In Scope
- Functional behaviour of auth and shop item CRUD routes
- JWT validation and authorization failures (401/404 behaviour)
- Correct HTTP status code validation (200, 201, 400, 401, 404)
- Error handling:
  - Missing fields
  - Missing/invalid auth headers
  - Invalid ObjectIds
  - Non-existent resources
- MongoDB persistence correctness
- Per-user data isolation (no cross-user access/update/delete)

### 4.2 Out of Scope
- Frontend/UI behaviour
- End-to-end browser flows
- Performance/load testing
- Deep security / penetration testing

---

## 5. Test Approach

### 5.1 Test Levels
- **Unit**: JWT middleware behaviour under header/token variations
- **Integration**: Route-level tests (Express + middleware + MongoDB) using Supertest
- **Backend-only system**: Backend treated as standalone system (no UI dependency)

### 5.2 Test Types
- Functional testing
- Security-related functional testing (authz, isolation)
- Negative testing (invalid/missing inputs)
- Error handling testing (invalid ObjectId, missing resources)

### 5.3 Test Design Techniques (ISTQB-aligned)
- Equivalence Partitioning (valid vs invalid requests)
- Decision Table Testing (credential combinations)
- State Transition Testing (token states, `isChecked` toggling)
- Error Guessing (malformed tokens/headers, cross-user access)
- Boundary Value Analysis (planned once input validation rules exist)

---

## 6. Test Environment

### 6.1 Components
- Node.js (ESM mode)
- Express app exported from `app.js` (no `.listen()` in tests)
- MongoDB test database: `shopping-list-test` (MongoDB Atlas)
- Jest test runner
- Supertest for HTTP request simulation

### 6.2 Configuration
- Environment variables loaded from `config.test.env`
  - `ATLAS_URI`
  - `JWT_SECRET`
  - `NODE_ENV=test`
  - `DB_NAME`

### 6.3 Test Data Strategy
- Database collections cleared between tests (`beforeEach`)
- Users/items created per test case (no persistent fixtures)
- Tokens generated via:
  - `/auth/login` (route tests), and/or
  - `jwt.sign()` (middleware unit tests where appropriate)

### 6.4 Execution Command
```
cd server
npm test
```

---

## 7. Entry and Exit Criteria

### 7.1 Entry Criteria
- Backend routes implemented and running in test configuration
- MongoDB test database accessible
- Jest + Supertest smoke run passes (test runner and env setup validated)

### 7.2 Exit Criteria
- All planned backend API test cases implemented and passing
- No open High severity backend defects
- Tests run reliably in local and CI environments
- All automated tests passing in CI
- Test documentation updated to reflect current behaviour

---

## 8. Risks and Mitigations

| Risk                                          | Impact                                | Mitigation                                                           |
|-----------------------------------------------|---------------------------------------|----------------------------------------------------------------------|
| DB lifecycle/config coupling causes flakiness | Non-deterministic tests / CI failures | Explicit DB lifecycle, consistent env loading, deterministic cleanup |
| MongoDB latency/timeouts                      | Slow runs, intermittent failures      | Keep DB ops minimal, adjust timeouts where justified                 |
| Incorrect cleanup between tests               | Cross-test pollution                  | Standardised `beforeEach` cleanup                                    |
| JWT/time-based behaviour                      | Expiry-related failures               | Avoid short expiries in tests; stub/extend where needed              |

---

## 9. Test Deliverables

- Backend API test cases (this portfolio)
- Automated Jest + Supertest test suites (in project repo)
- Defect reports linked to failed conditions (where applicable)
- Traceability references mapping conditions → cases → automation files

---

## 10. Maintenance

This test plan and associated test cases should be updated when:
- New routes/features are added
- Existing routes change behaviour or contracts
- Validation rules are introduced (enabling BVA-focused coverage)
- Auth/security rules change (authorization, isolation)