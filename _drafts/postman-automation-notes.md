---
title: "Postman Automation Notes"
date: 2023-10-29
---

Notes while automating a new workflow that should be compiled and organized.

# Collection Test Runner Notes
The collection test runner requires an "Enterprise Essentials" plan (Currently: $49/user) bundled with "Postman for API Test Automation" (An additional $49/user). The current limits on collection runs and the number of calls per month make other plans unusable for Enterprises.

## 

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

