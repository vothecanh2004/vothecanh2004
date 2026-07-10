---
title: "Lambda REST Functions"
date: 2024-01-01
weight: 6
pre: " <b> 5.6. </b> "
---

# Deploying Lambda REST Functions

In this step, you will deploy the first set of AWS Lambda functions. These functions handle traditional REST operations, such as creating/listing quizzes and managing rooms.

---

### 1. Create Quiz CRUD Function

This function manages standard CRUD operations for quizzes and questions.

1. Open the **[AWS Lambda console](https://console.aws.amazon.com/lambda/)**.
2. Click **Create function**.
![Image 1](/images/5-Workshop/5.6/5.6.1.png)
3. Select **Author from scratch** and configure:
   * **Function name:** `webquiz-dev-quiz-crud`
   * **Runtime:** Select **Node.js 20.x**.
   * **Architecture:** Select **x86_64**.
   * Open **Change default execution role**:
     * Select **Use an existing role**.
     * **Existing role:** Select `webquiz-dev-lambda-role`.
   * Click **Create function**.
![Image 2](/images/5-Workshop/5.6/5.6.2.png)
![Image 3](/images/5-Workshop/5.6/5.6.3.png)
4. In the function details screen, go to the **Configuration** tab:
   * **General configuration** → Click **Edit**:
     * Set **Memory** to `256` MB.
     * Set **Timeout** to `30` seconds.
     * Click **Save**.
   * **Environment variables** → Click **Edit** → **Add environment variable**:
     * Add the following keys and values:
       * `QUIZZES_TABLE` = `webquiz-dev-quizzes`
       * `QUESTIONS_TABLE` = `webquiz-dev-questions`
       * `ENVIRONMENT` = `dev`
     * Click **Save**.
5. Switch to the **Code** tab, delete the default template, and paste the following code:
   ```javascript
   const { DynamoDBClient } = require('@aws-sdk/client-dynamodb');
   const { DynamoDBDocumentClient, GetCommand, PutCommand, UpdateCommand, QueryCommand } = require('@aws-sdk/lib-dynamodb');
   const { randomUUID } = require('crypto');

   const client = new DynamoDBClient({});
   const docClient = DynamoDBDocumentClient.from(client);

   const QUIZZES_TABLE = process.env.QUIZZES_TABLE;
   const QUESTIONS_TABLE = process.env.QUESTIONS_TABLE;

   exports.handler = async (event) => {
       console.log('Event:', JSON.stringify(event));
       
       const { httpMethod, path, pathParameters, body, requestContext } = event;
       const userId = requestContext?.authorizer?.jwt?.claims?.sub;
       
       try {
           // GET /quizzes - List user's quizzes
           if (httpMethod === 'GET' && path === '/quizzes') {
               const result = await docClient.send(new QueryCommand({
                   TableName: QUIZZES_TABLE,
                   IndexName: 'userId-createdAt-index',
                   KeyConditionExpression: 'userId = :userId',
                   ExpressionAttributeValues: { ':userId': userId },
                   ScanIndexForward: false
               }));
               
               return response(200, { quizzes: result.Items });
           }
           
           // POST /quizzes - Create quiz
           if (httpMethod === 'POST' && path === '/quizzes') {
               const data = JSON.parse(body);
               const quizId = randomUUID();
               const now = new Date().toISOString();
               
               const quiz = {
                   quizId,
                   userId,
                   title: data.title,
                   description: data.description || '',
                   settings: data.settings || { timeLimit: 20, showLeaderboard: true },
                   questionCount: 0,
                   createdAt: now,
                   updatedAt: now
               };
               
               await docClient.send(new PutCommand({
                   TableName: QUIZZES_TABLE,
                   Item: quiz
               }));
               
               return response(201, { quiz });
           }
           
           // GET /quizzes/{quizId} - Get quiz with questions
           if (httpMethod === 'GET' && pathParameters?.proxy) {
               const quizId = pathParameters.proxy.split('/')[0];
               
               const [quizResult, questionsResult] = await Promise.all([
                   docClient.send(new GetCommand({
                       TableName: QUIZZES_TABLE,
                       Key: { quizId }
                   })),
                   docClient.send(new QueryCommand({
                       TableName: QUESTIONS_TABLE,
                       KeyConditionExpression: 'quizId = :quizId',
                       ExpressionAttributeValues: { ':quizId': quizId }
                   }))
               ]);
               
               if (!quizResult.Item) {
                   return response(404, { error: 'Quiz not found' });
               }
               
               return response(200, { 
                   quiz: quizResult.Item,
                   questions: questionsResult.Items 
               });
           }
           
           // PUT /quizzes/{quizId} - Update quiz
           if (httpMethod === 'PUT' && pathParameters?.proxy) {
               const quizId = pathParameters.proxy.split('/')[0];
               const data = JSON.parse(body);
               
               await docClient.send(new UpdateCommand({
                   TableName: QUIZZES_TABLE,
                   Key: { quizId },
                   UpdateExpression: 'SET title = :title, description = :desc, settings = :settings, updatedAt = :now',
                   ConditionExpression: 'userId = :userId',
                   ExpressionAttributeValues: {
                       ':title': data.title,
                       ':desc': data.description || '',
                       ':settings': data.settings,
                       ':now': new Date().toISOString(),
                       ':userId': userId
                   }
               }));
               
               return response(200, { message: 'Quiz updated' });
           }
           
           // POST /quizzes/{quizId}/questions - Add question
           if (httpMethod === 'POST' && pathParameters?.proxy?.includes('/questions')) {
               const quizId = pathParameters.proxy.split('/')[0];
               const data = JSON.parse(body);
               const questionId = randomUUID();
               
               const question = {
                   quizId,
                   questionId,
                   text: data.text,
                   type: data.type || 'multiple-choice',
                   options: data.options,
                   correctAnswer: data.correctAnswer,
                   points: data.points || 1000,
                   timeLimit: data.timeLimit || 20,
                   order: data.order || 0
               };
               
               await docClient.send(new PutCommand({
                   TableName: QUESTIONS_TABLE,
                   Item: question
               }));
               
               // Update question count
               await docClient.send(new UpdateCommand({
                   TableName: QUIZZES_TABLE,
                   Key: { quizId },
                   UpdateExpression: 'SET questionCount = questionCount + :inc',
                   ExpressionAttributeValues: { ':inc': 1 }
               }));
               
               return response(201, { question });
           }
           
           return response(404, { error: 'Not found' });
           
       } catch (error) {
           console.error('Error:', error);
           return response(500, { error: error.message });
       }
   };

   const response = (statusCode, body) => ({
       statusCode,
       headers: {
           'Content-Type': 'application/json',
           'Access-Control-Allow-Origin': '*'
       },
       body: JSON.stringify(body)
   });
   ```
6. Click **Deploy**.

---

### 2. Create Room Management Function

This function manages game rooms, room PIN code generation, and player registration.

1. Click **Create function**:
   * **Function name:** `webquiz-dev-room-management`
   * **Runtime:** **Node.js 20.x**.
   * **Existing role:** `webquiz-dev-lambda-role`.
2. Under the **Configuration** tab:
   * **General configuration** → Click **Edit**: Set **Memory** to `256` MB and **Timeout** to `30` seconds.
   * **Environment variables** → Click **Edit** → **Add environment variable**:
     * `ROOMS_TABLE` = `webquiz-dev-rooms`
     * `GAME_STATE_TABLE` = `webquiz-dev-game-state`
     * `ENVIRONMENT` = `dev`
3. Under the **Code** tab, paste the following code:
   ```javascript
   const { DynamoDBClient } = require('@aws-sdk/client-dynamodb');
   const { DynamoDBDocumentClient, GetCommand, PutCommand, UpdateCommand, QueryCommand } = require('@aws-sdk/lib-dynamodb');

   const client = new DynamoDBClient({});
   const docClient = DynamoDBDocumentClient.from(client);

   const ROOMS_TABLE = process.env.ROOMS_TABLE;
   const GAME_STATE_TABLE = process.env.GAME_STATE_TABLE;

   // Generate 6-digit PIN
   const generatePin = () => Math.floor(100000 + Math.random() * 900000).toString();

   exports.handler = async (event) => {
       console.log('Event:', JSON.stringify(event));
       
       const { httpMethod, path, pathParameters, body, requestContext } = event;
       const userId = requestContext?.authorizer?.jwt?.claims?.sub;
       
       try {
           // POST /rooms - Create room (Host)
           if (httpMethod === 'POST' && path === '/rooms') {
               const data = JSON.parse(body);
               const roomPin = generatePin();
               const now = new Date().toISOString();
               const ttl = Math.floor(Date.now() / 1000) + (24 * 60 * 60); // 24 hours
               
               const room = {
                   roomPin,
                   hostId: userId,
                   quizId: data.quizId,
                   status: 'waiting', // waiting, playing, finished
                   currentQuestion: 0,
                   playerCount: 0,
                   createdAt: now,
                   ttl
               };
               
               // Save room
               await docClient.send(new PutCommand({
                   TableName: ROOMS_TABLE,
                   Item: room,
                   ConditionExpression: 'attribute_not_exists(roomPin)'
               }));
               
               // Initialize game state
               await docClient.send(new PutCommand({
                   TableName: GAME_STATE_TABLE,
                   Item: {
                       pk: `ROOM#${roomPin}`,
                       sk: 'META',
                       status: 'waiting',
                       currentQuestion: 0,
                       startedAt: null,
                       ttl
                   }
               }));
               
               return response(201, { room });
           }
           
           // GET /rooms/{pin} - Get room info
           if (httpMethod === 'GET' && pathParameters?.pin) {
               const roomPin = pathParameters.pin;
               
               const result = await docClient.send(new GetCommand({
                   TableName: ROOMS_TABLE,
                   Key: { roomPin }
               }));
               
               if (!result.Item) {
                   return response(404, { error: 'Room not found' });
               }
               
               // Get player list from game state
               const playersResult = await docClient.send(new QueryCommand({
                   TableName: GAME_STATE_TABLE,
                   KeyConditionExpression: 'pk = :pk AND begins_with(sk, :sk)',
                   ExpressionAttributeValues: {
                       ':pk': `ROOM#${roomPin}`,
                       ':sk': 'PLAYER#'
                   }
               }));
               
               return response(200, { 
                   room: result.Item,
                   players: playersResult.Items.map(p => ({
                       orderId: p.orderId,
                       nickname: p.nickname,
                       score: p.score || 0
                   }))
               });
           }
           
           // POST /rooms/{pin}/join - Join room (Player)
           if (httpMethod === 'POST' && pathParameters?.pin && path.includes('/join')) {
               const roomPin = pathParameters.pin;
               const data = JSON.parse(body);
               const orderId = `player_${Date.now()}_${Math.random().toString(36).substr(2, 9)}`;
               const ttl = Math.floor(Date.now() / 1000) + (24 * 60 * 60);
               
               // Check room exists and is waiting
               const roomResult = await docClient.send(new GetCommand({
                   TableName: ROOMS_TABLE,
                   Key: { roomPin }
               }));
               
               if (!roomResult.Item) {
                   return response(404, { error: 'Room not found' });
               }
               
               if (roomResult.Item.status !== 'waiting') {
                   return response(400, { error: 'Game already started' });
               }
               
               // Add player to game state
               await docClient.send(new PutCommand({
                   TableName: GAME_STATE_TABLE,
                   Item: {
                       pk: `ROOM#${roomPin}`,
                       sk: `PLAYER#${orderId}`,
                       orderId,
                       nickname: data.nickname,
                       score: 0,
                       correctAnswers: 0,
                       joinedAt: new Date().toISOString(),
                       ttl
                   }
               }));
               
               // Update player count
               await docClient.send(new UpdateCommand({
                   TableName: ROOMS_TABLE,
                   Key: { roomPin },
                   UpdateExpression: 'SET playerCount = playerCount + :inc',
                   ExpressionAttributeValues: { ':inc': 1 }
               }));
               
               return response(200, { 
                   orderId,
                   roomPin,
                   nickname: data.nickname 
               });
           }
           
           return response(404, { error: 'Not found' });
           
       } catch (error) {
           console.error('Error:', error);
           return response(500, { error: error.message });
       }
   };

   const response = (statusCode, body) => ({
       statusCode,
       headers: {
           'Content-Type': 'application/json',
           'Access-Control-Allow-Origin': '*'
       },
       body: JSON.stringify(body)
   });
   ```
4. Click **Deploy**.
