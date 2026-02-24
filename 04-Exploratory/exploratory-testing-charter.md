# Exploratory Testing Charter  
Shopping List App – Authentication & Session Handling

## Charter ID
CH-2026-01-AUTH

## Mission
Evaluate authentication and session behaviour, focusing on failure modes, token lifecycle handling, and user-facing error feedback.

## Scope

**In Scope**
- Login and logout flows
- JWT storage and invalidation
- Protected route access
- 401 handling in the UI
- Backend unavailability during login

**Out of Scope**
- Performance or load testing
- Security penetration testing

## Risks Investigated
- Invalid or missing JWT handling
- Raw technical errors exposed to users
- Inconsistent redirect behaviour
- Session persistence issues

## Approach
- 60-minute timeboxed exploratory session
- Manual interaction + browser DevTools
- Token removal/tampering
- Backend availability toggling

## Exit Criteria
- Core flows exercised
- Failure scenarios explored
- Findings documented and linked to defects

## Related Artifacts
- EXP-2026-01-29 – Authentication and Session Behaviour  
- TR-2026-01-29 – Initial Regression Baseline  
- BR-001, BR-002
