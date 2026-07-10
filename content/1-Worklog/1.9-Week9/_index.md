---
title: "Week 9 Worklog"
date: 2024-01-01
weight: 1
chapter: false
pre: " <b> 1.9. </b> "
---


### Week 9 Objectives:

* Update WEBSOCKET_ENDPOINT environment variable for Lambda functions.
* Test full WebSocket flow (start game, next question, submit answer, end game).
* Configure API Gateway stages and optimize.
* Write detailed API documentation.


### Tasks to be deployed this week:
| Day | Task | Start Date | End Date | Reference Documentation |
| --- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------ | --------------- | ----------------------------------------- |
| Mon | - Get WebSocket Connection URL from API Gateway <br> - Update WEBSOCKET_ENDPOINT for ws-message Lambda <br>- Update WEBSOCKET_ENDPOINT for score-calculator Lambda | 15/06/2026   | 15/06/2026      | <https://docs.aws.amazon.com/apigateway/latest/developerguide/apigateway-websocket-api-data-from-backend.html> |
| Tue | - Test full WebSocket flow: create room, join room <br>- Test START_GAME action <br>- Test NEXT_QUESTION action <br>- Test SUBMIT_ANSWER action <br>- Test END_GAME action | 16/06/2026   | 16/06/2026      | <https://docs.aws.amazon.com/apigateway/latest/developerguide/apigateway-websocket-api-test.html> |
| Wed | - Configure logging for API Gateway stages <br>- Configure throttling if needed <br>- Check and optimize API Gateway performance | 17/06/2026   | 17/06/2026      | <https://docs.aws.amazon.com/apigateway/latest/developerguide/http-api-logging.html> |
| Thu | - Write API documentation for HTTP API (endpoints, request/response, auth) <br>- Write API documentation for WebSocket (actions, events, message format) | 18/06/2026   | 18/06/2026      | <https://swagger.io/> |
| Fri | - **Hands-on:** <br>&emsp; + Test full integration between HTTP API and WebSocket API <br>&emsp; + Verify score-calculator receives events from EventBridge <br>&emsp; + Verify WebSocket broadcasts messages to all clients <br>&emsp; + Finalize API documentation | 19/06/2026   | 19/06/2026      


### Key Outcomes Achieved in Week 9:

* Successfully updated WEBSOCKET_ENDPOINT environment variable for required Lambda functions (ws-message and score-calculator).
* Successfully tested full WebSocket flow: create room, join room, START_GAME, NEXT_QUESTION, SUBMIT_ANSWER, and END_GAME.
* Fully configured logging and optimized performance for API Gateway stages.
* Completed writing detailed API documentation for both HTTP API and WebSocket API, including endpoints, request/response format, authentication, and actions/events.
* Ensured full integration between HTTP API, WebSocket API, Lambda, and EventBridge, with all game flows working correctly.
