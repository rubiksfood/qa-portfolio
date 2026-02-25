## QA Engineering Approach

### Purpose

This document defines the high-level quality philosophy applied across the portfolio.  
It describes how testing decisions are prioritised, structured, and evaluated.

Quality is treated as a system property that includes:

- Functional correctness  
- Security-related behaviour  
- Infrastructure stability  
- CI reliability  
- Failure-mode user experience  

---

## Guiding Principles

### Risk-Based Prioritisation
Testing effort focuses first on high-impact areas:

- Authentication and authorization  
- Data isolation  
- Persistence and CRUD integrity  
- Error handling and session behaviour  

Coverage is driven by impact and likelihood rather than exhaustiveness.

---

### Layered Validation

Testing responsibilities are distributed across layers:

- Backend → business logic, security, data integrity  
- Frontend → UI behaviour and state transitions  
- End-to-End → critical integrated user journeys  
- Exploratory → failure modes and UX behaviour  

Layer separation reduces redundancy and improves maintainability.

---

### Deterministic Environments

Stable test execution requires:

- Explicit database lifecycle control  
- Environment isolation per test context  
- No import-time side effects  
- Controlled startup sequencing  

Infrastructure instability is treated as a defect, not an inconvenience.

---

### Structured Defect Classification

Defects are classified using two independent dimensions:

- **Severity** – impact on system or user  
- **Priority** – urgency of resolution  

Infrastructure defects may be high severity even if not user-facing.

---

## Objective

The goal is not only to detect defects, but to maintain predictable, secure, and maintainable systems through disciplined testing practices.