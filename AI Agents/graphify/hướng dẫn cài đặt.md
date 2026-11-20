Analyzed SKILL.md:L1-106
Ran list files in Graphify-Labs/graphify repo
Ran fetch remainder of graphify README
Ran inspect Graphify default branch and file tree
Analyzed skills.md:L1-70
Analyzed mcp_servers.md:L1-93
Ran check local Python, CLI tools, and config dirs
Ran fetch v8 README and install.py from Graphify
Ran fetch rest of install.py and pyproject.toml
Ran fetch README part 2 and serve.py
Analyzed SKILL.md:L1-55
Ran read lines 1-250 of graphify/install.py
Ran read lines 251-550 of graphify/install.py
Ran read lines 551-950 of graphify/install.py
Ran read lines 1-260 of graphify/cli.py
Ran read lines 951-1450 of graphify/install.py
Ran read lines 1451-2200 of graphify/install.py
Ran read graphify/__main__.py
Ran read antigravity & claude install in install.py
Ran read end of install.py (project_install & dispatch)
Ran check MCP documentation in v8 README.md
Ran inspect _PLATFORM_CONFIG, claude_install, and dispatch_install_cli
Ran read always_on rules for Antigravity and Claude Code
Ran check Antigravity and Claude Code config dirs locally
Ran inspect remaining sections of install.py
Analyzed cli.md:L1-41
Analyzed app.md:L1-84
Ran check existing mcp_config.json locations in ~/.gemini
Ran read install() and claude_install() in install.py
Ran check serve.py _main and docs directory
Ran read header of graphify/skill.md
Ran check dependencies and python versions in pyproject.toml
Ran read install() and claude_install() details

Dưới đây là hướng dẫn chi tiết cài đặt **graphify** (tên gói PyPI là `graphifyy`) cho cả **Claude Code** và **Google Antigravity** trên máy Mac của bạn.

*(Tôi đã kiểm tra nhanh môi trường trên máy bạn: hiện đã có sẵn `Python 3.14.7`, `uv` tại `/Users/phamhuy/.local/bin/uv` và `claude` CLI tại `/opt/homebrew/bin/claude`, nên bạn có thể cài trực tiếp bằng `uv` rất gọn mà không cần cài thêm `pipx`.)*

---

## Phần 1: Cài đặt gói CLI `graphifyy` (Dùng chung cho cả 2 công cụ)

Mở Terminal và chạy lệnh sau bằng `uv`:

```bash
# Bản tiêu chuẩn (đủ dùng cho skill /graphify và CLI)
uv tool install graphifyy

# HOẶC bản đầy đủ khuyến nghị (thêm MCP server, đọc PDF, Word/Excel và auto-watch)
uv tool install "graphifyy[mcp,pdf,office,watch]"
```

Kiểm tra cài đặt thành công:

```bash
graphify --version
```

> **Lưu ý:** Nếu Terminal báo `command not found: graphify`, hãy chạy lệnh sau rồi mở lại Terminal:
> ```bash
> uv tool update-shell
> ```

---

## Phần 2: Cài đặt cho Claude Code

`graphify` hỗ trợ 2 chế độ cài đặt cho Claude Code tùy theo bạn muốn dùng **toàn cục (mọi dự án)** hay **gói gọn trong từng repo**.

### Cách A: Cài Global (khuyên dùng cho cá nhân)
Chạy lệnh:
```bash
graphify install
```
Lệnh này sẽ tự động:
1. Cài skill vào `~/.claude/skills/graphify/SKILL.md` (kèm thư mục `references/`).
2. Đăng ký block `# graphify` vào `~/.claude/CLAUDE.md` để Claude nhận diện lệnh `/graphify`.

**Bật thêm Hook nhắc nhở thông minh cho từng dự án (tuỳ chọn):**
Khi bạn vào thư mục một dự án bất kỳ và chạy:
```bash
cd /duong/dan/toi/du-an
graphify claude install
```
Nó sẽ ghi rule vào `CLAUDE.md` (hoặc `AGENTS.md` nếu dự án đang dùng) và thêm `PreToolUse` / `SessionStart` hook vào `.claude/settings.json` để tự động nhắc Claude dùng `graphify query` trước khi chạy `Grep`/`Read` hàng loạt file.

### Cách B: Cài theo Project (nếu muốn commit lên Git cho cả team)
```bash
cd /duong/dan/toi/du-an
graphify install --project
```
*(Có thể thêm cờ `--strict` nếu muốn chặn lượt `Read`/`Grep` file thô đầu tiên trong session khi chưa query qua graph: `graphify install --project --strict`.)*

---

## Phần 3: Cài đặt cho Google Antigravity

`graphify` có hỗ trợ gốc (`native`) cho Antigravity thông qua lệnh `graphify antigravity install`.

### Bước 1: Cài Skill, Rule và Workflow

Di chuyển vào thư mục dự án bạn muốn dùng `graphify` và chọn 1 trong 2 cách:

* **Cách A — Cài Global Skill + Rule/Workflow cho dự án (Khuyên dùng):**
  ```bash
  cd /duong/dan/toi/du-an
  graphify antigravity install
  ```
  Lệnh này sẽ tạo:
  1. **Global Skill**: `~/.gemini/config/skills/graphify/SKILL.md` (kèm `references/`, dùng chung cho mọi workspace trong Antigravity).
  2. **Always-on Rule cho dự án**: `.agents/rules/graphify.md` (nhắc Antigravity ưu tiên truy vấn đồ thị khi file `graphify-out/graph.json` tồn tại).
  3. **Workflow cho dự án**: `.agents/workflows/graphify.md` (kích hoạt slash command `/graphify`).

* **Cách B — Cài hoàn toàn trong thư mục Project (dễ chia sẻ qua Git):**
  ```bash
  cd /duong/dan/toi/du-an
  graphify antigravity install --project
  ```
  Lệnh này cài cả skill vào `.agents/skills/graphify/SKILL.md` thay vì thư mục `~/.gemini/config/skills/`.

### Bước 2: (Tuỳ chọn) Kết nối MCP Server cho Antigravity

Khi chạy `graphify antigravity install`, CLI sẽ gợi ý cấu hình MCP. Tuy nhiên trên máy của bạn cần lưu ý 2 điểm quan trọng:

1. **Vị trí file cấu hình MCP chuẩn của Antigravity:**
   Trên máy Mac của bạn hiện có cả [config mcp_config.json](file:///Users/phamhuy/.gemini/config/mcp_config.json) (đường dẫn cấu hình toàn cục hiện hành của Antigravity 2.0) và [antigravity mcp_config.json](file:///Users/phamhuy/.gemini/antigravity/mcp_config.json) (đường dẫn cũ). Bạn nên thêm cấu hình vào **[config mcp_config.json](file:///Users/phamhuy/.gemini/config/mcp_config.json)** (hoặc cập nhật cả 2 nếu bạn dùng song song bản cũ/mới).
2. **File `graph.json` phải tồn tại trước khi bật MCP:**
   Hãy chạy `/graphify .` trong dự án ít nhất 1 lần trước để sinh ra `graphify-out/graph.json`, nếu không MCP server sẽ báo lỗi không tìm thấy file đồ thị.

Mở [config mcp_config.json](file:///Users/phamhuy/.gemini/config/mcp_config.json) và thêm mục `"graphify"` vào trong `"mcpServers"` (giữ nguyên `"claude-mem"` hiện có của bạn):

```json
{
  "mcpServers": {
    "claude-mem": {
      "...": "giữ nguyên cấu hình hiện tại của bạn"
    },
    "graphify": {
      "command": "/Users/phamhuy/.local/bin/graphify-mcp",
      "args": [
        "/duong/dan/tuyet/doi/toi/du-an/graphify-out/graph.json"
      ]
    }
  }
}
```

> **Mẹo:**
> - Nếu bạn **không** cài extra `[mcp]` ở Phần 1, thay `"command"` bằng `"/Users/phamhuy/.local/bin/uv"` và `"args"` bằng `["run", "--with", "graphifyy", "--with", "mcp", "-m", "graphify.serve", "/duong/dan/tuyet/doi/toi/du-an/graphify-out/graph.json"]`.
> - Trong snippet mặc định của `graphify` có dùng biến `${workspace.path}/graphify-out/graph.json`; nếu Antigravity báo lỗi không khởi động được MCP server, hãy thay `${workspace.path}` bằng **đường dẫn tuyệt đối** tới thư mục dự án như ví dụ trên.
> - Nếu bạn thường xuyên làm việc với **nhiều dự án khác nhau**, bạn **không bắt buộc** phải bật MCP — chỉ cần Bước 1 (Skill + Rules + Workflows) là Antigravity đã có thể chạy trực tiếp lệnh CLI `graphify query`, `graphify path`, `graphify explain` trên bất kỳ workspace nào rồi.

---

## Phần 4: Kiểm tra và sử dụng lần đầu

Sau khi cài xong, vào một thư mục dự án bất kỳ và thử các lệnh sau:

1. **Tạo Knowledge Graph lần đầu** (trong chat của Claude Code hoặc Antigravity):
   ```text
   /graphify .
   ```
   *(Hoặc chạy thẳng bằng terminal để test phân tích AST cục bộ trước: `graphify extract .` / mở giao diện chat gọi `/graphify .` để chạy đầy đủ pipeline tạo `graphify-out/graph.html`, `GRAPH_REPORT.md` và `graph.json`.)*

2. **Thử truy vấn đồ thị:**
   ```text
   /graphify query "luồng xử lý chính của dự án hoạt động như thế nào?"
   /graphify path "TenClassA" "TenClassB"
   /graphify explain "TenModule"
   ```

3. **Tự động cập nhật graph khi code thay đổi:**
   ```bash
   # Cập nhật thủ công (chỉ quét AST, không tốn token LLM)
   graphify update .

   # Hoặc cài git post-commit hook để tự động rebuild mỗi lần commit
   graphify hook install
   ```

4. **Xem danh sách MCP Server trong Antigravity** (nếu có cấu hình MCP):
   Mở menu **Additional Options (`...`) > MCP Servers** để kiểm tra trạng thái kết nối của `graphify` (nên khởi động lại Antigravity sau khi sửa JSON).

---

Ran check graphify installation and workspace files · Exited with code 1
Ran check for graphify-out, installed skills, and hooks
Analyzed SKILL.md:L60-250
Ran check graphify --help and mcp_config.json
Ran view middle section of graphify --help
Analyzed SKILL.md:L250-420
Ran check behavior of graphify update when no graph exists

Thông báo đó là **bình thường** sau bước cài đặt — nó cho biết bạn đã cài xong công cụ/skill/rule của `graphify`, nhưng **chưa chạy bước quét dữ liệu để sinh ra đồ thị** (`graphify-out/graph.json`).

### Tình trạng hiện tại trên máy bạn
Tôi đã kiểm tra nhanh thư mục `/Users/phamhuy/Syntra AI/Projects` và thấy:
* CLI `graphify 0.9.112` đã được cài thành công tại `/Users/phamhuy/.local/bin/graphify`.
* Cấu hình tích hợp đã được tạo cho cả 2 công cụ:
  * **Antigravity**: skill tại `~/.gemini/config/skills/graphify/SKILL.md`, kèm [graphify.md](file:///Users/phamhuy/Syntra%20AI/Projects/.agents/rules/graphify.md) (rules) và [graphify.md](file:///Users/phamhuy/Syntra%20AI/Projects/.agents/workflows/graphify.md) (workflow).
  * **Claude Code**: skill tại `~/.claude/skills/graphify/SKILL.md`, kèm [CLAUDE.md](file:///Users/phamhuy/Syntra%20AI/Projects/CLAUDE.md) và [settings.json](file:///Users/phamhuy/Syntra%20AI/Projects/.claude/settings.json) (hooks).
* Tuy nhiên, thư mục `graphify-out/` **chưa tồn tại** ở `/Users/phamhuy/Syntra AI/Projects` hay bất kỳ dự án con nào bên trong (vì các lệnh `install` chỉ cài đặt cấu hình, không tự quét code).

---

### Lưu ý quan trọng trước khi tạo graph
Thư mục `/Users/phamhuy/Syntra AI/Projects` đang chứa nhiều dự án con độc lập (như `backbone`, `s-artery`, `s-dekit`, `s-ecom-arm`, `s-ecom-bff`, `s-ecom-fe`, `s-retina`, `Beauty filter`, ...).

Bạn **không nên** quét toàn bộ thư mục cha này thành 1 graph chung vì:
* Tổng số file sẽ vượt ngưỡng cảnh báo (>500 file / >2 triệu từ), làm chậm và tốn tài nguyên.
* Code của nhiều dự án khác nhau sẽ bị gộp chung vào một đồ thị.

Thay vào đó, hãy **tạo graph riêng cho từng dự án con** bạn đang làm việc.

---

### Cách tạo `graphify-out/graph.json` ngay bây giờ

#### Cách 1: Chạy nhanh qua Terminal (chỉ phân tích code bằng AST, miễn phí, không tốn token LLM)
Ví dụ với dự án `s-ecom-fe`:
```bash
cd "/Users/phamhuy/Syntra AI/Projects/s-ecom-fe"
graphify update .
```
Lệnh này sẽ tạo ra `graphify-out/graph.json`, `graphify-out/GRAPH_REPORT.md` và `graphify-out/graph.html` ngay trong thư mục `s-ecom-fe`.

*(Nếu bạn mở từng dự án con thành workspace riêng trong Antigravity hoặc Claude Code, nên chạy thêm `graphify antigravity install` hoặc `graphify claude install` ngay bên trong thư mục dự án con đó).*

#### Cách 2: Chạy đầy đủ qua AI Chat (quét cả Code + Docs `.md` + PDF + Hình ảnh)
Mở chat trong **Antigravity** hoặc **Claude Code** và gõ:
```text
/graphify ./s-ecom-fe
```
*(Hoặc mở trực tiếp workspace `s-ecom-fe` rồi gõ `/graphify .`).*

---

Ran check running graphify processes, git hooks, root graph, and querylog
Ran check cache dir, check-update, and hook-guard in graphify

Khác với database hay web server, **`graphify` không phải dịch vụ chạy ngầm liên tục** (trừ khi bạn chủ động bật `graphify watch` hoặc MCP server `graphify-mcp`). Thông thường nó hoạt động theo kiểu **được gọi theo yêu cầu (on-demand)** thông qua CLI, Skill, Rule hoặc Hook.

Để kiểm tra xem `graphify` có đang hoạt động đúng hay không, bạn kiểm tra theo **4 lớp** dưới đây (kèm tình trạng thực tế tôi vừa kiểm tra trên máy của bạn):

---

### 1. Kiểm tra dữ liệu Graph & CLI có chạy được không
Vào một thư mục dự án đã tạo graph (ví dụ `s-relay`) và thử query trực tiếp:

```bash
cd "/Users/phamhuy/Syntra AI/Projects/s-relay"
graphify query "LinkDetailViewModel"
```

* **Hoạt động bình thường:** Trả về các node/edge (`EXTRACTED`, `INFERRED`) và đường dẫn file liên quan.
* **Kiểm tra graph có đang bị cũ so với code mới sửa không:**
  ```bash
  graphify check-update .
  ```

---

### 2. Kiểm tra AI (Antigravity / Claude Code) có thực sự dùng `graphify` khi chat không

Có **2 cách** nhận biết trực tiếp:

* **Cách A — Nhìn log tool call trong khung chat:**
  Khi bạn hỏi về kiến trúc hoặc luồng xử lý của một dự án có `graphify-out/graph.json`, hãy để ý bước chạy lệnh của AI: nếu thấy AI chạy `graphify query "..."`, `graphify path ...`, `graphify explain ...` hoặc đọc `GRAPH_REPORT.md` trước khi mở các file code thô → tức là rule/skill đang phát huy tác dụng.
* **Cách B — Bật Query Log để theo dõi real-time (chính xác 100%):**
  Mặc định `graphify` tắt file log để đảm bảo riêng tư (`querylog.py`). Bạn có thể bật bằng biến môi trường (thêm vào `~/.zshrc` rồi mở lại terminal / app):
  ```bash
  export GRAPHIFY_QUERY_LOG_ENABLE=1
  ```
  Sau đó mở một cửa sổ terminal theo dõi file log:
  ```bash
  tail -f ~/.cache/graphify-queries.log
  ```
  Mỗi lần bạn hoặc AI chạy `graphify query` / `path` / `explain`, một dòng JSON sẽ được ghi ngay vào file này.

> **Lưu ý với workspace hiện tại của bạn:**
> Hiện tại tôi kiểm tra thấy `/Users/phamhuy/Syntra AI/Projects/graphify-out` **chưa tồn tại** ở thư mục cha (chỉ có bên trong từng thư mục con `s-relay`, `s-ecom-bff`, `s-ecom-fe`, `s-ecom-arm`).
> * Nếu bạn mở Antigravity hoặc Claude Code ở **thư mục cha** `/Users/phamhuy/Syntra AI/Projects`, rule [graphify.md](file:///Users/phamhuy/Syntra%20AI/Projects/.agents/rules/graphify.md) và hook trong [settings.json](file:///Users/phamhuy/Syntra%20AI/Projects/.claude/settings.json) tìm `graphify-out/graph.json` ngay tại root sẽ không thấy file nên không kích hoạt.
> * Nếu mở trực tiếp **từng thư mục dự án con** (ví dụ `s-relay`), thì `graphify-out/graph.json` nằm ngay ở root dự án đó nên sẽ kích hoạt bình thường (hoặc gộp các repo con vào root bằng `graphify merge-graphs` như đã trao đổi ở trên).

---

### 3. Kiểm tra Hook của Claude Code có chạy không
Trong thư mục `/Users/phamhuy/Syntra AI/Projects/.claude/settings.json`, Claude Code đã gắn hook `PreToolUse` (`graphify hook-guard search` và `read`).
* Ở chế độ mặc định (soft mode), hook chỉ âm thầm chèn dòng nhắc vào ngữ cảnh của Claude khi trong thư mục làm việc hiện tại có `graphify-out/graph.json` tươi mới.
* Nếu muốn **kiểm chứng tận mắt** hook đang chạy (trong một thư mục đã có `graphify-out/graph.json` như `s-relay`), bạn có thể chạy Claude Code với biến `GRAPHIFY_HOOK_STRICT=1 claude` (hoặc cài với `--strict` trong dự án đó) — lần `Read`/`Grep` file code đầu tiên sẽ bị hook chặn lại và yêu cầu gọi `graphify query` trước.

---

### 4. Tình trạng các thành phần chạy nền trên máy bạn lúc này
Tôi vừa kiểm tra trực tiếp trên máy Mac của bạn lúc này:

| Thành phần | Trạng thái hiện tại | Ý nghĩa & Cách kiểm tra |
| :--- | :--- | :--- |
| **CLI `graphify`** | Đã cài (`0.9.112`) | Có tại `/Users/phamhuy/.local/bin/graphify`. |
| **Tiến trình nền (`watch` / `mcp`)** | **Không có tiến trình nào chạy** | Kiểm tra bằng `pgrep -fl graphify` (trống). Hiện chưa có `graphify watch` hay `graphify-mcp` chạy nền. |
| **MCP Server (Antigravity)** | **Chưa cấu hình** | [mcp_config.json](file:///Users/phamhuy/.gemini/config/mcp_config.json) hiện chỉ có `claude-mem`. Nếu có thêm MCP `graphify`, kiểm tra trạng thái kết nối tại **`...` (Additional Options) > MCP Servers**. |
| **Git Hooks (tự rebuild khi commit)** | **Chưa cài (`not installed`)** | Đã chạy `graphify hook status` trên `s-relay`, `s-ecom-bff`, `s-ecom-fe`, `s-ecom-arm` — cả 4 đều chưa cài hook `post-commit`. Hiện cần chạy `graphify update .` thủ công sau khi sửa code (hoặc chạy `graphify hook install` trong từng repo để tự động hóa). |
| **Root `graphify-out/graph.json`** | **Chưa có ở thư mục cha** | Mới có ở từng thư mục con (`s-relay`, `s-ecom-bff`, `s-ecom-fe`, `s-ecom-arm`). |
