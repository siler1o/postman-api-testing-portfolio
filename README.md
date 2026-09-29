# Postman API Testing Portfolio

[![API tests](https://github.com/siler1o/postman-api-testing-portfolio/actions/workflows/api-tests.yml/badge.svg)](https://github.com/siler1o/postman-api-testing-portfolio/actions/workflows/api-tests.yml)

Functional API test automation for the [Automation Exercise practice API](https://automationexercise.com/api_list), built with **Postman, JavaScript, Newman, and GitHub Actions**.

The collection implements **14 positive and negative scenarios** across product discovery, login verification, unsupported methods, and account lifecycle operations. It checks transport-level HTTP status separately from the API's JSON `responseCode`, then validates response data and messages.

[Collection](postman/Automation-Exercise-API-Testing.postman_collection.json) · [Latest CI runs](https://github.com/siler1o/postman-api-testing-portfolio/actions/workflows/api-tests.yml) · [Test-case workbook](test-cases/Automation-Exercise-API-Test-Cases.xlsx) · [Execution screenshots](screenshots/)

## What this project demonstrates

- GET, POST, PUT, and DELETE requests with form-data bodies and query parameters.
- Positive and negative validation, including missing parameters and unsupported methods.
- Product search checks against names, brands, and categories.
- Disposable account creation, valid login, update, retrieval, and deletion.
- Verification that an account cannot log in after deletion.
- Repeatable command-line execution and CI results preserved as JUnit artifacts.

## Scenarios

| ID | Scenario | Method | Endpoint | Expected `responseCode` |
|----|----------|--------|----------|-------------------------|
| AE-API-001 | Get All Products | GET | `/api/productsList` | 200 |
| AE-API-002 | POST Products List Not Supported | POST | `/api/productsList` | 405 |
| AE-API-003 | Get All Brands | GET | `/api/brandsList` | 200 |
| AE-API-004 | PUT Brands List Not Supported | PUT | `/api/brandsList` | 405 |
| AE-API-005 | Search Product | POST | `/api/searchProduct` | 200 |
| AE-API-006 | Search Product Missing Parameter | POST | `/api/searchProduct` | 400 |
| AE-API-007 | Verify Login Valid Credentials | POST | `/api/verifyLogin` | 200 |
| AE-API-008 | Verify Login Missing Email | POST | `/api/verifyLogin` | 400 |
| AE-API-009 | DELETE Login Verification Not Supported | DELETE | `/api/verifyLogin` | 405 |
| AE-API-010 | Verify Login Invalid Credentials | POST | `/api/verifyLogin` | 404 |
| AE-API-011 | Create Account | POST | `/api/createAccount` | 201 |
| AE-API-013 | Update Account | PUT | `/api/updateAccount` | 200 |
| AE-API-014 | Get User Details by Email | GET | `/api/getUserDetailByEmail` | 200 |
| AE-API-012 | Delete Account | DELETE | `/api/deleteAccount` | 200 |

The IDs follow the practice site's scenario numbering; execution follows dependencies.

## Run in Postman

1. Import **`postman/Automation-Exercise-API-Testing.postman_collection.json`**.
2. Import **`postman/Automation-Exercise-Sample.postman_environment.json`** and select it.
3. Run the entire collection in its saved order using Collection Runner.

No manually registered account or shared credentials are required. Remove older imported copies to avoid accidentally running stale scripts.

The execution order is **001–006 → 011 → 007–010 → 013 → 014 → 012**. Creation runs before valid login; deletion runs last. Run one iteration initially.

### Test data and isolation

AE-API-011 generates a UUID-based email and password. The collection stores them in `newUserEmail` and `newUserPassword`, and records successful creation in `createdUserEmail`. Dependent requests use those exact collection-owned values. Existing `validEmail` and `validPassword` environment variables are not changed or used.

To send account requests individually, run AE-API-011 first, then the dependent requests in order. Update, retrieval, login, and deletion are blocked if successful account creation has not been recorded.

AE-API-010 generates an unrelated email and password for its negative login check.

| Environment variable | Default | Purpose |
|---|---|---|
| `baseUrl` | `https://automationexercise.com` | Practice API host |
| `searchProduct` | `top` | Keyword sent and checked by AE-API-005 |
| `maxResponseTimeMs` | `0` | Optional AE-API-001 response-time threshold; 0 disables it |

The practice API has no project-defined two-second SLA. Enable a timing threshold deliberately; functional assertions remain active regardless.

## Run with Newman

Install Node.js 22 or later, then run from the repository root:

```powershell
npx --yes newman@6.2.2 run postman/Automation-Exercise-API-Testing.postman_collection.json -e postman/Automation-Exercise-Sample.postman_environment.json --timeout-request 30000 --reporters cli,junit --reporter-junit-export reports/api-tests.xml
```

Keep the default behavior of continuing after assertion failures so AE-API-012 can clean up the disposable account. Avoid `--bail` for a full lifecycle run.

## Continuous integration

The [workflow](.github/workflows/api-tests.yml) runs the collection on pushes and pull requests to `main`, and supports manual execution from **Actions → API tests → Run workflow**.

Failed assertions produce a failed job. JUnit results are uploaded when available, including on failure, and retained for 14 days. No GitHub secrets are needed for the generated practice accounts. This repository performs continuous testing; it does not deploy an application.

## Verification snapshot

On 29 September 2026, the revised collection completed a live Newman run with **14 scenarios, 15 HTTP requests, and 67 passing assertions**. The extra request verifies login rejection after deletion. A separate guard check confirmed that running deletion without prior account creation fails without sending an HTTP request. The Actions badge above reports current CI status.

## Evidence and limitations

- The workbook and screenshots are **historical manual execution evidence**. Their recorded pass results are not the status of the latest collection or CI run.
- The workbook uses the earlier `validEmail` naming and execution order. Follow this README and the canonical JSON collection for current automated runs.
- The API may return HTTP 200 with JSON codes such as 201, 400, 404, or 405; tests distinguish these layers.
- Search relevance checks cover the returned name, brand, and category fields. They do not independently prove completeness of the search results.
- A canceled run or network failure can interrupt cleanup. In Postman, retain the generated collection values and rerun AE-API-012 after recovery. A failed creation response may require checking whether the practice server created the account before the connection failed.
- Fourteen scenarios demonstrate the documented practice API cases, not exhaustive security, performance, or contract coverage.

## Repository contents

| Path | Contents |
|---|---|
| `postman/` | Canonical collection and usable sample environment |
| `.github/workflows/api-tests.yml` | Newman automation and JUnit artifact upload |
| `test-cases/` | Historical test-case workbook |
| `screenshots/` | Historical execution evidence |

## About the author

**Reuben Jherico Silerio — QA Engineer**

Professional experience in manual and functional testing across web, mobile, UAT, regression, and production validation. This project demonstrates API test design, JavaScript assertions, data isolation, and automated execution.

[GitHub profile](https://github.com/siler1o) · [Selenium automation portfolio](https://github.com/siler1o/selenium-qa-automation-portfolio)
