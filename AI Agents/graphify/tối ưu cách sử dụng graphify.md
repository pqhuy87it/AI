Ran inspect rcfile, reflect, and affected in graphify
Ran check .graphifyrc/.graphifyignore locally and in README
Ran check README tips and inspect s-relay graph manifest distribution
Ran check s-relay Packages and .gitignore entries for graphify

Để khai thác `graphify` (`0.9.112`) hiệu quả nhất (giảm tối đa token, truy vấn trúng đích ngay lần đầu và không bị nhiễu khi code nhiều dự án), bạn nên áp dụng **5 nhóm thực hành tối ưu** dưới đây — đặc biệt phù hợp với hệ repo hiện tại của bạn (`s-relay`, `s-ecom-bff`, `s-ecom-fe`, `s-ecom-arm`).

---

### 1. Làm sạch & tinh chỉnh Graph của từng repo (`.graphifyignore` + `.graphifyrc`)

Khi kiểm tra nhanh [GRAPH_REPORT.md](file:///Users/phamhuy/Syntra%20AI/Projects/s-relay/graphify-out/GRAPH_REPORT.md) của `s-relay` hiện tại (~7,606 nodes · 19,240 edges), có thể thấy vài **nút tiện ích** (`Decodable`, `Equatable`, `Foundation`, `.expect`, `.log`) đang lọt lên đầu mục *Community Hubs*. Bạn tối ưu lại bằng 3 bước:

1. **Loại bỏ file nhiễu với `.graphifyignore`** (đặt ở root repo, cú pháp giống `.gitignore`):
   * Bỏ qua file sinh tự động, mock nặng, hoặc thư mục tham khảo không cần AI đọc nhầm làm code chính (ví dụ `*.pb.go`, `MockData/`, `DerivedData/`...).
2. **Khử các "siêu nút" tiện ích (`--exclude-hubs`)** khi phân cụm để lộ ra đúng kiến trúc thật của app:
   ```bash
   cd "/Users/phamhuy/Syntra AI/Projects/s-relay"
   graphify cluster-only . --exclude-hubs 99 --no-label
   ```
   *(Ngưỡng `99` loại top 1% siêu nút như `Equatable`, `Foundation`, `.expect` khỏi vai trò tâm cụm, giúp `LinkDetailViewModel`, `RemoteAffiliateLinkRepository`, `AppEnvironment`... được gom cụm chuẩn hơn).*
3. **Đặt thông số mặc định riêng cho repo bằng `.graphifyrc`** ở root repo:
   ```ini
   # .graphifyrc
   query_budget=3500
   query_depth=3
   # Khai báo rõ đuôi file dễ nhầm ngôn ngữ (nếu repo có):
   # language.h=cpp
   # language.m=objc
   ```
   Với repo nhiều file như `s-relay`, tăng `query_budget` từ `2000` (mặc định) lên `3500–4000` và `query_depth=3` giúp một lần query đủ ngữ cảnh luôn thay vì AI phải hỏi lại nhiều vòng.

---

### 2. Chọn đúng lệnh Graphify cho đúng tình huống

Đừng chỉ dùng mỗi `graphify query` cho mọi việc — dùng đúng lệnh sẽ chính xác hơn và ít tốn token hơn nhiều:

| Tình huống | Lệnh tối ưu | Tác dụng |
| :--- | :--- | :--- |
| **Tìm hiểu tổng quan 1 tính năng** | `graphify query "LinkDetailViewModel orders"` | **BFS** (mặc định): lấy bức tranh rộng quanh tính năng. |
| **Lần theo luồng gọi sâu UI → API** | `graphify query "loadNextPage" --dfs --depth 4` | **DFS**: đi sâu theo một chuỗi gọi hàm cụ thể. |
| **Lọc bớt nhiễu khi query** | `graphify query "createLink" --context call` | Chỉ lấy cạnh gọi hàm (`call`), bỏ qua khai báo field. |
| **Kiểm tra 2 phần có liên hệ gì** | `graphify path "LinkDetailView" "RemoteAffiliateLinkRepository"` | Tìm đường đi ngắn nhất giữa 2 thành phần. |
| **Trước khi sửa/refactor 1 hàm hoặc DTO** | `graphify affected "AffiliateLinkDTO"` | **Cực hữu ích:** truy ngược mọi nơi bị ảnh hưởng nếu bạn sửa symbol này. |
| **Xem nhanh vai trò 1 class/struct** | `graphify explain "AffiliateLinkInteractor"` | Liệt kê node và toàn bộ liên kết quanh nó. |
| **Đọc 1 mục trong file `.md` dài** | `graphify section "CLAUDE.md#BFF contract"` | Trích đúng heading đó mà không cần mở cả file `.md`. |
| **Rà các tâm điểm kiến trúc** | `graphify god-nodes --top 15` | Xem nhanh 15 hub kết nối nhiều nhất trong dự án. |

> **Mẹo khi prompt cho AI:** Khi đặt câu hỏi cho Claude Code hoặc Antigravity, hãy nhúng **tên symbol hoặc tên file đặc thù** (ví dụ `AffiliateLinkInteractor`, `CtvOrdersRequest`) — AI truyền thẳng tên đó vào `graphify query` sẽ trúng đích ngay thay vì bị loãng từ khóa.

---

### 3. Giữ Graph luôn mới mà gần như **0 tốn kém**

* **Sửa code thường ngày (AST thuần, 0 tốn token LLM):**
  Chạy `graphify update .` (chỉ mất vài giây, chỉ xử lý lại các file có thay đổi nhờ `manifest.json` + cache SHA256).
* **Tự động hóa hoàn toàn bằng Git Hook (Khuyên dùng):**
  Vào từng repo bạn hay code (`s-relay`, `s-ecom-bff`, `s-ecom-fe`, `s-ecom-arm`) chạy 1 lần:
  ```bash
  graphify hook install
  ```
  Từ đó cứ mỗi lần `git commit`, `checkout` hoặc `merge`, graph tự động rebuild ngầm — bạn không bao giờ quên cập nhật graph (và kiểm tra `.gitignore` có `graphify-out/` như [s-relay/.gitignore](file:///Users/phamhuy/Syntra%20AI/Projects/s-relay/.gitignore) đã có để không commit nhầm).
* **Khi nhiều AI Agent / nhiều nhánh sửa file liên tục:**
  Mở 1 tab Terminal chạy `graphify watch .` trong repo đó — mỗi lần lưu file code là graph tự đồng bộ ngay.
* **Khi cập nhật tài liệu `.md`, PDF, ảnh kiến trúc:**
  `graphify update` chỉ quét code (AST). Khi bạn thêm/sửa tài liệu quan trọng muốn AI hiểu ngữ nghĩa nối vào code, hãy chạy `/graphify . --update` trong khung chat (chỉ tốn token cho những file doc mới/thay đổi).

---

### 4. Tối ưu cho workflow Multi-Repo & Tích hợp IDE (Antigravity + Claude Code)

1. **Làm việc trong từng repo con khi code tính năng:**
   Mở workspace trực tiếp ở `s-relay/`, `s-ecom-bff/`, hoặc `s-ecom-fe/` khi tập trung code ở repo đó (giúp `graphify section`, `graphify update .`, và rule/hook chạy chính xác 100%, không bị trùng tên `Order`/`Product` giữa nhiều repo).
2. **Dùng `graphify-mcp` (chế độ Multi-Project) thay cho CLI khi dùng Antigravity nhiều:**
   Mỗi lần gọi CLI `graphify query` mất ~0.5–1.5 giây khởi động Python và parse lại `graph.json` (đặc biệt file `s-relay` gần 10 MB). Nếu bạn cấu hình `graphify-mcp` trong [mcp_config.json](file:///Users/phamhuy/.gemini/config/mcp_config.json), server giữ sẵn đồ thị đã index trong RAM (truy vấn gần như tức thì) và hỗ trợ truyền **`project_path`** để 1 server đọc được cả `s-relay`, `s-ecom-bff`, `s-ecom-fe`!
3. **Ép Claude Code tuân thủ graph tuyệt đối khi cần (`--strict`):**
   Nếu thấy Claude trong repo lớn vẫn có thói quen `Grep`/`Read` hàng loạt file trước khi hỏi graph, có thể chạy Claude với:
   ```bash
   GRAPHIFY_HOOK_STRICT=1 claude
   ```
   (hoặc cài `graphify claude install --project --strict` trong repo đó): Claude sẽ bị hook chặn lượt `Read`/`Grep` thô đầu tiên và buộc phải chạy `graphify query` trước.

---

### 5. Hai tính năng nâng cao giúp AI ngày càng thông minh theo thời gian

1. **Xuất `--wiki` cho các repo kiến trúc phức tạp (`graphify export wiki`):**
   Sinh thêm thư mục `graphify-out/wiki/` gồm `index.md` và bài tóm tắt cho từng cụm chức năng. Nhờ rule đã cài sẵn (*"If `graphify-out/wiki/index.md` exists, navigate it instead of reading raw files"*), AI có thể duyệt kiến trúc repo như đọc Wikipedia rất tiết kiệm ngữ cảnh.
2. **Tích lũy trí nhớ làm việc (`save-result` + `reflect`):**
   Khi bạn và AI vừa tìm ra một luồng phức tạp hoặc phát hiện một hướng đi sai (`dead_end`), có thể lưu kết quả vào graph (`graphify save-result ... --outcome useful|dead_end|corrected`) rồi chạy `graphify reflect` để sinh ra `graphify-out/reflections/LESSONS.md`. Các phiên làm việc sau AI sẽ đọc trực tiếp bài học này để không đi lại vết xe đổ.
3. **Đo lại hiệu quả thực tế định kỳ:**
   ```bash
   graphify check-update .   # kiểm tra graph có bị cũ không
   graphify benchmark        # đo mức giảm token thực tế trên repo
   ```
