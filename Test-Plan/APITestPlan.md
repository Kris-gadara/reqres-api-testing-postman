# API Test Plan - ReqRes REST API Testing

| Project | ReqRes REST API Testing |
|---|---|
| **API Base URL** | `https://reqres.in` |
| **Author** | QA Engineer (Portfolio Project) |
| **Version** | 1.0.0 |
| **Date** | October 2026 |
| **Status** | Approved |

---

## 1. Introduction & Objectives
The purpose of this Test Plan is to define the testing scope, strategy, environment, resources, and schedule for testing the **ReqRes REST API** (`https://reqres.in/`). 

ReqRes is a hosted REST API service that simulates real-world user data operations. This project aims to validate the functional accuracy, data integrity, response formats, status codes, error handling, performance (response times), and contract adherence of ReqRes endpoints using **Postman**.

### 1.1 Objectives
- Ensure all API endpoints (`/api/users`, `/api/register`, `/api/login`, `/api/unknown`) behave according to standard REST specification.
- Validate HTTP response status codes for both positive (200, 201, 204) and negative scenarios (400, 404).
- Verify JSON schema, mandatory response keys, dynamic data types, and header values.
- Verify robust error messages when missing required fields or supplying invalid credentials.
- Measure API response latency to ensure SLA requirements (< 2000ms) are satisfied.

---

## 2. Scope of Testing

### 2.1 In-Scope
1. **User Management Endpoints**:
   - `GET /api/users?page={page}` (List Users & Pagination)
   - `GET /api/users/{id}` (Single User details - Existing & Non-existing)
   - `POST /api/users` (Create User)
   - `PUT /api/users/{id}` (Update User - Complete Update)
   - `PATCH /api/users/{id}` (Update User - Partial Update)
   - `DELETE /api/users/{id}` (Delete User)
   - `GET /api/users?delay={seconds}` (Delayed Response handling)
2. **Resource / Unknown Endpoints**:
   - `GET /api/unknown` (List Resource Data)
   - `GET /api/unknown/{id}` (Single Resource - Existing & Non-existing)
3. **Authentication & Authorization**:
   - `POST /api/register` (Register User - Successful & Unsuccessful)
   - `POST /api/login` (Login User - Successful & Unsuccessful)
4. **Validation Categories**:
   - Status Code Validation
   - Response Body Field & Schema Structure Validation
   - Header Checks (`Content-Type: application/json; charset=utf-8`)
   - Performance / Response Time Threshold Checks (< 2000ms)
   - Negative Testing (Invalid IDs, missing payloads, bad credentials)

### 2.2 Out-of-Scope
- Backend application code modification or database direct querying.
- Load testing / Heavy stress performance testing beyond basic single-request response time SLA checks.
- Mobile UI or Web UI frontend automation.

---

## 3. Test Strategy & Methodology

### 3.1 Test Automation Tool
- **Postman**: Used for designing test collections, sending HTTP requests, managing environment variables, and writing JavaScript test scripts.
- **Postman Test Runner / Newman**: CLI tool for executing the test collection in CI/CD or local automated runs.

### 3.2 Test Levels & Types
- **Functional API Testing**: Verifying end-to-end user flows (e.g., login, registration, user creation).
- **Negative Testing**: Validating failure responses when mandatory fields (`email`, `password`) are omitted or invalid parameters are supplied.
- **Boundary & Validation Testing**: Testing non-existent resource IDs (`999`, `-1`), page numbers (`0`, `9999`).
- **Performance SLA Testing**: Asserting that response duration does not exceed threshold limits (2000ms).

### 3.3 Test Assertion Framework (Postman JavaScript)
All Postman requests contain automated JavaScript assertions in the **Tests** tab covering:
```javascript
// Example assertion snippet used across collection
pm.test("Status code is 200 OK", function () {
    pm.response.to.have.status(200);
});
pm.test("Response time is less than 2000ms", function () {
    pm.expect(pm.response.responseTime).to.be.below(2000);
});
pm.test("Response header Content-Type contains application/json", function () {
    pm.response.to.have.header("content-type");
    pm.expect(pm.response.headers.get("content-type")).to.include("application/json");
});
```

---

## 4. Test Environment & Configuration

### 4.1 Environment Variables
| Variable Name | Sample / Default Value | Purpose |
|---|---|---|
| `{{baseUrl}}` | `https://reqres.in` | Base host URL for all API requests |
| `{{apiKey}}` | `""` (Empty / Placeholder) | Optional API key header placeholder |
| `{{userId}}` | `2` | Default existing user ID for GET/PUT/PATCH/DELETE |
| `{{invalidUserId}}` | `999` | Non-existent user ID for negative scenarios |
| `{{page}}` | `2` | Default page number for pagination |
| `{{job}}` | `leader` | Default job title payload |
| `{{userEmail}}` | `eve.holt@reqres.in` | Valid email for authentication tests |
| `{{userPassword}}` | `cityslicka` | Valid password for authentication tests |
| `{{unsuccessfulEmail}}` | `peter@klaven` | Email missing password for negative tests |

---

## 5. Entry & Exit Criteria

### 5.1 Entry Criteria
- ReqRes API endpoints are accessible online (`https://reqres.in/`).
- Postman Collection and Environment files are created and validated.
- Test Cases and Test Data documents are finalized.

### 5.2 Exit Criteria
- 100% of planned 30 test cases have been executed.
- All executed tests pass or any failed tests have logged defects in `BugReport.xlsx`.
- Test Summary Report (`TestSummaryReport.md`) is populated and published.

---

## 6. Risk Management

| Risk Description | Severity | Mitigation Strategy |
|---|---|---|
| ReqRes public API rate limiting or temporary downtime | High | Use retry logic, verify connectivity, or run tests with slight delays (`delay=1`). |
| Dynamic data changes on third-party public API | Medium | Structure assertions to validate data types and mandatory schema keys rather than static dynamic values where appropriate. |
| Potential requirement of API keys by host | Low | Configured `{{apiKey}}` environment variable placeholder to seamlessly attach headers without hardcoding real credentials. |

---

## 7. Deliverables
1. `APITestPlan.md` (This document)
2. `ReqRes-API-Testing.postman_collection.json` (30 Requests with Postman JS Assertions)
3. `ReqRes-Environment.postman_environment.json`
4. `APITestCases.xlsx` (30 Structured Test Cases)
5. `TestData.md` (Input data specifications)
6. `BugReport.xlsx` (Defect reporting log)
7. `TestExecutionReport.xlsx` (Execution tracking sheet)
8. `TestSummaryReport.md` (Final QA Summary)
9. `README.md` (Portfolio documentation & interview guide)
