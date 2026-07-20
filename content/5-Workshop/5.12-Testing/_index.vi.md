---
title: "Kiểm thử hệ thống"
date: 2024-01-01
weight: 12
chapter: false
pre: " <b> 5.12. </b> "
---

# Kiểm thử & Xác minh Hệ thống

Trong bước này, bạn sẽ tiến hành chạy thử nghiệm toàn bộ luồng tích hợp của ứng dụng serverless **WebQuiz** từ đầu đến cuối (end-to-end) sử dụng các API Client (Postman/curl) và client WebSocket dòng lệnh (`wscat`).

---

### 1. Đăng ký và Đăng nhập (Lấy ID Token)

Để gọi các API REST được bảo vệ, bạn cần lấy mã xác thực JWT ID Token từ Cognito.

1. **Đăng ký tài khoản:**
   Bạn có thể đăng ký tài khoản quản trò (host) qua giao diện Hosted UI của Cognito hoặc sử dụng lệnh AWS CLI:
   ```bash
   aws cognito-idp sign-up \
     --client-id YOUR_APP_CLIENT_ID \
     --username host@test.com \
     --password Password123!
   ```
2. **Xác nhận đăng ký:**
   Xác nhận trạng thái người dùng trên Cognito console hoặc qua CLI:
   ```bash
   aws cognito-idp admin-confirm-sign-up \
     --user-pool-id YOUR_USER_POOL_ID \
     --username host@test.com
   ```
3. **Đăng nhập để lấy JWT ID Token:**
   Thực hiện đăng nhập để hệ thống cấp mã token:
   ```bash
   aws cognito-idp initiate-auth \
     --client-id YOUR_APP_CLIENT_ID \
     --auth-flow USER_SRP_AUTH \
     --auth-parameters USERNAME=host@test.com,PASSWORD=Password123!
   ```
   *Hãy sao chép giá trị **`IdToken`** từ kết quả JSON trả về. Bạn sẽ cần nó để điền vào header Authorization ở bước sau.*

---

### 2. Tạo Bộ Câu Hỏi (REST HTTP API)

Mở **Postman** (hoặc dùng `curl`) để gửi request tạo bộ câu hỏi:

1. **Thông tin Request:**
   * **Phương thức:** `POST`
   * **URL:** `https://YOUR_HTTP_API_ID.execute-api.ap-southeast-1.amazonaws.com/dev/quizzes`
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
2. Nhấn gửi. Bạn sẽ nhận được phản hồi trạng thái `201 Created` kèm theo mã định danh **`quizId`**.

---

### 3. Khởi tạo Phòng Chơi (REST HTTP API)

Khởi tạo một game show trực tiếp:

1. **Thông tin Request:**
   * **Phương thức:** `POST`
   * **URL:** `https://YOUR_HTTP_API_ID.execute-api.ap-southeast-1.amazonaws.com/dev/rooms`
   * **Headers:**
     * `Authorization`: `Bearer YOUR_ID_TOKEN`
   * **Body (JSON):**
     ```json
     {
       "quizId": "YOUR_QUIZ_ID"
     }
     ```
2. Nhấn gửi. Hệ thống sẽ trả về mã PIN phòng đấu gồm 6 chữ số **`roomPin`** (ví dụ: `582914`).

---

### 4. Người chơi Tham gia Phòng (Public REST API)

1. **Thông tin Request:**
   * **Phương thức:** `POST`
   * **URL:** `https://YOUR_HTTP_API_ID.execute-api.ap-southeast-1.amazonaws.com/dev/rooms/582914/join` (thay `582914` bằng mã PIN phòng thực tế vừa tạo).
   * **Headers:**
     * `Content-Type`: `application/json`
   * **Body (JSON):**
     ```json
     {
       "nickname": "Alice"
     }
     ```
2. Nhấn gửi. Hệ thống phản hồi trạng thái `200 OK` kèm theo mã **`orderId`** định danh cho người chơi Alice.

---

### 5. Kết nối WebSocket Thời Gian Thực

Tiếp theo, mở hai cửa sổ terminal để mô phỏng tương tác giữa quản trò và người chơi trong game đấu trực tiếp.

#### 5.1 Kết nối phía Quản trò (Terminal 1)
Mở terminal và kết nối với vai trò host:
```bash
wscat -c "wss://YOUR_WS_API_ID.execute-api.ap-southeast-1.amazonaws.com/dev?roomPin=582914&role=host"
```

#### 5.2 Kết nối phía Người chơi (Terminal 2)
Mở cửa sổ terminal khác và kết nối với vai trò người chơi (Alice):
```bash
wscat -c "wss://YOUR_WS_API_ID.execute-api.ap-southeast-1.amazonaws.com/dev?roomPin=582914&orderId=YOUR_PLAYER_ORDER_ID&role=player"
```

---

### 6. Mô phỏng Luồng Chơi Game

Thực hiện gửi các tin nhắn điều phối trận đấu trên terminal:

1. **Quản trò Bắt đầu Game:**
   Tại Terminal 1 (Host), gõ và gửi:
   ```json
   {"action": "START_GAME"}
   ```
   *Kiểm tra thấy Terminal 2 (Player) ngay lập tức nhận được thông điệp `GAME_STARTED` từ hệ thống.*
2. **Quản trò Trình chiếu Câu hỏi:**
   Tại Terminal 1 (Host), gửi tiếp:
   ```json
   {"action": "NEXT_QUESTION"}
   ```
   *Kiểm tra thấy Terminal 2 nhận được thông điệp `NEXT_QUESTION` hiển thị câu hỏi.*
3. **Người chơi Nộp câu trả lời:**
   Tại Terminal 2 (Player), nộp đáp án trước khi hết giờ:
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
   *Kiểm tra thấy Player nhận được thông điệp phản hồi `SCORE_UPDATE` cho biết câu trả lời đúng hay sai và điểm số được cộng dồn.*
4. **Quản trò Kết thúc Game:**
   Tại Terminal 1 (Host), gửi lệnh:
   ```json
   {"action": "END_GAME"}
   ```
   *Kiểm tra thấy cả hai terminal đều nhận về kết quả bảng xếp hạng chung cuộc `GAME_ENDED`.*

---

### 7. Xác minh Kết quả trong Cơ sở dữ liệu

Truy cập **[Amazon DynamoDB console](https://console.aws.amazon.com/dynamodb/)**:
1. Chọn bảng **`webquiz-dev-game-results`**.
2. Nhấp chọn **Explore table items**.
3. Xác nhận sự hiện diện của bản ghi lưu trữ điểm số, thứ hạng và số câu trả lời đúng của Alice.

### Demo WebQuiz
Link demo: https://syncquiz-frontend.onrender.com/login
