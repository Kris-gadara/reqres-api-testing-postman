# ReqRes REST API Testing with Postman 🚀

![API Testing](https://img.shields.io/badge/Testing-API%20Testing-blue.svg)
![Postman](https://img.shields.io/badge/Postman-v11%2B-orange.svg)
![Newman](https://img.shields.io/badge/CLI-Newman-brightgreen.svg)
![JavaScript](https://img.shields.io/badge/Assertions-JavaScript-yellow.svg)
![QA Level](https://img.shields.io/badge/QA%20Level-Fresher%20%2F%20Junior%20QA-purple.svg)

A professional, production-grade **API Testing Portfolio Project** designed for **ReqRes REST API** (`https://reqres.in/`). 

This repository showcases real-world Quality Assurance (QA) methodologies including **API Test Planning**, **Test Case Design (30 Test Cases)**, **Postman Automated Test Scripting (JavaScript Assertions)**, **Environment Management**, **Defect Reporting Templates**, and **Execution Tracking Matrices**.

---

## 📌 Repository Structure

```text
reqres-api-testing-postman/
│
├── README.md                                    # Project documentation & QA interview guide
├── Test-Plan/
│   └── APITestPlan.md                           # Master API Test Plan document
├── Test-Cases/
│   └── APITestCases.xlsx                        # 30 detailed test cases covering CRUD, Auth & Edge cases
├── Postman/
│   ├── ReqRes-API-Testing.postman_collection.json # Postman collection with JS assertions
│   └── ReqRes-Environment.postman_environment.json# Postman environment variables
├── Test-Data/
│   └── TestData.md                              # Test data specifications & payload JSONs
├── Bug-Reports/
│   └── BugReport.xlsx                           # Standard defect reporting sheet template
├── Test-Execution/
│   └── TestExecutionReport.xlsx                 # Execution status tracking matrix (Initially Not Executed)
└── Test-Summary/
    └── TestSummaryReport.md                     # Final QA test summary & coverage report
```

---

## 🎯 Project Overview & Scope

- **API Under Test**: ReqRes REST API (`https://reqres.in/`)
- **Total Test Cases**: 30 Automated Postman Requests & Documented Scenarios
- **Key Modules Tested**:
  1. **User Management** (`GET`, `POST`, `PUT`, `PATCH`, `DELETE`)
  2. **Authentication & Authorization** (Register & Login - Positive & Negative)
  3. **Resource Data / Unknown Endpoints** (`/api/unknown`)
  4. **Pagination & Boundary Testing** (`page=1`, `page=2`, `page=-1`, `page=9999`)
  5. **Negative Testing & Validation** (Invalid IDs, missing payloads, missing credentials)
  6. **Headers & Performance SLA** (`Content-Type` checks, Response Time `< 2000ms`)

---

## 🧪 Postman Test Assertions (JavaScript)

Every request in the Postman Collection includes robust JavaScript assertions located under the **Tests** tab:

```javascript
// 1. Status Code Assertion
pm.test("Status code is 200 OK", function () {
    pm.response.to.have.status(200);
});

// 2. Response Time SLA Assertion (< 2000ms)
pm.test("Response time is within SLA limit (<2000ms)", function () {
    pm.expect(pm.response.responseTime).to.be.below(2000);
});

// 3. Header Validation
pm.test("Header Content-Type validation", function () {
    pm.expect(pm.response.headers.get("Content-Type")).to.include("application/json");
});

// 4. Response Body & Field Validation
pm.test("Verify single user payload structure", function () {
    var jsonData = pm.response.json();
    pm.expect(jsonData.data.id).to.eql(2);
    pm.expect(jsonData.data).to.have.property("email");
    pm.expect(jsonData.data).to.have.property("first_name");
});
```

---

## 🔑 Environment Variables Configuration

The environment file `Postman/ReqRes-Environment.postman_environment.json` uses dynamic variables:

| Variable | Placeholder / Default Value | Description |
|---|---|---|
| `{{baseUrl}}` | `https://reqres.in` | Base URL of ReqRes API |
| `{{apiKey}}` | `""` (Empty placeholder) | Authorization header key placeholder |
| `{{userId}}` | `2` | Valid target user ID |
| `{{invalidUserId}}` | `999` | Non-existent user ID |
| `{{page}}` | `2` | Pagination page number |
| `{{userEmail}}` | `eve.holt@reqres.in` | Valid email for login & registration |
| `{{userPassword}}` | `cityslicka` | Valid password for login & registration |
| `{{unsuccessfulEmail}}` | `peter@klaven` | Email missing password for negative tests |

---

## ⚡ How to Import & Run in Postman

### Option A: Using Postman GUI
1. Open **Postman**.
2. Click **Import** (top left).
3. Import both files from the `/Postman` folder:
   - `ReqRes-API-Testing.postman_collection.json`
   - `ReqRes-Environment.postman_environment.json`
4. Select `ReqRes-Environment` in the top-right environment dropdown.
5. Click on the `ReqRes-API-Testing` collection and select **Run Collection**.

### Option B: Running via Newman CLI
1. Install Node.js & Newman:
   ```bash
   npm install -g newman newman-reporter-htmlextra
   ```
2. Run the automated collection from the project root:
   ```bash
   newman run Postman/ReqRes-API-Testing.postman_collection.json \
     -e Postman/ReqRes-Environment.postman_environment.json \
     -r cli,htmlextra --reporter-htmlextra-export Test-Execution/ExecutionReport.html
   ```

---

## 📊 Summary of 30 API Test Cases

| Test Case ID | Module | Method | Endpoint / Scenario | Expected Status |
|---|---|---|---|---|
| **TC01** | Users | GET | List Users Page 1 | 200 OK |
| **TC02** | Users | GET | List Users Page 2 (`{{page}}`) | 200 OK |
| **TC03** | Users | GET | Single User Existing (`{{userId}}`) | 200 OK |
| **TC04** | Users | GET | Single User Not Found (`{{invalidUserId}}`) | 404 Not Found |
| **TC05** | Users | POST | Create User Valid Payload | 201 Created |
| **TC06** | Users | POST | Create User Empty Body | 201 Created |
| **TC07** | Users | PUT | Update User Complete Payload | 200 OK |
| **TC08** | Users | PATCH | Update User Partial Payload | 200 OK |
| **TC09** | Users | DELETE | Delete User Existing ID | 204 No Content |
| **TC10** | Users | GET | Delayed User Response (`delay=3`) | 200 OK |
| **TC11** | Users | GET | Users Invalid Negative Page (`page=-1`) | 200 OK |
| **TC12** | Users | GET | Users High Out of Range Page (`page=9999`) | 200 OK |
| **TC13** | Auth | POST | Register User Successful | 200 OK |
| **TC14** | Auth | POST | Register User Missing Password | 400 Bad Request |
| **TC15** | Auth | POST | Register User Missing Email | 400 Bad Request |
| **TC16** | Auth | POST | Register User Empty Payload | 400 Bad Request |
| **TC17** | Auth | POST | Login User Successful | 200 OK |
| **TC18** | Auth | POST | Login User Unsuccessful Missing Password | 400 Bad Request |
| **TC19** | Auth | POST | Login User Unsuccessful Missing Email | 400 Bad Request |
| **TC20** | Auth | POST | Login User Invalid Email Format | 400 Bad Request |
| **TC21** | Resource | GET | List Resource / Unknown | 200 OK |
| **TC22** | Resource | GET | Single Resource Existing (`id=2`) | 200 OK |
| **TC23** | Resource | GET | Single Resource Not Found (`id=23`) | 404 Not Found |
| **TC24** | Resource | GET | Single Resource String ID (`id=abc`) | 404 Not Found |
| **TC25** | Resource | GET | Resource Page 2 Pagination | 200 OK |
| **TC26** | Resource | GET | Resource Out of Bounds Page (`page=500`) | 200 OK |
| **TC27** | Users | GET | Single User Invalid String ID | 404 Not Found |
| **TC28** | Users | PUT | Update Non-Existent User (`id=9999`) | 200 OK / 404 |
| **TC29** | Users | DELETE | Delete Non-Existent User (`id=9999`) | 204 / 404 |
| **TC30** | Users | POST | Create User Special Characters In Payload | 201 Created |

---

## 💼 Interview Talking Points (Fresher QA Role)

When discussing this project in a QA Engineer technical interview:
1. **API Testing vs UI Testing**: Explain why API testing is faster, more reliable, and catches bugs earlier in the software development lifecycle (Shift-Left testing).
2. **Postman & JavaScript Assertions**: Detail how you validated status codes, headers (`Content-Type`), JSON keys (`id`, `email`, `token`), dynamic value data types, and latency SLA (<2000ms).
3. **Environment Management**: Discuss why `{{baseUrl}}` and `{{apiKey}}` placeholders were abstracted into environment files to prevent credential exposure and support multi-environment execution (Dev, Staging, Prod).
4. **Positive vs Negative Test Scenarios**: Highlight your coverage of edge cases such as missing passwords (400 Bad Request), invalid user IDs (404 Not Found), and boundary page queries.
5. **CI/CD Automation Readiness**: Mention how Newman CLI enables seamless integration into GitHub Actions or Jenkins pipelines.

---

## 📜 License & Acknowledgments
- API provided by [ReqRes](https://reqres.in/)
- Project created for QA Engineering Portfolio Demonstrations.