https://github.com/ghuntley/how-to-build-a-coding-agent

Mã nguồn **`how-to-build-a-coding-agent`** trên GitHub là một **kho lưu trữ hướng dẫn thực hành (workshop/tutorial)** dùng để dạy lập trình viên cách tự xây dựng một **AI Coding Agent** (tác nhân lập trình tự hành) từ con số 0 chỉ với vài trăm dòng mã. Thay vì đóng vai trò là một phần mềm hoàn chỉnh để cài đặt và dùng ngay cho công việc hằng ngày, dự án này giúp giải mã cơ chế vận hành cốt lõi đằng sau các công cụ nổi tiếng như Claude Code, Cursor Agent hay Devin.

*(Lưu ý: Vì bạn mới cung cấp tên kho mã mà chưa có tên tác giả hoặc đường dẫn đầy đủ, phần giải thích dưới đây dựa trên dự án nổi tiếng nhất mang tên này là **`ghuntley/how-to-build-a-coding-agent`** của Geoffrey Huntley, cùng các kho mã hướng dẫn cùng tên trên GitHub).*

## Mục đích sử dụng chính

* **Giải mã "hộp đen" của Coding Agent:** Nhiều lập trình viên cho rằng các công cụ tự động viết và sửa code cần kiến trúc trí tuệ nhân tạo cực kỳ đồ sộ. Kho mã này chứng minh điều ngược lại: về bản chất, một coding agent chỉ là một vòng lặp gọi API mô hình ngôn ngữ lớn (LLM) kết hợp với cơ chế gọi hàm (**Tool Use / Function Calling**).
* **Học từng bước qua mã nguồn tăng tiến:** Các kho mã dạng này thường chia bài học thành nhiều mốc hoặc nhiều tệp từ đơn giản đến nâng cao (từ một chatbot dòng lệnh bình thường → thêm khả năng đọc tệp → liệt kê thư mục → chỉnh sửa mã nguồn → chạy lệnh hệ thống).
* **Làm bộ khung khởi đầu (Boilerplate):** Lập trình viên có thể sao chép (fork) mã nguồn này để tự phát triển một agent nội bộ chuyên biệt cho dự án hoặc quy trình làm việc của riêng mình mà không bị ràng buộc bởi các framework nặng nề.

## Các thành phần cốt lõi trong mã nguồn

Khi đọc cấu trúc của một dự án `how-to-build-a-coding-agent`, bạn sẽ thấy hệ thống xoay quanh 4 cơ chế kỹ thuật chính:

1. **Vòng lặp tự hành (The Agentic Loop):** Chương trình gửi yêu cầu của người dùng kèm danh sách mô tả các công cụ (tools) tới API của LLM (như Anthropic Claude hoặc OpenAI). Khi mô hình trả về tín hiệu muốn dùng một công cụ (`tool_use`), mã nguồn sẽ thực thi hàm đó trên máy tính cục bộ và gửi ngược kết quả (`tool_result`) lại cho mô hình. Vòng lặp này tiếp diễn cho đến khi LLM xác nhận đã hoàn thành nhiệm vụ.
2. **Công cụ đọc và khám phá dự án (`read_file`, `list_files`):** Cho phép AI tự duyệt cây thư mục và đọc nội dung các tệp mã nguồn liên quan để hiểu ngữ cảnh dự án thay vì bắt người dùng sao chép thủ công.
3. **Công cụ chỉnh sửa mã nguồn (`edit_file` / `write_file`):** Cung cấp hàm để AI tạo tệp mới hoặc thay thế chính xác từng đoạn mã (search-and-replace) ngay trên ổ cứng của bạn.
4. **Công cụ thực thi dòng lệnh (`bash` / `run_command`):** Cho phép agent tự chạy lệnh biên dịch, kiểm thử (unit test) hoặc kiểm tra lỗi, từ đó tự đọc thông báo lỗi và sửa lại code nếu chạy thất bại.

## So sánh với dự án Devika (`stitionai/devika`)

Nếu đặt cạnh kho mã **`stitionai/devika`** bạn vừa xem trước đó, hai dự án này phục vụ hai mục đích hoàn toàn khác nhau:

| Tiêu chí | `how-to-build-a-coding-agent` | `stitionai/devika` |
| --- | --- | --- |
| **Mục tiêu chính** | Tài liệu học tập, hướng dẫn tự code agent từ đầu | Nền tảng ứng dụng hoàn chỉnh mô phỏng Devin |
| **Độ phức tạp mã nguồn** | Tối giản (thường chỉ vài trăm dòng code trong một vài tệp) | Đồ sộ (tách biệt Backend Python, Frontend Svelte, Docker) |
| **Giao diện người dùng** | Chạy trực tiếp trên cửa sổ dòng lệnh (CLI / Terminal) | Giao diện web đầy đủ (Chat UI, theo dõi trạng thái trực quan) |
| **Tính năng mở rộng** | Tập trung vào vòng lặp LLM + thao tác tệp/terminal cơ bản | Tích hợp thêm trình duyệt web Playwright, tìm kiếm, deploy |

Nếu bạn đang nhắm tới một kho mã cụ thể có cùng tên này (do có nhiều tác giả trên GitHub đặt tên repo là `how-to-build-a-coding-agent` bằng các ngôn ngữ khác nhau như Go, Python hoặc TypeScript), hãy gửi kèm **đường dẫn URL đầy đủ** để mình phân tích chi tiết ngôn ngữ lập trình, cấu trúc thư mục và cách chạy của đúng kho mã đó.
