# EXP-2026-01-29 – Authentication and Session Behaviour

## Session Overview

**Application:** Shopping List App  
**Session ID:** EXP-2026-01-29-AUTH  
**Objective:** Explore authentication workflows and session failure modes within the Shopping List App.  
**Timebox:** 60 minutes  
**Environment:**  
- OS: Windows 11  
- Browser: Firefox 147.0.2 (64-bit)  
- Frontend: Vite / React  
- Backend: Node.js 18  
- Database: MongoDB (local)  

---

## Risk Focus

This exploratory session focused on high-impact authentication and session risks:

- Invalid credential handling  
- Backend unavailability during login  
- Expired or missing JWT handling  
- Manual token removal or tampering  
- Access control for protected routes  
- 401 response handling in UI  
- Session persistence across refresh  

Authentication and session management were identified as high-risk areas due to their impact on user access and overall system trust.

---

## Test Ideas Explored

The following scenarios were explored without predefined scripted steps:

- Attempt login with backend intentionally stopped  
- Remove JWT from local storage during active session  
- Modify JWT manually to simulate tampering  
- Access protected routes after token removal  
- Refresh application after session invalidation  
- Rapid login/logout sequence testing  
- Attempt navigation between protected and public routes during session changes  

---

## Observations

- Backend correctly returned 401 responses for invalid or missing tokens.  
- When backend was unavailable, raw browser network errors were exposed in the UI.  
- Removing JWT resulted in logout without user-facing explanation.  
- No redirect loops or crash scenarios observed.  
- Core authentication logic functioned as expected under normal conditions.  

---

## Issues Raised

The following defects were logged as a result of this session:

- BR-001 – Login displays raw network error when backend is unavailable  
- BR-002 – No user feedback when session becomes invalid  

---

## Conclusion

Core authentication and authorisation logic is functionally stable.  
However, failure-mode behaviour reveals UX weaknesses in error handling and session feedback.

No security-critical flaws were identified during this session.  
Regression checklist items RC-06, RC-07, and RC-09 were updated to reflect findings.

Further exploratory sessions should examine rate limiting, brute-force resistance, and token expiry timing behaviour.