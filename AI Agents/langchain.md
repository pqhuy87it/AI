https://github.com/langchain-ai/langchain

Source code tại `[https://github.com/langchain-ai/langchain](https://github.com/langchain-ai/langchain)` là **LangChain** — một **framework phát triển ứng dụng AI và tác nhân tự hành (AI Agents) mã nguồn mở** bằng ngôn ngữ **Python** (hiện đạt khoảng 149,5 nghìn sao trên GitHub, phát hành theo giấy phép **MIT**).

Nếu như gọi trực tiếp API của các mô hình ngôn ngữ lớn (LLM) giống như việc thao tác với từng linh kiện rời rạc, thì LangChain đóng vai trò như bộ khung lắp ráp tiêu chuẩn, giúp lập trình viên kết nối mô hình AI với dữ liệu nội bộ, công cụ bên ngoài và các quy trình xử lý phức tạp một cách nhanh chóng.

## Các chức năng và bài toán giải quyết chính

Kho mã này (tập trung chủ yếu trong thư mục `libs/`) cung cấp các gói thư viện cốt lõi (`langchain-core`, `langchain`) cùng hàng trăm gói tích hợp đối tác để giải quyết 4 bài toán nền tảng khi làm ứng dụng AI:

* **Chuẩn hóa giao tiếp đa mô hình (Model Interoperability):** Cung cấp một giao diện lập trình thống nhất (ví dụ thông qua hàm `init_chat_model`). Bạn có thể chuyển đổi qua lại giữa các mô hình của OpenAI, Anthropic (Claude), Google (Gemini) hay mô hình chạy cục bộ chỉ bằng cách đổi tên cấu hình mà không phải viết lại toàn bộ mã nguồn ứng dụng.
* **Kết nối dữ liệu thực tế và RAG (Retrieval-Augmented Generation):** Trang bị đầy đủ các thành phần từ nạp tài liệu (PDF, cơ sở dữ liệu, trang web), chia nhỏ văn bản, tạo vector nhúng (Embeddings), lưu trữ trên Vector Stores cho đến truy xuất thông tin (Retrievers). Nhờ đó, AI có thể trả lời chính xác dựa trên kho dữ liệu riêng của doanh nghiệp thay vì chỉ dựa vào kiến thức huấn luyện sẵn.
* **Xây dựng AI Agents và gọi công cụ (Tool Calling):** Cho phép mô hình AI tương tác với hệ thống bên ngoài thông qua các hàm chức năng như tìm kiếm web, truy vấn SQL, gọi API hoặc đọc/ghi tệp tin, từ đó tự lập luận và thực hiện chuỗi hành động để hoàn thành yêu cầu.
* **Lắp ghép quy trình linh hoạt (Composable Chains):** Cho phép xâu chuỗi các bước xử lý (định dạng câu lệnh Prompt $\rightarrow$ gọi mô hình LLM $\rightarrow$ phân tích cấu trúc dữ liệu đầu ra) thành một luồng chạy nhất quán và dễ mở rộng.

## Hệ sinh thái mở rộng của LangChain

Ngoài thư viện nền tảng `langchain`, trang tài liệu của kho mã này cũng định vị rõ các mảnh ghép đi kèm trong cùng hệ sinh thái:

* **LangGraph:** Framework điều phối ở mức độ thấp hơn, chuyên dùng để xây dựng các AI Agent phức tạp có quản lý trạng thái (stateful), vòng lặp chặt chẽ hoặc hệ thống nhiều tác nhân phối hợp (multi-agent).
* **Deep Agents:** Gói thư viện bậc cao xây dựng trên nền tảng LangChain, tích hợp sẵn các năng lực phổ biến như tự lập kế hoạch (planning), chia việc cho tác nhân con (subagents) và thao tác hệ thống tệp.
* **LangSmith:** Nền tảng hỗ trợ theo dõi (observability), gỡ lỗi (debugging), đánh giá chất lượng câu trả lời (evals) và triển khai các ứng dụng AI lên môi trường thực tế (production).
* **LangChain.js:** Phiên bản tương đương dành cho hệ sinh thái JavaScript và TypeScript.

## Vị trí của LangChain so với hai kho mã bạn đã xem trước đó

Cả ba dự án bạn vừa tìm hiểu đều xoay quanh việc xây dựng AI Agent, nhưng nằm ở ba tầng hoàn toàn khác nhau:

| Kho mã GitHub | Bản chất | Đối tượng và mục đích sử dụng |
| --- | --- | --- |
| **`how-to-build-a-coding-agent`** | **Tài liệu hướng dẫn (Tutorial)** | Dành cho lập trình viên muốn tự viết vòng lặp agent từ con số 0 bằng code thuần để hiểu cơ chế hoạt động cốt lõi. |
| **`langchain-ai/langchain`** | **Bộ khung phát triển (Framework / SDK)** | Dành cho kỹ sư phần mềm dùng làm nền tảng thư viện để xây dựng các sản phẩm AI, chatbot RAG hoặc AI Agent hoàn chỉnh. |
| **`stitionai/devika`** | **Ứng dụng hoàn chỉnh (End-user App)** | Một phần mềm đóng gói sẵn giao diện web để người dùng nhập yêu cầu và để AI tự lập trình. |
