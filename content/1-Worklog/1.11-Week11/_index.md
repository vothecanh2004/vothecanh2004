---
title: "Week 11 Worklog"
date: 2024-01-01
weight: 2
chapter: false
pre: " <b> 1.11. </b> "
---


### Week 11 Objectives:

* Configure WAF (Web Application Firewall) for API Gateway to enhance security.
* Configure detailed API Gateway logging and monitoring.
* Write integration documentation for the entire API system.
* Aggregate and finalize all API and WebSocket related documentation.


### Tasks to be deployed this week:
| Day | Task | Start Date | End Date | Reference Documentation |
| --- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------ | --------------- | ----------------------------------------- |
| Mon | - Learn about AWS WAF and how to integrate with API Gateway <br>- Create WAF Web ACL <br>- Add basic rules (AWSManagedRulesCommonRuleSet) | 27/06/2026   | 27/06/2026      | <https://docs.aws.amazon.com/waf/latest/developerguide/waf-chapter.html> |
| Tue | - Configure detailed logging for API Gateway (log full request/response) <br>- Configure X-Ray tracing for API Gateway to trace requests <br>- Check logs on CloudWatch | 28/06/2026   | 28/06/2026      | <https://docs.aws.amazon.com/apigateway/latest/developerguide/set-up-logging.html> |
| Wed | - Write detailed integration documentation: <br>&emsp; + API architecture overview <br>&emsp; + How to authenticate (Cognito + JWT) <br>&emsp; + HTTP endpoints <br>&emsp; + WebSocket protocol <br>&emsp; + Example code to call APIs | 29/06/2026   | 29/06/2026      | <https://www.mkdocs.org/> |
| Thu | - Aggregate all documentation: <br>&emsp; + API reference <br>&emsp; + Postman Collection docs <br>&emsp; + Environment setup guide <br>&emsp; + Troubleshooting guide | 30/06/2026   | 30/06/2026       |
| Fri | - **Hands-on:** <br>&emsp; + Demo complete API system to team <br>&emsp; + Train team on API usage and Postman Collection <br>&emsp; + Put API system into production-ready state | 31/06/2026   | 31/06/2026      


### Key Outcomes Achieved in Week 11:

* Successfully integrated AWS WAF with API Gateway to protect system from common attacks.
* Fully configured logging and X-Ray tracing for API Gateway, easy to debug and monitor on CloudWatch.
* Completed writing detailed integration documentation, including architecture, authentication, HTTP endpoints, WebSocket protocol, and example code.
* Fully aggregated all API and WebSocket related documentation, ready for handover and maintenance.
* Successfully demoed complete API system to team and ensured system is production-ready.
