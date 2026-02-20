# Smoke & Regression Testing Approach

This document describes my general approach to designing and executing smoke and regression testing across software projects.

It is intended as a reference for how I apply QA principles in practice, rather than a checklist for any specific system.

---

## 1. Purpose

Smoke and regression testing are used to provide fast, meaningful confidence in application stability after changes.

- **Smoke testing** validates that the system is fundamentally usable.
- **Regression testing** verifies that existing functionality has not been unintentionally broken by new changes.

---

## 2. Design Principles

When designing smoke and regression coverage, I follow these principles:

- Focus on **critical user journeys**
- Prioritise **high-risk and high-impact areas**
- Keep execution time short enough to run after every change
- Prefer clarity and repeatability over exhaustive coverage

The goal is to detect severe or user-visible issues early, rather than to replace deeper test types such as performance or security testing.

---

## 3. Smoke Testing Strategy

Smoke tests are designed to answer one question:

> “Is the system healthy enough to continue testing or release?”

Typical smoke coverage includes:
- Application start-up and basic navigation
- Authentication or access control (where applicable)
- Core create/read/update/delete flows
- Basic data persistence
- Clear handling of invalid input

Smoke tests are executed:
- After major feature changes
- After environment setup
- Before demos or releases

---

## 4. Regression Testing Strategy

Regression tests focus on areas more likely to be affected by change, such as:
- State management
- Data integrity and persistence
- Authorization and data isolation
- Error handling and user feedback
- UI consistency after updates

Regression coverage is adapted per project based on:
- Architecture
- Change history
- Observed defect patterns

---

## 5. Practical Application

In practice, this approach is applied through:
- Project-specific regression checklists
- Documented test runs after feature changes
- Defect reports with clear reproduction steps
- Verification of fixes before closure

Example implementation (found in 'shopping-list-app' project):
- Shopping List App regression checklist
- Associated test runs and defect reports

---

## 6. Continuous Improvement

Checklists and regression scope are treated as living artefacts and are refined as the system evolves and new risks emerge.