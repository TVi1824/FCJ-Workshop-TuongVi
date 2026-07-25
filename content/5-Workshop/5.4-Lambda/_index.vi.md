---
title: "Triển khai Logic với AWS Lambda"
date: 2024-01-01
weight: 4
chapter: false
pre: " <b> 5.4. </b> "
---

Trong phần này, bạn sẽ triển khai mã nguồn, cấu hình biến môi trường và kiểm thử kết nối cho các hàm **AWS Lambda** của dự án Chrono Genesis Game. Hướng dẫn dưới đây sẽ đi qua 9 bước chi tiết để khởi tạo và cấu hình hoàn chỉnh một hàm Lambda.

---

## I. TỔNG QUAN
### Khởi tạo hàm Lambda mới
Truy cập vào giao diện quản lý của dịch vụ AWS Lambda trên AWS Management Console. Nhấn nút **Create function** để bắt đầu tạo mới một hàm Lambda.

**Mục đích kỹ thuật:** Khởi tạo môi trường tính toán serverless cho dự án Chrono Genesis Game, nơi sẽ chứa các đoạn mã xử lý nghiệp vụ của game (như kết nối, xử lý lượt đánh, v.v.).

![Tạo hàm Lambda](/images/5-Workshop/overrall-lambda/1.%20Vao%20service%20Lambda%20-%20chon%20Create%20Lambda.png)

---

### Cấu hình thông tin cơ bản và phân quyền (Execution Role)
Tại màn hình "Create function", thực hiện:
1. Chọn **Author from scratch**.
2. Tại **Function name**, nhập tên hàm (ví dụ: `StartMatch-function`).
3. Chọn Role đã được thiết lập sẵn cho dự án: `Chrono-lambda-execution-role`.
4. Nhấn **Create function**.

**Mục đích kỹ thuật:** Thiết lập tên gọi và cấp quyền (Permissions) cho hàm Lambda thông qua IAM Role. Việc gán role `Chrono-lambda-execution-role` đảm bảo Lambda có đủ quyền để truy cập vào DynamoDB, SQS, hoặc API Gateway nhằm phục vụ luồng dữ liệu game.

![Cấu hình tên và Role cho Lambda](/images/5-Workshop/overrall-lambda/2.%20dat%20ten%20lambda%20-%20enable%20custom%20executtion%20role%20-%20chon%20role%20Chrono-lambda-execution-role.png)

---

### Chuẩn bị tải lên mã nguồn (Source Code)
Sau khi hàm được tạo thành công, hệ thống chuyển hướng bạn đến màn hình chi tiết của hàm. Điều hướng đến tab **Code**. Tại mục **Code source**, chọn **Upload from** và click vào **.zip file**.

**Mục đích kỹ thuật:** Chuẩn bị đưa mã nguồn logic game (đã được đóng gói, tối ưu bằng công cụ esbuild và nén thành định dạng .zip) lên môi trường chạy của AWS Lambda.

![Upload file code zip](/images/5-Workshop/overrall-lambda/3.%20Sau%20khi%20tao%20xong%2C%20upload%20file%20code%20lambda%20da%20esbuild%20va%20nen%20thanh%20file%20zip.png)

---

### Khởi chạy hộp thoại Upload
Khi hộp thoại **Upload a .zip file** xuất hiện, nhấn nút **Upload** để mở cửa sổ chọn tệp tin (File Explorer) trên máy tính cá nhân của bạn.

**Mục đích kỹ thuật:** Bắt đầu quá trình chọn file chứa mã nguồn (.zip) từ máy cục bộ để chuyển lên đám mây AWS.

![Khởi chạy hộp thoại upload](/images/5-Workshop/overrall-lambda/4.png)

---

### Chọn file .zip và lưu cấu hình
Duyệt đến thư mục chứa mã nguồn của dự án trên máy của bạn, chọn file `.zip` tương ứng với logic của hàm Lambda hiện tại, sau đó nhấn nút **Update** để hệ thống bắt đầu quá trình tải lên.

**Mục đích kỹ thuật:** Tải mã nguồn thực thi đã được đóng gói kỹ lưỡng lên kho lưu trữ trực tiếp của hàm AWS Lambda, chuẩn bị cho việc thực thi code logic thực tế.

![Chọn file zip và save](/images/5-Workshop/overrall-lambda/5.png)

---

### Xác nhận tải mã nguồn thành công
Khi khung hiển thị thông báo **"Successfully updated the function..."**, điều đó xác nhận rằng tệp mã nguồn của bạn đã được tải lên và triển khai thành công.

**Mục đích kỹ thuật:** Đảm bảo rằng phiên bản mã nguồn mới nhất đã được ghi đè an toàn lên môi trường hàm Lambda và đã sẵn sàng hoạt động.

![Upload thành công](/images/5-Workshop/overrall-lambda/6.%20upload%20thanh%20cong.png)

---

### Cấu hình dữ liệu giả lập (Test Event)
Để đảm bảo Lambda xử lý đúng chức năng, chuyển sang tab **Test** trên giao diện. Tại đây:
1. Chọn **Edit saved event**.
2. Nhập tên sự kiện vào **Event name** (ví dụ: `TestConnectEvent`).
3. Dán/định dạng lại nội dung JSON vào khung **Event JSON** theo đúng chuẩn cấu trúc dữ liệu đầu vào (payload) dự kiến của game.
4. Nhấn **Save**.

**Mục đích kỹ thuật:** Cấu hình một sự kiện mô phỏng (Test Event) để kiểm tra cục bộ xem logic mã nguồn vừa tải lên có hoạt động đúng với thiết kế hay không trước khi tích hợp nó vào hệ thống API thực.

![Cấu hình Test Event](/images/5-Workshop/overrall-lambda/7.%20De%20dam%20bao%20lambda%20xu%20ly%20dung%20chuc%20nang%2C%20co%20the%20test%20truoc%20bang%20cach%20truy%20cap%20vao%20tab%20test%20event%20trong%20lambda.png)

---

### Chạy thử nghiệm và xác minh kết quả
Kiểm tra kết quả trả về trong khung **Execution result**.
Nếu màn hình xuất hiện trạng thái **succeeded** (ví dụ với `statusCode: 200` và nội dung body thành công), điều đó chứng tỏ hàm của bạn hoạt động chính xác.

**Mục đích kỹ thuật:** Xác minh độc lập chức năng của hàm Lambda có xử lý thành công yêu cầu từ người dùng.

![Kết quả test thành công](/images/5-Workshop/overrall-lambda/8.%20Vi%20du%20test%20connectHandler%20thanh%20cong.png)

---

### Kiểm tra Trigger kết nối WebSocket
Điều hướng đến tab **Configuration** > **Triggers**. Kiểm tra danh sách Trigger hiện tại.
Đảm bảo rằng nguồn kích hoạt (Trigger) được liên kết đúng với **API Gateway** và định tuyến vào đúng luồng của WebSocket API tương ứng.

**Mục đích kỹ thuật:** Tích hợp hàm Lambda vào kiến trúc mạng thực tế. Thao tác này thiết lập cầu nối bắt buộc để Amazon API Gateway chuyển tiếp các sự kiện thời gian thực từ người chơi (thông qua kết nối WebSocket) tới trực tiếp hàm Lambda xử lý nghiệp vụ.

![Kiểm tra Trigger WebSocket](/images/5-Workshop/overrall-lambda/9.%20kiem%20tra%20Trigger%2C%20dam%20bao%20cac%20lambda%20cho%20websocket%20deu%20trigger%20vao%20api%20websocket%20gateway%20qua%20route.png)

---
## II. CẤU HÌNH LAMBDA FUNCTION

### 1. Cấu hình Lambda HTTP API

Tiếp theo, chúng ta sẽ thiết lập hàm Lambda chuyên xử lý các yêu cầu HTTP API (RESTful) từ người chơi, đảm nhận các chức năng nền tảng như quản lý Deck, tra cứu Leaderboard, và Rank.

#### Bước 1: Khởi tạo hàm Lambda chrono-http-backend
Tại giao diện quản lý AWS Lambda, nhấn **Create function** và thực hiện cấu hình các thông số cơ bản:
1. Chọn tùy chọn **Author from scratch**.
2. **Function name:** Nhập tên hàm là `chrono-http-backend`.
3. **Runtime:** Lựa chọn môi trường thực thi phù hợp (ví dụ: `Node.js 20.x` hoặc `Node.js 24.x`).
4. **Execution role:** Mở rộng phần Change default execution role, chọn **Use an existing role** và gán quyền thông qua role `Chrono-lambda-execution-role`.
5. Nhấn nút **Create function** để hoàn tất.

**Mục đích kỹ thuật:** Tạo môi trường xử lý backend độc lập để tiếp nhận và phản hồi các HTTP Request tĩnh. Việc tái sử dụng role `Chrono-lambda-execution-role` đảm bảo hàm có đủ quyền truy cập đọc/ghi vào DynamoDB.

![Tạo hàm chrono-http-backend](/images/5-Workshop/Lambda%20HTTP/1.png)

---

#### Bước 2: Cấu hình mã nguồn và môi trường cho HTTP Backend
Sau khi hàm `chrono-http-backend` được tạo thành công, tiến hành triển khai mã nguồn và thiết lập môi trường:
1. Chuyển đến tab **Code**, nhấn **Upload from** > **.zip file** và tải lên file chứa logic xử lý HTTP API (ví dụ: `chrono-http-backend.zip`). Đợi thông báo cập nhật thành công.
2. Điều hướng qua tab **Configuration** > **Environment variables**. Bổ sung các biến môi trường bảo mật cần thiết để giao tiếp với Database (như `DB_SECRET_NAME` hoặc các thông tin cấu hình bảng).
3. Nhấn **Save** để áp dụng thay đổi và sẵn sàng tích hợp với HTTP API Gateway ở các bước tiếp theo.

**Mục đích kỹ thuật:** Cung cấp mã nguồn thực thi và các cấu hình động bảo mật để hàm Lambda có thể xử lý các API RESTful và tương tác trơn tru với kho lưu trữ dữ liệu tập trung.

![Cấu hình chrono-http-backend](/images/5-Workshop/Lambda%20HTTP/2.chrono-http-backend.png)

---

## 2. Cấu hình Lambda WebSocket API

Trong phần này, chúng ta sẽ đi sâu vào cấu hình chi tiết cho từng hàm Lambda chịu trách nhiệm xử lý các luồng sự kiện thời gian thực (real-time) qua WebSocket. 

#### Hàm Lambda: connectHandle
**Vai trò:** Xử lý ghi nhận thiết bị người chơi khi kết nối WebSocket ban đầu được thiết lập, lưu trữ thông tin nhận diện người dùng vào hệ thống.
- **Bước 1: Khởi tạo hàm:** Tạo hàm Lambda mới với tên `ConnectHandler`. Gán quyền thực thi bằng role `Chrono-lambda-execution-role`.
- **Bước 2: Triển khai mã nguồn:** Tại màn hình chi tiết, chọn tab **Code**, nhấn **Upload from > .zip file** và tải lên file mã nguồn xử lý kết nối gốc.
- **Bước 3: Thiết lập Trigger:** Mở giao diện API Gateway, chọn WebSocket API hiện có của dự án. Điều hướng đến route `$connect` và thiết lập Integration type trỏ về hàm `ConnectHandler` vừa tạo.

![Tạo mã nguồn connectHandle](/images/5-Workshop/3.%20Lambda%20websocket/connectHandle/Screenshot%202026-07-21%20024153.png)
![Thiết lập trigger connectHandle](/images/5-Workshop/3.%20Lambda%20websocket/connectHandle/Screenshot%202026-07-25%20170040.png)

#### Hàm Lambda: disconnectHandler
**Vai trò:** Dọn dẹp dữ liệu kết nối đã cũ, tự động xóa `connectionId` khỏi cơ sở dữ liệu khi người chơi thoát game hoặc mất tín hiệu rớt mạng.
- **Bước 1: Tạo hàm và cấu hình Code:** Khởi tạo hàm `DisconnectHandler` và upload mã nguồn `.zip` tương ứng.
- **Bước 2: Cấu hình Route ngắt kết nối:** Tương tự hàm connect, quay lại API Gateway và trỏ route `$disconnect` vào hàm `DisconnectHandler` này để hệ thống AWS tự động gọi khi mất kết nối.

![Cấu hình disconnectHandler](/images/5-Workshop/3.%20Lambda%20websocket/disconnectHandler/1.png)

#### Hàm Lambda: startMatch
**Vai trò:** Được kích hoạt khi hệ thống Matchmaking (tìm trận) đã gom đủ số lượng người chơi. Hàm chịu trách nhiệm chia bài ban đầu, thiết lập HP và thông báo bắt đầu trận đấu.
- **Bước 1: Triển khai hàm StartMatch:** Upload mã nguồn chứa logic khởi tạo trận đấu (Game State Initialization).
- **Bước 2: Cấu hình biến môi trường (nếu có):** Nhập các thông số quy định lượng máu khởi điểm hoặc số lượng thẻ bài tối đa qua tab Configuration > Environment variables.
- **Bước 3: Cấu hình Test Event:** Nhấn **Test > Configure test event**. Dán payload giả lập chứa 2 `connectionId` của người chơi vào để đảm bảo logic chia bài không phát sinh lỗi. Chạy thử nghiệm và xác nhận kết quả `statusCode: 200`.

![Cấu hình startMatch](/images/5-Workshop/3.%20Lambda%20websocket/startMatch/Screenshot%202026-07-25%20171353.png)

#### Hàm Lambda: processGameEngine
**Vai trò:** Đây là bộ não (Core Engine) phân xử mọi logic mỗi khi người chơi đánh một lá bài, thi triển kỹ năng hoặc kết thúc lượt.
- **Bước 1: Upload mã nguồn:** Do dung lượng logic Game Engine lớn, cần upload file `.zip` chứa toàn bộ tập luật (ruleset) của game.
- **Bước 2: Gắn WebSocket Route:** Tại API Gateway, tạo một custom route (ví dụ: `action` hoặc `playCard`) và chọn Lambda proxy integration trỏ trực tiếp vào hàm `ProcessGameEngine`.
- **Bước 3: Cấp quyền truy cập DynamoDB:** Xác minh lại IAM Role của hàm để đảm bảo nó có đủ quyền `UpdateItem` và `GetItem` để liên tục thay đổi trạng thái ván đấu.

![Cấu hình processGameEngine](/images/5-Workshop/3.%20Lambda%20websocket/processGameEngine/1.png)

#### Hàm Lambda: handleTimeout
**Vai trò:** Nhận nhiệm vụ đếm ngược, tự động bỏ qua (skip) lượt của người chơi nếu họ không có bất kỳ hành động nào trong khung thời gian quy định.
- **Bước 1: Khởi tạo hàm:** Upload mã nguồn của hàm `HandleTimeout`.
- **Bước 2: Thiết lập SQS Trigger:** Tại tab Configuration > Triggers, nhấn **Add trigger**, chọn dịch vụ **SQS** và kết nối đến hàng đợi `Chrono-Timeout-Queue`.
- **Bước 3: Tạo Test Event cho SQS:** Chuyển sang tab Test, tạo event mới mô phỏng định dạng tin nhắn của SQS (SQS Message payload) chứa `matchId`.
- **Bước 4: Chạy kiểm thử:** Nhấn Test để kiểm tra. Xem Execution result báo Succeeded, đảm bảo thông điệp ép kết thúc lượt được sinh ra chuẩn xác.

![Tạo hàm handleTimeout](/images/5-Workshop/3.%20Lambda%20websocket/handleTimeout/1..png)
![Thiết lập trigger handleTimeout](/images/5-Workshop/3.%20Lambda%20websocket/handleTimeout/2.png)
![Tạo test event handleTimeout](/images/5-Workshop/3.%20Lambda%20websocket/handleTimeout/3.%20Test%20event.png)
![Kết quả test handleTimeout](/images/5-Workshop/3.%20Lambda%20websocket/handleTimeout/4.%20Test%20result.png)

#### Hàm Lambda: cancelMatch
**Vai trò:** Bắt tín hiệu từ người chơi khi họ chủ động nhấn nút "Hủy tìm trận", gỡ bỏ thông tin của họ khỏi hàng đợi chờ đấu.
- **Bước 1: Upload Code:** Khởi tạo hàm và tải lên đoạn code xóa Record trong bản Matchmaking của DynamoDB.
- **Bước 2: Map Route API Gateway:** Tạo route WebSocket riêng (ví dụ `$cancelMatch`) và trỏ vào hàm này.

![Cấu hình cancelMatch](/images/5-Workshop/3.%20Lambda%20websocket/cancelMatch/1.png)

#### Hàm Lambda: endMatch
**Vai trò:** Tính toán kết quả cuối cùng (thắng/thua), cộng trừ Elo/Rank và đóng luồng trận đấu khi máu (HP) của một bên về 0.
- **Bước 1: Cấu hình hàm EndMatch:** Cập nhật file mã nguồn `.zip`. Đảm bảo code có cơ chế gọi SQS để chuyển việc tính toán rank nặng nề sang Background Worker nếu cần thiết.
- **Bước 2: Phân quyền mở rộng:** Nếu hàm này gọi các service bên thứ 3 hoặc EventBridge, hãy bổ sung quyền thích hợp vào `Chrono-lambda-execution-role`.

![Cấu hình endMatch](/images/5-Workshop/3.%20Lambda%20websocket/endMatch/1.png)

#### Hàm Lambda: rebuildLeaderBoardRanks
**Vai trò:** Hàm Cron Job chạy ngầm định kỳ, tổng hợp toàn bộ điểm số của người chơi và sắp xếp lại bảng xếp hạng (Global Leaderboard).
- **Bước 1: Cập nhật chỉ mục (Index) Database:** Truy cập giao diện bảng DynamoDB, chọn **Create index**. Cấu hình GSI (Global Secondary Index) với Partition Key và Sort Key phù hợp để query điểm xếp hạng tốc độ cao. Chờ đến khi Status hiển thị Active.
- **Bước 2: Triển khai hàm:** Upload mã nguồn `rebuildLeaderboardRanks.zip`.
- **Bước 3: Thiết lập EventBridge Rule:** Mở bảng điều khiển Amazon EventBridge. Chọn **Create rule**. Đặt tên rule là `DailyLeaderboardUpdate`.
- **Bước 4: Cấu hình lịch trình (Schedule):** Chọn định dạng Cron expression để quy định chu kỳ chạy tự động (ví dụ: `cron(0 0 * * ? *)` chạy mỗi ngày).
- **Bước 5: Gắn Target:** Thiết lập Target của rule này là dịch vụ AWS Lambda và chỉ định chính xác tên hàm `rebuildLeaderBoardRanks`.

![Cập nhật DB Index 1](/images/5-Workshop/3.%20Lambda%20websocket/rebuildLeaderBoardRanks/update%20index%20DB/Screenshot%202026-07-24%20011616.png)
![Cập nhật DB Index 2](/images/5-Workshop/3.%20Lambda%20websocket/rebuildLeaderBoardRanks/update%20index%20DB/Screenshot%202026-07-24%20011625.png)
![Cập nhật DB Index 3](/images/5-Workshop/3.%20Lambda%20websocket/rebuildLeaderBoardRanks/update%20index%20DB/Screenshot%202026-07-24%20011636.png)
![Setup EventBridge 1](/images/5-Workshop/3.%20Lambda%20websocket/rebuildLeaderBoardRanks/setup%20eventBridge%20for%20lambda%20rebuildLeaderboardRanks/Screenshot%202026-07-24%20014857.png)
![Setup EventBridge 2](/images/5-Workshop/3.%20Lambda%20websocket/rebuildLeaderBoardRanks/setup%20eventBridge%20for%20lambda%20rebuildLeaderboardRanks/Screenshot%202026-07-24%20014949.png)
![Setup EventBridge 3](/images/5-Workshop/3.%20Lambda%20websocket/rebuildLeaderBoardRanks/setup%20eventBridge%20for%20lambda%20rebuildLeaderboardRanks/Screenshot%202026-07-24%20015016.png)
![Setup EventBridge 4](/images/5-Workshop/3.%20Lambda%20websocket/rebuildLeaderBoardRanks/setup%20eventBridge%20for%20lambda%20rebuildLeaderboardRanks/Screenshot%202026-07-24%20015039.png)
![Setup EventBridge 5](/images/5-Workshop/3.%20Lambda%20websocket/rebuildLeaderBoardRanks/setup%20eventBridge%20for%20lambda%20rebuildLeaderboardRanks/Screenshot%202026-07-24%20015045.png)
![Setup EventBridge 6](/images/5-Workshop/3.%20Lambda%20websocket/rebuildLeaderBoardRanks/setup%20eventBridge%20for%20lambda%20rebuildLeaderboardRanks/Screenshot%202026-07-24%20015052.png)
