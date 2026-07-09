---
title: "Week 12 Worklog"
date: 2024-01-01
weight: 2
chapter: false
pre: " <b> 1.12. </b> "
---


### Week 12 Objectives:

* Conduct full end-to-end integration testing between Frontend and Backend, validating the concurrent stability of both REST and WebSocket APIs.
* Execute final acceptance testing for all core functional workflows (Authentication, Quiz creation, Room management, and multiplayer interactions).
* Monitor system health via CloudWatch and SQS to identify, debug, and resolve any remaining software defects.
* Optimize database (RDS) queries, review access control permissions (S3), and complete the final regression check for project sign-off.
### Tasks to be deployed this week:
| Day | Task | Start Date | End Date | Reference Documentation |
| --- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------ | --------------- | ----------------------------------------- |
| Mon | - Verify the connection between Frontend and Backend <br> - Test Amazon Cognito integration with API Gateway <br> - Verify Lambda connections to Amazon RDS <br> - Test Lambda access to Amazon S3 <br> - Verify API Gateway REST and WebSocket APIs operating concurrently | 06/08/2026   | 06/08/2026      | |
| Tue | - Test the Sign Up → Login user flow <br> - Test Quiz creation mechanics <br> - Test Room creation flows <br> - Test multi-player room joining capabilities | 07/08/2026   | 07/08/2026      | |
| Wed | - Monitor Amazon CloudWatch Metrics <br> - Inspect Amazon CloudWatch Logs <br> - Verify Amazon SQS queue processing behaviors | 08/08/2026   | 08/08/2026      | |
| Thu | - Debug and resolve issues identified during testing <br> - Optimize API performance and Database Queries <br> - Double-check S3 bucket access permissions | 09/08/2026   | 09/08/2026      | |
| Fri | - Execute final full-feature regression testing | 10/08/2026   | 10/08/2026      


### Key Outcomes Achieved in Week 12:

* Successfully conducted end-to-end integration testing across the entire system infrastructure, validating seamless connectivity between Frontend, Backend, Cognito, API Gateway, Lambda, RDS, and S3.
* Confirmed system stability while running concurrent workflows for both API Gateway REST and WebSocket APIs in a live production simulation.
* Finalized user acceptance testing (UAT) for all core application modules: Sign Up/Sign In, Quiz authoring, Room configuration, and real-time multiplayer connections.
* Strictly monitored performance metrics and queue operations through Amazon SQS while digging deep into execution traces with Amazon CloudWatch Metrics/Logs.
* Resolved all remaining software bugs, successfully optimized relational database (RDS) queries, and tightened S3 security permissions, ensuring the full-stack architecture is 100% production-ready.