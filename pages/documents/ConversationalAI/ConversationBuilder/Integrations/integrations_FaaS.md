---
pagename: FaaS integrations
Keywords:
sitesection: Documents
categoryname: "Conversational AI"
documentname: Conversation Builder
subfoldername: Integrations
permalink: conversation-builder-integrations-faas-integrations.html
indicator: both
---

Use tha FaaS integration to invoke a function (lambda) that is deployed to the [LivePerson Functions](liveperson-functions-overview.html) (Function as a Service or FaaS) platform. There are no constraints here; if there is some custom logic (a function) you want to invoke with a bot, you can do it with a FaaS integration.

### To add a FaaS integration

1. Open the automation, and click **Integrations** in the upper-left corner.
2. Configure the integration settings:
    - **Integration Name**: Enter the name of integration. Enter a name that's meaningful (it describes well the integration's purpose), concise, and follows a consistent pattern. This helps with organization, and it makes it easier for bot developers to work with the integration during bot development.
    - **Response Data Variable Name**: Enter the name of the response data variable.
    - **Integration Type**: Select **FaaS**.
    - **Function**: Select the function (`lambda`) to invoke via this integration. You can select from all functions added under your LivePerson account. Each is listed with its status.
    <img class="fancyimage" style="width:500px" src="img/ConvoBuilder/integrations_faaSFunctionList.png">
    The Function list provides an easy pass-through to the Functions UI in case you still need to create or modify the relevant function. Clicking **Create New Function** or <img style="width:20px" src="img/ConvoBuilder/icon_pencilModify.png"> (pencil icon) beside a function name opens a new browser window and prompt you to log into the Functions UI where you can do the work. Once done, return here and click <img style="width:25px" src="img/ConvoBuilder/icon_functionReload.png"> (reload icon) to refresh the Function list if needed.
    - **Function Headers**: Add the necessary data in key/value pairs to pass into the request via the header.
    - **Function Payload**: Enter the payload to pass into the function.
    - **Transform Result Script**: If applicable, use this section to write JavaScript code that transforms the raw result (typically in JSON format), so you can use the information in the bot's dialogs. For more on this, see [Transform an API result](conversation-builder-integrations-integration-basics.html#transform-an-api-result).
    - **Custom Data Fields**: Add the fields that will store the result data in key/value pairs. Users who are tasked with creating automations can use and display this data in interactions by referencing these fields as described [here](conversation-builder-interactions-interaction-basics.html#display-variables-in-interactions).
3. Click **Save**.
