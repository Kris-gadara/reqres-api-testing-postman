# Test Summary Report - ReqRes REST API Testing

| Project Details | Information |
|---|---|
| **Project Name** | ReqRes REST API Testing |
| **API Under Test** | `https://reqres.in` |
| **Testing Tool** | Postman v11+ / Newman CLI |
| **Author** | QA Engineer (Fresher Portfolio Project) |
| **Date** | October 2026 |
| **Test Execution Status** | **Executed** (Postman Collection Runner) |

---

## 1. Executive Summary

This document provides the final Test Summary for the **ReqRes REST API Testing** portfolio project. The suite covers functional, validation, negative, schema, header, and latency testing for `https://reqres.in`.

**Design phase:** 30 test cases were designed, cataloged, and implemented in `Postman/ReqRes-API-Testing.postman_collection.json`.

**Execution phase:** All **30 test cases (collection requests)** were executed in the Postman Collection Runner. At the **assertion level**, **115** Postman tests were evaluated: **112 passed**, **3 failed**, **0 errors**, in approximately **15.9 seconds**.

This report does **not** claim that all 30 test cases passed every assertion. Three assertion failures are documented below as **known validation issues** tied to shared `Content-Type` checks on **204 No Content** responses and a response-time SLA on the **delayed user** endpoint.

Evidence: see `Screenshots/01_ReqRes_Postman_30_Test_Cases.png` and `Screenshots/02_ReqRes_Postman_Run_Result_30_Tests_3_Failures.png`, and the [README execution section](../README.md#-latest-execution-results-postman-collection-runner).

---

## 2. Test Scope & Coverage Summary

The test suite provides comprehensive coverage across the following key operational modules:

| Test Module | Endpoint Routes Covered | Planned Test Cases | Coverage Focus |
|---|---|---|---|
| **User Management** | `/api/users`, `/api/users/{id}`, `/api/users?page={n}`, `/api/users?delay={s}` | 12 | GET, POST, PUT, PATCH, DELETE, Pagination, Delay SLA |
| **Auth & Access** | `/api/register`, `/api/login` | 8 | Positive register/login, missing credentials, bad formats |
| **Resource / Unknown** | `/api/unknown`, `/api/unknown/{id}` | 6 | List resources, single resource, 404 non-existent resources |
| **Headers & Boundary** | `/api/users/{invalid}`, `/api/users/9999` | 4 | Special characters, invalid path params, resilient method behaviors |
| **TOTAL** | **All Core ReqRes Endpoints** | **30** | **100% Core Endpoint Method Coverage** |

---

## 3. Test Requirement Matrix Verification

| Requirement Category | Covered Test Cases | Postman Assertion Type | Status |
|---|---|---|---|
| **User APIs** | TC01 - TC12 | `pm.response.to.have.status`, schema check | Executed |
| **Login APIs** | TC17 - TC20 | Token presence & error message string assertions | Executed |
| **Registration APIs** | TC13 - TC16 | ID & Token presence, 400 Bad Request error validation | Executed |
| **HTTP Methods (GET/POST/PUT/PATCH/DELETE)** | TC01 - TC30 | Complete CRUD coverage | Executed |
| **Positive & Negative Scenarios** | TC01 - TC30 | 16 Positive, 14 Negative/Edge cases | Executed |
| **Pagination & Boundaries** | TC01, TC02, TC11, TC12, TC25, TC26 | `page`, `per_page`, empty array checks | Executed |
| **Status Code & Response Body** | All Requests | Strict HTTP status & JSON payload validation | Executed (see Section 4 for assertion exceptions) |
| **Headers & Latency SLA** | All Requests | `Content-Type` validation & `< 2000ms` response time | Executed — 3 assertion failures (Section 4) |

---

## 4. Execution Results (Postman Collection Runner)

### 4.1 Summary metrics

| Metric | Result |
|---|---|
| Test cases (requests) executed | **30** |
| Total assertions evaluated | **115** |
| Assertions passed | **112** |
| Assertions failed | **3** |
| Errors | **0** |
| Duration (approx.) | **~15.9 s** |

### 4.2 Failed assertions (known / expected validation issues)

| ID | Test Case | What passed | Failed assertion | Root cause (QA note) |
|---|---|---|---|---|
| 1 | **TC09** — DELETE Existing User | HTTP **204 No Content** | Header `Content-Type` includes `application/json` | **204** responses have no body; global JSON `Content-Type` assertion is not appropriate for this response type. |
| 2 | **TC10** — GET Delayed User Response | HTTP **200**, response data validation | Response time **below 2000 ms** | Request uses intentional delay (`delay=3`); measured time can exceed the SLA on a live public API. |
| 3 | **TC29** — DELETE Non-Existent User ID | HTTP **204** or **404** (as configured) | Header `Content-Type` includes `application/json` | Same as TC09 when the service returns **204 No Content**. |

**Recommendation (documentation only — out of scope for this update):** In a follow-up iteration, assertions could be scoped per status code (skip JSON `Content-Type` on 204) or relax SLA on the delayed endpoint; collection scripts were not changed for this execution report.

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

The ReqRes API test suite was **fully executed** (30/30 requests). Functional and negative coverage is strong: **112 of 115 assertions passed** with **no runner errors**.

The three failing assertions are **understood and documented** — they stem from applying JSON header and global SLA checks to **204 No Content** and **delayed** scenarios where primary status and payload validations still passed.

For portfolio review, distinguish clearly between:

- **30 test cases executed** (all requests ran), and  
- **112 / 115 assertions passed** (three assertion-level failures, not “30/30 all green”).

The Postman collection remains suitable for regression runs, Newman CI integration, and fresher-level QA interviews when results are explained at the correct granularity.
