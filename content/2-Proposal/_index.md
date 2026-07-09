---
title: "Proposal"
date: 2024-01-01
weight: 2
chapter: false
pre: " <b> 2. </b> "
---

# WebQuiz - Real-time Quiz System
## Serverless AWS Solution for Real-Time Synchronized Quiz Gameplay

### 1. Executive Summary
**WebQuiz** is a real-time quiz platform designed to allow hosts to create quizzes, open live rooms, invite players using a room PIN, and run synchronized quiz sessions. The system is specifically engineered using a serverless architecture on AWS to handle high concurrency and provide low-latency, real-time interactive gameplay without the need for managing always-on infrastructure.

---

### 2. Concept & Objectives (Problem Statement)

#### 2.1 Context & Problem
*   **System Purpose:**
    **WebQuiz** is a lightweight, scalable, and cost-efficient platform designed for interactive learning, classroom activities, online training, workshops, and team engagement sessions.
*   **Target Audience:**
    The platform serves two primary groups: hosts (teachers, instructors, event/workshop organizers, and corporate teams) who manage and run the quiz sessions, and participants (students, event attendees, and employees) who join and play the quiz in real time from their devices.
*   **Problem Statement:**
    Traditional quiz platforms often suffer from a lack of real-time communication between host and players, delayed score updates, and high infrastructure costs due to always-on servers. **WebQuiz** resolves these issues by using a Serverless architecture on AWS, incorporating WebSocket API for instant two-way communication and DynamoDB for low-latency state management, ensuring a seamless and budget-friendly experience.

#### 2.2 Specific Goals
*   **Desired Output:**
    * A fully serverless cloud infrastructure on AWS for quiz management and real-time gameplay.
    * A comprehensive Monitoring system: CloudWatch Dashboards, centralized Logs, and automated Alarms for tracking system performance.
    * A detailed end-to-end deployment guide for the serverless application.
*   **Success Criteria:**
    * The system supports a large volume of concurrent players in one room without throttling or timeout.
    * Response time from player submitting answer to receiving score update is near-instant in real-time.
    * Total monthly operational cost for the development environment is optimized to a bare minimum.

#### 2.3 Program Alignment
*   **FCAJ / AWS Use-case:**
    The project simulates interactive real-time educational and training environments, applying core serverless and application integration services on AWS (Lambda, API Gateway, DynamoDB, Cognito, EventBridge, CloudFront, S3) covered in the cloud curriculum.
*   **Cloud Focus:**
    The solution focuses on Serverless efficiency, High Availability, Security, and Scalability, perfectly aligning with modern cloud application architecture and cost-optimization principles.
*   **Proposed Solution:**
    Implement a serverless real-time architecture on AWS:
    1.  **Frontend:** Single Page Application (SPA) hosted on **Amazon S3** and globally delivered via **Amazon CloudFront** for high availability and fast content delivery.
    2.  **API Gateway (HTTP API):** Provides REST endpoints for HTTP operations (quiz CRUD, room management) protected by **Cognito User Pool** JWT authentication.
    3.  **API Gateway (WebSocket API):** Establishes persistent two-way connections between hosts, players, and backend for low-latency, real-time communication during gameplay.
    4.  **Compute Tier (AWS Lambda):** Executes backend business logic (CRUD operations, connection handlers, score calculation, game state updates) on-demand, scaling automatically without server management.
    5.  **Database & Event Integration:** Stores all application data in **Amazon DynamoDB** with on-demand capacity, and uses **Amazon EventBridge** to handle asynchronous event-driven workflows (e.g., calculation of scores, saving final results).
    6.  **Security & Identity:** Utilizes **Amazon Cognito** for secure user registration and login, and **AWS IAM Roles** with the Least Privilege principle.
*   **Benefits and ROI (Return on Investment):**
    *   **Resource & Cost Optimization:** The serverless pay-as-you-go model ensures that infrastructure costs remain at a bare minimum based on actual usage, scaling down to zero when the application is idle.
    *   **High Reliability & Low Latency:** Persistent WebSocket connections enable sub-second updates. DynamoDB offers microsecond responses, while AWS Lambda ensures high availability across multiple Availability Zones.
    *   **Strong Security:** Strict IAM roles, Cognito JWT authentication, and S3 Origin Access Control (OAC) keep resources protected.

---

### 3. Solution Architecture

The system utilizes a serverless event-driven architecture on AWS to maximize scalability and cost efficiency.

#### 3.1 Architecture Diagram

![System Architecture](/images/2-Proposal/webquiz_architecture.png)

#### 3.2 AWS Services Details

| Category | AWS Service | Selection Rationale & Configuration |
| :--- | :--- | :--- |
| **Compute** | AWS Lambda | Runs Node.js 20 runtime to execute backend logic on-demand (quiz CRUD, connection handlers, score calculation), automatically scaling up and down based on invocation volume. |
| **Storage** | Amazon S3 & CloudFront | **Amazon S3** stores frontend static assets. **Amazon CloudFront** caches and distributes frontend globally with HTTPS, using Origin Access Control (OAC) to restrict direct S3 bucket access. |
| **Database** | Amazon DynamoDB | Serverless NoSQL database utilizing On-Demand capacity mode. Stores all persistent data: users, quizzes, questions, rooms, connections, game state, and final results. |
| **Networking** | Amazon API Gateway | Deploys two APIs: HTTP API for low-latency REST endpoints (quiz CRUD and room management) and WebSocket API for persistent real-time bidirectional communication. |
| **Security** | Amazon Cognito, IAM, WAF | **Cognito** manages user signup, login, and JWT token issuance. **IAM Roles** enforce the least-privilege principle. **WAF** (optional) protects endpoints from web exploits. |
| **Monitoring** | Amazon CloudWatch | Collects application logs, gathers performance metrics, triggers email/SNS alerts on errors, and visualizes system status via CloudWatch Dashboards. |
| **Messaging** | Amazon EventBridge | Routes events asynchronously to decouple core actions (e.g., answer submissions trigger background score calculations and result saving). |
| **CI/CD** | AWS CodePipeline, CodeBuild | (Optional) Automates testing and software deployment from commit to production. |

---

### 4. Technical Implementation

#### Implementation Phases
The project is divided into 2 specific phases over a 2-week period:
1.  **Foundation Setup, Database & API Development (Week 1):** Setup AWS account, configure Cognito User Pool, design and provision DynamoDB tables, and develop Lambda functions for quiz CRUD and room management. Build API Gateway HTTP API and WebSocket API with connection handlers.
2.  **Event Integration, Frontend Deployment & Monitoring (Week 2):** Configure EventBridge to route answer submissions, calculate scores, and save results. Build and deploy frontend SPA to S3 with CloudFront global delivery, establish CloudWatch logs and alarms, and perform end-to-end testing.

#### Prerequisites
*   **Resources & Tools:** AWS Account with required IAM permissions, configured AWS CLI, Node.js runtime, npm/yarn, API testing tools (Postman), and WebSocket CLI tools (wscat).
*   **Basic Knowledge:** Serverless Architecture, AWS Lambda, DynamoDB NoSQL design, API Gateway (HTTP & WebSocket), EventBridge, and frontend-backend integration.

---

### 5. Timeline & Milestones
*   **Week 1: Foundation Setup, Database & API Development**
    *   Define project scope, configure AWS CLI, and prepare developer IAM roles.
    *   Set up Amazon Cognito User Pool for Host authentication.
    *   Design and provision DynamoDB tables (quizzes, questions, rooms, connections, game states, results).
    *   Develop core Lambda functions for quiz CRUD operations and room creation logic.
    *   Create API Gateway HTTP API (REST) and WebSocket API, configuring routes, integration, and Cognito JWT Authorizer.
    *   Test bidirectional connection stability between host and players.
*   **Week 2: Event Integration, Frontend Deployment & Delivery**
    *   Configure Amazon EventBridge rules and event buses for gameplay routing.
    *   Develop `score-calculator` and `game-results-saver` Lambda functions to handle asynchronous scoring and results.
    *   Build the frontend Single Page Application (SPA) and deploy static assets to S3.
    *   Configure Amazon CloudFront CDN and integrate frontend with HTTP and WebSocket API endpoints.
    *   Establish CloudWatch Logs, alarms for errors, and build operational dashboards.
    *   Perform end-to-end load tests, resolve bugs, and complete final documentation.

---

### 6. Budget Estimation
Below is the detailed monthly cost estimation of the system based on the AWS Pricing Calculator (configured for the WebQuiz architecture in the Workshop environment):

| No. | AWS Service | Configuration Details | Monthly Cost |
| :--- | :--- | :--- | :--- |
| 1 | AWS Lambda | 1 million requests/month, 256MB memory, 1s execution time | ~ $5.00 |
| 2 | Amazon DynamoDB | On-demand capacity, 1 million RCU/WCU per month | ~ $10.00 |
| 3 | Amazon API Gateway | HTTP API: 1 million reqs; WebSocket: 1 million messages | ~ $5.00 |
| 4 | Amazon CloudFront | 10GB data transfer out per month, CloudFront Free Tier | ~ $5.00 |
| 5 | Amazon S3 | 5GB Standard Storage, 100,000 GET/PUT requests | ~ $0.50 |
| 6 | Amazon EventBridge | 1 million custom events matched and processed | ~ $1.00 |
| 7 | Amazon CloudWatch | 10GB log storage, 10 custom metrics and dashboards | ~ $3.00 |
| **Total** | **Estimated Monthly Total** | | **~ $29.50** |

*Note:* The cloud infrastructure cost can be further optimized in development environments by:
*   Staying within the AWS Free Tier limits for AWS Lambda, S3, and CloudFront.
*   Optimizing DynamoDB reads and writes, keeping payload sizes small to reduce capacity usage.
*   Reducing CloudWatch log retention duration to a short period instead of storing infinitely to minimize storage cost.

---

### 7. Risk Assessment

#### Risk Matrix

| Identified Risk | Probability | Impact | Mitigation Strategy |
| :--- | :--- | :--- | :--- |
| **WebSocket disconnection mid-game** | Medium | High | Use DynamoDB TTL to auto-expire stale connections; enable clients to reconnect using room PIN and unique player ID. |
| **DynamoDB throttling during spikes** | Medium | Medium | Optimize table index patterns, implement local/global index optimization, and configure Auto Scaling for provisioned capacity if needed. |
| **Lambda timeout during broadcasts** | Medium | High | Design lightweight and single-responsibility Lambda handlers, and batch connection deliveries or use queues. |
| **Incorrect score calculation logic** | Low | High | Store raw player answer history temporarily in DynamoDB to allow re-evaluation and recalculation if an error occurs. |
| **Cognito config error blocking host login** | Medium | Medium | Maintain an isolated staging environment and establish test credentials with local mock options for offline development. |
| **Security risk from over-privileged IAM** | Medium | High | Apply the AWS principle of Least Privilege strictly using inline IAM policies and customer-managed roles. |

#### Contingency Plan
*   **Failed Connection Recovery:** If a user loses their WebSocket connection mid-game, they can easily reconnect using the active room PIN and their existing player ID without losing accumulated points or progress.
*   **DynamoDB Scale Out:** If DynamoDB performance hits capacity limits, administrators can rapidly toggle the database from On-Demand to Provisioned mode with automated write/read scaling settings.
*   **Broadcast Batching:** If Lambda executions timeout while sending updates to large groups of users, the broadcast logic will automatically chunk connection lists and process them asynchronously in small batches.

---

### 8. System Processing Flows & Technical Strengths

The system utilizes a serverless event-driven architecture to decouple user-facing operations from backend workflows. The core processing flows and architectural strengths include:

#### 8.1 Core Processing Flows

*   **Authentication Flow:**
    Integrated with Amazon Cognito for identity management. API requests are authenticated using JWT validation middleware, ensuring secure and stateless authorization between the client and backend services.
*   **Room Management & Connection Lifecycle:**
    When a host opens a room, a unique room PIN is generated and saved in DynamoDB. Players join by verifying the PIN. WebSocket connection IDs are tracked in a connection registry table, enabling persistent state mapping.
*   **Asynchronous Event Processing:**
    Using Amazon EventBridge, resource-intensive gameplay events (such as answer submissions) are decoupled from the WebSocket connection handlers. Once an answer is received, it triggers EventBridge which routes the payload to the score-calculator.
*   **Leaderboard Updates & State Cleanup:**
    As scores are updated by the calculator, messages are broadcast to all connected WebSocket clients to refresh their leaderboards in real time. Upon game completion, a cleanup workflow purges connection data and updates the final results table.

#### 8.2 Architectural Strengths

*   **Scalability and Decoupling:**
    The Frontend, REST API, and WebSocket layers function independently. AWS Lambda scaling handles demand spikes seamlessly without server sizing administration, and EventBridge decouples real-time response from analytical write actions.
*   **Data Consistency:**
    DynamoDB transactions and standard consistent reads ensure that quiz rooms, player connections, and quiz questions remain synchronized during active sessions, preventing duplicate entries.
*   **Modern Frontend Stack:**
    Built as a Single Page Application (SPA), the client integrates directly with AWS SDK and WebSocket endpoints to render local state changes and leaderboard ranks smoothly.

---

### 9. Expected Outcomes
*   **Technical Improvements:**
    *   High-performance serverless scaling that effortlessly handles concurrent user spikes without manual infrastructure sizing.
    *   Low-latency bidirectional messaging using API Gateway WebSocket, keeping quiz updates and response times near-instant in real-time.
    *   Clean separation of business workflows and asynchronous processes by routing game events through EventBridge.
*   **Long-Term Value:**
    *   Establishes a reusable, production-ready AWS Serverless architecture template for real-time transactional and event-driven applications.
    *   Minimizes ongoing maintenance efforts and operational cloud budgets while ensuring high availability.
    *   Provides an extensible system architecture ready to integrate with third-party Learning Management Systems (LMS) or support advanced multi-game features.
