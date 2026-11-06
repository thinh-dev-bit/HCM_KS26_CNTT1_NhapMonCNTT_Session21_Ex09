# Phần 1: Thiết kế Prompt 3 Thành phần Chuẩn hóa

**1. Phân tích nguyên nhân AI đưa ra câu trả lời sai lệch (AI Hallucination):**
*   Câu hỏi của nhân viên mới (`AI ơi phần mềm trạm sạc của tôi bị hỏng rồi làm sao đây?`) hoàn toàn thiếu dữ kiện kỹ thuật (không có tên hệ điều hành, không có bối cảnh, không có mã lỗi nguyên văn).
*   Khi bị thiếu dữ kiện, Trợ lý AI sẽ rơi vào trạng thái **Ảo giác AI (AI Hallucination)**. Nó sẽ bắt đầu đoán mò, tự bịa ra các nguyên nhân chung chung (như lỗi phần cứng, dây điện) thay vì xác định đúng đây là một lỗi phần mềm (lỗi phân quyền). Việc này không những không giải quyết được vấn đề mà còn có thể xúi giục kỹ thuật viên làm hỏng thêm thiết bị.

**2. Soạn thảo Prompt 3 Thành phần chuẩn kỹ sư:**
Để nhận được đáp án chính xác tuyệt đối, kỹ thuật viên cần gửi đoạn Prompt được cấu trúc theo 3 khối sau cho AI:

> Tôi là nhân viên trực ban đang vận hành hệ thống phần mềm Kiosk sạc VoltCharge trên nền tảng Windows 11. Phần mềm đột ngột dừng cung cấp điện và văng ra lỗi trên màn hình Terminal. Dưới đây là nguyên văn thông báo lỗi:

> `[TERMINAL ERROR LOG - VOLTCHARGE CHARGER #04]`

> `2026-09-21 21:15:30 [FATAL] ControllerDaemon failed to start.`

 `System.UnauthorizedAccessException: Access to the path 'C:\Program Files\VoltCharge\data\session.log' is denied.`

> `  at System.IO.FileStream.ValidateFileHandle(SafeFileHandle handle)`

> `  at VoltCharge.Core.SessionManager.Initialize()`

> `InnerException: None. Process terminated with exit code 5 (0x00000005).`

> `Suggestion: Run the application with elevated administrator privileges or verify NTFS file permissions.`

> Hãy đóng vai một chuyên gia IT, giải thích nguyên nhân gốc rễ của lỗi này bằng tiếng Việt đơn giản, đời thường cho người mới. Sau đó, hướng dẫn 3 bước cụ thể trên Windows 11 để khắc phục tại chỗ.

---

# Phần 2: Giải mã Lỗi & Kịch bản Khắc phục

**3. Trợ lý AI giải mã lỗi:**
*   **Bản dịch:** Lỗi *UnauthorizedAccessException: Access to the path... is denied* có nghĩa là "Hệ điều hành từ chối quyền truy cập vào đường dẫn...".
*   **Giải thích nguyên nhân:** Phần mềm VoltCharge của trạm sạc đang cố gắng ghi nhật ký hoạt động (file `session.log`) vào một thư mục được bảo vệ nghiêm ngặt của Windows 11 (nằm trong ổ `C:\Program Files\...`). Vì phần mềm đang chạy dưới quyền nhân viên bình thường (không có chìa khóa Quản trị viên cao nhất), hệ điều hành Windows đã lập tức chặn hành động ghi file này lại để tự vệ, dẫn đến phần mềm bị treo cứng.

**4. Kịch bản 3 bước khắc phục tại chỗ (Trên Windows 11):**
Để cấp lại quyền và đưa trạm sạc hoạt động trở lại, hãy làm theo 3 bước sau:
*   **Bước 1:** Đóng hoàn toàn phần mềm VoltCharge đang bị treo hoặc khởi động lại máy tính của Kiosk.
*   **Bước 2 (Chạy quyền Quản trị):** Nhấp chuột phải vào biểu tượng phần mềm VoltCharge trên màn hình, chọn **Run as administrator** (Chạy dưới quyền Quản trị viên) và chọn *Yes* nếu hộp thoại UAC hiện lên. Phần mềm sẽ ngay lập tức có quyền ghi file và hoạt động lại bình thường.

**5. Xử lý Bẫy an ninh (Chứa mã bí mật API_KEY):**
*   **Hành động bắt buộc:** Nếu trong đoạn log có chứa chuỗi bí mật như `API_KEY: volt_secret_token_889922`, kỹ thuật viên **bắt buộc phải xóa hoặc che giấu chuỗi này** (thay thế bằng `API_KEY: ***` hoặc `your_api_key_here`) TRƯỚC KHI dán vào khung chat của ChatGPT/Gemini.
*   **Lý do:** Tuân thủ nguyên tắc an toàn số 3: Tuyệt đối không gửi Secret/API Key lên máy chủ AI. Các nền tảng AI công cộng có thể lưu trữ lịch sử chat của bạn trên máy chủ của họ hoặc dùng dữ liệu đó để huấn luyện mô hình. Nếu bạn dán nguyên văn khóa bí mật lên, tài sản và quyền điều khiển hệ thống sạc của công ty có nguy cơ bị rò rỉ ra toàn cầu (Data Leak).