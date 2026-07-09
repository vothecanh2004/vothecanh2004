---
title: "Hàm Lambda Realtime"
date: 2024-01-01
weight: 7
pre: " <b> 5.7. </b> "
---

# Triển khai các hàm Lambda Realtime & Event

Trong bước này, bạn sẽ triển khai 5 hàm AWS Lambda còn lại. Các hàm này phụ trách duy trì kết nối WebSocket thời gian thực (kết nối, ngắt kết nối, định tuyến hành động chơi game) và xử lý sự kiện bất đồng bộ dưới nền (tính điểm, lưu kết quả game).

---

### 1. Tạo hàm WebSocket Connect (`webquiz-dev-ws-connect`)

Hàm này tiếp nhận cái bắt tay WebSocket đầu tiên từ client và lưu thông tin đăng ký kết nối vào bảng Connections.

1. Bấm **Create function** trong Lambda console:
   * **Function name:** `webquiz-dev-ws-connect`
   * **Runtime:** **Node.js 20.x**.
   * **Existing role:** `webquiz-dev-lambda-role`.
2. Tại tab **Configuration**:
   * **General configuration:** Đặt Memory là `256` MB, Timeout là `30` giây.
   * **Environment variables:**
     * `CONNECTIONS_TABLE` = `webquiz-dev-connections`
     * `GAME_STATE_TABLE` = `webquiz-dev-game-state`
3. Tại tab **Code**, dán đoạn mã sau:
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
                   role: role || 'player', // 'host' hoặc 'player'
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
4. Bấm **Deploy**.

---

### 2. Tạo hàm WebSocket Disconnect (`webquiz-dev-ws-disconnect`)

Hàm này xóa kết nối của người chơi khỏi cơ sở dữ liệu khi họ đóng tab hoặc mất kết nối.

1. Bấm **Create function**:
   * **Function name:** `webquiz-dev-ws-disconnect`
   * **Runtime:** **Node.js 20.x**.
   * **Existing role:** `webquiz-dev-lambda-role`.
2. Tại tab **Configuration**:
   * **Environment variables:**
     * `CONNECTIONS_TABLE` = `webquiz-dev-connections`
     * `GAME_STATE_TABLE` = `webquiz-dev-game-state`
3. Tại tab **Code**, dán đoạn mã sau:
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
           // Lấy thông tin kết nối trước khi xóa
           const connResult = await docClient.send(new GetCommand({
               TableName: CONNECTIONS_TABLE,
               Key: { connectionId }
           }));
           
           // Xóa kết nối
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
4. Bấm **Deploy**.

---

### 3. Tạo hàm xử lý WebSocket Message (`webquiz-dev-ws-message`)

Hàm này chịu trách nhiệm điều phối các hành động chính trong game (`START_GAME`, `NEXT_QUESTION`, `SUBMIT_ANSWER`, `END_GAME`), đẩy sự kiện lên EventBridge và phát (broadcast) trạng thái tới người chơi.

1. Bấm **Create function**:
   * **Function name:** `webquiz-dev-ws-message`
   * **Runtime:** **Node.js 20.x**.
   * **Existing role:** `webquiz-dev-lambda-role`.
2. Tại tab **Configuration**:
   * **General configuration:** Thiết lập Memory `512` MB, Timeout `30` giây.
   * **Environment variables**:
     * `CONNECTIONS_TABLE` = `webquiz-dev-connections`
     * `ROOMS_TABLE` = `webquiz-dev-rooms`
     * `GAME_STATE_TABLE` = `webquiz-dev-game-state`
     * `EVENT_BUS_NAME` = `webquiz-dev-game-events`
     * `WEBSOCKET_ENDPOINT` = `https://temp-endpoint.com` (Lưu ý: URL này sẽ được cập nhật sau khi tạo WebSocket API Gateway ở phần sau).
3. Tại tab **Code**, dán đoạn mã sau:
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
4. Bấm **Deploy**.

---

### 4. Tạo hàm Tính điểm (`webquiz-dev-score-calculator`)

Hàm này được kích hoạt bởi EventBridge khi có người chơi nộp câu trả lời. Nó lấy đáp án đúng từ game-state, chấm điểm dựa trên tốc độ phản hồi và chuỗi trả lời đúng, cập nhật điểm và phát thông báo qua WebSocket.

1. Bấm **Create function**:
   * **Function name:** `webquiz-dev-score-calculator`
   * **Runtime:** **Node.js 20.x**.
   * **Existing role:** `webquiz-dev-lambda-role`.
2. Tại tab **Configuration**:
   * **Environment variables:**
     * `GAME_STATE_TABLE` = `webquiz-dev-game-state`
     * `CONNECTIONS_TABLE` = `webquiz-dev-connections`
     * `WEBSOCKET_ENDPOINT` = `https://temp-endpoint.com` (Sẽ cập nhật sau).
3. Tại tab **Code**, dán đoạn mã sau:
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
4. Bấm **Deploy**.

---

### 5. Tạo hàm Lưu kết quả game (`webquiz-dev-game-results-saver`)

Hàm này được kích hoạt bởi EventBridge khi quản trò kết thúc trận đấu. Nó lấy toàn bộ dữ liệu người chơi trong game-state để lưu trữ vĩnh viễn vào bảng kết quả.

1. Bấm **Create function**:
   * **Function name:** `webquiz-dev-game-results-saver`
   * **Runtime:** **Node.js 20.x**.
   * **Existing role:** `webquiz-dev-lambda-role`.
2. Tại tab **Configuration**:
   * **General configuration:** Đặt Timeout thành `60` giây.
   * **Environment variables:**
     * `GAME_RESULTS_TABLE` = `webquiz-dev-game-results`
     * `GAME_STATE_TABLE` = `webquiz-dev-game-state`
3. Tại tab **Code**, dán đoạn mã sau:
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
4. Bấm **Deploy**.
