---
title: "Week 10 Worklog"
date: 2024-01-01
weight: 2
chapter: false
pre: " <b> 1.10. </b> "
---


### Week 10 Objectives:

* Final testing for all APIs (HTTP and WebSocket).
* Create complete Postman Collection with all endpoints and WebSocket requests.
* Load test API Gateway to ensure performance.
* Fix API and WebSocket related bugs.


### Tasks to be deployed this week:
| Day | Task | Start Date | End Date | Reference Documentation |
| --- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------ | --------------- | ----------------------------------------- |
| Mon | - Final test all HTTP API endpoints (Quiz CRUD, Question CRUD, Room APIs) <br> - Test error cases (400, 401, 403, 404, 500) <br>- Test rate limiting if configured | 22/06/2026   | 22/06/2026      | https://learning.postman.com/ |
| Tue | - Final test WebSocket API with all actions (START_GAME, NEXT_QUESTION, SUBMIT_ANSWER, END_GAME) <br>- Test WebSocket events (GAME_STARTED, NEXT_QUESTION, ANSWER_RECEIVED, SCORE_UPDATE, GAME_ENDED) | 23/06/2026   | 23/06/2026      |  |
| Wed | - Create complete Postman Collection: <br>&emsp; + Folder for Authentication <br>&emsp; + Folder for Quiz APIs <br>&emsp; + Folder for Room APIs <br>&emsp; + Folder for WebSocket Testing <br>- Add examples for request/response | 24/06/2026   | 24/06/2026      | https://learning.postman.com/ |
| Thu | - Load test API Gateway with many concurrent requests <br>- Check CloudWatch Metrics for latency and error rate | 25/06/2026   | 25/06/2026      | https://docs.aws.amazon.com/ |
| Fri | - **Hands-on:** <br>&emsp; + Fix all bugs found during testing <br>&emsp; + Optimize API performance if needed <br>&emsp; + Export Postman Collection and share with team <br>&emsp; + Write usage guide for Postman Collection | 26/06/2026   | 26/06/2026      


### Key Outcomes Achieved in Week 10:

* Successfully final tested all HTTP API and WebSocket API, including both success and error cases.
* Created complete Postman Collection with scientific structure, including Authentication, Quiz APIs, Room APIs, and WebSocket Testing, with examples.
* Performed load testing and confirmed API Gateway handles many concurrent requests, checked Metrics on CloudWatch.
* Fixed all API and WebSocket related bugs found during testing.
* Exported Postman Collection and wrote usage guide, shared with team for easy testing and development.
