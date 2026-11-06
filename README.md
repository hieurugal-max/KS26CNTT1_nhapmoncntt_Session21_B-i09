# Bài 9: Xây dựng Prompt 3 thành phần giải mã thông báo lỗi trạm sạc

## Phần 1: Thiết kế Prompt 3 thành phần

### 1. Vì sao câu hỏi ban đầu khiến AI trả lời sai?

- Câu hỏi quá chung chung, không nêu rõ thiết bị, hệ điều hành và lỗi đang gặp.
- Không cung cấp thông báo lỗi cụ thể nên AI khó xác định vấn đề.
- AI phải tự suy đoán nguyên nhân, dễ dẫn đến câu trả lời sai hoặc không sát thực tế.

### 2. Prompt 3 thành phần

**Context – Ngữ cảnh:**

Tôi là nhân viên kỹ thuật đang trực tại trạm sạc VoltCharge số 04. Máy sử dụng Windows 11. Khi khởi động phần mềm quản lý trạm sạc, chương trình không chạy được.

**Raw Error – Lỗi nguyên văn:**

[FATAL] ControllerDaemon failed to start.

System.UnauthorizedAccessException: Access to the path 'C:\Program Files\VoltCharge\data\session.log' is denied.

InnerException: None.

Process terminated with exit code 5 (0x00000005).

Suggestion: Run the application with elevated administrator privileges or verify NTFS file permissions.

**Instruction – Yêu cầu:**

Hãy giải thích lỗi trên bằng tiếng Việt dễ hiểu, chỉ ra nguyên nhân và hướng dẫn 3 bước khắc phục trên Windows 11. Ưu tiên cách làm an toàn, không hướng dẫn những thao tác không liên quan đến lỗi.

## Phần 2: Giải mã lỗi và khắc phục

### 3. Giải thích lỗi và nguyên nhân

Lỗi `UnauthorizedAccessException: Access is denied` có nghĩa là phần mềm không được Windows cho phép truy cập hoặc ghi dữ liệu vào file `session.log`.

Nguyên nhân có thể là tài khoản chạy phần mềm chưa có đủ quyền ghi vào thư mục `Program Files`, khiến chương trình không thể khởi động.

### 4. Ba bước khắc phục trên Windows 11

- **Bước 1:** Nhấn chuột phải vào phần mềm VoltCharge, chọn **Run as administrator**, sau đó chọn Yes nếu được hỏi.
- **Bước 2:** Nếu vẫn lỗi, vào `C:\Program Files\VoltCharge`, nhấn chuột phải thư mục `data` → Properties → Security → Edit. Chọn đúng tài khoản chạy phần mềm và cấp quyền Write hoặc Modify theo hướng dẫn của quản trị viên.
- **Bước 3:** Nhấn Apply → OK, mở lại phần mềm và kiểm tra xem trạm sạc đã hoạt động bình thường chưa.

**Lưu ý:** Không cấp quyền Full Control cho Everyone vì có thể làm giảm an toàn hệ thống. Chỉ thay đổi quyền khi được người có trách nhiệm cho phép.

### 5. Xử lý API Key trước khi gửi cho AI

Nếu trong log có dòng:

`API_KEY: volt_secret_token_889922`

Cần xóa hoặc che mã bí mật trước khi gửi, thay bằng:

`API_KEY: [REDACTED]`

API Key được dùng để truy cập hệ thống. Nếu bị lộ, người khác có thể lợi dụng để truy cập trái phép. Nếu mã đã bị lộ, cần báo quản trị viên thu hồi và tạo mã mới.
