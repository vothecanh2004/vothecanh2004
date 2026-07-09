---
title: "Proposal"
date: 2026-04-20
weight: 2
chapter: false
pre: " <b> 2. </b> "
---

# PROJECT PROPOSAL

## Project Name
SyncQuiz - Real-time Quiz System

## Short Description
SyncQuiz is a real-time quiz platform that allows hosts to create quizzes, open live rooms, invite players using a room PIN, and run synchronized quiz sessions. Players can join from their devices, submit answers instantly, receive score updates, and view the leaderboard.

---

## 1. Executive Summary

### Project Introduction
SyncQuiz is a real-time quiz platform that allows hosts to create quizzes, open live rooms, invite players using a room PIN, and run synchronized quiz sessions in real time. Players can join from their devices, submit answers instantly, receive score updates, and view the final leaderboard after the game ends.

### Problem to Solve
- Lack of real-time communication between host and players.
- Difficulty managing live quiz rooms and participant states.
- Delayed score calculation and leaderboard updates.
- Limited scalability when many players join simultaneously.
- High infrastructure costs using always-on servers.
- Complex deployment and maintenance for small teams or student projects.
- Weak monitoring, making it hard to detect errors during live quiz sessions.

### Solution Overview
The project uses a serverless architecture on AWS to reduce infrastructure management, improve scalability, and optimize cost. Frontend is hosted on Amazon S3 and delivered via CloudFront. Users are authenticated via Amazon Cognito. Data is stored in DynamoDB. The system uses API Gateway HTTP API for regular REST operations and API Gateway WebSocket API for live gameplay. AWS Lambda handles backend logic. EventBridge is used to trigger asynchronous tasks.

### Benefits
- Smooth real-time gameplay experience for both hosts and players.
- Serverless architecture minimizes infrastructure operational effort.
- Pay-as-you-go pricing helps minimize cost during low traffic.
- DynamoDB on-demand capacity supports flexible workloads.
- WebSocket API enables instant communication without page refresh.
- EventBridge improves system modularity and separates business logic.
- CloudWatch monitoring helps detect errors, throttling, and performance issues.
- System can be extended later for classrooms, events, online courses, and corporate training.

---

## 2. Idea & Objectives (Problem Statement)

### 2.1 Context & Problem

#### System Purpose
Provide a lightweight, scalable, and cost-efficient platform for interactive learning, classroom activities, online training, workshops, and team engagement sessions.

#### Target Users
- Teachers, instructors in classes and courses.
- Event and workshop organizers.
- Corporate teams organizing internal engagement activities.
- Students, event and workshop participants.

#### Problem to Solve
- Lack of real-time communication between host and players.
- Difficulty managing live quiz rooms and participant states.
- Delayed score calculation and leaderboard updates.
- Limited scalability when many players join simultaneously.
- High infrastructure costs using always-on servers.
- Complex deployment and maintenance for small teams or student projects.
- Weak monitoring, making it hard to detect errors during live quiz sessions.

### 2.2 Specific Objectives

#### Expected Outputs
- **Output 1:** System runs stably, allows hosts to create quizzes, open rooms with PIN, players join, and play real-time quiz.
- **Output 2:** Scores are automatically calculated, leaderboard updates continuously, final results are stored.
- **Output 3:** System is fully monitored, operational cost is optimized, and scalable.

#### Success Criteria
- **KPI 1:** System supports at least 50 concurrent players in one room without throttling or timeout.
- **KPI 2:** Response time from player submitting answer to receiving score update does not exceed 2 seconds.
- **KPI 3:** Total monthly operational cost for development environment does not exceed $30.

### 2.3 Alignment with Program

#### Use-cases
- **Use-case 1:** Teacher creates quiz for quick in-class test, opens room, students join via PIN, take real-time test, view results immediately after.
- **Use-case 2:** Workshop organizer creates quiz to check participants' knowledge, opens multiple rooms simultaneously, views overall leaderboard after workshop.
- **Use-case 3:** Company organizes internal engagement activity, creates fun quizzes, divides into teams, views team leaderboard.

#### Cloud/AWS Alignment
Project is fully aligned with Cloud model and AWS services, especially Serverless architecture:
- No need to manage servers (EC2), minimizing operational effort.
- Pay-as-you-go pricing, optimizing cost.
- Auto-scaling based on traffic.
- Full integration of necessary services (authentication, database, messaging, monitoring...).

#### Proposed Solution
- **Frontend:** Single Page Application (SPA), hosted on Amazon S3 and globally delivered via Amazon CloudFront.
- **Backend:** AWS Lambda functions processing business logic, integrated with Amazon API Gateway.
- **Database:** Amazon DynamoDB (NoSQL) stores all system data.
- **Cache:** Use CloudFront for frontend caching, can extend with Elasticache for hot data caching if needed.
- **Authentication:** Amazon Cognito manages user authentication.
- **Messaging:** Amazon API Gateway WebSocket API for real-time communication, Amazon EventBridge for asynchronous event processing.
- **Monitoring:** Amazon CloudWatch provides logs, metrics, alarms and dashboards.
- **Security:** AWS IAM manages permissions, AWS WAF (optional) for web protection.
- **CI/CD:** Can extend with AWS CodePipeline, CodeBuild, CodeDeploy for automated deployment.

#### Benefits & ROI
- **Cost:** Estimated total monthly cost for development environment is $2 - $30, much lower than using always-on EC2 servers.
- **Performance:** System responds quickly, supports auto-scaling based on traffic.
- **Security:** Good security with Cognito, IAM, HTTPS, OAC for S3.
- **Scalability:** Easy to scale system to support more players, add new features.

---

## 3. Solution Architecture

### 3.1 System Architecture Diagram
(Insert architecture diagram here)

### 3.2 AWS Services Used

| Category | AWS Service | Role |
| --- | --- | --- |
| Compute | AWS Lambda | Execute backend logic without managing servers |
| Storage | Amazon S3, Amazon CloudFront | Host static frontend and global delivery |
| Database | Amazon DynamoDB | Store all application data (users, quizzes, questions, rooms, connections, game state, results) |
| Networking | Amazon API Gateway (HTTP & WebSocket) | Provide REST and WebSocket endpoints for frontend-backend communication |
| Security | Amazon Cognito, AWS IAM, AWS WAF (optional) | Manage user authentication and access permissions |
| Monitoring | Amazon CloudWatch | System monitoring, logs, metrics, alarms, dashboards |
| Messaging | Amazon EventBridge | Process asynchronous event-driven workflows |
| CI/CD | (Optional) AWS CodePipeline, CodeBuild, CodeDeploy | Automated testing and deployment |

---

## 4. Technical Implementation

### Implementation Phases

#### Phase 1: Foundation Setup (Weeks 1-3)
- Project planning, define scope and architecture.
- Setup AWS account, configure AWS CLI, prepare IAM permissions.
- Setup Cognito User Pool, design and create DynamoDB tables.
- Create IAM role and policies, develop Lambda functions for quiz CRUD and room management.

#### Phase 2: API & Real-time Features Development (Weeks 4-6)
- Create API Gateway HTTP API, configure routes, integrate Lambda, add Cognito JWT Authorizer.
- Create API Gateway WebSocket API, develop connect/disconnect/message handlers.
- Configure EventBridge, develop answer submission workflow, score calculator, result saver.

#### Phase 3: Frontend Deployment & Monitoring (Weeks 7-8)
- Build frontend application, deploy to S3, configure CloudFront.
- Connect frontend with HTTP and WebSocket APIs.
- Create CloudWatch alarms and dashboard, perform end-to-end testing.
- Optimize cost, fix bugs, finalize documentation.

### Prerequisites

#### Resources & Tools
- AWS account.
- AWS CLI installed and configured.
- Node.js and npm/yarn for frontend development.
- API testing tools (Postman, curl...).
- WebSocket testing tools (wscat...).

#### Required Knowledge
- Basic AWS knowledge (Lambda, API Gateway, DynamoDB, Cognito, S3, CloudFront, CloudWatch, EventBridge, IAM).
- Programming knowledge (JavaScript/TypeScript for frontend and Lambda).
- REST API and WebSocket knowledge.
- NoSQL database knowledge (DynamoDB).

---

## 5. Timeline & Milestones

| Week | Milestone | Main Tasks |
| --- | --- | --- |
| Week 1 | Project Planning & AWS Setup | Define project scope, review architecture, create AWS account, configure AWS CLI, prepare IAM access. |
| Week 2 | Authentication & Database Design | Set up Cognito User Pool, design DynamoDB tables (users, quizzes, questions, rooms, connections, game state, results), create tables. |
| Week 3 | Backend Foundation | Create IAM role, configure Lambda permissions, develop Lambda functions for quiz CRUD and room management. |
| Week 4 | HTTP API Development | Create API Gateway HTTP API, configure routes, integrate Lambda, add Cognito JWT Authorizer, test quiz and room APIs. |
| Week 5 | WebSocket Real-time System | Create WebSocket API, develop connect/disconnect/message handlers, test host and player live communication. |
| Week 6 | Event-driven Game Logic | Configure EventBridge, develop answer submission workflow, score calculator, result saver and leaderboard logic. |
| Week 7 | Frontend Deployment | Build frontend application, deploy to S3, configure CloudFront, connect frontend with HTTP and WebSocket APIs. |
| Week 8 | Monitoring, Testing & Optimization | Create CloudWatch alarms and dashboard, perform end-to-end testing, optimize cost, fix bugs, finalize documentation. |

---

## 6. Budget Estimation

### 6.1 Development / Workshop Environment

| No. | Service | Configuration | Monthly Cost |
| --- | --- | --- | ---: |
| 1 | AWS Lambda | 1 million requests/month, average execution time 1 second, 256 MB memory | $0 - $5 |
| 2 | Amazon DynamoDB | On-demand, 1 million RCU/WCU/month | $0 - $10 |
| 3 | Amazon API Gateway | HTTP API: 1 million requests/month, WebSocket: 1 million messages/month | $1 - $5 |
| 4 | Amazon CloudFront | 10 GB data transfer/month | $0 - $5 |
| 5 | Amazon S3 | 5 GB storage, 100k requests/month | ~$0.50 |
| 6 | Amazon EventBridge | 1 million events/month | $0 - $1 |
| 7 | Amazon CloudWatch | 10 GB log storage, 10 metrics | $0 - $3 |
| | | **Total Estimated Cost** | **~$2 - $30** |

**Cost Optimization Notes:**
- Use DynamoDB on-demand to auto-adjust based on actual load.
- Use Lambda instead of maintaining always-on EC2 servers.
- Use CloudFront caching for efficient frontend delivery.
- Set TTL (Time to Live) on temporary data to auto-clean DynamoDB.
- Adjust CloudWatch log retention policy to avoid unnecessary log storage cost.
- Avoid using NAT Gateway and unnecessary VPC resources in dev environment.

### 6.2 Production Environment

| No. | Service | Configuration | Monthly Cost |
| --- | --- | --- | ---: |
| 1 | AWS Lambda | 10 million requests/month, average execution time 1 second, 256 MB memory | $10 - $20 |
| 2 | Amazon DynamoDB | On-demand, 10 million RCU/WCU/month | $20 - $50 |
| 3 | Amazon API Gateway | HTTP API: 10 million requests/month, WebSocket: 10 million messages/month | $10 - $30 |
| 4 | Amazon CloudFront | 100 GB data transfer/month | $5 - $15 |
| 5 | Amazon S3 | 50 GB storage, 1 million requests/month | $1 - $5 |
| 6 | Amazon EventBridge | 10 million events/month | $1 - $5 |
| 7 | Amazon CloudWatch | 50 GB log storage, 30 metrics | $5 - $15 |
| 8 | AWS WAF | Basic web protection | $10 - $20 |
| | | **Total Estimated Cost** | **~$62 - $160** |

---

## 7. Risk Assessment

### Risk Matrix

| Risk | Likelihood | Impact | Mitigation |
| --- | --- | --- | --- |
| WebSocket disconnection mid-game | Medium | High | Use DynamoDB TTL to auto-expire rooms and connections; players can reconnect via room PIN and player ID |
| DynamoDB throttling with many concurrent players | Medium | Medium | Analyze access patterns to optimize indexes, switch to provisioned capacity with auto-scaling |
| Lambda timeout when broadcasting to many connections | Medium | High | Design small, focused Lambda functions, split connection list into small batches |
| Incorrect score calculation logic | Low | High | Store raw player answers for re-processing later; thoroughly test score logic |
| Cognito config error blocking Host login | Medium | Medium | Temporarily open public API endpoint in internal testing environment to continue backend debugging |
| API Gateway route or authorizer config error | Medium | Medium | Thoroughly test APIs before going live |
| CloudFront/S3 deployment error making frontend inaccessible | Low | Medium | Developers can directly access static S3 for debugging first |
| CloudWatch cost spikes from excessive logging | Medium | Low | Adjust CloudWatch log retention policy; remove unnecessary logs |
| Security risk from over-provisioned IAM permissions | Medium | High | Apply least privilege principle when configuring IAM policies |

### Contingency Plan

- **Scenario 1: WebSocket disconnection**
  Players can reconnect using room PIN and current player ID without losing points.

- **Scenario 2: DynamoDB throttling**
  Analyze access patterns to optimize indexes or switch to provisioned capacity with auto-scaling.

- **Scenario 3: Lambda broadcast timeout**
  Split connection list into small batches or use asynchronous queue.

- **Scenario 4: Score calculation error**
  Store raw player answers for re-processing later.

- **Scenario 5: CloudFront deployment error**
  Developers can directly access static S3 for debugging first.

- **Scenario 6: Unexpected AWS cost spike**
  Use Cost Explorer to audit, reduce log retention, and remove unused resources.

---

## 8. System Workflows & Technical Characteristics

### 8.1 Main Workflows

#### Authentication Workflow
1. User (host) signs up/signs in with email via Amazon Cognito.
2. Cognito returns JWT token.
3. Frontend uses JWT token to call protected APIs (create quiz, create room...).
4. API Gateway validates JWT token with Cognito before allowing Lambda access.

#### Business Workflow (Create quiz & room, join room)
1. Host creates quiz by calling `POST /quizzes` API (protected by JWT).
2. `quiz-crud` Lambda processes request, saves quiz and questions to DynamoDB.
3. Host creates room by calling `POST /rooms` API (protected by JWT).
4. `room-management` Lambda generates random room PIN, saves room to DynamoDB.
5. Player gets room PIN from host, calls `GET /rooms/{pin}` API to get room info.
6. Player calls `POST /rooms/{pin}/join` API to join room, `room-management` Lambda saves player info to DynamoDB.

#### Asynchronous Workflow (Score calculation, save results)
1. Player submits answer via WebSocket with `SUBMIT_ANSWER` action.
2. `ws-message` Lambda processes, saves answer to DynamoDB, then sends `AnswerSubmitted` event to EventBridge.
3. EventBridge triggers `score-calculator` Lambda to calculate score based on correctness, remaining time, and streak.
4. `score-calculator` Lambda updates player's score in DynamoDB, sends score update message to all players via WebSocket.
5. When host ends game with `END_GAME` action, `ws-message` Lambda sends `GameEnded` event to EventBridge.
6. EventBridge triggers `game-results-saver` Lambda to save final results to DynamoDB.

#### Data Flow
- User data: Stored in `users` table (Cognito + DynamoDB).
- Quiz and question data: Stored in `quizzes` and `questions` tables.
- Room data: Stored in `rooms` table.
- WebSocket connection data: Stored in `connections` table.
- Game state, player, answer, score data: Stored in `game-state` table.
- Final result data: Stored in `game-results` table.

#### Notification Flow
- Real-time notifications via WebSocket: Start game, next question, score update, end game, leaderboard update.

### 8.2 Architectural Characteristics

#### Scalability
- Easy to scale system to support more players with serverless Lambda and DynamoDB on-demand.
- No manual server scaling management needed.

#### High Availability
- AWS services (Lambda, API Gateway, DynamoDB, S3, CloudFront) have built-in high availability.
- Data is automatically replicated across multiple AZs (Availability Zones).

#### Data Consistency
- DynamoDB supports both strong consistency and eventual consistency, suitable for different use-cases.
- Use EventBridge to ensure asynchronous processing is done at-least-once.

#### Security
- Good security with Cognito managing user authentication.
- Fine-grained permissions with IAM, applying least privilege principle.
- All frontend-backend communication over HTTPS/WSS.
- S3 bucket protected with OAC, no public access.
- (Optional) Use AWS WAF to protect web from common attacks.

#### Performance
- System responds quickly with serverless architecture, no server cold start overhead (minimized).
- Frontend globally delivered via CloudFront CDN, reducing latency for users in different regions.
- WebSocket API enables low-latency real-time two-way communication.

---

## 9. Expected Outcomes

### Technical Improvements
- **Result 1:** Flexible scalability with serverless Lambda and DynamoDB.
- **Result 2:** Reduced system management and operational effort with serverless model.
- **Result 3:** Efficient real-time two-way communication with API Gateway WebSocket API.
- **Result 4:** Separated business logic and asynchronous processing with event-driven architecture using EventBridge.
- **Result 5:** Better security with Cognito user management and detailed IAM role permissions.
- **Result 6:** Faster global frontend response with CloudFront CDN.
- **Result 7:** Better observability with CloudWatch metrics, logs, dashboards.

### Long-term Value

#### Business Value
- Provide a real-time interaction platform for training and internal engagement activities.
- Reduce system operational cost compared to traditional solutions.
- Easy to customize and extend based on actual needs.

#### User Value
- Smooth, interactive real-time quiz experience.
- Easy to join without installing complex apps.
- Get quick feedback and results.

#### Future Extensibility
- Add Admin Dashboard for report analysis and advanced management.
- Support more game modes (practice mode, competition mode, exam mode).
- In-depth analysis of player performance and question accuracy rates.
- Team-based quiz mode.
- Integrate with LMS or corporate training platforms.
- Multi-language support.
- Better mobile-optimized interface.
- Production deployment with custom domain, AWS WAF, and strict security policies.
- Complete CI/CD pipeline for automated testing and deployment.
