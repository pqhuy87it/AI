https://github.com/langflow-ai/langflow

**[langflow-ai/langflow](https://github.com/langflow-ai/langflow)** là một nền tảng mã nguồn mở (low-code/visual studio) viết bằng Python và TypeScript, dùng để **xây dựng, kiểm thử và triển khai các AI Agent cũng như luồng xử lý dữ liệu (RAG, multi-agent workflows) bằng giao diện kéo-thả dạng node** ([DeepWiki](https://deepwiki.com/langflow-ai/langflow), [Railway](https://railway.com/deploy/langflow-4)).

Khác với hai repo trước (`ponytail` và `agency-agents`) vốn là các bộ quy tắc/prompt cài vào IDE để hỗ trợ viết code, [Langflow](https://github.com/langflow-ai/langflow) là một **hệ thống phần mềm full-stack độc lập** giúp bạn lắp ghép các thành phần AI trên một bảng vẽ trực quan (canvas) rồi xuất bản ngay thành REST API hoặc MCP Server để ứng dụng khác gọi tới ([Langflow Blog](https://www.langflow.org/blog/build-mcp-server-langflow/)).

### Các tính năng cốt lõi của Langflow

* **Thiết kế luồng AI trực quan nhưng tùy biến sâu bằng Python:** Cho phép kéo thả các khối (LLM, Prompt, Vector Database, API, Tools) và nối dây dữ liệu trên giao diện React Flow, đồng thời mở toàn quyền chỉnh sửa mã nguồn Python bên dưới từng khối để tùy biến logic ([GitHub - langflow-ai/langflow](https://github.com/langflow-ai/langflow)).
* **Độc lập với mô hình và cơ sở dữ liệu (Model & Vector Store Agnostic):** Hỗ trợ kết nối hầu hết các nhà cung cấp LLM (OpenAI, Anthropic, Google Gemini, Ollama chạy local...) và các Vector DB phổ biến (Astra DB, Qdrant, Pinecone, pgvector) để làm hệ thống hỏi đáp tài liệu RAG ([Railway](https://railway.com/deploy/langflow-4)).
* **Kiểm thử từng bước với Interactive Playground:** Cung cấp khung chat thử nghiệm ngay bên cạnh bảng vẽ (như cửa sổ *Playground* trong ảnh minh họa) để chạy thử, kiểm soát đầu vào/đầu ra của từng node và tinh chỉnh prompt theo thời gian thực ([GitHub - langflow-ai/langflow](https://github.com/langflow-ai/langflow), [Arize AX Docs](https://arize.com/docs/ax/integrations/platforms/langflow/langflow-tracing)).
* **Xuất bản thành API hoặc MCP Server chỉ với một cú nhấp:** Tự động biến bất kỳ workflow nào bạn vẽ thành một HTTP REST API endpoint (hoặc xuất ra file JSON cho ứng dụng Python/mobile gọi tới), hoặc biến cả luồng đó thành một **MCP Server** để các công cụ như Claude Desktop, Claude Code hay Cursor gọi như một tool nội bộ ([GitHub - langflow-ai/langflow](https://github.com/langflow-ai/langflow), [Langflow Blog](https://www.langflow.org/blog/build-mcp-server-langflow/)).
* **Giám sát và truy vết (Observability):** Tích hợp sẵn với LangSmith, LangFuse và chuẩn OpenTelemetry (như Arize AX) để theo dõi chi phí token, độ trễ và hành trình suy luận của hệ thống multi-agent ([GitHub - langflow-ai/langflow](https://github.com/langflow-ai/langflow), [Arize AX Docs](https://arize.com/docs/ax/integrations/platforms/langflow/langflow-tracing)).

### Kiến trúc mã nguồn trong Repository

Mã nguồn của [Langflow](https://deepwiki.com/langflow-ai/langflow) được chia thành 4 thành phần chính để vừa phục vụ giao diện đồ họa vừa chạy độc lập như một runtime engine trên server:

| Thành phần trong Repo | Đường dẫn chính | Vai trò kỹ thuật |
| --- | --- | --- |
| **Frontend UI** | `src/frontend/` | Ứng dụng web viết bằng TypeScript/React (dựa trên React Flow) cung cấp bảng vẽ kéo thả node, quản lý trạng thái luồng và khung chat Playground ([DeepWiki](https://deepwiki.com/langflow-ai/langflow)). |
| **Backend Foundation** | `src/backend/base/` | Máy chủ Python xây dựng trên FastAPI, Uvicorn và SQLModel/PostgreSQL, quản lý xác thực, lưu trữ flow, quản lý biến môi trường/API keys, chạy ngầm các job và phát luồng sự kiện (event streaming) ([DeepWiki](https://deepwiki.com/langflow-ai/langflow), [Railway](https://railway.com/deploy/langflow-4)). |
| **LFX (Executor Package)** | `src/lfx/` | Bộ thực thi lõi (executor engine) kèm công cụ CLI giúp phân tích đồ thị (graph processing) và chạy các flow độc lập mà không cần tải toàn bộ giao diện nặng nề ([DeepWiki](https://deepwiki.com/langflow-ai/langflow)). |
| **Component Bundles** | `pyproject.toml` & `src/` | Kho thư viện các khối tích hợp sẵn với LangChain, LLM providers, Vector Stores và các công cụ bên ngoài ([DeepWiki](https://deepwiki.com/langflow-ai/langflow), [Workflow Builder](https://www.workflowbuilder.io/compare/langflow)). |

### Cách cài đặt và chạy thử nhanh

Bạn có thể tải bản **Langflow Desktop** (dành cho macOS và Windows, đã đóng gói sẵn môi trường Python) hoặc chạy trực tiếp qua công cụ quản lý gói `uv` và Docker tại cổng mặc định `7860` ([GitHub - langflow-ai/langflow](https://github.com/langflow-ai/langflow)):

```bash
# Cách 1: Cài đặt và chạy bằng uv (yêu cầu Python 3.10 - 3.14)
uv pip install langflow -U
uv run langflow run

# Cách 2: Chạy nhanh bằng Docker
docker run -p 7860:7860 -e LANGFLOW_SUPERUSER_PASSWORD=your_password langflowai/langflow:latest

```

Sau khi khởi chạy, bạn mở trình duyệt tại `[http://127.0.0.1:7860](http://127.0.0.1:7860)` để bắt đầu kéo thả workflow ([GitHub - langflow-ai/langflow](https://github.com/langflow-ai/langflow)).
