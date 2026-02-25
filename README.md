# QA Portfolio

This repository contains selected QA artifacts demonstrating a structured, risk-based approach to software testing and quality assurance.

The portfolio reflects an evolving system, which is based on practical testing work carried out on a full-stack MERN application. It is designed to show how testing is planned, prioritised, executed, and stabilised across multiple layers.

The focus is on clarity, traceability, and realistic QA workflows within a collaborative product environment.

---

## What This Portfolio Demonstrates

- Risk-based test strategy and prioritisation  
- Structured test design using ISTQB-aligned techniques  
- Clear, reproducible, and impact-focused defect reporting  
- Backend API integration testing  
- Frontend integration testing with mocked services  
- Controlled end-to-end validation of critical user journeys  
- Deterministic test environments and CI-based regression execution  
- Structured exploratory testing with documented findings  
- Requirements-to-test traceability  

The documentation demonstrates not only *what* was tested, but *why* certain areas were prioritised based on risk and impact.

---

## Repository Structure

### 00 – Foundation
Defines the overarching quality engineering philosophy and guiding principles for the repository.

### 01 – Quality Strategy  
High-level QA approach, defect classification model, risk assessment, and project-level test strategy.

### 02 – Test Design  
Layered test design across backend, frontend, and end-to-end levels, including defined test conditions and techniques applied.

### 03 – Defect Management  
Structured bug reports including reproduction steps, expected vs. actual results, severity justification, and root cause analysis where applicable.

### 04 – Exploratory Testing  
Timeboxed exploratory session notes focused on authentication, session handling, and failure-mode behaviour.

### 05 – API Testing  
Backend API test design aligned with automated Jest + Supertest integration tests.

### 06 – Automation  
Automation strategy, architecture overview, and test pyramid implementation across backend, frontend, and E2E layers.

### 07 – Traceability  
Requirements Traceability Matrix mapping core functional requirements to corresponding test levels and automation references.

### 08 – Engineering Notes  
Implementation and test stabilisation insights, including CI reliability improvements, database lifecycle management, and environment isolation practices.

---

## Approach

Testing is guided by the following principles:

- Focus on high-risk areas first (authentication, data isolation, CRUD integrity)  
- Separate responsibilities across test layers to avoid redundancy  
- Maintain deterministic and isolated test environments  
- Treat CI stability as a quality requirement  
- Document findings clearly and proportionally to system scope  

The objective is to demonstrate practical QA thinking applied in a realistic development context.

---

## Project Context

The artifacts in this repository are based on testing work carried out on a full-stack MERN application with:

- JWT-based authentication  
- Protected routes  
- Per-user data isolation  
- CRUD functionality  
- Automated regression pipelines  

The emphasis is on product stability, security-related functional validation, and controlled failure behaviour rather than exhaustive coverage.

---

## Notes

- This repository focuses on QA deliverables rather than application source code.  
- Where relevant, artifacts reference real automated tests and issue tracking for traceability.  
- Documentation is intentionally proportional to project size and reflects realistic QA responsibilities within a structured team environment.  

This portfolio aims to show disciplined, risk-aware testing practices aligned with modern product development workflows.