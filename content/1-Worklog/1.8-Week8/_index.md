---
title: "Week 8 Worklog"
date: 2024-01-01
weight: 1
chapter: false
pre: " <b> 1.8. </b> "
---


### Week 8 Objectives:

* Create and configure WebSocket API on AWS API Gateway.
* Set up WebSocket routes ($connect, $disconnect, $default).
* Attach Integrations between WebSocket routes and corresponding Lambda functions.
* Practice testing WebSocket API with wscat tool.


### Tasks to be deployed this week:
| Day | Task | Start Date | End Date | Reference Documentation |
| --- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------ | --------------- | ----------------------------------------- |
| Mon | - Learn about WebSocket API on API Gateway <br> - Understand route selection expression <br>- How Lambda handles WebSocket events | 08/06/2026   | 08/06/2026      | <https://docs.aws.amazon.com/apigateway/latest/developerguide/apigateway-websocket-api.html> |
| Tue | - Create WebSocket API on API Gateway <br>- Configure route selection expression: $request.body.action <br>- Create routes: $connect, $disconnect, $default | 09/06/2026   | 09/06/2026      | <https://docs.aws.amazon.com/apigateway/latest/developerguide/apigateway-websocket-api-create.html> |
| Wed | - Create Integrations for WebSocket routes <br>- Attach $connect route to ws-connect Lambda <br>- Attach $disconnect route to ws-disconnect Lambda <br>- Attach $default route to ws-message Lambda | 10/06/2026   | 10/06/2026      | <https://docs.aws.amazon.com/apigateway/latest/developerguide/apigateway-websocket-api-integrations.html> |
| Thu | - Create dev stage for WebSocket API <br>- Enable auto-deploy <br>- Get WebSocket URL and Connection URL | 11/06/2026   | 11/06/2026      | <https://docs.aws.amazon.com/apigateway/latest/developerguide/apigateway-websocket-api-stages.html> |
| Fri | - **Hands-on:** <br>&emsp; + Install wscat <br>&emsp; + Connect to WebSocket with wscat <br>&emsp; + Test sending messages over WebSocket <br>&emsp; + Check Lambda logs to confirm successful processing | 12/06/2026   | 12/06/2026      


### Key Outcomes Achieved in Week 8:

* Mastered knowledge of WebSocket API on AWS API Gateway and how Lambda handles WebSocket events.
* Successfully created WebSocket API with route selection expression $request.body.action.
* Fully created and configured all WebSocket routes: $connect, $disconnect, and $default.
* Successfully attached Integrations between WebSocket routes and corresponding Lambda functions (ws-connect, ws-disconnect, ws-message).
* Created dev stage for WebSocket API and obtained both WebSocket URL and Connection URL.
* Successfully practiced testing WebSocket API with wscat: connecting, sending messages, and confirming Lambda processes correctly via CloudWatch Logs.
