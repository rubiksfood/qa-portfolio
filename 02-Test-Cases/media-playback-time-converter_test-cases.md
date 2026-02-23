# Manual Test Cases – Media Playback Time Converter

Author: Joshua Pearson  
Test level: System (manual)  
Test design basis: Requirements, UI behaviour, ISTQB test design techniques  
Scope: Media Playback Time Converter (frontend-only)

---

## 1. Purpose

This document defines a set of manual test cases for the Media Playback Time Converter app. 
The test cases focus on input handling, calculation correctness, error handling, and user feedback.

Test cases are designed using ISTQB-aligned techniques such as:
- Equivalence Partitioning
- Boundary Value Analysis
- Error Guessing

---

## 2. Test Scope

### In scope
- Valid and invalid time input formats
- Boundary values for hours, minutes, and seconds
- Conversion accuracy
- Error messaging and user feedback
- Behaviour on empty or partial input

### Out of scope
- Performance testing
- Cross-browser compatibility testing
- Accessibility compliance verification (to be covered separately via exploratory testing)

---

## 3. Assumptions

- Input fields accept numeric values only
- Conversion logic is client-side
- No persistence or backend interaction is involved

---

## 4. Test Cases

### TC-01 – Convert valid HH:MM:SS input

| Field           | Value                                                                 |
|-----------------|-----------------------------------------------------------------------|
| Priority        | High                                                                  |
| Preconditions   | Application loaded                                                    |
| Steps           | 1. Enter valid hours, minutes, and seconds <br> 2. Trigger conversion |
| Expected result | Converted playback time is displayed correctly                        |
| Notes           | Equivalence class: valid full input                                   |

---

### TC-02 – Convert valid MM:SS input (hours omitted)

| Field           | Value                                                                                           |
|-----------------|-------------------------------------------------------------------------------------------------|
| Priority        | High                                                                                            |
| Preconditions   | Application loaded                                                                              |
| Steps           | 1. Leave hours empty or zero <br> 2. Enter valid minutes and seconds <br> 3. Trigger conversion |
| Expected result | Converted playback time is displayed correctly                                                  |
| Notes           | Equivalence class: valid partial input, optional field handling                                 |

---

### TC-03 – Boundary value (below): seconds = 59

| Field           | Value                                                   |
|-----------------|---------------------------------------------------------|
| Priority        | Medium                                                  |
| Preconditions   | Application loaded                                      |
| Steps           | 1. Enter seconds value of 59 <br> 2. Trigger conversion |
| Expected result | Conversion succeeds without error                       |
| Notes           | Boundary value analysis                                 |

---

### TC-04 – Boundary value (at): seconds = 60

| Field           | Value                                                                                          |
|-----------------|------------------------------------------------------------------------------------------------|
| Priority        | Medium                                                                                         |
| Preconditions   | Application loaded                                                                             |
| Steps           | 1. Enter seconds value of 60; enter speed value of 1.0x <br> 2. Trigger conversion             |
| Expected result | Conversion succeeds without error; value normalised to 1m 0s                                   |
| Notes           | Boundary value analysis; normalisation is expected behaviour (based on current implementation) |

---

### TC-05 – Negative input values

| Field           | Value                                                           |
|-----------------|-----------------------------------------------------------------|
| Priority        | High                                                            |
| Preconditions   | Application loaded                                              |
| Steps           | 1. Enter negative value in any field <br> 2. Trigger conversion |
| Expected result | Error message displayed; input rejected                         |
| Notes           | Error guessing                                                  |

---

### TC-06 – Empty input submission

| Field           | Value                                                      |
|-----------------|------------------------------------------------------------|
| Priority        | High                                                       |
| Preconditions   | Application loaded                                         |
| Steps           | 1. Leave all input fields empty <br> 2. Trigger conversion |
| Expected result | User-friendly error message is displayed                   |
| Notes           | Required field validation                                  |

---

### TC-07 – Non-numeric input attempt

| Field           | Value                                      |
|-----------------|--------------------------------------------|
| Priority        | Medium                                     |
| Preconditions   | Application loaded                         |
| Steps           | 1. Attempt to enter non-numeric characters |
| Expected result | Non-numeric input is rejected or sanitised |
| Notes           | Input validation                           |

---

### TC-08 – Missing speed value

| Field           | Value                                                                                 |
|-----------------|---------------------------------------------------------------------------------------|
| Priority        | High                                                                                  |
| Preconditions   | Application loaded                                                                    |
| Steps           | 1. Enter valid value for minutes <br> 2. Leave speed empty <br> 3. Trigger conversion |
| Expected result | User-friendly error message is displayed                                              |
| Notes           | Negative testing; ambiguous input handling                                            |

---

### TC-09 – Valid input at speed > 1.0x

| Field           | Value                                                                           |
|-----------------|---------------------------------------------------------------------------------|
| Priority        | High                                                                            |
| Preconditions   | Application loaded                                                              |
| Steps           | 1. Enter 10 for minutes <br> 2. Enter speed of 1.25x <br> 3. Trigger conversion |
| Expected result | Converted playback time is displayed correctly (i.e. 8 minutes)                 |
| Notes           | Equivalence class: speed > 1.0x                                                 |

### TC-10 – Valid input at speed < 1.0x

| Field           | Value                                                                           |
|-----------------|---------------------------------------------------------------------------------|
| Priority        | High                                                                            |
| Preconditions   | Application loaded                                                              |
| Steps           | 1. Enter 10 for minutes <br> 2. Enter speed of 0.75x <br> 3. Trigger conversion |
| Expected result | Converted playback time is displayed correctly (i.e. 13 minutes, 20 seconds)    |
| Notes           | Equivalence class: speed < 1.0x                                                 |

---

Note: 
1. Unless stated otherwise, test cases assume a valid speed multiplier is selected.
2. Test case IDs in this document are local to this manual test suite.  
Therefore, they do not correspond one-to-one with the test IDs found in the original project repository.  

---

## 5. Exit Criteria

- All high-priority test cases executed
- No unresolved high-severity defects related to conversion correctness or error handling