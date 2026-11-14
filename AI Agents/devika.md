https://github.com/stitionai/devika

Source code tại `[https://github.com/stitionai/devika](https://github.com/stitionai/devika)` là **Devika** — một **Kỹ sư phần mềm AI tự hành mã nguồn mở (Agentic AI Software Engineer)**, được phát triển như một giải pháp thay thế mã nguồn mở cho **Devin** (của Cognition AI). Phát hành theo giấy phép **MIT** (hiện đạt khoảng 19,5 nghìn sao trên GitHub), dự án này cho phép lập trình viên giao các yêu cầu phát triển phần mềm ở mức tổng quát bằng ngôn ngữ tự nhiên để AI tự động lập kế hoạch, tra cứu tài liệu trên internet và viết mã nguồn hoàn chỉnh cho dự án.

## Chức năng và cơ chế hoạt động chính

Thay vì chỉ gợi ý từng đoạn mã ngắn như các công cụ autocomplete thông thường, Devika vận hành như một tác nhân (agent) có khả năng tự điều phối quy trình làm việc qua các bước:

* **Lập kế hoạch và suy luận (Planning & Reasoning):** Tiếp nhận yêu cầu đầu vào (ví dụ: tạo tính năng mới, sửa lỗi, hoặc khởi tạo một ứng dụng từ đầu) và tự chia nhỏ thành các bước thực thi tuần tự.
* **Trích xuất từ khóa và duyệt web tự động:** Sử dụng thuật toán trích xuất từ khóa theo ngữ cảnh kết hợp với trình duyệt tự động để tìm kiếm tài liệu kỹ thuật, thư viện hoặc cách xử lý lỗi trên web trước khi viết code.
* **Sinh mã nguồn đa ngôn ngữ và quản lý theo dự án:** Viết mã bằng nhiều ngôn ngữ lập trình khác nhau, tổ chức tệp tin theo từng dự án riêng biệt và cho phép người dùng theo dõi, chỉnh sửa hoặc bổ sung yêu cầu tiếp theo.
* **Theo dõi trạng thái tác nhân trực quan:** Cung cấp giao diện trò chuyện (chat UI) hiển thị trực tiếp tiến trình suy nghĩ, các bước đang thực hiện và mã nguồn được tạo ra theo thời gian thực.

## Mô hình AI và công cụ tích hợp

Devika hỗ trợ linh hoạt nhiều nhà cung cấp mô hình ngôn ngữ lớn (LLM) lẫn dịch vụ bên ngoài thông qua trang cài đặt hoặc tệp cấu hình `config.toml`:

* **Mô hình AI đám mây (Cloud LLMs):** Hỗ trợ API của **Anthropic Claude 3** (được nhóm tác giả khuyến nghị để đạt hiệu suất tốt nhất), **OpenAI (GPT-4)**, **Google Gemini**, **Mistral** và **Groq**.
* **Mô hình chạy cục bộ (Local LLMs):** Hỗ trợ kết nối với **Ollama** và **LM Studio** để chạy các mô hình mã nguồn mở ngay trên máy cá nhân.
* **Công cụ tìm kiếm & Triển khai:** Tích hợp **Bing Search API**, **Google Search API**, **DuckDuckGo** để phục vụ tác vụ nghiên cứu thông tin, đồng thời hỗ trợ **Netlify API** để triển khai (deploy) các dự án web.

## Kiến trúc kỹ thuật và cách vận hành

Mã nguồn được chia thành hai phần rõ rệt (Backend và Frontend) cùng các tệp cấu hình triển khai nhanh:

* **Backend (`devika.py`, thư mục `src/`):** Viết bằng **Python** (yêu cầu phiên bản từ 3.10 đến dưới 3.12), sử dụng trình quản lý gói `uv` và thư viện **Playwright** để điều khiển trình duyệt phục vụ việc thu thập thông tin trên web.
* **Frontend (thư mục `ui/`):** Giao diện web xây dựng chủ yếu bằng **Svelte** và JavaScript, chạy trên nền tảng **Node.js** (từ phiên bản 18 trở lên) kết hợp với **Bun** (mặc định mở tại `[http://127.0.0.1:3001](http://127.0.0.1:3001)`).
* **Hỗ trợ Docker:** Kho mã cung cấp sẵn `docker-compose.yaml`, `devika.dockerfile` và `app.dockerfile` để dựng nhanh toàn bộ hệ thống trong môi trường container.

## Lưu ý thực tế trước khi sử dụng

Theo thông báo trực tiếp từ nhóm phát triển trong tệp `README.md`, dự án hiện vẫn ở **giai đoạn thử nghiệm rất sớm (early experimental stage)**, còn nhiều tính năng chưa hoàn thiện hoặc phát sinh lỗi trong quá trình chạy thực tế. Bên cạnh đó, tác giả cũng giới thiệu **Opcode** là thế hệ kế nhiệm tiếp theo của Devika. Vì vậy, kho mã nguồn này phù hợp nhất để tham khảo kiến trúc xây dựng AI Agent, thử nghiệm nghiên cứu hoặc đóng góp mã nguồn mở, thay vì dùng làm công cụ lập trình ổn định trong môi trường sản xuất (production).
