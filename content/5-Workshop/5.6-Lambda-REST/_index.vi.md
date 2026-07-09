---
title: "Hàm Lambda REST API"
date: 2024-01-01
weight: 6
pre: " <b> 5.6. </b> "
---

# Triển khai các hàm Lambda REST API

Trong bước này, bạn sẽ triển khai nhóm hàm AWS Lambda đầu tiên. Các hàm này phụ trách xử lý các thao tác REST API truyền thống như tạo/lấy danh sách bộ câu hỏi (quiz) và quản lý trạng thái phòng chơi.

---

### 1. Tạo hàm Quiz CRUD (`webquiz-dev-quiz-crud`)

Hàm này thực hiện các thao tác CRUD cơ bản cho bộ câu hỏi (quizzes) và các câu hỏi chi tiết (questions).

1. Mở **[AWS Lambda console](https://console.aws.amazon.com/lambda/)**.
2. Nhấp chọn **Create function**.
3. Chọn **Author from scratch** và cấu hình các thông số:
   * **Function name:** `webquiz-dev-quiz-crud`
   * **Runtime:** Chọn **Node.js 20.x**.
   * **Architecture:** Chọn **x86_64**.
   * Mở mục **Change default execution role**:
     * Chọn **Use an existing role**.
     * **Existing role:** Chọn `webquiz-dev-lambda-role`.
   * Nhấp chọn **Create function**.
4. Sau khi tạo xong, di chuyển tới tab **Configuration**:
   * Tại mục **General configuration** → Nhấp chọn **Edit**:
     * Đặt **Memory** là `256` MB.
     * Đặt **Timeout** là `30` giây.
     * Nhấp chọn **Save**.
   * Tại mục **Environment variables** → Nhấp chọn **Edit** → **Add environment variable**:
     * Nhập 3 biến môi trường sau:
       * `QUIZZES_TABLE` = `webquiz-dev-quizzes`
       * `QUESTIONS_TABLE` = `webquiz-dev-questions`
       * `ENVIRONMENT` = `dev`
     * Nhấp chọn **Save**.
5. Di chuyển tới tab **Code**, xóa toàn bộ mã nguồn mẫu mặc định và dán đoạn code sau:
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
           // GET /quizzes - Lấy danh sách quizzes của user
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
           
           // POST /quizzes - Tạo mới quiz
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
           
           // GET /quizzes/{quizId} - Lấy chi tiết quiz kèm các câu hỏi
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
           
           // PUT /quizzes/{quizId} - Cập nhật thông tin quiz
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
           
           // POST /quizzes/{quizId}/questions - Thêm câu hỏi vào quiz
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
               
               // Cập nhật số lượng câu hỏi trong bảng Quiz
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
6. Nhấp chọn **Deploy** để triển khai code.

---

### 2. Tạo hàm quản lý phòng chơi (`webquiz-dev-room-management`)

Hàm này thực hiện tạo phòng chơi, sinh mã PIN ngẫu nhiên 6 chữ số và xử lý cho người chơi tham gia.

1. Nhấp chọn **Create function**:
   * **Function name:** `webquiz-dev-room-management`
   * **Runtime:** **Node.js 20.x**.
   * **Existing role:** Chọn `webquiz-dev-lambda-role`.
2. Tại tab **Configuration**:
   * **General configuration** → Nhấp chọn **Edit**: Cấu hình **Memory** `256` MB và **Timeout** `30` giây.
   * **Environment variables** → Nhấp chọn **Edit** → **Add environment variable**:
     * `ROOMS_TABLE` = `webquiz-dev-rooms`
     * `GAME_STATE_TABLE` = `webquiz-dev-game-state`
     * `ENVIRONMENT` = `dev`
3. Tại tab **Code**, xóa code mẫu và dán đoạn mã sau:
   ```javascript
   const { DynamoDBClient } = require('@aws-sdk/client-dynamodb');
   const { DynamoDBDocumentClient, GetCommand, PutCommand, UpdateCommand, QueryCommand } = require('@aws-sdk/lib-dynamodb');

   const client = new DynamoDBClient({});
   const docClient = DynamoDBDocumentClient.from(client);

   const ROOMS_TABLE = process.env.ROOMS_TABLE;
   const GAME_STATE_TABLE = process.env.GAME_STATE_TABLE;

   // Sinh mã PIN ngẫu nhiên 6 chữ số
   const generatePin = () => Math.floor(100000 + Math.random() * 900000).toString();

   exports.handler = async (event) => {
       console.log('Event:', JSON.stringify(event));
       
       const { httpMethod, path, pathParameters, body, requestContext } = event;
       const userId = requestContext?.authorizer?.jwt?.claims?.sub;
       
       try {
           // POST /rooms - Tạo phòng chơi (Dành cho Quản trò)
           if (httpMethod === 'POST' && path === '/rooms') {
               const data = JSON.parse(body);
               const roomPin = generatePin();
               const now = new Date().toISOString();
               const ttl = Math.floor(Date.now() / 1000) + (24 * 60 * 60); // Hết hạn sau 24h
               
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
               
               // Lưu phòng đấu
               await docClient.send(new PutCommand({
                   TableName: ROOMS_TABLE,
                   Item: room,
                   ConditionExpression: 'attribute_not_exists(roomPin)'
               }));
               
               // Khởi tạo trạng thái game (Game State meta)
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
           
           // GET /rooms/{pin} - Lấy thông tin phòng chơi
           if (httpMethod === 'GET' && pathParameters?.pin) {
               const roomPin = pathParameters.pin;
               
               const result = await docClient.send(new GetCommand({
                   TableName: ROOMS_TABLE,
                   Key: { roomPin }
               }));
               
               if (!result.Item) {
                   return response(404, { error: 'Room not found' });
               }
               
               // Lấy danh sách người chơi từ bảng game state
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
           
           // POST /rooms/{pin}/join - Người chơi tham gia phòng đấu
           if (httpMethod === 'POST' && pathParameters?.pin && path.includes('/join')) {
               const roomPin = pathParameters.pin;
               const data = JSON.parse(body);
               const orderId = `player_${Date.now()}_${Math.random().toString(36).substr(2, 9)}`;
               const ttl = Math.floor(Date.now() / 1000) + (24 * 60 * 60);
               
               // Kiểm tra phòng có tồn tại và đang chờ không
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
               
               // Thêm người chơi vào game state
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
               
               // Cập nhật số lượng người chơi của phòng
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
4. Nhấp chọn **Deploy** để hoàn tất triển khai.
