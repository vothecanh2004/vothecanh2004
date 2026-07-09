---
title: "System Testing"
date: 2024-01-01
weight: 12
pre: " <b> 5.12. </b> "
---

# System Testing & Verification

In this step, you will test and verify the entire end-to-end integration of the **WebQuiz** serverless application using API clients (Postman/curl) and terminal WebSocket clients (`wscat`).

---

### 1. Register and Log In (Get ID Token)

To access the protected REST APIs, you need a Cognito JWT ID token.

1. **Register a user:**
   You can register a host user via the Cognito hosted UI or directly through the AWS CLI:
   ```bash
   aws cognito-idp sign-up \
     --client-id YOUR_APP_CLIENT_ID \
     --username host@test.com \
     --password Password123!
   ```
2. **Confirm user registration:**
   Confirm the user status in the Cognito User Pool console or via CLI:
   ```bash
   aws cognito-idp admin-confirm-sign-up \
     --user-pool-id YOUR_USER_POOL_ID \
     --username host@test.com
   ```
3. **Log in to get the JWT ID Token:**
   Simulate login to receive the tokens:
   ```bash
   aws cognito-idp initiate-auth \
     --client-id YOUR_APP_CLIENT_ID \
     --auth-flow USER_SRP_AUTH \
     --auth-parameters USERNAME=host@test.com,PASSWORD=Password123!
   ```
   *Record the **`IdToken`** from the JSON response. You will use this token in the next step's Authorization header.*

---

### 2. Create a Quiz (REST HTTP API)

Open **Postman** (or use `curl`) to send a request to your HTTP API Gateway endpoint:

1. **Request Details:**
   * **Method:** `POST`
   * **URL:** `https://YOUR_HTTP_API_ID.execute-api.us-east-1.amazonaws.com/dev/quizzes`
   * **Headers:**
     * `Authorization`: `Bearer YOUR_ID_TOKEN`
     * `Content-Type`: `application/json`
   * **Body (JSON):**
     ```json
     {
       "title": "AWS Serverless Trivia",
       "description": "Test your serverless knowledge",
       "settings": { "timeLimit": 20 }
     }
     ```
2. Send the request. You should receive a `201 Created` status returning a generated **`quizId`**.

---

### 3. Create a Game Room (REST HTTP API)

Initialize a live quiz game session:

1. **Request Details:**
   * **Method:** `POST`
   * **URL:** `https://YOUR_HTTP_API_ID.execute-api.us-east-1.amazonaws.com/dev/rooms`
   * **Headers:**
     * `Authorization`: `Bearer YOUR_ID_TOKEN`
   * **Body (JSON):**
     ```json
     {
       "quizId": "YOUR_QUIZ_ID"
     }
     ```
2. Send the request. You should receive a `201 Created` response containing a generated 6-digit **`roomPin`** (e.g., `582914`).

---

### 4. Players Join the Room (Public REST API)

1. **Request Details:**
   * **Method:** `POST`
   * **URL:** `https://YOUR_HTTP_API_ID.execute-api.us-east-1.amazonaws.com/dev/rooms/582914/join` (replace `582914` with your active room PIN).
   * **Headers:**
     * `Content-Type`: `application/json`
   * **Body (JSON):**
     ```json
     {
       "nickname": "Alice"
     }
     ```
2. Send the request. You will receive a `200 OK` response with a player-specific **`orderId`** (e.g., `player_172900_abc`).

---

### 5. Establish Real-Time WebSocket Connections

Now, simulate the live gameplay using two terminal windows.

#### 5.1 Host Connection (Terminal 1)
Open your terminal and connect as the game host:
```bash
wscat -c "wss://YOUR_WS_API_ID.execute-api.us-east-1.amazonaws.com/dev?roomPin=582914&role=host"
```

#### 5.2 Player Connection (Terminal 2)
Open another terminal window and connect as Alice (the player):
```bash
wscat -c "wss://YOUR_WS_API_ID.execute-api.us-east-1.amazonaws.com/dev?roomPin=582914&orderId=YOUR_PLAYER_ORDER_ID&role=player"
```

---

### 6. Simulate Gameplay Loop

Execute the game state flow:

1. **Host Starts Game:**
   In Terminal 1 (Host), type and send:
   ```json
   {"action": "START_GAME"}
   ```
   *Verify that Terminal 2 (Player) immediately receives a `GAME_STARTED` payload message.*
2. **Host Displays Question:**
   In Terminal 1 (Host), send:
   ```json
   {"action": "NEXT_QUESTION"}
   ```
   *Verify that Terminal 2 receives a `NEXT_QUESTION` payload.*
3. **Player Submits Answer:**
   In Terminal 2 (Player), submit an answer before time expires:
   ```json
   {
     "action": "SUBMIT_ANSWER",
     "data": {
       "questionId": "your-question-id",
       "answer": "A",
       "timeRemaining": 15
     }
   }
   ```
   *Verify that the Player receives a `SCORE_UPDATE` response indicating whether the answer was correct and their updated score.*
4. **Host Ends Game:**
   In Terminal 1 (Host), type and send:
   ```json
   {"action": "END_GAME"}
   ```
   *Verify that both terminals receive a `GAME_ENDED` message containing the game leaderboard.*

---

### 7. Verify Game Results in Database

Open the **[Amazon DynamoDB console](https://console.aws.amazon.com/dynamodb/)**:
1. Select the **`webquiz-dev-game-results`** table.
2. Click **Explore table items**.
3. Verify that a record exists representing Alice's nickname, score, total correct answers, and final ranking.
