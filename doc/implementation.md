# Implementation

## Achievement 1: Trello workflow implemented

### Receiving webhooks on AWS

AWS API Gateway seems to be the proper solution to receive webhooks on AWS. So I start by having a look at this.

- Create a new API on AWS API Gateway.
- Set a CloudWatch Logs role ARN in settings to enable logging:
  - in IAM, create a new role using the _Amazon API Gateway_ predefined role type,
  - select the provided policy,
  - when done, display the role and copy the ARN,
  - back into API Gateway settings, paste the ARN.
- Define an endpoint for Trello webhooks:
  - create a "/trello" resource,
  - create an _ANY_ action using a _Mock_ integration type.
- Create a "production" stage:
  - enable CloudWatch Logs, at log level _INFO_, logging full requests/responses data.
- Deploy the API to the "production" stage.
  - Display the "production" stage and the invoke URL. Copy the invoke URL to setup the Trello webhook (next step).

### Configuring a Trello webhook to the AWS endpoint

- **Looking at how to configure a Trello webhook:**
  - create an application token,
  - get an user token,
  - perform an API call to Trello to add the webhook.
- **Creating an application token:**
  - Get the app key [here](https://trello.com/app-key) and start with a developer user token.
  - Use the sandbox to get the board's ID and create a webook for it (do not forget to add the `/trello` suffix to the invoke URL copied before).

### Trigger a Lambda function when a webhook is received

- Create a new Lambda function:
  - select _API Gateway_ as the integration,
  - select the created API endpoint,
  - select _Open_ for security (this is technical debt, we will have to implement the Trello authenticity signature verification).
- Going on with [ClaudiaJS](https://claudiajs.com) to build Lambda functions to ease deployment and handle local testing.
