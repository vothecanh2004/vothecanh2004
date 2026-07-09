---
title: "Lambda Realtime & Event Functions"
date: 2024-01-01
weight: 7
pre: " <b> 5.7. </b> "
---

# Deploying Lambda Realtime & Event Functions

In this step, you will deploy the remaining 5 AWS Lambda functions that handle persistent WebSocket connections (connecting, disconnecting, processing actions) and asynchronous background events (calculating scores, saving results).

---

### 1. Create WebSocket Connect Function (`webquiz-dev-ws-connect`)

This function manages incoming WebSocket handshakes and registers client connections in DynamoDB.

1. Click **Create function** in the Lambda console:
   * **Function name:** `webquiz-dev-ws-connect`
   * **Runtime:** **Node.js 20.x**.
   * **Existing role:** `webquiz-dev-lambda-role`.
2. Under **Configuration** tab:
   * **General configuration:** Set Memory to `256` MB, Timeout to `30` seconds.
   * **Environment variables:**
     * `CONNECTIONS_TABLE` = `webquiz-dev-connections`
     * `GAME_STATE_TABLE` = `webquiz-dev-game-state`
3. Paste the code into the **Code** tab:
   ```javascript
   const { DynamoDBClient } = require('@aws-sdk/client-dynamodb');
   const { DynamoDBDocumentClient, PutCommand } = require('@aws-sdk/lib-dynamodb');

   const client = new DynamoDBClient({});
   const docClient = DynamoDBDocumentClient.from(client);

   const CONNECTIONS_TABLE = process.env.CONNECTIONS_TABLE;

   exports.handler = async (event) => {
       console.log('Connect Event:', JSON.stringify(event));
       
       const connectionId = event.requestContext.connectionId;
       const { roomPin, orderId, role } = event.queryStringParameters || {};
       
       if (!roomPin) {
           return { statusCode: 400, body: 'Missing roomPin' };
       }
       
       const ttl = Math.floor(Date.now() / 1000) + (24 * 60 * 60);
       
       try {
           await docClient.send(new PutCommand({
               TableName: CONNECTIONS_TABLE,
               Item: {
                   connectionId,
                   roomPin,
                   orderId: orderId || null,
                   role: role || 'player', // 'host' or 'player'
                   connectedAt: new Date().toISOString(),
                   ttl
               }
           }));
           
           console.log(`Connection ${connectionId} saved for room ${roomPin}`);
           return { statusCode: 200, body: 'Connected' };
           
       } catch (error) {
           console.error('Error:', error);
           return { statusCode: 500, body: 'Failed to connect' };
       }
   };
   ```
4. Click **Deploy**.

---

### 2. Create WebSocket Disconnect Function (`webquiz-dev-ws-disconnect`)

This function removes the registered connection ID from DynamoDB when a user closes their connection.

1. Click **Create function**:
   * **Function name:** `webquiz-dev-ws-disconnect`
   * **Runtime:** **Node.js 20.x**.
   * **Existing role:** `webquiz-dev-lambda-role`.
2. Under **Configuration** tab:
   * **Environment variables:**
     * `CONNECTIONS_TABLE` = `webquiz-dev-connections`
     * `GAME_STATE_TABLE` = `webquiz-dev-game-state`
3. Paste the code into the **Code** tab:
   ```javascript
   const { DynamoDBClient } = require('@aws-sdk/client-dynamodb');
   const { DynamoDBDocumentClient, DeleteCommand, GetCommand } = require('@aws-sdk/lib-dynamodb');

   const client = new DynamoDBClient({});
   const docClient = DynamoDBDocumentClient.from(client);

   const CONNECTIONS_TABLE = process.env.CONNECTIONS_TABLE;

   exports.handler = async (event) => {
       console.log('Disconnect Event:', JSON.stringify(event));
       
       const connectionId = event.requestContext.connectionId;
       
       try {
           // Get connection info before deleting
           const connResult = await docClient.send(new GetCommand({
               TableName: CONNECTIONS_TABLE,
               Key: { connectionId }
           }));
           
           // Delete connection
           await docClient.send(new DeleteCommand({
               TableName: CONNECTIONS_TABLE,
               Key: { connectionId }
           }));
           
           console.log(`Connection ${connectionId} removed`);
           return { statusCode: 200, body: 'Disconnected' };
           
       } catch (error) {
           console.error('Error:', error);
           return { statusCode: 500, body: 'Failed to disconnect' };
       }
   };
   ```
4. Click **Deploy**.

---

### 3. Create WebSocket Message Function (`webquiz-dev-ws-message`)

This function routes real-time game actions (`START_GAME`, `NEXT_QUESTION`, `SUBMIT_ANSWER`, `END_GAME`), publishes events to EventBridge, and broadcasts updates to connected players.

1. Click **Create function**:
   * **Function name:** `webquiz-dev-ws-message`
   * **Runtime:** **Node.js 20.x**.
   * **Existing role:** `webquiz-dev-lambda-role`.
2. Under **Configuration** tab:
   * **General configuration:** Set Memory to `512` MB, Timeout to `30` seconds.
   * **Environment variables:**
     * `CONNECTIONS_TABLE` = `webquiz-dev-connections`
     * `ROOMS_TABLE` = `webquiz-dev-rooms`
     * `GAME_STATE_TABLE` = `webquiz-dev-game-state`
     * `EVENT_BUS_NAME` = `webquiz-dev-game-events`
     * `WEBSOCKET_ENDPOINT` = `https://temp-endpoint.com` (Note: You will update this URL after creating the WebSocket API Gateway in the next sections).
3. Paste the code into the **Code** tab:
   ```javascript
   const { DynamoDBClient } = require('@aws-sdk/client-dynamodb');
   const { DynamoDBDocumentClient, GetCommand, PutCommand, UpdateCommand, QueryCommand, DeleteCommand } = require('@aws-sdk/lib-dynamodb');
   const { ApiGatewayManagementApiClient, PostToConnectionCommand } = require('@aws-sdk/client-apigatewaymanagementapi');
   const { EventBridgeClient, PutEventsCommand } = require('@aws-sdk/client-eventbridge');

   const ddbClient = new DynamoDBClient({});
   const docClient = DynamoDBDocumentClient.from(ddbClient);
   const eventBridge = new EventBridgeClient({});

   const CONNECTIONS_TABLE = process.env.CONNECTIONS_TABLE;
   const ROOMS_TABLE = process.env.ROOMS_TABLE;
   const GAME_STATE_TABLE = process.env.GAME_STATE_TABLE;
   const EVENT_BUS_NAME = process.env.EVENT_BUS_NAME;

   exports.handler = async (event) => {
       console.log('Message Event:', JSON.stringify(event));
       
       const connectionId = event.requestContext.connectionId;
       const endpoint = `https://${event.requestContext.domainName}/${event.requestContext.stage}`;
       const apigw = new ApiGatewayManagementApiClient({ endpoint });
       
       let body;
       try {
           body = JSON.parse(event.body);
       } catch (e) {
           return { statusCode: 400, body: 'Invalid JSON' };
       }
       
       const { action, data } = body;
       
       try {
           // Get connection info
           const connResult = await docClient.send(new GetCommand({
               TableName: CONNECTIONS_TABLE,
               Key: { connectionId }
           }));
           
           if (!connResult.Item) {
               return { statusCode: 400, body: 'Connection not found' };
           }
           
           const { roomPin, role } = connResult.Item;
           
           switch (action) {
               case 'START_GAME':
                   if (role !== 'host') {
                       return { statusCode: 403, body: 'Only host can start game' };
                   }
                   await handleStartGame(roomPin, apigw);
                   break;
                   
               case 'NEXT_QUESTION':
                   if (role !== 'host') {
                       return { statusCode: 403, body: 'Only host can advance questions' };
                   }
                   await handleNextQuestion(roomPin, apigw);
                   break;
                   
               case 'SUBMIT_ANSWER':
                   await handleSubmitAnswer(roomPin, connResult.Item.orderId, data, apigw);
                   break;
                   
               case 'END_GAME':
                   if (role !== 'host') {
                       return { statusCode: 403, body: 'Only host can end game' };
                   }
                   await handleEndGame(roomPin, apigw);
                   break;
                   
               default:
                   return { statusCode: 400, body: 'Unknown action' };
           }
           
           return { statusCode: 200, body: 'OK' };
           
       } catch (error) {
           console.error('Error:', error);
           return { statusCode: 500, body: error.message };
       }
   };

   async function handleStartGame(roomPin, apigw) {
       await docClient.send(new UpdateCommand({
           TableName: ROOMS_TABLE,
           Key: { roomPin },
           UpdateExpression: 'SET #status = :status',
           ExpressionAttributeNames: { '#status': 'status' },
           ExpressionAttributeValues: { ':status': 'playing' }
       }));
       
       await docClient.send(new UpdateCommand({
           TableName: GAME_STATE_TABLE,
           Key: { pk: `ROOM#${roomPin}`, sk: 'META' },
           UpdateExpression: 'SET #status = :status, startedAt = :now, currentQuestion = :q',
           ExpressionAttributeNames: { '#status': 'status' },
           ExpressionAttributeValues: { 
               ':status': 'playing',
               ':now': new Date().toISOString(),
               ':q': 1
           }
       }));
       
       await broadcastToRoom(roomPin, { type: 'GAME_STARTED', currentQuestion: 1 }, apigw);
   }

   async function handleNextQuestion(roomPin, apigw) {
       const stateResult = await docClient.send(new GetCommand({
           TableName: GAME_STATE_TABLE,
           Key: { pk: `ROOM#${roomPin}`, sk: 'META' }
       }));
       
       const nextQuestion = (stateResult.Item?.currentQuestion || 0) + 1;
       
       await docClient.send(new UpdateCommand({
           TableName: GAME_STATE_TABLE,
           Key: { pk: `ROOM#${roomPin}`, sk: 'META' },
           UpdateExpression: 'SET currentQuestion = :q, questionStartedAt = :now',
           ExpressionAttributeValues: { 
               ':q': nextQuestion,
               ':now': new Date().toISOString()
           }
       }));
       
       await broadcastToRoom(roomPin, { type: 'NEXT_QUESTION', currentQuestion: nextQuestion }, apigw);
   }

   async function handleSubmitAnswer(roomPin, orderId, data, apigw) {
       const { questionId, answer, timeRemaining } = data;
       const ttl = Math.floor(Date.now() / 1000) + (24 * 60 * 60);
       
       await docClient.send(new PutCommand({
           TableName: GAME_STATE_TABLE,
           Item: {
               pk: `ROOM#${roomPin}`,
               sk: `ANSWER#${questionId}#${orderId}`,
               orderId,
               questionId,
               answer,
               timeRemaining,
               submittedAt: new Date().toISOString(),
               ttl
           }
       }));
       
       await eventBridge.send(new PutEventsCommand({
           Entries: [{
               Source: 'quiz.game',
               DetailType: 'AnswerSubmitted',
               EventBusName: EVENT_BUS_NAME,
               Detail: JSON.stringify({
                   roomPin,
                   orderId,
                   questionId,
                   answer,
                   timeRemaining
               })
           }]
       }));
       
       await broadcastToRoom(roomPin, { 
           type: 'ANSWER_RECEIVED', 
           orderId,
           questionId 
       }, apigw, 'host');
   }

   async function handleEndGame(roomPin, apigw) {
       await docClient.send(new UpdateCommand({
           TableName: ROOMS_TABLE,
           Key: { roomPin },
           UpdateExpression: 'SET #status = :status',
           ExpressionAttributeNames: { '#status': 'status' },
           ExpressionAttributeValues: { ':status': 'finished' }
       }));
       
       const playersResult = await docClient.send(new QueryCommand({
           TableName: GAME_STATE_TABLE,
           KeyConditionExpression: 'pk = :pk AND begins_with(sk, :sk)',
           ExpressionAttributeValues: {
               ':pk': `ROOM#${roomPin}`,
               ':sk': 'PLAYER#'
           }
       }));
       
       const leaderboard = playersResult.Items
           .map(p => ({ nickname: p.nickname, score: p.score || 0 }))
           .sort((a, b) => b.score - a.score);
       
       await broadcastToRoom(roomPin, { type: 'GAME_ENDED', leaderboard }, apigw);
       
       await eventBridge.send(new PutEventsCommand({
           Entries: [{
               Source: 'quiz.game',
               DetailType: 'GameEnded',
               EventBusName: EVENT_BUS_NAME,
               Detail: JSON.stringify({ roomPin, leaderboard })
           }]
       }));
   }

   async function broadcastToRoom(roomPin, message, apigw, roleFilter = null) {
       const connsResult = await docClient.send(new QueryCommand({
           TableName: CONNECTIONS_TABLE,
           IndexName: 'roomPin-index',
           KeyConditionExpression: 'roomPin = :roomPin',
           ExpressionAttributeValues: { ':roomPin': roomPin }
       }));
       
       const connections = roleFilter 
           ? connsResult.Items.filter(c => c.role === roleFilter)
           : connsResult.Items;
       
       const promises = connections.map(async (conn) => {
           try {
               await apigw.send(new PostToConnectionCommand({
                   ConnectionId: conn.connectionId,
                   Data: JSON.stringify(message)
               }));
           } catch (error) {
               if (error.statusCode === 410) {
                   await docClient.send(new DeleteCommand({
                       TableName: CONNECTIONS_TABLE,
                       Key: { connectionId: conn.connectionId }
                   }));
               }
           }
       });
       
       await Promise.all(promises);
   }
   ```
4. Click **Deploy**.

---

### 4. Create Score Calculator Function (`webquiz-dev-score-calculator`)

This function is triggered by EventBridge rules when a player submits an answer. It reads the correct answer from DynamoDB, calculates the score based on remaining time and streak, updates the player state, and notifies the player via WebSocket.

1. Click **Create function**:
   * **Function name:** `webquiz-dev-score-calculator`
   * **Runtime:** **Node.js 20.x**.
   * **Existing role:** `webquiz-dev-lambda-role`.
2. Under **Configuration** tab:
   * **Environment variables:**
     * `GAME_STATE_TABLE` = `webquiz-dev-game-state`
     * `CONNECTIONS_TABLE` = `webquiz-dev-connections`
     * `WEBSOCKET_ENDPOINT` = `https://temp-endpoint.com` (You will update this later).
3. Paste the code into the **Code** tab:
   ```javascript
   const { DynamoDBClient } = require('@aws-sdk/client-dynamodb');
   const { DynamoDBDocumentClient, GetCommand, UpdateCommand, QueryCommand } = require('@aws-sdk/lib-dynamodb');
   const { ApiGatewayManagementApiClient, PostToConnectionCommand } = require('@aws-sdk/client-apigatewaymanagementapi');

   const ddbClient = new DynamoDBClient({});
   const docClient = DynamoDBDocumentClient.from(ddbClient);

   const GAME_STATE_TABLE = process.env.GAME_STATE_TABLE;
   const CONNECTIONS_TABLE = process.env.CONNECTIONS_TABLE;
   const WEBSOCKET_ENDPOINT = process.env.WEBSOCKET_ENDPOINT;

   const calculateScore = (isCorrect, timeRemaining, streak) => {
       if (!isCorrect) return 0;
       const baseScore = 1000;
       const timeBonus = Math.floor(timeRemaining * 20); 
       const streakBonus = streak > 1 ? 200 : 0; 
       return baseScore + timeBonus + streakBonus;
   };

   exports.handler = async (event) => {
       console.log('Score Calculator Event:', JSON.stringify(event));
       
       const { roomPin, orderId, questionId, answer, timeRemaining } = event.detail;
       
       try {
           const questionState = await docClient.send(new GetCommand({
               TableName: GAME_STATE_TABLE,
               Key: { pk: `ROOM#${roomPin}`, sk: `QUESTION#${questionId}` }
           }));
           
           const correctAnswer = questionState.Item?.correctAnswer;
           const isCorrect = answer === correctAnswer;
           
           const playerState = await docClient.send(new GetCommand({
               TableName: GAME_STATE_TABLE,
               Key: { pk: `ROOM#${roomPin}`, sk: `PLAYER#${orderId}` }
           }));
           
           const currentStreak = isCorrect ? (playerState.Item?.streak || 0) + 1 : 0;
           const score = calculateScore(isCorrect, timeRemaining, currentStreak);
           const newTotalScore = (playerState.Item?.score || 0) + score;
           const newCorrectAnswers = (playerState.Item?.correctAnswers || 0) + (isCorrect ? 1 : 0);
           
           await docClient.send(new UpdateCommand({
               TableName: GAME_STATE_TABLE,
               Key: { pk: `ROOM#${roomPin}`, sk: `PLAYER#${orderId}` },
               UpdateExpression: 'SET score = :score, correctAnswers = :correct, streak = :streak',
               ExpressionAttributeValues: {
                   ':score': newTotalScore,
                   ':correct': newCorrectAnswers,
                   ':streak': currentStreak
               }
           }));
           
           if (WEBSOCKET_ENDPOINT && !WEBSOCKET_ENDPOINT.includes('temp-endpoint.com')) {
               const apigw = new ApiGatewayManagementApiClient({ endpoint: WEBSOCKET_ENDPOINT });
               
               const connsResult = await docClient.send(new QueryCommand({
                   TableName: CONNECTIONS_TABLE,
                   IndexName: 'roomPin-index',
                   KeyConditionExpression: 'roomPin = :roomPin',
                   FilterExpression: 'orderId = :orderId',
                   ExpressionAttributeValues: {
                       ':roomPin': roomPin,
                       ':orderId': orderId
                   }
               }));
               
               if (connsResult.Items.length > 0) {
                   try {
                       await apigw.send(new PostToConnectionCommand({
                           ConnectionId: connsResult.Items[0].connectionId,
                           Data: JSON.stringify({
                               type: 'SCORE_UPDATE',
                               isCorrect,
                               scoreEarned: score,
                               totalScore: newTotalScore,
                               streak: currentStreak
                           })
                       }));
                   } catch (error) {
                       console.error('Failed to send score update:', error);
                   }
               }
           }
           
           return { statusCode: 200 };
           
       } catch (error) {
           console.error('Error:', error);
           throw error;
       }
   };
   ```
4. Click **Deploy**.

---

### 5. Create Game Results Saver Function (`webquiz-dev-game-results-saver`)

This function is triggered by EventBridge rules when the host ends the game. It extracts all player stats from the game-state and saves them permanently into the game results table.

1. Click **Create function**:
   * **Function name:** `webquiz-dev-game-results-saver`
   * **Runtime:** **Node.js 20.x**.
   * **Existing role:** `webquiz-dev-lambda-role`.
2. Under **Configuration** tab:
   * **General configuration:** Set Timeout to `60` seconds.
   * **Environment variables:**
     * `GAME_RESULTS_TABLE` = `webquiz-dev-game-results`
     * `GAME_STATE_TABLE` = `webquiz-dev-game-state`
3. Paste the code into the **Code** tab:
   ```javascript
   const { DynamoDBClient } = require('@aws-sdk/client-dynamodb');
   const { DynamoDBDocumentClient, PutCommand, QueryCommand } = require('@aws-sdk/lib-dynamodb');

   const ddbClient = new DynamoDBClient({});
   const docClient = DynamoDBDocumentClient.from(ddbClient);

   const GAME_RESULTS_TABLE = process.env.GAME_RESULTS_TABLE;
   const GAME_STATE_TABLE = process.env.GAME_STATE_TABLE;

   exports.handler = async (event) => {
       console.log('Game Results Saver Event:', JSON.stringify(event));
       
       const { roomPin, leaderboard } = event.detail;
       
       try {
           const playersResult = await docClient.send(new QueryCommand({
               TableName: GAME_STATE_TABLE,
               KeyConditionExpression: 'pk = :pk AND begins_with(sk, :sk)',
               ExpressionAttributeValues: {
                   ':pk': `ROOM#${roomPin}`,
                   ':sk': 'PLAYER#'
               }
           }));
           
           const now = new Date().toISOString();
           
           for (let i = 0; i < playersResult.Items.length; i++) {
               const player = playersResult.Items[i];
               const rank = leaderboard.findIndex(l => l.nickname === player.nickname) + 1;
               
               await docClient.send(new PutCommand({
                   TableName: GAME_RESULTS_TABLE,
                   Item: {
                       roomPin,
                       orderId: player.orderId,
                       nickname: player.nickname,
                       score: player.score || 0,
                       correctAnswers: player.correctAnswers || 0,
                       rank,
                       totalPlayers: playersResult.Items.length,
                       completedAt: now
                   }
               }));
           }
           
           console.log(`Saved results for ${playersResult.Items.length} players in room ${roomPin}`);
           return { statusCode: 200 };
           
       } catch (error) {
           console.error('Error:', error);
           throw error;
       }
   };
   ```
4. Click **Deploy**.
