---
title: "Week 9 Worklog"
date: 2024-01-01
weight: 1
chapter: false
pre: " <b> 1.9. </b> "
---



### Week 9 Objectives:

* Differentiate between HTTP REST APIs and WebSocket APIs, and master connection state management via API Gateway, Lambda, and DynamoDB.
* Understand the core concepts of Amazon SQS queues for system decoupling and asynchronous answer submission processing.
* Gain hands-on proficiency in provisioning WebSocket APIs, integrating SQS/RDS, and validating multi-player real-time quiz synchronization.

### Tasks to be deployed this week:
| Day | Task | Start Date | End Date | Reference Documentation |
| --- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------ | --------------- | ----------------------------------------- |
| Mon | - Research WebSockets <br> - Compare HTTP REST APIs and WebSocket APIs <br> - Understand how Lambda handles WebSockets and stores `ConnectionId` in DynamoDB | 15/06/2026   | 15/06/2026      | |
| Tue | - Analyze game flows: Room Creation, Joining Room, Game Start, Question Switching, Countdown Timer, and Leaderboard Updates <br> - Design the DynamoDB data schema | 16/06/2026   | 16/06/2026      | |
| Wed | - Research core Amazon SQS concepts <br> - Determine when to leverage SQS within the Quizizz system | 17/06/2026   | 17/06/2026      | |
| Thu | - Create a WebSocket API in API Gateway <br> - Develop Lambda functions to handle Connect/Disconnect events <br> - Persist `ConnectionId` into DynamoDB <br> - Create an SQS Queue to process Answer Submissions <br> - Save quiz results into Amazon RDS post-processing | 18/06/2026   | 18/06/2026      | |
| Fri | - **Hands-on:** <br>&emsp; + Create a live quiz room and enable multi-user joining <br>&emsp; + Test real-time question broadcasting via WebSockets <br>&emsp; + Handle real-time player answer submissions <br>&emsp; + Push real-time leaderboard and score updates to all active players | 19/06/2026   | 19/06/2026      


### Key Outcomes Achieved in Week 9:

* Clearly differentiated the operational mechanics between HTTP REST APIs and WebSocket APIs, and mastered how AWS Lambda manages connection states using `ConnectionId` inside Amazon DynamoDB.
* Established an optimized data schema on DynamoDB and fully grasped complex real-time game loops: Room creation/joining, game start, countdowns, question transitions, and live leaderboard refreshes.
* Developed a deep understanding of Amazon SQS queues and successfully applied them to decouple the system, handling asynchronous answer submission streams efficiently before saving data to RDS.
* Provisioned and fully configured an API Gateway WebSocket API integrated with targeted Lambda handlers for `$connect` and `$disconnect` routes.
* Successfully built and tested the multi-player online examination workflow: Broadcasting questions, collecting student answers, and computing/updating active match scores in real time for all players.