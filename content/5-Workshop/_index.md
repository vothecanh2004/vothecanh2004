---
title: "Workshop"
date: 2024-01-01
weight: 5
chapter: false
pre: " <b> 5. </b> "
---

# WebQuiz - Deploying a Real-Time Serverless Quiz Platform on AWS

#### Overview

**WebQuiz** is a real-time quiz application designed to simulate engaging live interactive games (similar to Kahoot). In this workshop, you will build and deploy a fully functional serverless architecture on AWS that is highly scalable and optimized for cost.

You will learn how to:
* Configure **Amazon Cognito** for user registration and secure host login.
* Design and provision **Amazon DynamoDB** tables for persistent storage and dynamic game-state management (replacing Redis).
* Write and configure on-demand **AWS Lambda** functions to process REST operations, handle WebSocket connections, calculate scores, and save final game results.
* Set up **Amazon API Gateway** (both HTTP API and WebSocket API) to provide low-latency endpoints for frontend-backend integration.
* Route game events asynchronously using **Amazon EventBridge** rules.
* Deploy a Single Page Application (SPA) frontend to **Amazon S3** globally distributed via **Amazon CloudFront** using **Origin Access Control (OAC)**.
* Construct **Amazon CloudWatch** alarms, metrics, logs, and dashboards to observe system health.

#### Content

1. [Workshop Overview](5.1-Workshop-overview/)
2. [Prerequisites](5.2-Prerequiste/)
3. [Cognito Authentication](5.3-Cognito/)
4. [DynamoDB Tables](5.4-DynamoDB/)
5. [IAM Permissions & EventBridge](5.5-IAM-EventBridge/)
6. [Lambda REST Functions](5.6-Lambda-REST/)
7. [Lambda Realtime Functions](5.7-Lambda-Realtime/)
8. [API Gateway - HTTP API](5.8-APIGateway-HTTP/)
9. [API Gateway - WebSocket API](5.9-APIGateway-WebSocket/)
10. [Frontend Hosting with S3 & CloudFront](5.10-Frontend-Hosting/)
11. [CloudWatch Monitoring & Dashboard](5.11-Monitoring-Dashboard/)
12. [System Testing](5.12-Testing/)
13. [Resource Cleanup](5.13-Cleanup/)