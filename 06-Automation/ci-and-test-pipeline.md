# 04 – CI Pipeline  
## Continuous Integration & Test Execution

### Purpose

The CI pipeline ensures consistent, automated validation on every change.  
Its objective is early regression detection and stable, repeatable execution.

---

## Pipeline Structure

Execution order:

1. Backend tests  
2. Frontend tests  
3. End-to-End tests  

Fast feedback layers execute before full-system validation.

---

## Backend Stage

Validates:

- Authentication logic  
- JWT middleware  
- CRUD behaviour  
- Access control and isolation  

Runs against a dedicated test database with isolated configuration.

---

## Frontend Stage

Validates:

- Component behaviour  
- Routing and protected pages  
- Authentication state transitions  
- UI error handling  

Runs in JSDOM with mocked API responses.

---

## End-to-End Stage

Validates:

- Login/logout lifecycle  
- Protected routes  
- Item creation and deletion  
- Cross-user isolation  

Environment controls include:

- Dedicated E2E database  
- Controlled startup sequence  
- Database reset before execution  
- Browser isolation per run  

Diagnostics include screenshots and execution traces.

---

## Quality Gates

A change must not be merged if it breaks:

- Authentication  
- Protected routes  
- CRUD functionality  
- Data isolation guarantees  

CI functions as a mandatory regression gate supporting release confidence.