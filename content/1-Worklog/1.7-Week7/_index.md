---
title: "Week 7 Worklog"
date: 2024-01-01
weight: 1
chapter: false
pre: " <b> 1.7. </b> "
---


### Week 7 Objectives:

* Create and configure HTTP API on AWS API Gateway.
* Configure CORS for HTTP API to support frontend.
* Create Authorizer (Cognito JWT) for API to protect endpoints.
* Create Integrations and Routes for Quiz and Room features.


### Tasks to be deployed this week:
| Day | Task | Start Date | End Date | Reference Documentation |
| --- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------ | --------------- | ----------------------------------------- |
| Mon | - Create HTTP API on API Gateway <br> - Configure API name and IP address type <br>- Create dev stage with auto-deploy enabled | 01/06/2026   | 01/06/2026      | <https://docs.aws.amazon.com/apigateway/latest/developerguide/http-api.html> |
| Tue | - Configure CORS for HTTP API <br>- Set up Access-Control-Allow-Origin, Headers, Methods, Max-Age | 02/06/2026   | 02/06/2026      | <https://docs.aws.amazon.com/apigateway/latest/developerguide/http-api-cors.html> |
| Wed | - Create JWT Authorizer <br>- Link Authorizer to Cognito User Pool <br>- Configure audience (App Client ID) | 03/06/2026   | 03/06/2026      | <https://docs.aws.amazon.com/apigateway/latest/developerguide/http-api-jwt-authorizer.html> |
| Thu | - Create Integrations for Lambda functions (quiz-crud, room-management) <br>- Create routes: /quizzes, /quizzes/{proxy+}, /rooms, /rooms/{pin}, /rooms/{pin}/join | 04/06/2026   | 04/06/2026      | <https://docs.aws.amazon.com/apigateway/latest/developerguide/http-api-develop-integrations-lambda.html> |
| Fri | - **Hands-on:** <br>&emsp; + Attach Integrations to Routes <br>&emsp; + Attach Authorizer to protected routes <br>&emsp; + Test HTTP API with Postman <br>&emsp; + Verify endpoints work correctly | 05/06/2026   | 05/06/2026      


### Key Outcomes Achieved in Week 7:

* Successfully created HTTP API on AWS API Gateway with dev stage and auto-deploy enabled.
* Fully configured CORS for API, allowing frontend to call API from different domain.
* Successfully created and configured JWT Authorizer, linked to Cognito User Pool to protect endpoints.
* Successfully created Integrations between API Gateway and Lambda functions (quiz-crud and room-management).
* Fully created all required routes for the system (quizzes, rooms), while also attaching authorizer to authenticated routes.
* Successfully tested API endpoints via Postman, ensuring JWT authentication flow works correctly.
