---
title: "Cypress Automation Notes"
date: 2023-10-30
---

Notes while I work to be fleshed out better later.

# Authentication (Get a bearer token first and reuse in all tests, unless it has expired.)

# Data Driven Tests
Passing in a *.csv file of data or a *Theory* in xUnit.

# PluralSight Course
Cypress 9 Fundamentals

## Test Automation Pyramid
As you ascend the pyramid, the tests become more costly in resources for creation, maintenance, and time and computing resources to run.

1. Unit Test (10000's)
2. Integration Tests (1000's)
3. End to End (e2e) (100's)

## Manual Testing
* Slower than automated
* Tedious and error prone
* Costly

## Cypress Claims / Features
* Faster than traditional e2e tests to create
* Easier than tradtional e2e to create
* Cost effective
* Time Travel & Debugging (Very Pretty)
* Automatic Waiting (Any well developed library will implement this)
* Reliable and Fast
* Screenshots and Videos
* Spies, Stubs and Clocks
* Network Traffic Control

## Test Code Basics
***describe*** keyword is used to group tests.
***it*** keyword is used to explain and encapsulate a test.
This comes from Mocha, another testing framework.
***cy.{command}*** is a cypress command.

## Smack Talk -> Other Frameworks
Mocha, Jasmine, Karma

### Assertion Library
Chai, Expect.js

### Selenium Wrapper
Protractor, Nightwatch

Sinon, TestDouble

## With Cypress it combines all of these in one framework
Including:
* Mocking
* Assertions
* Stubbing

## Tradeoffs
* Not a general-purpose tool (It does end to end automation testing)
* Runs inside the browser (JavaScript/TypeScript is the only languages supported, anything outside of the browser is extra work)
* No Multi-tab, Multi-browser support
* Single domain testing in single test, but you can cross domains in separate tests.

## Visual Studio Code
Plugins

* Prettier
* cypress snippets

I installed the Cypress Extension Pack

## Project Settings
package.json file contains a node for `"devDependencies": { "cypress": "^9.5.4"...`

1. `$ npm install` at the root folder and the cypress package will be installed.
2. `$ npx cypress open ` to open cypress.

## Intellisense 
I am not sure this is real. I get intellisense even if I don't add the line at the top.
Add `/// <reference types="cypress" />` to the top of any test file *.spec.js

## Cypress Commands
`cy.get("h1").contains("Some Text")` will check all of the h1's on the page and find the one with "Some Text".

## Cypress Test Folder Organization
* fixtures - Holds *.json, image files or whatever you may needed to ensure that there is a well known and fixed environment for tests to run consitently. Access fixtures with the `cy.fixture()` command.
* integration - Holds all of the test (spec) files.
* plugins - This may not be included by default anymore.
* support - Support folder has commands.js, where you include reusable commands used that Cypress includes in every run.

## Hooks (Mocha)
```javascript
before(() => {
  // root-level hook that runs once before any tests have been executed.
})

beforeEach(() => {
  // root-level hook that runs before each and every test.
})

afterEach(() => {
  // runs after each test
})

after (() => {
  // runs once all tests are done.
})
```

## Interacting with Elements
Divided into parent, child, and dual commands

### Parent Commands
Begin a new chain of commands.

cy.visit() - Visits a page
cy.get() - Gets an element
cy.request() - Sends a request
cy.exec() - Executes a command as a terminal command
cy.intercept() - 

### Child Commands
Chained off a parent command, or another child command

cy.get("...").click()
cy.get("...").type()
cy.get("...").find("footer")

### Dual Commands
Can either start a chain or be chained off an existing one.

cy.contains()
cy.screenshot()
cy.scrollTo()
cy.wait()
