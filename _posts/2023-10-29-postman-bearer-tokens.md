---
title: "Postman Bearer Tokens"
date: 2023-10-29
---

Each API request will need to use some sort of authentication. Postman has an *Authorization* tab for every collection, folder, or request. 

![Text Reader Text](https://rolandchristensen.github.io/developer-journal/images/2023-10-28-postman-authentication-collection-folder-authorization-tab.png "Postman Collection Authorization Tab")

It is best to set the authorization type for the services under test in the highest level folder you can use. The collection folder, as shown above is ideal. Then, set the *Authorization Type* drop down of all requests to *Inherit auth from parent* as shown below. This reduces the amount of work that will be needed if/when the authentication method is changed in the future. By default all folders and requests are set to **Inherit auth from parent** meaning you do not need to think about authorization again, once set in the highest level folder you can.

![Text Reader Text](https://rolandchristensen.github.io/developer-journal/images/2023-10-28-postman-authentication-request-inheriting-from-collection.png "Postman Request Inheriting Collection Authorization")

1. Collection: If all requests in a collection use the same type of authorization use the **collection** folder to set all child requests' authorization.
2. Folders / Sub-folders: If you are testing multiple services with different authentication needs, use **folders** / **sub-folders** to set all child requests' authorization.
3. Request: In complex end-to-end tests that require multiple services, it may be necessary to include authorization in the request itself, but should be avoided whenever possible. 

## Bearer Tokens
Enter the call to get a bearer token in the **Pre-request Script** of the collection, folder, or request in the same place as you did above for the **Authorization** tab. 

There are several approaches to getting tokens to authorize a request.
1. ~~Send a request for a token each time.~~ This reduces performance and wastes resources.
2. ~~If a request gets a 401 or 403, then get a token and try again.~~ This is much more performant, but is slower than below and more difficult to code.
3. Save the expiration time returned with the token and proactively get a new token when the token has expired.

### Environment Variables
To get a token you will need several pieces of information, which are generally environment specific. We keep the following in the **Environment Variables** tab.

* Token Server URL
* Client ID
* Client Secret
* Scope (Some services may require *scope* be passed to further restrict access.)
* Audience (Some services may require *audience* be passed to manage access.)

#### Obfuscation
There is a **Type** drop down for each variable that has two options.
1. Default (no obfuscation)
2. Secret (obfuscation, so that the value is displayed as a row of bullet points unless you click the eye icon)

#### Initial / Current Values
***Important note for security***: The client secret must not be entered in the *Initial value* field. If you enter it in the *Initial value* field it will be shared with everyone who has access to your workspace and will be exported with the environment. For security, each user will enter the client secret manually into the *Current value* field before executing the tests. The secrets should be safely stored in a password vault somewhere, with access restricted to valid users. The variable *Type* should be set to *secret* for client secret in order to obfuscate the value with bullet points. 

Client ID can be considered like a user name and client secret is like a password. When you log into a system, the user name is exposed, but the password will be hidden behind asterisks or bullet points. Following this assumption, we set the variable *Type* field to *default* for client ID and populate the *Initial value* of the variable to the client ID. You may decide to keep both the client ID and client secret obfuscated and secured for an additional layer of security.

![Text Reader Text](https://rolandchristensen.github.io/developer-journal/images/2023-10-28-postman-authentication-bearer-tokens.png "Example Postman Environment")

### Example script in Pre-request of Collection / Folder / Request
```Javascript
// This will get a token if the old one has expired.
if (pm.environment.get("service_name_bearer_token_expiration")) {
    return;
}

console.log("Need a new token for service name.");

const tokenRequest = {
    url: pm.environment.get("bearer_token_url"),
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

