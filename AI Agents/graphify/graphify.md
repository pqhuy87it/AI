https://github.com/Graphify-Labs/graphify

Searched "Graphify-Labs/graphify" github
Ran fetch Graphify-Labs/graphify metadata and README

[**Graphify-Labs/graphify**](https://github.com/Graphify-Labs/graphify) là một công cụ dòng lệnh (CLI) kiêm **AI Agent Skill (`/graphify`)** dùng để **biến toàn bộ một thư mục hỗn hợp (source code, tài liệu Markdown, SQL schema, bài báo PDF, ảnh chụp màn hình/sơ đồ) thành một Đồ thị Tri thức (Knowledge Graph) có thể truy vấn và tương tác trực tiếp**.

Mục tiêu lớn nhất của `graphify` là giúp lập trình viên và các AI Coding Agent (như Claude Code, Cursor, Codex, Gemini CLI) hiểu cấu trúc của một dự án hoặc kho tài liệu khổng lồ với **lượng token tiêu thụ ít hơn tới ~71.5 lần** so với việc đọc từng file thô, mà **không cần cài đặt Vector Database (RAG) hay Server phức tạp**.

---

### Các tính năng và công dụng chính của `graphify`

1. **Xử lý đa phương thức (Multimodal Ingestion)**:
   * **Source code** (`.py`, `.ts`, `.js`, `.go`, `.rs`, `.java`, `.c`, `.cpp`, `.rb`, `.cs`, `.kt`, `.scala`, `.php`): Phân tích cú pháp cục bộ cực nhanh và chính xác bằng **AST (`tree-sitter`)** + đồ thị lời gọi hàm (call-graph) mà không tốn token LLM.
   * **Tài liệu & Bài báo** (`.md`, `.txt`, `.rst`, `.pdf`): Trích xuất khái niệm, mối quan hệ và các trích dẫn (citation mining).
   * **Hình ảnh** (`.png`, `.jpg`, `.webp`, `.gif`): Dùng thị giác máy tính (Vision) để đọc ảnh chụp màn hình, sơ đồ kiến trúc, ảnh chụp bảng trắng (whiteboard) và kết nối chúng thẳng vào chung một đồ thị với code.

2. **Tạo bộ đầu ra trực quan và đa năng (`graphify-out/`)**:
   Khi bạn chạy `/graphify .` trên một thư mục, nó sẽ sinh ra thư mục `graphify-out/` gồm:
   * `graph.html`: Giao diện đồ thị tương tác trên trình duyệt (click vào từng node, tìm kiếm, lọc theo cụm/community).
   * `obsidian/`: Một thư mục chuẩn để bạn mở trực tiếp bằng **Obsidian Vault**.
   * `wiki/` (khi bật `--wiki`): Các bài viết kiểu Wikipedia cho từng cụm kiến thức để AI Agent dễ dàng đọc và điều hướng.
   * `GRAPH_REPORT.md`: Báo cáo chỉ ra các **"God nodes"** (những thành phần/khái niệm trung tâm kết nối mọi thứ), các **mối liên hệ bất ngờ** (ví dụ: đoạn code nào đang cài đặt công thức trong bài báo PDF nào), và các câu hỏi gợi ý.
   * `graph.json` & `cache/`: Lưu đồ thị bền vững và cache bằng mã băm `SHA256` (những lần chạy sau chỉ xử lý các file có thay đổi).

3. **Truy vấn đồ thị và Giải thích mối liên hệ**:
   * Mọi cạnh nối (edge) trong đồ thị đều được gắn nhãn rõ ràng: `EXTRACTED` (trích xuất chắc chắn từ code/tài liệu), `INFERRED` (suy luận), hoặc `AMBIGUOUS` (mơ hồ).
   * Hỗ trợ các lệnh truy vấn nhanh:
     * `/graphify query "..."`: Hỏi đáp dựa trên đồ thị.
     * `/graphify path "NodeA" "NodeB"`: Tìm đường đi/mối liên hệ giữa 2 thành phần trong hệ thống.
     * `/graphify explain "Tên_Class_Hoặc_Khái_Niệm"`: Giải thích chi tiết một thực thể.

4. **Tự động đồng bộ theo thời gian thực (`--watch` & Git Hook)**:
   * Chế độ `--watch` hoặc `graphify hook install` (gắn vào `post-commit` của Git) giúp đồ thị tự động cập nhật ngay lập tức mỗi khi bạn sửa code hoặc commit (rất hữu ích khi nhiều AI agent cùng viết code song song).
   * Hỗ trợ xuất ra nhiều định dạng khác như **Neo4j (`--neo4j`)**, **Gephi/GraphML (`--graphml`)**, **SVG (`--svg`)**, hoặc chạy làm **MCP Server (`--mcp`)**.

---

### Điểm khác biệt giữa `claude-obsidian` và `graphify` (nếu bạn đang phân vân)

| Tiêu chí | `claude-obsidian` | `graphify` |
| :--- | :--- | :--- |
| **Mục đích chính** | Xây dựng "Bộ não thứ 2" (PKM), viết và quản lý ghi chú lâu dài trong Obsidian. | Phân tích cấu trúc **Codebase + Tài liệu + Ảnh + PDF** thành đồ thị để hiểu hệ thống và tiết kiệm token cho AI. |
| **Cách hoạt động** | AI đọc nguồn trong `inbox/` -> viết thành các bài note Markdown chuẩn chỉnh, kiểm tra lỗi link. | Dùng `tree-sitter` (AST) quét code + AI quét ảnh/PDF + thuật toán phân cụm mạng lưới (`NetworkX` + `Leiden`) để vẽ đồ thị. |
| **Đầu ra chính** | Một Obsidian Vault được chăm chút từng bài viết. | File đồ thị tương tác `graph.html`, `GRAPH_REPORT.md`, `graph.json`, kèm thư mục xuất sang Obsidian/Wiki. |

### Cách cài đặt nhanh `graphify`
Nếu muốn dùng thử trên máy Mac của bạn (yêu cầu Python 3.10+):
```bash
# Cài qua pipx hoặc uv (khuyên dùng trên macOS)
pipx install graphifyy
# (hoặc: uv tool install graphifyy / pip install graphifyy)

# Đăng ký skill /graphify vào AI Agent
graphify install
```
*(Lưu ý: Tên gói trên PyPI tạm thời là `graphifyy` có 2 chữ `y`, còn câu lệnh chạy vẫn là `graphify`).*
