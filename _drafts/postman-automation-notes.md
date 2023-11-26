---
title: "Postman Automation Notes"
date: 2023-10-29
---

Notes while automating a new workflow that should be compiled and organized.

# Data Driven Testing and Collection Test Runner Notes
Postman limits the number of collection runs you can use per month. Currently, on the free plan this is 25 runs per month. Paying for the "Professional" plan will give you 250 runs per month and the "Enterprise Essentials" plan (Currently: $49/user) bundled with "Postman for API Test Automation" (An additional $49/user) will give you unlimited runs.  
Before adopting Postman as your solution for running API tests, you should keep this in mind.

## Data Driven Testing
The goal is to reduce the number of requests and code written, while increasing test coverage.

*Data driven tests* use a CSV file of data. For each row in the data file you will reuse the same Postman requests with different values, increasing test coverage with less upfront work and lower maintenance costs.

**Example CSV Data File**:  
ssn, email, income  
911891234, test@email.com, 65000  
000891234, test@email.com, 65000  
811001234, test@email.com, 65000  
..., ..., ...  

### Reading Data File


## General Guidelines for Postman Automation
Reduce duplication of effort, by using data driven tests to reuse folders of requests for multiple tests.

* Reduce the number of requests in your collection, by using variables in your requests and scripts to set those variables for each test.
* Organize requests into folders and subfolders to make use of inheritance using the *Authorization*, *Pre-request Script*, and *Tests* tabs of folders.
    * Use the *Authorization* tab of your collection or folder to authenticate all requests, reducing the amount of upfront work and maintenance needed.
    * Use the *Pre-request Script* tab of collections and folders to set up variables for multiple requests within the folder.
    * Use the *Tests* tab of collections and folders to test things that are repeatedly tested in each request and for setting variables needed for subsequent requests.
* Make use of CSV files for data driven testing, reusing entire folders of requests for different data sets.

## Folder Organization
Using the collection runner for API testing requires good folder organization and use or tabs above. 
The following structure is generally used for testing a single service.

* Test Setup Folder: Contains a request to read a *.csv file for data driven tests.


# Tests
Organize ways to test responses here.

## Assert all elements in an array do not have a certain value
```Javascript
pm.test("Verify no messages in message array are critical", () => {
    const responseJson = pm.response.json()

    _.each(responseJson.messages, (item) => {
        pm.expect(item.severity).to.not.eql('critical')
    })
})
```

## Setting a postman variable and verifying it is not null / empty
```Javascript
var responseJson = JSON.parse(responseBody)
pm.test("Verify request succeeded and field is not null or empty", () => {
    pm.expect(responseJson.someField).not.eq(undefined)
})

pm.collectionVariables.set("SomeVariable", responseJson.someField)
console.log("SomeVariable: " + pm.collectionVariables.get("SomeVariable"))

pm.test(" variables were set correctly", () => {
    pm.response.to.have.status(200)
    pm.expect(pm.collectionVariables.get("SomeVariable")).is.not.undefined
})
```

