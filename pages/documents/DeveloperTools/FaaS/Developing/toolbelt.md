---
pagename: Toolbelt
keywords:
sitesection: Documents
categoryname: "Developer Tools"
documentname: LivePerson Functions
subfoldername: Developing with FaaS
permalink: liveperson-functions-development-toolbelt.html
indicator: both
redirect_from:
  - function-as-a-service-developing-with-faas-toolbelt.html
---

As mentioned in the [Getting Started document](function-as-a-service-getting-started.html), we offer you access to our `lp-faas-toolbelt` Node.js module, which is a language-specific utility library for lambdas.

Currently, the Toolbelt offers the following methods:

| Method | Description |
| :------- | :----- |
| Toolbelt.SFClient() | Returns a Salesforce Client, that is configured to work with the FaaS Proxy. |
| Toolbelt.HTTPClient() | Returns a HTTP Client, that is configured to work with the FaaS Proxy. |
| Toolbelt.LpClient() | Returns the LivePerson (LP) Client. This is a wrapper for the [HTTP Client](liveperson-functions-development-toolbelt.html#http-client). It simplifies the usage of LivePerson APIs by providing automatic service discovery as well as taking care of the authorization. |
| Toolbelt.SecretClient() | Returns an Secret Storage Client, that is configured to work with the FaaS Secret Storage. |
| Toolbelt.ConversationUtil() | Returns a Conversation Util instance. |
| Toolbelt.GDPRUtil() | Returns a GDPR Util instance. Provides GDPR related functionality, such as replacing files of a conversation. |

Here are usage example, which are taken out of the official templates:

### Salesforce Client:

Salesforce Client that is based on [jsforce](https://www.npmjs.com/package/jsforce) for connecting LivePerson Functions to any Salesforce system.

```javascript
const { Toolbelt } = require("lp-faas-toolbelt");
const sfClient = Toolbelt.SFClient(); // for API docs look @ hhtps://jsforce.github.io/

//This will establish a connection with SF. And leverage Access Token / Refresh Token to login
const con = sfClient.connectToSalesforce({
  loginUrl: "https://test.salesforce.com",
  accessToken: "PROVIDE_YOUR_ACCESS_TOKEN", //Obtain it from Secret Store
  refreshToken: "PROVIDE_YOUR_REFRESH_TOKEN" // Obtain it from Secret Store
});

con.query(query, function(err, queryResult) {});
```

### HTTP Client:

HTTP Client that is based on [request-promise](https://www.npmjs.com/package/request-promise) for opening external HTTP connections.

```javascript
const { Toolbelt } = require("lp-faas-toolbelt");
//Obtain an HTTPClient instance from the Toolbelt
const httpClient = Toolbelt.HTTPClient(); // For API Docs look @ https:/www.npmjs.com/package/request-promise

const URL = "https://www.liveperson.com/";

httpClient(URL, {
	method: "GET", //HTTP VERB
	headers: {}, //Your headers
	simple: false, //IF true => Status Code != 2xx & 3xx will throw
	json: true, // Automatically parses the JSON string in the response
	resolveWithFullResponse: false //IF true => Includes Status Code, Headers etc.
})
.then(response ==> {
	...
})
```

### LivePerson Client:

The LivePerson (LP) Client is a wrapper for the [HTTP Client](liveperson-functions-development-toolbelt.html#http-client). It simplifies the usage of LivePerson APIs by providing automatic service discovery as well as taking care of the authorization. To use the LP Client you need to [whitelist](liveperson-functions-development-whitelisting-domains.html) `api.liveperson.net`.

 Every LivePerson API has a service name. This is documented in the respective page on [developers.liveperson.com](https://developers.liveperson.com). The [Messaging Interactions API](messaging-interactions-api-overview.html) for instance has the service name `msgHist`. The LP Client expects the LpService name as the first argument. This can be done by using our `LpServices` enum or by manually providing the service name as a string. Each of the API-domains related to the `LpServices` enum is whitelisted in LivePerson Functions by default. (s. [Whitelisted Domains](liveperson-functions-development-whitelisting-domains.html#domains-whitelisted-by-default) for more information)

Additionally, most of the LivePerson API calls need authorization. The LP Client takes care of that by automatically creating the respective HTTP headers. In order to perform this authorization, the LP Client uses credentials from an API-key. Each account using Liveperson Functions has an API-key called `lp-faas-default` by default, which is visible in Live Engage. This API-key is able to authenticate to the following APIs:

<table style="width: 100%;">
<thead>
<tr>
<th>API</th>
<th>Read/ Write access?</th>
</tr>
</thead>
<tbody>
<tr>
<td><a href="data-access-api-overview.html">Data Access API</a></td>
<td>Yes</td>
</tr>
<tr>
<td><a href="messaging-interactions-api-overview.html">Messaging Interactions API</a></td>
<td>Yes</td>
</tr>
<tr>
<td><a href="operational-realtime-api-overview.html">Operational Realtime API</a></td>
<td>Yes</td>
</tr>
<tr>
<td><a href="personal-data-deletion-api-overview.html">Personal Data Deletion API</a></td>
<td>Read only</td>
</tr>
<tr>
<td><a href="users-api-overview.html">Users API</a></td>
<td>Read only</td>
</tr>
<tr>
<td><a href="skills-api-overview.html">Skills API</a></td>
<td>Read only</td>
</tr>
<tr>
<td><a href="agent-groups-api-overview.html">Agent Groups API</a></td>
<td>Read only</td>
</tr>
<tr>
<td><a href="profiles-api-overview.html">Profiles API</a></td>
<td>Read only</td>
</tr>
<tr>
<td><a href="lobs-api-overview.html">LOBs API</a></td>
<td>Read only</td>
</tr>
<tr>
<td><a href="workdays-api-overview.html">Workdays API</a></td>
<td>Read only</td>
</tr>
<tr>
<td><a href="visit-information-api-overview.html">Visit Information API</a></td>
<td>Yes</td>
</tr>
<tr>
<td><a href="engagement-attributes-api-overview.html">Engagement Attributes API</a></td>
<td>Yes</td>
</tr>
<tr>
<td><a href="validate-engagement-api-overview.html">Validate Engagement API</a></td>
<td>Yes</td>
</tr>
<tr>
<td><a href="ivr-engagement-api-overview.html">IVR Engagement API</a></td>
<td>Yes</td>
</tr>
<tr>
<td><a href="predefined-content-api-overview.html">Predefined Content API</a></td>
<td>Read only</td>
</tr>
<tr>
<td><a href="automatic-messages-api-overview.html">Automatic Messages API</a></td>
<td>Read only</td>
</tr>
<tr>
<td><a href="predefined-categories-api-introduction.html">Predefined Categories API</a></td>
<td>Read only</td>
</tr>
<tr>
<td><a href="unified-automatic-messages-api-overview.html">Unified Automatic Messages API</a></td>
<td>Read only</td>
</tr>
<tr>
<td><a href="agent-status-reason-api-overview.html">Agent Status Reason API</a></td>
<td>Read only</td>
</tr>
</tbody>
</table>


Currently, only APIs that use [API Key](guides-gettingstarted.html) authorization are supported.

#### Using APIs not covered in default API-key/Whitelisting

If you need to access an API which is not covered by the default API-key/Whitelisting, you need to perform the following steps:
 1. Create and maintain the API Key credentials
 * Create an API Key as described [here](guides-gettingstarted.html). (the [Messaging Interactions API](messaging-interactions-api-overview.html) for instance needs the permission `Data -> Engagement History / Messaging Interactions`)
 * Create a new [secret](liveperson-functions-development-storing-secrets.html) of the type JSON to save the API Key credentials. The JSON has to have the structure as displayed below. Provide the name of the created secret when using the LP Client (see Sample Usage below for an example).

    ```javascript
    {
        "consumerKey": "...", // App Key
        "consumerSecret": "...", // Secret
        "token": "...", // Access token
        "tokenSecret": "...", // Access token secret
    }
    ```

 2. Whitelist the API you want to use
  * Retrieve the domain for the service you want to use as described [here](agent-domain-domain-api.html).
  * Go to `Settings -> Domain Whitelist`
  * Add the domain

**Sample Usage**

```javascript
const { Toolbelt, LpServices } = require("lp-faas-toolbelt");

// Obtain an LpClient instance from the Toolbelt
const lpClient = Toolbelt.LpClient();

/**
 * Same options as the HTTPClient
 * For possible options look @ https://github.com/request/request#requestoptions-callback
 */
const options = {
    method: "POST",
    body: {
        conversationId
    },
    json: true,
    /** insert the name of your custom authentication secret here - Use only
    *   if you want to bypass the default API-key and understand the ramifications.
    */
    appKeySecretName: 'my-custom-secret-name'
}

lpClient(
  LpServices.MSG_HIST, // LP service name
  `/messaging_history/api/account/${process.env.BRAND_ID}/conversations/conversation/search`, // endpoint of the service
  options // options
)
.then(response => {
	...
})
```

### Secret Storage Client:

Storage Client that is able to read & update secret values. The following methods exist:

**SecretClient.readSecret**

Searches the secret that belongs to the provided key. Will raise an error if there is no secret for the provided key.

<table style="width: 100%;">
<thead>
<tr>
<th >Parameter</th>
<th >Description</th>
</tr>
</thead>
<tbody>
<tr>
<td >key</td>
<td >Name of the secret</td>
</tr>
</tbody>
</table>

<table style="width: 100%;">
<thead>
<tr>
<th >Returns</th>
<th >Description</th>
</tr>
</thead>
<tbody>
<tr>
<td >secretEntry</td>
<td >Object with properties <code>key</code> &amp; <code>value</code></td>
</tr>
</tbody>
</table>

**SecretClient.updateSecret**

Updates the secret with the provided update entry.

<table style="width: 100%;">
<thead>
<tr>
<th >Parameter</th>
<th >Description</th>
</tr>
</thead>
<tbody>
<tr>
<td >secretEntry</td>
<td >Object with properties <code>key</code> &amp; <code>value</code></td>
</tr>
</tbody>
</table>

<table style="width: 100%;">
<thead>
<tr>
<th >Returns</th>
<th >Description</th>
</tr>
</thead>
<tbody>
<tr>
<td >secretEntry</td>
<td >Created entry</td>
</tr>
</tbody>
</table>

**Sample Usage**

```javascript
// import FaaS Toolbelt
const { Toolbelt } = require("lp-faas-toolbelt");
// obtain SecretClient from Toolbelt
const secretClient = Toolbelt.SecretClient();
// this is how you can access your stored secret
secretClient
  .readSecret("my_Secret-Key")
  .then(mySecret => {
    // Fetching the secret value
    const value = mySecret.value;
    // you can also update your secret e.g. if you received a new OAuth2 token
    mySecret.value = "nEw.oaUtH2-tOKeN!!11!";
    return secretClient.updateSecret(mySecret);
  })
  .then(_ => {
    callback(null, { message: "Successfully updated secret" });
  })
  .catch(err => {
    console.error(`Failed during secret operation with ${err.message}`);
    callback(err, null);
  });
```

### Conversation Util

The Conversation Util allows to perform conversation related methods, which are listed below. Authorization is configured during instance creation. This Util works for **Messaging-Use Cases only**!

#### Get Conversation By ID

This method retrieves a conversation from the [Messaging Interactions API](messaging-interactions-api-methods-get-conversation-by-conversation-id.html). It expects a conversation ID and returns a `Promise` that resolves to a conversation object.

**Sample Usage**

```javascript
  // import FaaS Toolbelt
  const { Toolbelt } = require("lp-faas-toolbelt");

  // Create instance
  const conversationUtil = Toolbelt.ConversationUtil();

  // Get conversation
  conversationUtil.getConversationById(conversationId)
  .then(conversation => //TODO: react on the response)
  .catch(err => //TODO: React to error);
```

#### Scan Conversation For Keywords

This method scans a conversation that has been retrieved with `getConversationById()` (see method above) for messages containing certain keywords. Those keywords can be freely determined and are case insensitive.

**Sample Usage**

```javascript
// import FaaS Toolbelt
const { Toolbelt } = require("lp-faas-toolbelt");

// Create instance
const conversationUtil = Toolbelt.ConversationUtil();

// Get conversation
const conversation = await conversationUtil.getConversationById(conversationId);

// Determine Keywords
const keywords = ["Keyword", "awesome"];

// Scan Conversation for Keywords
const scannerResult = conversationUtil.scanConversationForKeywords(
  conversation,
  keywords
);
```

**Sample Result**

The method collects every message which contains a keyword in an array. It retrieves a timestamp, information on who sent the message and adds a tag detailing the keyword for which the message has been selected. If one message contains more than one keyword it will appear as often in the array. (see example underneath)

<table style="width: 100%;">
<thead>
<tr>
<th>Attribute</th>
<th>Description</th>
<th>Type/Value</th>
</tr>
</thead>
<tbody>
<tr>
<td>message</td>
<td>The whole message which is containing at least one keyword</td>
<td>string</td>
</tr>
<tr>
<td>sentTimestamp</td>
<td>Timestamp (Current Unix epoch time in milliseconds) when the message was sent</td>
<td>number</td>
</tr>
<tr>
<td>sentBy</td>
<td>Who the conversation was sent by</td>
<td>string</td>
</tr>
</tbody>
</table>

```javascript
[
  {
    message: "Will we use Keywords in this conversation?",
    sentTimestamp: 1560764690328,
    sentBy: "Consumer",
    tag: "keywordRef:Keyword"
  },
  {
    message: "We definitely will, because keywords are awesome!",
    sentTimestamp: 1560764734592,
    sentBy: "Agent",
    tag: "keywordRef:Keyword"
  },
  {
    message: "We definitely will, because keywords are awesome!",
    sentTimestamp: 1560764734592,
    sentBy: "Agent",
    tag: "keywordRef:awesome"
  }
];
```

### GDPR Util

This method provides GDPR related functionality, such as deleting transcripts of a conversation. This Util works for **Messaging-Use Cases only**!

#### Replace files of a conversation

<div class="important">This will remove all files and transcripts of a conversation permanently! Contact your Account Manager to get access.</div>

This method replaces all files of a conversation from LivePerson's [file storage](file-sharing-file-sharing-for-web-messaging.html#introduction). It expects a conversation, the credentials for the file storage, a callback for filtering files and replacement image.

**Sample Usage**

```javascript
// import FaaS Toolbelt
const { Toolbelt } = require("lp-faas-toolbelt");

// set file storage credentials (get from Account Manager)
const fileStorageCredentials = {
  username: '...',
  password: '...'
}

// Create instance
const conversationUtil = Toolbelt.ConversationUtil();

// Create GDPR Util instance
const gdprUtil = Toolbelt.GDPRUtil();
const shouldReplace = (filePath) => ... // filter here by returning boolean
const replacementFile = {
  body: Buffer.from('...', 'base64'), // create file from base64
  contentType: 'image/png',
};

// Get conversation and replace files
conversationUtil.getConversationById(conversationId)
  .then(conversation => gdprUtil.replaceConversationFiles(
        conversation,
        fileStorageCredentials,
        shouldReplace, //(optional) defaults to (path) => true
        replacementFile, //(optional) defaults to a black 1px*1px png
  ))
  .then(replacedFiles => //TODO: react on the response)
  .catch(err => //TODO: React to error);
```
