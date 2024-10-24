---
title: "TestOps Notes"
date: 2023-11-22
---

TestOps notes

https://en.wikipedia.org/wiki/TestOps

# Test Operations
* Planning
* Management (organization and governance, tools, test environment, test lifecycle [efficient and scalable])
* Control (Source control, collaboration tools, ownership)
* Insights (data, coverage, risk analysis)

## Service Oriented Architechture Testing
***Front-end***  
* Verify all States of controls (buttons, text fields, dropdowns, ...)
    * Enabled / Disabled (Buttons, dropdowns, input text fields)
    * Options of dropdowns
    * Text field values
    * Output only fields
    * ...
* Verify GET request fields are properly displayed on front-end
* Verify POST, PUT request fields are properly routed into payload / query parameters
* ...

## Test Automation Pyramid
As you ascend the pyramid, the tests become more costly in resources for creation, maintenance, and time and computing resources to run.

1. Unit Test (10000's)
2. Integration Tests (1000's)
3. End to End (e2e) (100's)

## Manual Testing
* Slower than automated
* Tedious and error prone
* Costly
* Essential :)

### Testing Fundamentals
* **Testing Early**: Prevent defects by testing the story requirements (Acceptance Criteria) before work is begun. Finding defects after the coding is done is *reactive* rather than *preventative*.
* **Verification**: Verify whether all specified requirements (AC) have been fulfilled. (Not to be confused with **Validation**.)
* **Validation**: what the client actually wants, not neccessarily what the requirements gave them. If the AC do not match what the client asked for then verification can pass, but validation will fail.
* **Risk Assessment**: provide detailed test reports to allow stakeholders sufficient information to make informed decisions about releases.
    * Organize defects and apply severity level to each.
    * Delivering software with minor issues on time, is better than delivering late.
    * Delivering software with severe issues is never a good idea regardless of time constraints.
    * Perfectionists never finish a project. Know when it is good enough. (Try to find ways to fix minor issues next time you touch a piece of code. TODOs in code will help the next developer to find them.)
* **Compliance**: Verify all legal and regulatory standards are met.
* **Subject Matter Expert**: Strive to become a subject matter expert in your domain to provide clear test evidence that the company is complying with regulations.
* **Debugging**: Finding root causes. (Testing is not debugging)

## ISTQB Foundation Levbel Syllabus v4.0
Keywords
* coverage: 
* debugging:
* defect:
* error:
* failure:
* quality:
* quality assurance:
* root cause:
* test analysis:
* test basis:
* test case:
* test completion:
* test condition:
* test control:
* test data:
* test design:
* test execution:
* test implementation:
* test monitoring:
* test object:
* test objective:
* test planning:
* test procedure:
* test result:
* testing:
* testware:
* validation:
* verification:

Corresponds to the syllabus "1.1 What is Testing?"
