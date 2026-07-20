---
title: "Week 9 Worklog"
date: 2024-01-01
weight: 1
chapter: false
pre: " <b> 1.9. </b> "
---

### Week 9 Objectives:

* Test the full WebSocket flow (start game, next question, submit answer, end game).
* Configure API Gateway.
* Write detailed API documentation.

### Tasks to be Executed This Week:

| Day | Task | Start Date | Completion Date | Documentation Source |
| --- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------ | --------------- | ----------------------------------------- |
| Mon | - Obtain the WebSocket Connection URL from API Gateway. | 2026-06-15 | 2026-06-15 | https://docs.aws.amazon.com/ |
| Tue | - Test the complete WebSocket flow: create room, join room. <br>- Test `START_GAME` action. <br>- Test `NEXT_QUESTION` action. <br>- Test `SUBMIT_ANSWER` action. <br>- Test `END_GAME` action. | 2026-06-16 | 2026-06-16 | https://docs.aws.amazon.com/ |
| Wed | - Configure stage logging for API Gateway. <br>- Audit and optimize API Gateway performance. | 2026-06-17 | 2026-06-17 | https://docs.aws.amazon.com/ |
| Thu | - Write API documentation for HTTP APIs. <br>- Write API documentation for WebSocket APIs. | 2026-06-18 | 2026-06-18 | |
| Fri | - **Hands-on Practice:** <br>&emsp; + Conduct end-to-end integration testing between HTTP APIs and WebSocket APIs. <br>&emsp; + Verify that WebSocket broadcasts messages to all connected clients. | 2026-06-19 | 2026-06-19 | |

### Week 9 Achievements:

* Updated the `WEBSOCKET_ENDPOINT` environment variable.
* Successfully tested the full WebSocket flow: room creation, joining room, `START_GAME`, `NEXT_QUESTION`, `SUBMIT_ANSWER`, and `END_GAME`.
* Ensured seamless integration among HTTP APIs, WebSocket APIs, and AWS Lambda functions.