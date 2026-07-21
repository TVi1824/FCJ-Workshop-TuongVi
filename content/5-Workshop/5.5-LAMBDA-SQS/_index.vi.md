---
title : "Triển khai Logic với AWS Lambda & SQS"
date : 2024-01-01
weight : 5
chapter : false
pre : " <b> 5.5. </b> "
---

Trong phần này, bạn sẽ triển khai mã nguồn, cấu hình biến môi trường và kiểm thử kết nối cho hàm **ConnectHandler-function** trên AWS Lambda.

---

### 1. Triển khai mã nguồn cấu hình cho hàm Lambda

1. Trong giao diện chi tiết của hàm Lambda **ConnectHandler-function**, chuyển đến tab **Code**. Tìm nút **Upload from** và chọn tùy chọn **.zip file**.
![Upload file zip cho hàm Lambda](/images/5-Workshop/5.5-Policy/10.png)
![Upload file zip cho hàm Lambda](/images/5-Workshop/5.5-Policy/806.png)

2. Một cửa sổ popup hiện ra, click vào **Upload** (hoặc **Choose file**) và chọn tệp `index.zip` đã được đóng gói sẵn từ máy tính cá nhân. Sau đó nhấn nút **Open** để tải code lên.

![Upload file zip cho hàm Lambda](/images/5-Workshop/5.5-Policy/55.png)

3. Đợi vài giây, hệ thống sẽ hiển thị thông báo màu xanh **"Successfully updated the function ConnectHandler-function"** xác nhận quá trình tải lên thành công.
![Upload file zip cho hàm Lambda](/images/5-Workshop/5.5-Policy/818.png)

4. Chuyển sang tab **Configuration**, sau đó chọn mục **Environment variables** ở menu bên trái.
![Cấu hình Environment Variables cho Lambda](/images/5-Workshop/5.5-Policy/153.png)

5. Click vào nút **Edit** và sau đó chọn **Add environment variable**.

6. Điền các thông tin sau để cấu hình kết nối an toàn đến Database:

   | Key | Value |
   |-----|-------|
   | `DB_SECRET_NAME` | `ChronoGame/PartnerDB/Credentials` |

7. Nhấn **Save** để lưu cấu hình.

![Cấu hình Environment Variables cho Lambda](/images/5-Workshop/5.5-Policy/850.png)

8. Chuyển sang tab **Test**. Tại mục **Test event**, chọn **Create new event** và đặt tên cho sự kiện là `Test-connection`. Sau đó nhấn **Save**

9. Click vào nút **Test** màu cam để chạy thử hàm Lambda.

10. Mở rộng mục **Execution results (Details)** để xem kết quả kiểm thử. Nếu hệ thống trả về `statusCode: 200` và `body: "Connected successfully."` cùng với thông báo **"Executing function: succeeded"** màu xanh, nghĩa là bạn đã cấu hình thành công.

![Cấu hình Environment Variables cho Lambda](/images/5-Workshop/5.5-Policy/936.png)

---

### 2. Cấu hình Lambda PostMatchWorker
Bước 1: Trong giao diện chi tiết của hàm **Lambda PostMatchWorker-function**, chuyển sang tab **Configuration**, chọn mục **Environment variables** và click **Edit** để cấu hình các biến môi trường kết nối cơ sở dữ liệu (như `DB_ACCESS_KEY_ID`, `DB_REGION`, `DB_SECRET_ACCESS_KEY`)
![Cấu hình Environment Variables cho Lambda](/images/5-Workshop/5.5-Policy/postMatch.png)

Bước 2: Chuyển sang tab **Test**, tại mục **Test event** chọn **Create new event** (hoặc **Edit saved event**), đặt tên sự kiện là **test-postMatchWorker**, dán đoạn mã JSON giả lập SQS message vào khung **Event JSON** rồi nhấn **Save** để lưu.
![Cấu hình Environment Variables cho Lambda](/images/5-Workshop/5.5-Policy/723.png)

Bước 3: Click nút **Test** màu cam để chạy thử hàm. Sau đó, mở rộng mục **Execution results (Details)** để kiểm tra: nếu thấy thông báo **"Executing function: succeeded"** màu xanh và kết quả trả về gồm `statusCode: 200` cùng `body: "Post match processing complete."` , tức là luồng xử lý của bạn đã hoạt động chính xác.
![Cấu hình Environment Variables cho Lambda](/images/5-Workshop/5.5-Policy/641.png)

---

### 3. Cấu hình Lambda ProcessGameEngine
Bước 1: Trong giao diện chi tiết của hàm **Lambda ProcessGameEngine-function**, tại tab **Code**, tiến hành cập nhật mã nguồn bằng cách chọn upload từ tệp **.zip**. Khi quá trình tải lên hoàn tất, hệ thống sẽ hiển thị thông báo màu xanh **"Successfully updated the function ProcessGameEngine-function"** để xác nhận.
![Cấu hình Environment Variables cho Lambda](/images/5-Workshop/5.5-Policy/842.png)

Bước 2: Chuyển sang cấu hình kiểm thử (thường ở tab **Test**), tạo một sự kiện mới và đặt tên tại ô **Event name** là **Test-EndTurn**. Trong khung **Event JSON**, dán đoạn mã giả lập một request gửi từ API Gateway (chứa các thông tin như connectionId và body chứa hành động END_TURN của người chơi). Sau đó nhấn **Save** để chuẩn bị cho quá trình chạy thử.
![Cấu hình Environment Variables cho Lambda](/images/5-Workshop/5.5-Policy/708.png)

### 4. Tương tự thực hiện các Lambda còn lại
- Lambda SaveDeck, StartMatch, HandleTimeout.

---

Bước 1: Truy cập dịch vụ **Amazon SQS** và chọn **Create queue**. Tại mục **Details**, chọn loại hàng đợi là **Standard** và đặt tên tại ô **Name** là **Match-Result-queue**. Các thông số Configuration khác giữ nguyên mặc định.
![Cấu hình Environment Variables cho Lambda](/images/5-Workshop/5.5-Policy/246.png)

Bước 2: Cuộn xuống mục **Encryption**, đảm bảo Server-side encryption được đặt là **Enabled** với loại key là **Amazon SQS key (SSE-SQS)**.
![Cấu hình Environment Variables cho Lambda](/images/5-Workshop/5.5-Policy/131.png)

Bước 3: Tại mục **Access policy**, chọn phương thức **Basic** và thiết lập quyền gửi/nhận message mặc định dành cho **Only the queue owner**.
![Cấu hình Environment Variables cho Lambda](/images/5-Workshop/5.5-Policy/108.png)

Bước 4: Cuộn xuống cuối trang, giữ nguyên cài đặt **Disabled** cho phần **Redrive allow policy** và **Dead-letter queue**. Sau đó tiến hành tạo hàng đợi.
![Cấu hình Environment Variables cho Lambda](/images/5-Workshop/5.5-Policy/313.png)

Bước 5: Chuyển sang giao diện của hàm **Lambda PostMatchWorker-function**. Tại tab **Configuration**, chọn mục **Triggers** ở menu trái và nhấn **Add trigger**. Trong menu thả xuống, chọn nguồn là **SQS**, tìm và chọn **Match-Result-queue** vừa tạo, thiết lập **Batch size** là **10** và tích chọn **Activate trigger**. Sau đó nhấn nút **Add**.
![Thêm SQS Trigger cho Lambda](/images/5-Workshop/5.5-Policy/351.png)

Bước 6: Trở lại tab **Configuration**, bạn sẽ thấy **SQS trigger** vừa được thêm vào danh sách với trạng thái hiển thị là **Creating** (Đang khởi tạo). Đợi vài phút để AWS thiết lập kết nối, trạng thái này sẽ chuyển sang hoạt động.
![Thêm SQS Trigger cho Lambda](/images/5-Workshop/5.5-Policy/324.png)

