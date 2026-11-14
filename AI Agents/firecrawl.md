https://github.com/firecrawl/firecrawl

**Firecrawl** (`firecrawl/firecrawl`) là một bộ công cụ mã nguồn mở (open-source) và hệ thống API chuyên dụng để **tìm kiếm, thu thập (scrape/crawl) và tương tác với dữ liệu trên web**, sau đó chuyển đổi nội dung đó thành định dạng sạch (**Markdown** hoặc **JSON có cấu trúc**) sẵn sàng nạp trực tiếp vào các mô hình ngôn ngữ lớn (LLM) và AI Agent.

Thay vì chỉ tải mã nguồn HTML thô như các thư viện truyền thống (BeautifulSoup, Scrapy), Firecrawl đóng vai trò là **Context API (lớp cung cấp ngữ cảnh web)** giúp AI đọc hiểu thông tin trên internet một cách chính xác và tiết kiệm token nhất.

---

---

Như minh họa trong sơ đồ kiến trúc trên, khi nhận một yêu cầu từ **User / SDK**, **API (Express)** sẽ đẩy tác vụ vào hàng đợi **Redis (BullMQ)** để các **Worker (Node.js)** xử lý bất đồng bộ. Worker sử dụng **Playwright / Fetch** để tải và render đầy đủ các trang web chạy JavaScript nặng, sau đó đẩy HTML thô qua **Rust/Go Module** (xử lý native tốc độ cao) để lọc bỏ thẻ thừa và chuyển thành Markdown sạch. Nếu người dùng yêu cầu trích xuất theo cấu trúc (Schema), dữ liệu sẽ tiếp tục đi qua **AI Provider (OpenAI/Gemini)** để trả về JSON chuẩn xác.

## Vấn đề cốt lõi mà Firecrawl giải quyết

* **Xử lý trang web động (JS-heavy) & chống chặn:** Tự động render các trang Single Page Application (React, Vue, Next.js), xoay vòng proxy (rotating proxies), vượt qua cơ chế giới hạn tốc độ (rate limits) mà lập trình viên không cần cấu hình thủ công.
* **Tối ưu hóa Token cho LLM:** Mã HTML gốc của một trang web chứa rất nhiều thẻ `<div>`, CSS, script quảng cáo gây nhiễu và tốn chi phí API của LLM. Firecrawl tự động loại bỏ phần rườm rà (header, footer, nav) để chỉ giữ lại nội dung chính dưới dạng Markdown tinh gọn.
* **Hỗ trợ đa định dạng tài liệu:** Ngoài trang web, Firecrawl còn bóc tách được nội dung từ các tệp **PDF, DOCX** sang Markdown hoặc JSON.

## Các tính năng và API Endpoint chính

Repo này cung cấp cả SDK (Python, Node.js, Rust, Go) lẫn công cụ dòng lệnh (`firecrawl-cli`) xoay quanh các endpoint cốt lõi sau:

| Endpoint / Tính năng | Chức năng chính | Ứng dụng thực tế |
| --- | --- | --- |
| **`/scrape`** | Chuyển đổi 1 URL cụ thể thành Markdown, HTML, ảnh chụp màn hình (screenshot) hoặc JSON. | Tóm tắt bài báo, lấy thông tin sản phẩm từ một link duy nhất. |
| **`/crawl`** | Tự động dò tìm và cào toàn bộ các trang con (subpages) thuộc một domain chỉ với 1 lệnh. | Kéo toàn bộ trang tài liệu kỹ thuật (documentation) của một thư viện để làm dữ liệu cho RAG. |
| **`/map`** | Liệt kê siêu nhanh toàn bộ danh sách URL đang tồn tại trên một website. | Khảo sát cấu trúc website trước khi quyết định chỉ scrape những đường dẫn quan trọng. |
| **`/search`** | Tìm kiếm trên web theo từ khóa và trả về luôn nội dung đầy đủ (Markdown) của các trang kết quả. | Thay thế công cụ search truyền thống cho AI Agent để tổng hợp tin tức thời gian thực. |
| **`/interact`** | Cho phép AI tương tác trực tiếp với trang web bằng prompt hoặc code (click nút, cuộn trang, điền form, chờ tải) trước khi lấy dữ liệu. | Thu thập dữ liệu ẩn sau nút "Xem thêm", thanh tìm kiếm nội bộ hoặc các bước điều hướng phức tạp. |
| **`/agent`** | Tác tử AI tự chủ (chạy trên model `spark-2`): chỉ cần mô tả dữ liệu cần tìm bằng ngôn ngữ tự nhiên (kèm Pydantic/JSON Schema), AI tự tìm nguồn, duyệt nhiều trang và trả về kết quả mà không cần đưa trước URL. | Nghiên cứu thị trường, so sánh bảng giá đối thủ tự động. |
| **Batch & Monitor** | Scrape hàng nghìn URL cùng lúc bất đồng bộ (`Batch Scrape`) hoặc gửi thông báo khi nội dung một trang web thay đổi (`Monitor`). | Theo dõi biến động giá, cập nhật phiên bản thư viện/API. |

## Ứng dụng trong AI Workflow & Cách triển khai

* **Cấp quyền truy cập web cho AI Coding Agent:** Repository tích hợp sẵn **MCP Server (`firecrawl-mcp`)** và bộ **Skills (`firecrawl-cli`)**. Bạn có thể gắn trực tiếp vào Claude Code, Cursor hoặc các hệ thống multi-agent để AI tự động tra cứu tài liệu SDK/API mới nhất trên mạng trong lúc lập trình.
* **3 hình thức triển khai linh hoạt:**
1. **Firecrawl Cloud (`firecrawl.dev`):** Dùng trực tiếp qua API Key, bao gồm đầy đủ hạ tầng proxy chống chặn và kho tích hợp dữ liệu mở rộng (Alexandria).
2. **Firecrawl Local (`firecrawl-local`):** Chạy dạng single-binary gọn nhẹ trực tiếp trên máy cá nhân cho các tác vụ scrape, crawl và map cơ bản.
3. **Self-Host (Tự dựng server):** Repo cung cấp sẵn tệp `docker-compose.yaml` và tài liệu `SELF_HOST.md` (giấy phép AGPL-3.0) để bạn tự triển khai toàn bộ hệ thống Express, Redis, Playwright và Worker lên hạ tầng riêng.
