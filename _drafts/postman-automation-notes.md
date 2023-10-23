---
title: "Postman Automation Notes"
date: 2023-09-28
---

Notes while automating a new workflow that should be compiled and organized.

# Authentication / Authorization
Each request will need to use some sort of authentication. Postman has an *Authorization* tab for every collection, folder, or request. It is best to set the authorization type for the services under test in the highest level collection or folder needed and set the *Authorization Type* drop down to *Inherit auth from parent* value. This makes development quicker and easier. In complex end-to-end tests that require multiple services, it may be necessary to include authorization in the request itself, but should be avoided whenever possible. For testing a single service you will put the *Authorization* in the collection folder and the *Pre-request Script* of the collection will be used to get a bearer token, if used. 

## Bearer Tokens
Enter the call to get a bearer token in the **Pre-request Script** of the collection or folder. 

The rookie mistake is to include a request to get the JWT and then copy and paste it into a request. The intermediate mistake is to create a variable that is populated by manually running the request, which is a big improvement. This is also a good approach for documenting an API. Another intermediate mistake is to create a script to get a token, but we get a token every time a request is run wasting time and resources each call. Now, we are talking about automated tests, so we write a script to get the token and only when the previous token is about to expire.

### Environment Variables
The client ID and client secrets should be kept in the environment variables to keep them secure. 

Client ID can be considered like a user name and client secret is like a password. When you log into a system, the user name is exposed, but the password will be hidden behind asterisks or bullet points. Following this assumption, we set the variable *Type* field to *default* for client ID and populate the *Initial value* of the variable to the client ID. 

***Important note for security***: The client secret must not be entered in the *Initial value* field. If you enter it in the *Initial value* field it will be shared with everyone who has access to your workspace and will be exported with the environment. For security, each user will enter the client secret manually into the *Current value* field before executing the tests. The secrets should be safely stored in a password vault somewhere, with access restricted to valid users. The variable *Type* should be set to *secret* for client secret in order to obfuscate the value with bullet points. 

You may decide to keep both the client ID and client secret obfuscated and secured for an additional layer of security.

### Example script in Pre-request of Collection / Folder / Request
```Javascript
// This will get a token if the old one has expired.
if (pm.environment.get("service_name_bearer_token_expiration")) {
    return;
}

console.log("Need a new token for service name.");

const tokenRequest = {
    url: pm.environment.get("BearerTokenUrl"),
    method: 'POST',
    header: {
        'Content-Type': 'application/x-www-form-urlencoded'
    },
    body: {
        mode: 'urlencoded',
        urlencoded: [
            {key: "grant_type", value: "client_credentials"},
            {key: "audience", value: "optional_audience"},
            {key: "scope", value: "optional_scope"},
            {key: "client_id", value: pm.environment.get("service_name_client_id")},
            {key: "client_secret", value: pm.environment.get("service_name_client_secret")}
        ]
    }
};
pm.sendRequest(tokenRequest, (error, response) => {
    if (error) {
        console.log(error);
        return;
    }
    const token = response.json().access_token;
    if (token == null) {
        throw Error("The token was null. This usually means you need to set the client secret in the environment variables.")
    }

    pm.environment.set("service_name_bearer_token", token);
    var tokenExpiration = Date.now().valueOf() + response.json().expires_in * 1000 - 5000; 
    pm.environment.set("service_name_bearer_token_expiration", tokenExpiration);
});
```

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

