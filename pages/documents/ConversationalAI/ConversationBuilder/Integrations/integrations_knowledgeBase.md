---
pagename: Knowledge Base integrations
Keywords:
sitesection: Documents
categoryname: "Conversational AI"
documentname: Conversation Builder
subfoldername: Integrations
permalink: conversation-builder-integrations-knowledge-base-integrations.html
indicator: both
---

Use a Knowledge Base integration when you want to search one of your knowledge bases for articles. You’ll need to create the knowledge base first; for help with that, see [this page](knowledge-base-overview.html).

One use case for this type of integration is within a fallback dialog to funnel user utterances into a knowledge base search. If an appropriate search result is found, it can be displayed; if no results are found, you might then display a "sorry" message or [escalate the conversation](conversation-builder-integrations-liveperson-agent-escalation-integrations.html) to a human agent.

**To add a Knowledge Base integration**

1. Open the bot, and click **Integrations** in the upper-right corner.
2. Configure the integration settings (required fields are marked with asterisks):
    - **Integration Name**: Enter the name of integration. Enter a name that's meaningful (it describes well the integration's purpose), concise, and follows a consistent pattern. This helps with organization, and it makes it easier for bot developers to work with the integration during bot development.
    - **Response Data Variable Name**: Enter the name of the response data variable.
    - **Integration Type**: Select **Knowledge Base**.
    
    <img class="fancyimage" style="width:600px" src="img/ConvoBuilder/integrations_kb.png">
    
    - **Knowledge Base**: Select the knowledge base to search. You can select from all public knowledge bases that exist under your organization.
    - **Methods**:
        - *Phrase Search*: Select this method to use NLU to search for the phrase defined in the request parameter against the articles and their associated "standard" tags. You can define "standard" tags via the Knowledge Base UI or a CSV file import.
        - *Special Tag Search*: Select this method to search for the special tags defined in the request parameter against the special tags associated with the articles. Special tags are different from standard tags. Special tags have a prescribed format; they don't support NLU searches, and they function more like attributes. Typically, special tags are used for returning products or items. You can only define special tags via a CSV file import.
    - **Request Parameters for a Phrase search**:
        - *mode*: 
            - *Intents*: Select this to perform an NLU-based search first, and, *if no results are found*, to perform a text-based search second.
            - *Intents Only*: Select this to perform only an NLU-based search (no text-based search).
            - *All*: Select this to perform only a text-based search (no NLU).
        - *threshold*: The knowledge base uses Natural Language Understanding (NLU) algorithms to match articles to the input phrases, and it assigns a score based on the confidence level of the match: VERY GOOD, GOOD, FAIR PLUS, FAIR, and POOR. Use this field to specify the minimum score that a result must have in order to be returned. You can select from VERY GOOD, GOOD, or FAIR PLUS. The default value is GOOD. If you downgrade the threshold to FAIR PLUS, be sure to test whether the quality of the results meets your expectations.
        - *phrases*:   Enter the phrase for which to search. The default value is {$query}, which represents/stores the last, complete response sent by the consumer.
        - *multipleResults*: Select the number of results to return from the knowledge base, anywhere from one to five. The default value is 1.
    - **Request Parameters for a Special Tag search**:
        - *mode*: Indicates whether to perform an "and" or "or" search using the special tags.
        - *specialTags*: The comma-delimited list of tags for which to search. The default value is {$query}, which represents/stores the last, complete response sent by the consumer.
    - **Transform Result Script**: If applicable, use this section to write JavaScript code that transforms the raw result (typically in JSON format), so you can use the information in the bot's dialog. For more on this, see [Transform an API result](conversation-builder-integrations-integration-basics.html#transform-an-api-result).
    - **Custom Data Fields**: Add the fields that will store the result data in key/value pairs. Users who are tasked with creating bots can use and display this data in interactions by referencing these fields as described [here](conversation-builder-interactions-interaction-basics.html#display-variables-in-interactions).
3. Click Save.