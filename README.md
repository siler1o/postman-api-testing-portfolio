# Postman API Testing Portfolio

A Postman API testing project created using the [Automation Exercise](https://automationexercise.com/api_list) practice API.

## Project Overview

This project demonstrates functional API testing using Postman. It contains 14 positive and negative test scenarios covering products, brands, product search, login verification, unsupported request methods, and account management.

## Tools and Technologies

* Postman
* JavaScript
* JSON
* REST API
* Git and GitHub
* Microsoft Excel / Google Sheets

## Testing Coverage

* GET, POST, PUT, and DELETE requests
* Query parameters and form-data
* Environment variables
* Dynamic test email generation
* HTTP status validation
* JSON response validation
* Response structure and value assertions
* Positive and negative testing
* Account lifecycle testing

## Account Lifecycle

The account tests should be executed in this order:

1. AE-API-011 – Create Account
2. AE-API-013 – Update Account
3. AE-API-014 – Get User Details by Email
4. AE-API-012 – Delete Account

## Project Structure

* `postman/` – Postman collection and sample environment
* `test-cases/` – Documented API test cases
* `screenshots/` – Evidence of successful test execution

## Important Note

The sample environment does not contain private credentials. Users should enter their own disposable test credentials after importing it into Postman.

Some Automation Exercise endpoints return HTTP status 200 while providing a different application-level `responseCode` in the JSON body. Both values are validated where applicable.

## About Me

Hi, I'm **Reuben Jherico Silerio**, a QA Engineer with more than three years of hands-on experience in manual and functional testing across web, mobile, and production environments. My experience includes smoke, regression, end-to-end, UAT, and production validation testing.

I am expanding my automation skills through API testing with Postman and JavaScript, as well as UI automation using Python, Selenium WebDriver, Pytest, and the Page Object Model. I focus on creating clear test documentation, reliable assertions, and evidence-driven quality validation.

[View my GitHub profile](https://github.com/siler1o)
