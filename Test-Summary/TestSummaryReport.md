# Test Summary Report - ReqRes REST API Testing

| Project Details             | Information                             |
| --------------------------- | --------------------------------------- |
| **Project Name**            | ReqRes REST API Testing                 |
| **API Under Test**          | `https://reqres.in`                     |
| **Testing Tool**            | Postman v11+ / Newman CLI               |
| **Author**                  | QA Engineer (Fresher Portfolio Project) |
| **Date**                    | October 2026                            |
| **Final Case-Level Status** | **30 Pass, 0 Fail, 0 Pending**          |

---

## 1. Executive Summary

This document provides the final Test Summary for the **ReqRes REST API Testing** portfolio project. The suite covers functional, validation, negative, schema, header, and latency testing for `https://reqres.in`.

**Design phase:** 30 test cases were designed, cataloged, and implemented in `Postman/ReqRes-API-Testing.postman_collection.json`.

**Archived execution evidence:** All **30 requests** ran in the saved Postman Collection Runner result. At the assertion level, **112 of 115** checks passed; the three failures were generic checks that conflicted with expected 204 response semantics and the intentionally delayed TC10 route. Their functional status/body assertions passed, and the collection has since been corrected.

**Final case-level assessment:** 30 Pass, 0 Fail, 0 Pending. No confirmed API defects remain, so the defect log contains no entries. A current Newman rerun attempted on 2026-10-06 encountered HTTP **429 rate_limit_exceeded** from ReqRes; this was recorded as a rate-limit limitation, not an endpoint failure.

Evidence: see `Screenshots/01_ReqRes_Postman_30_Test_Cases.png`, `Screenshots/02_ReqRes_Postman_Run_Result_30_Tests_3_Failures.png`, and the [README execution section](../README.md#-execution-results).

---

## 2. Test Scope & Coverage Summary

The test suite provides comprehensive coverage across the following key operational modules:

| Test Module            | Endpoint Routes Covered                                                        | Planned Test Cases | Coverage Focus                                                      |
| ---------------------- | ------------------------------------------------------------------------------ | ------------------ | ------------------------------------------------------------------- |
| **User Management**    | `/api/users`, `/api/users/{id}`, `/api/users?page={n}`, `/api/users?delay={s}` | 12                 | GET, POST, PUT, PATCH, DELETE, Pagination, Delay SLA                |
| **Auth & Access**      | `/api/register`, `/api/login`                                                  | 8                  | Positive register/login, missing credentials, bad formats           |
| **Resource / Unknown** | `/api/unknown`, `/api/unknown/{id}`                                            | 6                  | List resources, single resource, 404 non-existent resources         |
| **Headers & Boundary** | `/api/users/{invalid}`, `/api/users/9999`                                      | 4                  | Special characters, invalid path params, resilient method behaviors |
| **TOTAL**              | **All Core ReqRes Endpoints**                                                  | **30**             | **100% Core Endpoint Method Coverage**                              |

---

## 3. Test Requirement Matrix Verification

| Requirement Category                         | Covered Test Cases                 | Postman Assertion Type                                              | Status                                                           |
| -------------------------------------------- | ---------------------------------- | ------------------------------------------------------------------- | ---------------------------------------------------------------- |
| **User APIs**                                | TC01 - TC12                        | `pm.response.to.have.status`, schema check                          | Executed                                                         |
| **Login APIs**                               | TC17 - TC20                        | Token presence & error message string assertions                    | Executed                                                         |
| **Registration APIs**                        | TC13 - TC16                        | ID & Token presence, 400 Bad Request error validation               | Executed                                                         |
| **HTTP Methods (GET/POST/PUT/PATCH/DELETE)** | TC01 - TC30                        | Complete CRUD coverage                                              | Executed                                                         |
| **Positive & Negative Scenarios**            | TC01 - TC30                        | 16 Positive, 14 Negative/Edge cases                                 | Executed                                                         |
| **Pagination & Boundaries**                  | TC01, TC02, TC11, TC12, TC25, TC26 | `page`, `per_page`, empty array checks                              | Executed                                                         |
| **Status Code & Response Body**              | All Requests                       | Strict HTTP status & JSON payload validation                        | Executed (see Section 4 for assertion exceptions)                |
| **Headers & Latency SLA**                    | All Requests                       | Applicable `Content-Type` checks; `< 2000ms` for non-delayed routes | Archived assertions reviewed; three checks corrected (Section 4) |

---

## 4. Execution Results (Postman Collection Runner)

### 4.1 Summary metrics

| Metric                         | Result      |
| ------------------------------ | ----------- |
| Test cases (requests) executed | **30**      |
| Total assertions evaluated     | **115**     |
| Assertions passed              | **112**     |
| Assertions failed              | **3**       |
| Errors                         | **0**       |
| Duration (approx.)             | **~15.9 s** |

### 4.2 Corrected assertion mismatches

| ID  | Test Case                              | What passed                             | Failed assertion                                              | Root cause (QA note)                                                                               |
| --- | -------------------------------------- | --------------------------------------- | ------------------------------------------------------------- | -------------------------------------------------------------------------------------------------- |
| 1   | **TC09** — DELETE Existing User        | HTTP **204 No Content**                 | Removed response `Content-Type: application/json` check       | **204** has no response body; the test now validates status and latency only.                      |
| 2   | **TC10** — GET Delayed User Response   | HTTP **200**, response data validation  | Replaced `< 2000 ms` with `>= 3000 ms` configured-delay check | Request uses `delay=3`; the normal endpoint SLA does not apply to this intentional delay scenario. |
| 3   | **TC29** — DELETE Non-Existent User ID | HTTP **204** or **404** (as configured) | Removed response `Content-Type: application/json` check       | When response is **204 No Content**, a JSON response content type is not applicable.               |

These were assertion-design issues rather than API defects. The current collection includes the corrections, and no defects were added to `BugReport.xlsx`.

### 4.3 Current live rerun limitation

Newman sent all 30 requests on 2026-10-06 using the configured ReqRes demo key. ReqRes returned HTTP **429 rate_limit_exceeded** during the run; a later paced attempt remained throttled. This is a service/rate-limit constraint, not sufficient evidence to fail individual endpoint cases. The execution matrix records archived functional evidence and calls out this limitation explicitly.

### 4.3 How to reproduce

1. Import `Postman/ReqRes-Environment.postman_environment.json` and `Postman/ReqRes-API-Testing.postman_collection.json`.
2. Select the **ReqRes-Environment** in Postman.
3. Run the **ReqRes-API-Testing** collection via **Collection Runner** (all 30 requests).

**Newman (optional):**

```bash
newman run Postman/ReqRes-API-Testing.postman_collection.json \
  -e Postman/ReqRes-Environment.postman_environment.json \
  --reporters cli,html --reporter-html-export Test-Execution/ExecutionReport.html
```

---

## 5. Conclusion & Recommendations

The archived ReqRes execution covered **30/30 requests**. Final case-level assessment is **30 Pass, 0 Fail, 0 Pending**; the archived run shows **112 of 115 assertions passed** before three assertion-design mismatches were corrected.

The three archived assertion mismatches are corrected: JSON content-type checks no longer apply to 204 responses, and TC10 checks its configured delay instead of the normal response SLA. A current rerun was rate-limited by ReqRes (429), so it did not supersede the archived functional result. No confirmed API defects are logged.

For portfolio review, distinguish clearly between:

- **30 test cases assessed as Pass** from archived functional/status evidence, and
- the current live rerun was throttled, so its 429 responses are not treated as functional failures.

The Postman collection remains suitable for regression runs, Newman CI integration, and fresher-level QA interviews when results are explained at the correct granularity.
