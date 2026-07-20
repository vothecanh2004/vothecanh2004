---
title: "Week 7 Worklog"
date: 2024-01-01
weight: 1
chapter: false
pre: " <b> 1.7. </b> "
---

### Week 7 Objectives:

* Create and configure HTTP APIs on AWS API Gateway.
* Create a JWT Authorizer using AWS Cognito to secure API endpoints.
* Create Integrations and Routes for Quiz and Room features.

### Tasks to be Executed This Week:

| Day | Task | Start Date | Completion Date | Documentation Source |
| --- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------ | --------------- | ----------------------------------------- |
| Mon | - Create an HTTP API on API Gateway. <br> - Configure API name and IP address type. | 2026-06-01 | 2026-06-01 | https://docs.aws.amazon.com/ |
| Tue | - Configure CORS settings for the HTTP API. | 2026-06-02 | 2026-06-02 | https://docs.aws.amazon.com/ |
| Wed | - Create a JWT Authorizer. <br> - Link the Authorizer with AWS Cognito User Pool. <br> - Configure the audience (App Client ID). | 2026-06-03 | 2026-06-03 | https://docs.aws.amazon.com/ |
| Thu | - Create Integrations for Lambda functions (`quiz-crud`, `room-management`). <br> - Create corresponding API routes. | 2026-06-04 | 2026-06-04 | https://docs.aws.amazon.com/ |
| Fri | - **Hands-on Practice:** <br>&emsp; + Attach the Authorizer to protected routes. <br>&emsp; + Test HTTP APIs using Postman. <br>&emsp; + Verify that endpoints function properly. | 2026-06-05 | 2026-06-05 | |

### Week 7 Achievements:

* Created an HTTP API on AWS API Gateway.
* Successfully created and configured a Cognito JWT Authorizer.
* Created Integrations between API Gateway and Lambda functions.
* Set up all required routes and attached the Authorizer to routes demanding authentication.
* Successfully tested API endpoints via Postman, ensuring the JWT authentication flow works seamlessly.