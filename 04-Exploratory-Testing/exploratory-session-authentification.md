# Exploratory Session – Authentication & Session Handling

**Project:** Shopping List App  
**Charter ID:** CH-2026-01-AUTH  
**Session ID:** EXP-2026-01-29-AUTH  
**Tester:** Joshua Pearson  
**Test Type:** Risk-Based Exploratory Testing  
**Timebox:** 60 minutes  
**Environment:** Local development (React frontend, Express backend, MongoDB)

---

## 1. Mission

Evaluate authentication and session behaviour with emphasis on:

- Failure modes  
- Token lifecycle handling  
- Protected route enforcement  
- User-facing error feedback  
- Data isolation integrity  

Authentication and session handling were treated as high-risk areas due to their impact on security, access control, and system trust.

---

## 2. Scope

### In Scope

- Login and logout flows  
- JWT storage, removal, and tampering  
- Access to protected routes  
- 401 handling in the UI  
- Backend unavailability during login  
- Cross-user access attempts via API manipulation  

### Out of Scope

- Performance or load testing  
- Accessibility testing  
- Cross-browser validation  
- Penetration/security testing  

---

## 3. Risks Investigated

- Invalid or missing JWT handling  
- Raw technical errors exposed to users  
- Inconsistent redirect behaviour  
- Session persistence inconsistencies  
- Cross-user data leakage  

---

## 4. Approach

- 60-minute timeboxed exploratory session  
- Manual UI interaction  
- Browser Developer Tools (network inspection, local storage manipulation)  
- Direct API manipulation via Postman  
- Manual backend shutdown and restart to simulate service disruption  

Derived from project risk areas defined in the test strategy.

---

## 5. Test Ideas Explored

The following scenarios were exercised:

- Attempt login while backend is stopped  
- Remove JWT from local storage during active session  
- Modify JWT manually to simulate tampering  
- Refresh application after token removal  
- Access protected routes after token invalidation  
- Rapid login/logout sequence testing  
- Attempt cross-user data access via API manipulation  

---

## 6. Observations

### 6.1 Authentication Logic

- Valid credentials returned JWT tokens as expected.  
- Invalid credentials returned appropriate 401 responses.  
- No authentication bypass scenarios were identified.  

Authentication logic behaved correctly under normal conditions.

---

### 6.2 Session Handling

#### Scenario: Backend Restart During Active Session

- Backend was stopped and restarted during an active session.  
- Existing JWT remained valid.  

Assessment:  
Expected behaviour for stateless JWT authentication.

---

#### Scenario: Manual JWT Removal

- JWT removed from browser local storage.  
- Application refreshed.

Observed behaviour:
- User logged out successfully.  
- No explanatory message displayed.

Expected behaviour:
- User logged out.  
- Clear session-expired or re-authentication message displayed.

Result:  
Baseline UX gap identified.

---

### 6.3 Backend Unavailability

#### Scenario: Backend Shut Down During Login Attempt

Observed behaviour:
- Browser-native network error displayed in UI.  
- CORS preflight failure visible in network console.

Expected behaviour:
- Controlled, user-friendly error message.

Result:  
Baseline gap identified in error handling.

---

### 6.4 Authorization and Data Isolation

- Attempted cross-user item access via API manipulation.  
- 404 responses returned for non-owned resources.  
- No data leakage observed.

Security-related functional behaviour confirmed as correct.

---

## 7. Regression Baseline Summary

| Area                          | Result | Notes                   |
|-------------------------------|--------|-------------------------|
| Cross-user access control     | Pass   | No data leakage         |
| Invalid or missing JWT        | Pass   | 401 responses returned  |
| Session invalidation feedback | Gap    | Silent logout           |
| Backend outage handling       | Gap    | Raw network error shown |
| Core smoke checks             | Pass   | No crash scenarios      |

Status:  
Pass with known baseline gaps.

The identified gaps serve as reference points for future regression comparison and verification after remediation.

---

## 8. Defects Raised

The following defects were logged:

- BR-001 – Raw network error exposed to user during backend outage  
- BR-002 – Session termination provides no user-facing explanation  

Both classified as medium severity (UX-impacting, non-security-critical).

---

## 9. Exit Criteria

- Core authentication flows exercised  
- Failure scenarios explored  
- Data isolation validated  
- Findings documented  
- Related defects logged and traceable  

---

## 10. Conclusion

Core authentication, authorization, and data isolation mechanisms are functionally stable.

Security-related behaviours operate correctly under tested conditions.

Failure-mode behaviour revealed UX weaknesses in:

- Session expiry communication  
- Backend outage handling  

This session establishes a documented baseline for authentication and session-related regression testing.

Future exploratory focus areas may include:

- Token expiry timing behaviour  
- Rate limiting and brute-force mitigation  
- Improved user-facing failure feedback  