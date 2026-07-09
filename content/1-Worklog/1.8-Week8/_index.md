---
title: "Week 8 Worklog"
date: 2024-01-01
weight: 1
chapter: false
pre: " <b> 1.8. </b> "
---



### Week 8 Objectives:

* Analyze and design the REST API architecture and data relationship models for the Quiz, Question, and Room entities.
* Master the implementation, routing, and integration of the API Gateway → Lambda → RDS architectural pattern.
* Understand multimedia asset management (Image/Audio) and successfully integrate AWS Lambda with Amazon S3.
* Gain hands-on proficiency in testing and validating all baseline Backend endpoints using Postman.

### Tasks to be deployed this week:
| Day | Task | Start Date | End Date | Reference Documentation |
| --- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------ | --------------- | ----------------------------------------- |
| Mon | - Analyze Quizizz Backend architecture <br> - Identify key modules <br> - Research the API Gateway → Lambda → RDS model <br> - Build a list of REST APIs to be developed | 08/06/2026   | 08/06/2026      | |
| Tue | - Design REST APIs <br> - Design schemas for Quiz, Question, and Room tables <br> - Build the data relationship diagram | 09/06/2026   | 09/06/2026      | |
| Wed | - Create Lambda Functions for Quiz APIs <br> - Configure API Gateway Routes <br> - Connect Lambda to RDS <br> - Develop APIs: Create Quiz, Get Quiz List | 10/06/2026   | 10/06/2026      | |
| Thu | - Build Question CRUD functionalities <br> - Research the process for uploading Image/Audio to Amazon S3 <br> - Integrate Lambda with S3 | 11/06/2026   | 11/06/2026      | |
| Fri | - **Hands-on:** <br>&emsp; + Test APIs using Postman <br>&emsp; + Verify Quiz CRUD operations <br>&emsp; + Verify Question CRUD operations <br>&emsp; + Upload images to S3 <br>&emsp; + Finalize core Backend APIs | 12/06/2026   | 12/06/2026      | 


### Key Outcomes Achieved in Week 8:

* Successfully analyzed the API Gateway → Lambda → RDS architectural model and clearly defined the key modules and required REST APIs for the Quizizz Backend.
* Finalized the REST API specifications and the entity data relationships for the Quiz, Question, and Room structures.
* Successfully developed and established connections between AWS Lambda and RDS, launching core APIs such as Create Quiz and Get Quiz List via API Gateway.
* Completed the CRUD feature set for Questions and integrated Lambda with Amazon S3 to handle image and audio media uploads.
* Thoroughly tested all baseline Backend APIs using Postman, ensuring stable data processing and file attachment flows to S3.