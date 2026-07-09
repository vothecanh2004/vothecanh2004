---
title: "Workshop Overview"
date: 2024-01-01
weight: 1
pre: " <b> 5.1. </b> "
---

# Introduction

### Introduction to WebQuiz Application
**WebQuiz** is a multi-tenant, real-time interactive online quiz application (similar to Kahoot) that is highly scalable, secure, and cost-optimized. The main goal of this workshop is to guide you step-by-step to build and deploy a complete **Serverless architecture** on **AWS** by combining **on-demand** compute services, auto-scaling **NoSQL databases**, asynchronous **event routing**, and low-latency **bidirectional messaging**.

### Workshop Overview
In this workshop, you will practice building the system with the following core components:

*   **Web Frontend Tier:** Static Page Application (**SPA**) hosted on **Amazon S3** (**Static Website Hosting**), globally distributed via **Amazon CloudFront** using **Origin Access Control (OAC)** to secure the bucket from direct public access.
*   **Authentication & Security Tier:** User registration and secure host login managed via **Amazon Cognito User Pools**. **JSON Web Tokens (JWT)** issued by Cognito are verified by **API Gateway** to secure backend endpoints.
*   **Communication & API Gateway Tier:** Establish **API Gateway HTTP API** for RESTful requests (quiz/room management) and **API Gateway WebSocket API** to maintain persistent **real-time bidirectional communication** with low latency between the host and players.
*   **Compute Tier (API Compute):** Program and configure **AWS Lambda** functions (**Node.js runtime**) to execute backend logic **on-demand** (quiz CRUD, room management, WebSocket connections/message routing).
*   **Database Tier:** Utilize **Amazon DynamoDB** NoSQL database in **On-demand** capacity mode to store user data, quizzes, connections, and live game states, eliminating the need for expensive **Redis** clusters.
*   **Asynchronous Event Routing Tier:** Use an **Amazon EventBridge Custom Event Bus** (`webquiz-dev-game-events`) to route real-time game events (answer submission, game ending) asynchronously to background Lambda workers (scoring, results saving).
*   **Monitoring & Dashboard Services:** Configure **Amazon CloudWatch** logs for error tracking, metrics for performance, **CloudWatch Alarms** for system alerts, and build a **CloudWatch Dashboard** for real-time system observability.