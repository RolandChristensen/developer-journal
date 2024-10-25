---
title: "Cypress Beginner Tutorial Part One"
date: 2023-09-28
---

Cypress.io (Cypress) is a stack agnostic browser testing tool that is free and open source.  
It is used for automated end-to-end / integration / scenario testing on the front end.

**Benefits**
* It runs tests inside a genuine browser, so you can verify browser version compatibility.
* It also is able to access the network layer of the application, allowing us to control requests such as simulating failures server-side.
* Handles the visibility of elements as if it were a user out of the box.
* Scripting syntax is easily readable and learned by those with little programming experience.

The following was all performed on Windows 11. 

Prerequisites:
1. Node.js: download and install Node.js from Nodejs.org (v18.16.1 was used during the initial writing of this).
1. NPM (version 9.8.1) npm was installed when installing Node.js.
1. Cypress (version 13.15.0) Installation is done in the tutorial below, so you don't need to install this now, but the tutorial is using this version and is not guaranteed to be compatible as future releases change.
1. JavaScript (ES6) proficiency or the willingness to do the research.
1. Basic knowledge of HTML and browser developer tools (Chrome Dev Tools to be able to read the Document Object Model (DOM) and get elements.

***Notes on this tutorial (Any tutorial really)***
It should be noted that all tutorials quickly become error-prone as new versions of software are introduced.  
I will attempt to keep this up-to-date, but if the versions are not as indicated problems may exist.  
I am following the guides on docs.cypress.io and they are out-of-date as I write this. As always, I follow along and use analytical thinking and a search engine to resolve the discrepancies.

## Initial Setup for Tests

1. Create folder for the Repo
1. Open a terminal in the folder. (CMD, PowerShell, or built in terminal of Visual Studio Code will work)
1. `$ npm init -y` to initialize the directory as an NPM package and generate the package.json file.
1. `$ npm install -D cypress` to install cypress in the directory.
    * The `-D or --save-dev` parameter means the package will appear in the devDependencies of your package.json file.
    * This installs the desktop app and the CLI. You will use the desktop application in your day to day work and the CLI is used for CI testing.
    * At the time of writing this installed version 12.17.3. Future versions could break this tutorial, especially *major* version changes.
1. `$ npx cypress open` to open the cypress desktop application.
    * The first time you run it on Windows, you will need to tell Windows Defender to allow it.
    * The *Launchpad* will open and you will be given the option of selecting **E2E Testing** or **Component Testing**. You can test individual components in isolation or load the entire application for testing.
1. Select the E2E option for the purpose of following along with these steps.
    * Cypress will scaffold out the *cypress* directory with needed folders (*fixtures* and *support*) and configuration files.
    * The UI will display the configuration files for you to inspect if you wish. 
1. Hit the **Continue** button
1. You will be given a choice of browsers to test from.
    * Your choices depend on what browsers you have installed.
1. Select a browser and click the **Start E2E Testing in {Browser}** button
    * A browser window will open with Cypress running inside it.
    * Two cards in the UI will give you the options to **Scaffold example specs** or **Create new spec**. Examples are useful to figure things out and see what cypress looks like.
1. Click the **Create new spec** button to continue.
    * The path that will be created for the new spec file appears in the dialog.
    * Choose a descriptive test name for the spec, but for this tutorial use "test-the-kitchen-sink".
1. Click the **Create spec** button.
    * A folder called e2e is created in the cypress directory with the spec file named "test-the-kitchen-sink.cy.js" inside.
    * The JavaScript to run the test appears in the dialog box. This will be auto-generated JavaScript you will want to replace with useful code to test your app.
    * There are two buttons, the **Okay, run the spec** button and the **+ Create another spec** button. If you have a good idea of the tests you want to run you can keep pushing the **+ Create another spec** to stub out a bunch of test cases in advance, otherwise  push the **Okay, run the spec** button.
1. Click the **Okay, run the spec** button
    * The test will run and pass, because it was a template. This will give you an idea of what a successful run will look like.
    * The UI will display the example.cypress.io page that will help you to write future tests if you read it. You should read it, but save that for another day.
    * Do not navigate away from this page, or figure out how to get back here, because we will expect this page for the next steps.

## Testing Basics
For this tutorial, I am going to use "https://example.cypress.io/" and "https://docs.cypress.io/". This is a good first step. Trying to adapt what you learn here to a site that is meaningful to you will help to make these skills stick.

Cypress desktop should be on the test you wrote earlier with the test "test-the-kitchen-sink.cy.js" on the left and the example page on the right.

1. Hover your mouse over the test file name "test-the-kitchen-sink.cy.js".
    * The hover text says "Open in IDE"
    * You can open the file directly from file explorer also.
1. Click the file name "test-the-kitchen-sink.cy.js"
    * The first time you click this, you will be given the chance to choose your favorite IDE to use. Visual Studio Code is a very popular IDE and I will be using that, but any IDE will work.
1. Select and confirm your IDE
    * The IDE (VS Code) should open the spec file.
    * The tests are written using the Mocha and Chai test framework syntax, so if you are familiar this will be easy. We are going to cover the syntax here regardless, so skim if you are already familiar.

The example code, at the time of writing, is:
```javascript
describe('template spec', () => {
  it('passes', () => {
    cy.visit('https://example.cypress.io')
  })
})
```
* Where *describe()* is used to organize your tests and make them easier to read. A well written description (*describe()*) will be appreciated by the rest of your team. The *context()* statement is an alias for *describe()* and can be used synonymously. 
* The parameters of *describe(string, function())* are the string description "template spec" and an anonymous function using arrow (lambda) notation: "() => { ... }".
* The *it()* is used to specify a single test and has another equivalent alias *specify()* if you prefer.
* The parameters of *it(string, function())* are the string description of the test and the anonymous function that is the action (*act* or *when*) we are testing. Combining the *describe* and the *it* will create a complete description of the test. This is a dumb one that simply says "template spec passes", but we will write better ones.
* The `cy.visit('https://example.cypress.io')` is a cypress command defined in the [Cypress API](https://example.cypress.io/api/). Anything beginning with cy.* will be a Cypress command.
* There are a lot of examples on the internet that will suggest the following improvement over hard coding the URL in the tests, which is to add the base URL in the "cypress.config.js" file. 

```javascript
const { defineConfig } = require('cypress')

module.exports = defineConfig({
  e2e: {
    baseUrl: 'http://localhost:8484',
  },
})
```

This is only a slight improvement and we will skip to the better example for an enterprise testing solution with multiple environments.

### Set Base URL in Environment Specific Configuration Files
Examples exist everywhere on the internet setting a bad *example*. :)  
Use a base URL in an environment specific configuration file for use in the `cy.visit()` or `cy.request()` commands. Do not hard code full URLs.

1. Create a new file in the root folder called "local.settings.json".
2. For now the only content to add is the base URL. Add `"baseUrl": "https://docs.cypress.io/"` to the file. Ordinarily, the local environment would point to some port on localhost, but for this tutorial we are using a production app for ease of use.
```json
{
  "baseUrl": "https://docs.cypress.io/"
}
```
In real life examples, we would create a "local.settings.json", "dev.settings.json", "stage.settings.json", and "prod.settings.json" for environment testing. This is a bare minimum for modern enterprises and there may be more environments. These files would contain environment specific data. The base URL is just the start.

3. For learning, create a second file in the root folder called "prod.settings.json".
4. Add the following to the file:
```json
{
  "baseUrl": "https://example.cypress.io/"
}
```
5. Open the "cypress.config.js" file found in the root directory of your cypress test folder. (This was created in step 1 of the [Initial Setup for Tests](#initial-setup-for-tests) section. It will look similar to this:

```javascript
const { defineConfig } = require("cypress");

module.exports = defineConfig({
  e2e: {
    setupNodeEvents(on, config) {
      // implement node event listeners here
    },
  },
});
```

6. Add the code shown below in the setupNodeEvents callback code.

```javascript
const { defineConfig } = require("cypress");

module.exports = defineConfig({
  e2e: {
    async setupNodeEvents(on, config) {
      // The environment will default to 'local' if you do not set the environmentName when starting cypress
      // Start cypress using: $ npx cypress open --env environmentName={dev|stage|prod}
      const environmentName = config.env.environmentName || 'local' 
      const environmentFilename = `./${environmentName}.settings.json`
      const settings = require(environmentFilename)
      if (settings.baseUrl) {
        config.baseUrl = settings.baseUrl
      }
      if (settings.env) {
        config.env = {
          ...config.env,
          ...settings.env
        }
      }

      return config
    },
  },
});
```

7. Save the file.
8. Go back to the "test-the-kitchen-sink.cy.js" file and change the `cy.visit('https://example.cypress.io')` to `cy.visit('/')` and save the file.
    * You should see the URL defined in the "local.settings.json" file in the cypress browser URL field after it gets done refreshing.
9. To test the "prod.settings.json" we need to close Cypress. Switch to the Cypress Desktop application and click the **Close** button.
    * This will close the Cypress browser
10. Close the Cypress Desktop application by clicking the 'X'.
11. Start Cypress again using the prod environment by using the following command: `$ npx cypress open --env environmentName=prod`
    * You could add any number of environment files with the naming convention of "environmentName.settings.json"
    * Running the appropriate environment by using the `$ npx cypress open --env environmentName={environmentName}` command.
12. Click the E2E Testing button.
13. Select your browser and click the Start E2E Testing in Chrome button.
    * The Cypress browser will open with the URL in the "prod.settings.json" file displayed in the URL field of the browser.
14. Close everything out and start again using the "local.settings.json" file.
  
These environment specific files will hold environment specific values. Do not use this file for anything that does not change from environment to environment.

### Testing Redirects
Frequently we expect someone typing a base URL into the browser will be redirected to another page. The next couple of tests will verify the page title and URL are what we expect when the redirect happens.
First we will follow good testing practice and change the `describe()` and `it()` to a good description of the test.
1. Change the first parameter of the `describe()` function to `describe('Test the redirect from the base URL' () => {`
1. Change the first parameter in the `it()` to `it('has the redirected page title', () => {`
1. Use the `cy.title()` command to get the page title displayed in the browser tab (`document.title` in JavaScript or `<head><title>Title<title><head>` in HTML).

Modify the "test-the-kitchen-sink.cy.js" file to test the title of the page. Add the code below to test the title.
```javascript
describe('Test the redirect from the base URL', () => {
  it('has the redirected page title', () => {
    cy.visit('/')

    cy.title().should('equal', 'Why Cypress? | Cypress Documentation')
  })
})
```

`should()` is the way cypress handles assertions. In this case, we are asserting that the title equals "Why Cypress? | Cypress Documentation" as seen using dev tools or hovering over the browser tab.  
The `cy.title().should('equal', 'Why Cypress? | Cypress Documentation')` line is a basic assertion when navigating to a page to ensure you are on the correct page.  
We could also use *contains* in the first parameter to indicate that the title is not an exact match, but only contains a value. Example: `cy.title().should('contains', 'Why Cypress?')`

If you haven't already, check what the Cypress window looks like to make sure the test has passed as expected.  
A best practice is to double check your tests by making sure you can make it fail, by either changing the actual application under test or the test to see it fail.  
Change the expected text by deleting a character or adding one.  
**Note**: the test will not immediately fail, but will instead retry for a while before giving up. This is a good thing, because page loads will vary depending on the application and we should not expect instantaneous results.

### Getting the URL of the page
This will build on the previous test to verify that the URL in the browser is the redirect URL.
1. Add a second `it()` to the `describe()` function with the description as shown. `it('has the redirected page URL', () => { })`
1. Add the `cy.visit('/')` command as in the previous test.
1. Use `cy.url()` to get the url in the browser. We then add the assertion that the redirected URL is the expected result. `cy.url().should('include', '/guides/overview/why-cypress')`

`should('include', '..')` is another way to say that the URL *contains* the expected text.

The full `describe()` looks like this now: 
```javascript
describe('Test the redirect from the base URL', () => {
  it('has the redirected page title', () => {
    cy.visit('/')

    cy.title().should('equal', 'Why Cypress? | Cypress Documentation')
    cy.title().should('contains', 'Why Cypress?')
  })
  it('has the redirected page URL', () => {
    cy.visit('/')
    
    cy.url().should('include', '/guides/overview/why-cypress')
  })
})
```

One could say that the `cy.visit('/')` command in the second `it()` is redundant, but this is an example of the important principle of *Test Isolation*.  
**Tests should always be able to be run independently from one another and still pass.**

As before, if you haven't already, check what the Cypress window looks like to make sure the test has passed as expected.  
Again, a best practice is to double check your tests by making sure you can make it fail, by either changing the actual application under test or the test to see it fail.

### Back to the Kitchen Sink
Before continuing, let's change the base URL back to "https://example.cypress.io". Try this on your own to test yourself and build stronger memories before going back to [Set Base URL in Environment Specific Configuration Files](#set-base-url-in-environment-specific-configuration-files).

Lets clean up the previous spec file "test-the-kitchen-sink.cy.js" for the new URL. Replace the contents with the following:
```javascript
describe('Test the Querying Page', () => {
  it('has the appropriate heading', () => {
    cy.visit('/')
  })
})
```

Finally, let's rename the spec file to make it specific to the test.  
1. Rename it to "test-querying-page.cy.js" in file explorer or from Visual Studio Code.  
1. Go back to Cypress and you will see an error that the test could not be found.  
1. Refresh the page using the browser refresh button and all will be fine again.

As you should be able to tell from the description, we are now going to learn how to query the page for elements. Specifically, we are going to find the heading (h1) on a specific page and test the text is what we expect.

### Querying Page for Elements
Use the `cy.get()` command to get elements on the page. The parameter used in the get is a string that will allow you to find elements by tagname, class, attributes, links, id, and many other ways.

#### Querying By Tag Name and Testing Inner Text
1. Change the `cy.visit('/')` to `cy.visit('/commands/querying')`.
    * This is a relative path that is tacked onto the end of the base URL.
    * Notice that it is smart enough to handle the slashes '/'. If you leave off or include leading slashes in the `visit()` or leave off or include the slash in the base URL, it will resolve the URL as expected.
1. Add a line to get the heading (h1) on the page: `cy.get('h1')`
    * To get tag names simply use the tag name in the quotes. Of course duplicates could exist on a page, but for this page there is only one *h1*.
1. Next we get the text property of the element using the `.invoke()` method to read the text of the heading. Add the method to invoke text: `cy.get('h1').invoke('text')`
    * `invoke()` is a connector function, one that invokes a function of the element we got and then we assert something based on the results of the invocation.
    * The `invoke('text')` command gets the inner text of the element (`<h1>inner text<h1>` in HTML or `h1Element.innerText` in JavaScript)
1. Next add the assertion that the text should equal what we see on the page: `cy.get('h1').invoke('text').should('equal', 'Querying')`

#### Querying By Class, Typing Text, and Testing Value Attribute
To get an element by class name you precede the value by the '.' operator like `cy.get('.action-email')`. Frequently, there will be a list of class names in the class attribute in the HTML but only use one of the class names to find the element.  
Example: the HTML looks like `<input class="form-control action-email">`, but we only use the "action-email" class to find the element.

1. Create a new spec file titled "test-actions-page.cy.js".
1. Add a `describe()` to the file with the description "Test the Actions Page".
1. Add a `it()` to the `describe()` with the description "accepts email address".
1. Add code to navigate to "https://example.cypress.io/commands/actions" remembering the base URL is already set.
5. Now add a constant string to hold the email address `const emailAddress = 'testy.testerson@email.com'`.
    * Define constants for any strings that you are expecting to use more than once in the code. This makes the code more maintainable since you will only need to change the value once and it will be updated everywhere it is used.
    * This will complete the set up of the test (Given / Arrange)
    * The file should look like the following so far:

```javascript
describe('Test the Actions Page', () => {
  it('accepts email address', () => {
    cy.visit('commands/actions')
    const emailAddress = 'testy.testerson@email.com'
  })
})
```

6. Add `cy.get('.action-email')`, which will find the email field on the page.
1. Append `type(emailAddress)` to the `get()` in order to type the email address constant value in the input email field.
1. Append `should('have.value', emailAddress)` to the `type()` in order to assert that the input email fields *value* property is set to the email entered.
1. Save the file.

The final file will look like this:
```javascript
describe('Test the Actions Page', () => {
  it('accepts email address', () => {
    cy.visit('commands/actions')
    const emailAddress = 'testy.testerson@email.com'

    cy.get('.action-email').type(emailAddress).should('have.value', emailAddress)
  })
})
```

This is a good place to stop. I think a user has enough information to begin to automate things on their desktop and begin UI testing using Cypress. Future posts will build on what was learned here.

