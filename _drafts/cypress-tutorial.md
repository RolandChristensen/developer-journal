---
title: "Cypress JavaScript Testing"
date: 2023-08-18
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
1. Cypress (version 12.17.3) Installation is done in the tutorial below, so you don't need to install this now, but the tutorial is using v12.17.3 and is not guaranteed to be compatible as future releases change.
1. JavaScript (ES6) proficiency or the willingness to do the research.
1. Browser developer tools (Chrome Dev Tools to be able to read the Document Object Model (DOM) and get elements.

***Notes on this tutorial (Any tutorial really)***
It should be noted that all tutorials quickly become error-prone as new versions of software are introduced.  
I will attempt to keep this up-to-date, but if the versions are not as indicated, I cannot guarantee results and you will need to follow along with another source.  
I am following the guides on docs.cypress.io and they are out-of-date as I write this and I need to use some analytical thinking and a search engine to resolve the discrepancies.

# Initial Setup for Tests

1. Create folder for the Repo
1. Open a terminal in the folder. (CMD, PowerShell, or built in terminal of Visual Studio Code will work)
1. `$ npm init -y` to initialize the directory as an NPM package and generate the package.json file.
1. `$ npm install -D cypress` to install cypress in the directory.
    * The `-D or --save-dev` parameter means the package will appear in the devDependencies of your package.json file.
    * This installs the desktop app and the CLI. You will use the desktop application in your day to day work and the CLI is used for use in CI testing.
    * At the time of writing this installed version 12.17.3. Future versions could break this tutorial, especially *major* version changes.
1. `$ npx cypress open` to open cypress desktop application.
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
    * Choose a descriptive test name for the spec, but for this tutorial use "test-this".
1. Click the **Create spec** button.
    * A folder called e2e is created in the cypress directory with the spec file named "test-this.cy.js" inside.
    * The JavaScript to run the test appears in the dialog box. This will be auto-generated JavaScript you will want to replace with useful code to test your app.
    * There are two buttons, the **Okay, run the spec** button and the **+ Create another spec** button. If you have a good idea of the tests you want to run you can keep pushing the **+ Create another spec** to stub out a bunch of test cases in advance, otherwise  push the **Okay, run the spec** button.
1. Click the **Okay, run the spec** button
    * The test will run and pass, because it was a template. This will give you an idea of what a successful run will look like.
    * The UI will display the example.cypress.io page that will help you to write future tests if you read it. You should read it, but save that for another day.
    * Do not navigate away from this page, or figure out how to get back here, because we will expect this page for the next steps.

# Testing Basics
For this tutorial, find a site on the internet to test. I am going to use https://www.greatplacetowork.com/ throughout the rest of this tutorial.

Cypress desktop should be on the test you wrote earlier with the test "test-this.cy.js" on the left and the example page on the right.

1. Hover your mouse over the test file name "test-this.cy.js".
    * The hover text says "Open in IDE"
    * You can open the file directly from file explorer also.
1. Click the file name "test-this.cy.js"
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
* The parameters of *describe(string, function())* are the string description "template spec" and an anonymous function using arrow "() => { ... }" syntax.
* The *it()* is used to specify a single test and has another equivalent alias *specify()* if you prefer.
* The parameters of *it(string, function())* are the string description of the test and the anonymous function that is the action (*act* or *when*) we are testing. Combining the *describe* and the *it* will create a complete description of the test. This is a dumb one that simply says "template spec passes", but we will write better ones.
* The `cy.visit('https://example.cypress.io')` is a cypress command defined in the [Cypress API](https://docs.cypress.io/api/). Anything beginning with cy.* will be a Cypress command.
* There are a lot of examples on the internet that will suggest the following improvement over hard coding the URL in the tests, which is to hard code the base URL in the "cypress.config.js" file. 
```javascript
const { defineConfig } = require('cypress')

module.exports = defineConfig({
  e2e: {
    baseUrl: 'http://localhost:8484',
  },
})
```

This is only a slight improvement and we will skip to the better example for an enterprise testing solution with multiple environments.

## Set Base URL in Environment Specific Configuration Files
Examples exist everywhere on the internet setting a bad *example*. :)  
Use a base URL in an environment specific configuration file for use in the `cy.visit()` or `cy.request()` commands. Do not hard code full URLs.

1. Create a new file in the root folder called "local.settings.json".
2. For now the only content to add is the base URL you chose in the [Testing Basics](#tesing-basics) section above. (Mine was "https://www.greatplacetowork.com/" and I add the line `baseUrl: "https://www.greatplacetowork.com/",` in the file as shown below. Ordinarilly, the local environment would point to some port on localhost, but for this tutorial we are making it easy. If you are a dev with an existing local site up and running, feel free to use that instead.
```json
{
  "baseUrl": "https://www.greatplacetowork.com/"
}
```
3. In real life examples, we would create a "local.settings.json", "dev.settings.json", and a "stage.settings.json" for environment testing. This is a bare minimum for modern enterprises and there may be more. These files would contain environment specific data. The base URL is just the start.
4. Open the "cypress.config.js" file found in the root directory of your cypress test folder. (This was created in step 1 of the [Initial Setup for Tests](#initial-setup-for-tests) section. It will look similar to this:
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
5. Add the code shown below in the setupNodeEvents callback code.
```javascript
const { defineConfig } = require("cypress");

module.exports = defineConfig({
  e2e: {
      // The environment will default to 'local' if you do not set the environmentName when starting cypress
      // Start cypress using: $ npx cypress open --env environmentName={dev|stage|prod}
      const environmentName = config.env.environmentName || 'local' 
      const environmentFilename = `./${environmentName}.settings.json`
      const settings = require(environmentFilename)
      if (settings.baseUrl) {
        config.baseUrl = settings.baseUrl
      }
      return config
    },
  },
});
```
6. Save the file.
7. Go back to the "test-this.cy.js" file and change the `cy.visit('https://example.cypress.io')` to `cy.visit('/')` and save the file.
    * You should see the URL defined in the "local.settings.json" file in the cypress browser URL field after it gets done refreshing.

# Pitfalls
The following are things that I found problematic at one point in time or another and the work around used.

## You cannot test multiple browser windows or tabs in the same test
This will require either ordered tests to overcome some problems where you want to set up the tests using another application before running the tests.

Another situation is where you want to validate that clicking a link opens the correct page. You would need to verify this manually to verify the URL is correct.

Testing chat applications, which require two browsers, could be overcome using stubbing.

## You cannot visit two different domains in the same test
Even if using the same browser, you cannot navigate to a different domain (google.com to amazon.com).  
Workaround...
