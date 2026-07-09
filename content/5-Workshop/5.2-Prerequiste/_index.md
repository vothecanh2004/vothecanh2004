---
title: "Prerequisites"
date: 2024-01-01
weight: 2
pre: " <b> 5.2. </b> "
---

# Prerequisites & Setup

Before starting this workshop, make sure you have prepared the following tools and environment:

### 1. AWS Account & Access

*   **AWS Account:** You need an active AWS Account. If you don't have one, sign up at [aws.amazon.com](https://aws.amazon.com/).
*   **Administrator Permissions:** Ensure your IAM user or role has administrative rights to create Cognito User Pools, DynamoDB Tables, Lambda Functions, IAM Roles, EventBridge rules, API Gateway APIs, S3 Buckets, and CloudFront Distributions.

---

### 2. Local Environment Tools

*   **Node.js & npm:** Install Node.js (version 20.x is recommended for compatibility). Check your version by running:
    ```bash
    node -v
    npm -v
    ```
*   **AWS CLI:** Install the AWS CLI and configure it with your credentials:
    ```bash
    aws configure
    ```
    Ensure you specify a default region (such as `us-east-1`).
*   **Git:** Install Git to manage deployment repositories if needed.
*   **Code Editor:** Visual Studio Code or any preferred IDE.

---

### 3. API & WebSocket Testing Tools

*   **Postman or curl:** Required for testing the REST HTTP API Gateway endpoints (creating quizzes, rooms).
*   **wscat:** A command-line tool for testing WebSocket connections. Install it globally using npm:
    ```bash
    npm install -g wscat
    ```
    You will use this tool to simulate real-time players and host communication in the terminal.