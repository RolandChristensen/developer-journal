---
title: "Cypress Automation Notes"
date: 2023-10-30
---

Notes while I work to be fleshed out better later.

# Authentication (Get a bearer token first and reuse in all tests, unless it has expired.)

# API Testing
`$ npm install cypress-plugin-api`

Use cy.api() instead of cy.request().

# Data Driven Tests
Passing in a *.csv file of data or a *Theory* in xUnit.

## Looping through CSV in the same ***it*** block.
`$ npm install neat-csv@5.2.0` to install the Node package. Version 5 is required. Later versions will fail to run.

```javascript
let csvData

describe('Test using *.csv file', () => {
  before(() => {
    cy.fixture('file-name.csv').then((data) => {
      csvData = data
    })
  })

  it('Output rows of CSV', () => {
    for (let i = 0; i < csvData.length; i++)
    {
      cy.log("Iteration " + i + " First column: " + csvData[i]['name_of_column_1']
      cy.log("Iteration " + i + " Second column: " + csvData[i]['name_of_column_2']
    }
  }
}
```

## Looping through CSV and run multiple ***it*** blocks on each row in the file

cypress.config.js

```javascript
const { defineConfig } = require('cypress')

const fs = require('fs')
const path = require('path')
const neatCSV = require('neat-csv')

module.exports = defineConfig({
  e2e: {
    async setupNodeEvents (on, config) {
      // Load the CSV file
      const filename = path.join(__dirname, 'cypress/fixtures/filename.csv')
      const text = fs.readFileSync(filename, 'utf8')
      const csv = await neatCSV(text)

      // Make available in the environment via Cypress.env("csvData")
      config.env.csvData = csv

      return config
    },
  },
})
```

Spec file:

```javascript
const csvData = Cypress.env('csvData')

describe('Test reading a csv file', () => {
  
  csvData.forEach((row) => {
    it(`Run example for ${row['column_1']} ${row['column_2']}`, () => {
      cy.log(`First column: ${row['column_1']}`)
      cy.log(`Second column: ${row['column_2']}`)
    })

    it('Run a second test on same data', () => {
      cy.log("Test 2 First column: " + row['column_1'])
      cy.log("Test 2 Second column: " + row['column_2'])
    })
  })
})
```

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

## Examples
cy.get("h1").contains("Some Text").click() // finds the H1 with "Some Text" and clicks it.

cy.contains("SomeText").type("I want to type this") // Finds the *label* "SomeText" and types into that field

cy.get("form").submit() // Gets the form on the page and triggers the submit.

cy.contains("Some success message") // This will validate the successful submission.

## Selectors
Cypress will automatically calculate a unique selector to use targeted element. 
Primary selectors:
* data-cy
* data-test
* data-testid

Secondary selectors:
* id
* class
* tag
* attributes
* nth-child

### Cypress.SelectorPlayground API
You can change the priority of selectors from the above to a custom strategy. (id's if your devs are conscientious?)

###
Fragile selectors are those tied to styling, auto generated, or anything subject to change frequently.

Looking for the visible text is a much better option, but duplicates can still exist and the text can change.

data-cy, data-test, data-testid are isolated from changes related to styling and page html. 
This is developer created and should not change if devs follow best practices. (I thought *id* was the same?)
This seems designed to ensure vendor lock in with Cypress.
It also ensures developers will need to do extra work every time the develop, but it is also making sure they follow best practices that allow us automators have an easier time.

cy.get("[data-cy=value]").click

## Assertions
Cypress bundles Chai assertion library and extensions Sinon, and jQuery.

### Chai
BDD assertions
Should are chainers, used on objects obtained from the DOM or elsewhere.
Expect is like assert, a parent expression

Chainable Getters are used to make expressions more readable: 
* to
* be
* been
* is
* that
* which
* and
* has
* have
* with
* at
* of
* same

### Chai-jQuery - DOM assertions from jQuery
cy.get().should(...), cy.contains().should(...), etc..
expect($el).to.contain('text')

.should('have.attr', 'something')
expect($el).to.have.attr('key', 'value')

.should('be.visible')
expect($el).to.be.visible

.should('include', 'string value')
.should('include', 2) // for an array
expect([1,2,3]).to.include(2)

.should('have.length', 100) // array or string length
expect('test').to.have.lengthOf(4)

.should('not.exist')
expect($el).not.to.exist

### Sinon-Chai
Chainers used on cy.stub() and cy.spy()

### Multiple Assertions
.should('...', '...').and('...', '...').and(...)

## Cypress Retry
The claim is its built in retry feature. Of course, you can easily implement this in any tool with upfront cost.
* Cypress retries DOM queries
    * .get()
    * .find()
    * .contains()
* Does not retry .click() or other commands because these change the state of the application.
* Only the last command before an assertion is retried, so chaining can get you in trouble.

## Aliases
.as("alias name for future reference") -> .get("@alias name")
Looks good on the test runner as well.

In a beforeEach (or a POM based file), create get statements with aliases you can reference in the rest of the describe().
`cy.get("[data-cy=thatButton]").as("ThatButton")`

Then use it like:
cy.get("@ThatButton")

## API Testing
Cypress Testing
* Stub Responses - Mock response from servers
* Use Server Response - Integration Test

Use stub responses for testing the front end code.
Use server responses for testing the back end code not handled by unit tests and to ensure the integration works.

The Cypress test runner will display a section for routes and let you know when it has been stubbed.

Don't use stubbed responses for server-side rendering architecture. (PHP that delivers HTML to be rendered on the page.)

Avoid using stubs for critical paths, such as logins.

## Cypress Intercept
cy.intercept() 
Used to spy and stub.
Defines the response body, status code, headers, and other items.
You can also stub responses from a fixture file *.json

Using an aliased route we can await a response for slow responses.
You can declaratively wait for requests and responses using cy.wait()

### Spying a Request
Used during front end testing to wait for the request to finish before moving on to other statements.
cy.intercept("METHOD", "/endpoint", "hostname") // passively listen for matching routes and apply aliases to them without manipulating the request or its response

It assumes the GET method and the host is also defined in the setup, so both can be left off. 
cy.intercept("/endpoint") // works for any GET on the expected host.

cy.intercept("/endpoint").as("getSomeData")
cy.get("@button").click()
cy.wait("@getSomeData") // Waits before moving to the next line of code

cy.wait("@getSomeData", { requestTimeout: 60000 }) // waits even longer for long running endpoints

### Pattern Matching URLs
Use ** or * to match any characters in the URL
Use regex to match characters

### Stubbing a Request
Use to customize responses from a server

```
cy.intercept(
  {
    method: 'GET',   // Route all GET requests
    url: '/users/*', // that have a URL that matches
  []                 // and return an empty array
  }
).as('someResponse')
```

Return specific status codes, bodies, and headers.
```
cy.intercept("/someEndpoint",
  {
    statusCode: 404,
    body: "404 Not Found",
    headers: {
      "x-not-found": "true"
    }
  }
).as('someResponse')
```

Using fixtures in the fixtures folder.
```
cy.intercept('POST', 'someEndpoint', { fixture: 'response.json' })
```

### Waiting for Multiple responses
```cy.wait(["@aliasOne", "@aliasTwo"])```

### Request Command
cy.request() will try a request once and will time out at the default time.

This can be used for backend testing, but we really should use a tool meant for that.

```cy.request("method", "url").as("alias")```

```cy.get("@alias").should()```

## Cypress Screenshots and Video
`$ npx cypress run` run tests headless and creates a videos folder that holds recordings of the test run.

### Video
The configuration options in cypress.json allow: 

* Trash videos before run.
* Compression
* Folder
* Capture video or not
* Upload to dashboard on pass or not. Fails are usually more useful, but...

### Screenshot
Cypress creates screenshots in a screenshots folder automatically when a test fails, when using the runner.

Configuration for screenshots is also available in the cypress.json

`cy.screenshot()` takes a screenshot in code.

## Custom Commands
cypress/support/commands.js

```javascript
// This will return an element by data-cy.
Cypress.Commands.add("dataCy", (value) => {
    return cy.get('[data-cy=${value}]')
});
```

```javascript
Cypress.Commands.add("doABunchOfWork", () => {
    cy.visit("\")
    cy.get("...")
    ...
});
```

## Cross Browser Support
There is a full list online.
Cypress finds the browsers you have installed and gives you a list based on that.

`cypress run --browser firefox` to choose browser.

## Plugins
Plugins can be written to modify or extend Cypress or to use Node outside of the browser.

* There are a lot available. Find a list on docs.cypress.io/plugins.
* You can create your own custom code to extend Cypress or tap into Node.
* Manipulate DBs

Place plugins you want in the cypress/plugins/index.js file.

```javascript
// on is the event handler bit
// config comes from the Cypress config file
module.exports = (on, config) => {
  on('<event>', (arg1, arg2) => {
    // plugin stuff here
  }
}
```

## TypeScript
You can configure Cypress to use Typescript by editing cypress/tsconfig.json file.

TypeScript helps us avoid Types or other runtime errors you will find with JavaScript.
