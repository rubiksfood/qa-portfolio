# Shopping List App – Backend API Test Cases

Author: Joshua Pearson  
Test level: Integration / Backend-only system  
Automation alignment: Implemented using Jest + Supertest against the dedicated `shopping-list-test` database.

---

## 1. Purpose

This document lists backend API test cases for the **Shopping List App** backend, grouped by suite. It is designed for portfolio traceability and aligns with the implemented automated regression coverage.

**Notation**
- Techniques: EP (Equivalence Partitioning), DT (Decision Table), ST (State Transition), EG (Error Guessing), BVA (Boundary Value Analysis), SEC (Security-related functional)
- *Planned* cases indicate intended coverage once backend validation rules exist.

---

## 2. Authentication Suite – `/auth`

### Test Conditions
- TCON-AUTH-REG-01: Valid registration
- TCON-AUTH-REG-02: Missing fields in registration
- TCON-AUTH-REG-03: Duplicate registration
- TCON-AUTH-LOG-01: Valid login
- TCON-AUTH-LOG-02: Login with wrong password
- TCON-AUTH-LOG-03: Login with unknown email
- TCON-AUTH-LOG-04: Login fails with missing fields
- TCON-AUTH-ME-01: `/auth/me` with valid token
- TCON-AUTH-ME-02: `/auth/me` without/invalid token

### Test Cases (Registration)

| TC ID           | Objective                       | Preconditions        | Request                                       | Expected Result | Technique | Automation Ref |
|-----------------|---------------------------------|----------------------|-----------------------------------------------|--------------------------------|----|---|
| API-AUTH-REG-01 | Register with valid credentials | Email not registered | POST `/auth/register` body `{email,password}` | `201 Created`, success message | EP | `tests/auth.routes.test.js` |
| API-AUTH-REG-02 | Reject missing fields | None | POST `/auth/register` body `{}` / missing email/password | `400 Bad Request`, validation message | EP, EG | `tests/auth.routes.test.js` |
| API-AUTH-REG-03 | Reject duplicate email | User exists | Register same email twice | Second attempt `409 Conflict`, “User already exists” | DT, EG | `tests/auth.routes.test.js` |
| API-AUTH-REG-04* | Password boundary validation | Password rules implemented | Vary password length below/at minimum | Below-min `4xx`, at-min success | BVA | Planned |

### Test Cases (Login)

| TC ID           | Objective                             | Preconditions | Request                                    | Expected Result          | Technique | Automation Ref |
|-----------------|---------------------------------------|---------------|--------------------------------------------|--------------------------|-----------|---|
| API-AUTH-LOG-01 | Login succeeds with valid credentials | User exists | POST `/auth/login` body `{email,password}` | `200 OK`, token returned | EP, DT | `tests/auth.routes.test.js` |
| API-AUTH-LOG-02 | Reject wrong password | User exists | POST `/auth/login` valid email + wrong password | `401 Unauthorized`, “Invalid credentials” | DT, EG | `tests/auth.routes.test.js` |
| API-AUTH-LOG-03 | Reject unknown email | None | POST `/auth/login` unknown email | `401 Unauthorized`, “Invalid credentials” | EP, EG | `tests/auth.routes.test.js` |
| API-AUTH-LOG-04 | Reject missing fields | None | POST `/auth/login` missing email/password | `401 Unauthorized`, "Invalid credentials" | EP, EG | `tests/auth.routes.test.js` |

### Test Cases (`/auth/me`)

| TC ID          | Objective                            | Preconditions | Request                                       | Expected Result | Technique | Automation Ref |
|----------------|--------------------------------------|---------------|-----------------------------------------------|-----------------|-----------|---|
| API-AUTH-ME-01 | Return current user with valid token | Logged in | GET `/auth/me` with `Authorization: Bearer <jwt>` | `200 OK`, user info returned | EP, ST | `tests/auth.routes.test.js` |
| API-AUTH-ME-02 | Reject missing/invalid token | None | GET `/auth/me` without token / invalid token | `401 Unauthorized` | DT, EG | `tests/auth.routes.test.js` |

---

## 3. Authorization Suite – JWT Middleware

### Test Conditions
- TCON-AUTHZ-MW-01: No Authorization header
- TCON-AUTHZ-MW-02: Wrong header scheme
- TCON-AUTHZ-MW-03: Invalid token
- TCON-AUTHZ-MW-04: Valid token populates `req.userId`

### Test Cases

| TC ID           | Objective                           | Preconditions | Request/Setup | Expected Result | Technique | Automation Ref |
|-----------------|-------------------------------------|---------------|---------------|-----------------|-----------|---|
| API-AUTHZ-MW-01 | Reject missing Authorization header | None | Call protected route without header | `401 Unauthorized`, “Authorization header missing” | EP, EG | `tests/auth.middleware.test.js` |
| API-AUTHZ-MW-02 | Reject wrong scheme | None | `Authorization: Token abc` | `401 Unauthorized`, “Invalid Authorization header” | DT | `tests/auth.middleware.test.js` |
| API-AUTHZ-MW-03 | Reject invalid token | None | `Authorization: Bearer invalid.token` | `401 Unauthorized`, “Invalid or expired token” | EG | `tests/auth.middleware.test.js` |
| API-AUTHZ-MW-04 | Accept valid token and set user context | None | Valid JWT payload includes userId | `200 OK` and `req.userId` populated/observable | ST, EP | `tests/auth.middleware.test.js` |

---

## 4. CRUD Suite – `/shopItem`

### Test Conditions
- List: TCON-ITEM-LIST-01..02
- Create: TCON-ITEM-CREATE-01..02
- Get: TCON-ITEM-GET-01..03
- Update: TCON-ITEM-UPDATE-01..03
- Delete: TCON-ITEM-DELETE-01..03

### Test Cases (List & Create)

| TC ID            | Objective                         | Preconditions       | Request         | Expected Result | Technique | Automation Ref |
|------------------|-----------------------------------|---------------------|-----------------|-----------------|-----------|---|
| API-ITEM-LIST-01 | Empty list when user has no items | Auth user, no items | GET `/shopItem` | `200 OK`, `[]` | EP | `tests/shopItem.routes.test.js` |
| API-ITEM-LIST-02 | List only returns current user’s items | Two users with items | GET `/shopItem` as User A | Response contains only User A items | DT, SEC | `tests/shopItem.routes.test.js` |
| API-ITEM-CR-01 | Create item with valid data | Auth user | POST `/shopItem` body includes required fields | `201 Created`, insertedId/acknowledged | EP | `tests/shopItem.routes.test.js` |
| API-ITEM-CR-02* | Reject missing required fields | Auth user | POST `/shopItem` missing `name` (or required field) | `4xx` validation error | EP, EG | Planned |

### Test Cases (Get by ID)

| TC ID           | Objective                | Preconditions      | Request             | Expected Result              | Technique | Automation Ref |
|-----------------|--------------------------|--------------------|---------------------|------------------------------|-----------|---|
| API-ITEM-GET-01 | Get own item by valid ID | Item owned by user | GET `/shopItem/:id` | `200 OK`, item returned | EP | `tests/shopItem.routes.test.js` |
| API-ITEM-GET-02 | Non-existent item returns 404 | None | GET `/shopItem/<valid ObjectId not in DB>` | `404 Not Found` | EG | `tests/shopItem.routes.test.js` |
| API-ITEM-GET-03 | Cross-user access returns 404 | Item owned by other user | GET `/shopItem/:id_other` | `404 Not Found`, no data leakage | SEC, EG | `tests/shopItem.routes.test.js` |

### Test Cases (Update)

| TC ID           | Objective                       | Preconditions      | Request  | Expected Result | Technique | Automation Ref |
|-----------------|---------------------------------|--------------------|----------|-----------------|-----------|----------------|
| API-ITEM-UPD-01 | Update `isChecked` for own item | Item owned by user | PATCH `/shopItem/:id` body `{isChecked:true}` | `200 OK`, updated item returned | ST | `tests/shopItem.routes.test.js` |
| API-ITEM-UPD-02 | Update non-existent item returns 404 | None | PATCH `/shopItem/<random id>` | `404 Not Found` | EG | `tests/shopItem.routes.test.js` |
| API-ITEM-UPD-03 | Cannot update other user’s item | Item owned by other user | PATCH `/shopItem/:id_other` | `404 Not Found`, unchanged for owner | SEC, EG | `tests/shopItem.routes.test.js` |

### Test Cases (Delete)

| TC ID           | Objective       | Preconditions | Request | Expected Result | Technique | Automation Ref |
|-----------------|-----------------|---------------|---------|-----------------|-----------|----------------|
| API-ITEM-DEL-01 | Delete own item | Item owned by user | DELETE `/shopItem/:id` | `200 OK`, `{deletedCount: 1}` (or equivalent) | ST | `tests/shopItem.routes.test.js` |
| API-ITEM-DEL-02 | Delete non-existent item returns 404 | None | DELETE `/shopItem/<random id>` | `404 Not Found` | EG | `tests/shopItem.routes.test.js` |
| API-ITEM-DEL-03 | Cannot delete other user’s item | Item owned by other user | DELETE `/shopItem/:id_other` | `404 Not Found`, item still present | SEC, EG | `tests/shopItem.routes.test.js` |

---

## 5. Notes on Traceability

- Test condition IDs (TCON-*) represent what must be tested.
- Test case IDs (API-*) represent how it is verified.
- “Automation Ref” indicates the current Jest + Supertest test suite mapping.

These identifiers are local to the portfolio documents and are intended for clear cross-referencing to:
- automated tests
- issues/bug reports
- traceability matrices