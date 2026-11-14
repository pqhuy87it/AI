https://github.com/langgenius/dify

**[langgenius/dify](https://github.com/langgenius/dify)** là một nền tảng mã nguồn mở phát triển ứng dụng LLM và **LLMOps toàn diện (all-in-one)**, kết hợp trình thiết kế AI workflow kéo-thả, hệ thống quản lý tri thức RAG, Agent tự chủ, quản lý mô hình và hệ thống giám sát vận hành (observability) trong cùng một workspace cộng tác ([GitHub - langgenius/dify](https://github.com/langgenius/dify), [DeepWiki](https://deepwiki.com/langgenius/dify)).

Nếu đặt cạnh `langflow` ở câu hỏi trước: **Langflow** thiên về một *visual studio dành cho lập trình viên Python/LangChain* để lắp ghép và tinh chỉnh sâu code của từng node, còn **[Dify](https://www.promptquorum.com/power-local-llm/dify-ai-workflow-builder-review)** được thiết kế theo hướng **Backend-as-a-Service (BaaS) hoàn chỉnh để đưa sản phẩm lên Production ngay** — tích hợp sẵn từ khâu quản lý kho tài liệu (Knowledge Base), giao diện Web App/Widget nhúng cho người dùng cuối, quản lý phân quyền workspace cho đến theo dõi log và gán nhãn dữ liệu thực tế ([GitHub - langgenius/dify](https://github.com/langgenius/dify), [PromptQuorum](https://www.promptquorum.com/power-local-llm/dify-ai-workflow-builder-review)).

### 6 trụ cột chức năng chính của Dify

* **Thiết kế AI Workflow & Chatflow trực quan:** Cung cấp bảng vẽ kéo thả các khối chức năng (LLM, phân nhánh điều kiện *Question Classifier*, gọi HTTP Request, chạy code thực thi, tạm dừng chờ người dùng nhập liệu *Human Input* hoặc rẽ nhánh theo quyết định của Agent) và kiểm thử trực tiếp trong khung *Debug and Preview* ([GitHub - langgenius/dify](https://github.com/langgenius/dify), [DeepWiki](https://deepwiki.com/langgenius/dify)).
* **Hệ thống RAG Pipeline đóng gói sẵn (Knowledge Base):** Tự động hóa toàn bộ quy trình bóc tách văn bản từ PDF, PPT, tài liệu, chia nhỏ (chunking), tạo vector embedding, đánh chỉ mục và truy xuất dữ liệu (hỗ trợ hybrid search, lọc metadata) mà không cần bạn phải tự nối từng khối xử lý file từ đầu ([GitHub - langgenius/dify](https://github.com/langgenius/dify), [PromptQuorum](https://www.promptquorum.com/power-local-llm/dify-ai-workflow-builder-review)).
* **Autonomous Agent có Sandbox riêng:** Cho phép tạo các Agent tự chủ có môi trường thực thi cô lập (sandbox) để tự chạy lệnh shell, xử lý file và gọi hơn 50 công cụ tích hợp sẵn (Google Search, tạo ảnh, gọi MCP server hoặc plugin từ [Dify Marketplace](https://marketplace.dify.ai/creator/langgenius?publisher_type=organization)) ([GitHub - langgenius/dify](https://github.com/langgenius/dify)).
* **Quản lý hơn 100+ mô hình (Model Management):** Kết nối tập trung hàng trăm LLM thương mại lẫn mã nguồn mở (OpenAI, Claude, Gemini, DeepSeek, Llama 3, Mistral, Ollama/vLLM tự host) và cho phép so sánh hiệu năng giữa các model ngay trên *Prompt IDE* ([GitHub - langgenius/dify](https://github.com/langgenius/dify), [DeepWiki](https://deepwiki.com/langgenius/dify)).
* **Xuất bản đa dạng & Backend-as-a-Service:** Mỗi luồng tạo ra đều tự động có sẵn một trang Web App dùng ngay, mã nhúng chat widget vào website, REST API chuẩn hóa (kèm SDK) để gọi từ ứng dụng di động/backend riêng, hoặc xuất bản thành công cụ chuẩn MCP ([GitHub - langgenius/dify](https://github.com/langgenius/dify), [PromptQuorum](https://www.promptquorum.com/power-local-llm/dify-ai-workflow-builder-review)).
* **Giám sát vận hành LLMOps:** Ghi nhận toàn bộ lịch sử hội thoại, chi phí token, độ trễ và cho phép gán nhãn (annotation) trên dữ liệu chạy thực tế để cải thiện dần chất lượng câu trả lời, đồng thời kết nối trực tiếp với Langfuse, Opik và Arize Phoenix ([GitHub - langgenius/dify](https://github.com/langgenius/dify), [DeepWiki](https://deepwiki.com/langgenius/dify)).

### Cấu trúc Monorepo của mã nguồn

Repository **[langgenius/dify](https://github.com/langgenius/dify)** được tổ chức theo cấu trúc monorepo gồm nhiều dịch vụ tách biệt phối hợp qua Docker network ([DeepWiki](https://deepwiki.com/langgenius/dify)):

| Thư mục / Thành phần | Công nghệ sử dụng | Vai trò trong hệ thống |
| --- | --- | --- |
| `api/` | Python (Flask, Celery, SQLAlchemy, Pydantic) | Dịch vụ backend trung tâm xử lý REST API, điều phối chạy workflow, xử lý ngầm việc bóc tách tài liệu RAG qua Celery worker và quản lý cấu hình mô hình ([DeepWiki](https://deepwiki.com/langgenius/dify)). |
| `web/` & `packages/` | TypeScript (Next.js, React, pnpm workspaces) | Giao diện quản trị Dify Studio (canvas thiết kế workflow, quản lý Knowledge Base, Prompt IDE, bảng theo dõi logs) và giao diện Web App phía người dùng cuối ([GitHub](https://github.com/langgenius/dify), [DeepWiki](https://deepwiki.com/langgenius/dify)). |
| `dify-agent/` & `dify-agent-runtime/` | Python & Go (`local_sandbox` / `shellctl`) | Bộ runtime thực thi độc lập và môi trường sandbox viết bằng Go giúp cô lập an toàn các lệnh shell hoặc đoạn code do AI Agent sinh ra ([DeepWiki](https://deepwiki.com/langgenius/dify)). |
| `cli/` | CLI (`difyctl`) | Công cụ dòng lệnh giúp tạo, import/export các gói ứng dụng (`.ifpkg`) và triển khai tự động các ứng dụng Workflow/Chatflow ([PromptQuorum](https://www.promptquorum.com/power-local-llm/dify-ai-workflow-builder-review), [DeepWiki](https://deepwiki.com/langgenius/dify)). |
| `sdks/` | Python, Node.js, PHP... | Các bộ thư viện client giúp tích hợp nhanh API của Dify vào ứng dụng có sẵn của bạn ([GitHub - langgenius/dify](https://github.com/langgenius/dify)). |
| `docker/` | Docker Compose, SSRF Proxy, Vector DBs | Cấu hình khởi chạy toàn bộ cụm dịch vụ (API, Worker, Web, PostgreSQL, Redis, Vector DB, Sandbox, SSRF Proxy chống tấn công mạng nội bộ) ([GitHub](https://github.com/langgenius/dify), [DeepWiki](https://deepwiki.com/langgenius/dify)). |

### Lưu ý về Giấy phép (License) và Cách chạy thử

* **Giấy phép sử dụng (Modified Apache 2.0):** Bạn được toàn quyền tự host (self-host) miễn phí cho nội bộ công ty hoặc dùng làm backend AI cho sản phẩm thương mại của riêng mình; tuy nhiên, giấy phép **không cho phép** bạn dùng mã nguồn này để mở dịch vụ SaaS đa người thuê (multi-tenant SaaS bán lại quyền truy cập Dify studio cho nhiều khách hàng) hoặc xóa logo bản quyền trên giao diện frontend nếu chưa có thỏa thuận thương mại với LangGenius ([PromptQuorum](https://www.promptquorum.com/power-local-llm/dify-ai-workflow-builder-review), [GitHub - langgenius/dify](https://github.com/langgenius/dify)).
* **Cách tự dựng nhanh bằng Docker Compose (Yêu cầu tối thiểu 2 CPU, 4 GB RAM):**

```bash
cd docker
cp .env.example .env
# Tạo mã token bảo mật bằng lệnh `openssl rand -hex 32` và điền vào biến DIFY_AGENT_LOCAL_SANDBOX_AUTH_TOKEN trong file .env
docker compose up -d

```

Sau khi các container khởi động xong, bạn chỉ cần truy cập `http://localhost/install` để tạo tài khoản quản trị và sử dụng ([GitHub - langgenius/dify](https://github.com/langgenius/dify)).
