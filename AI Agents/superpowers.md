https://github.com/obra/superpowers

Source code tại `[https://github.com/obra/superpowers](https://github.com/obra/superpowers)` (với neo `#claude-code` trỏ tới mục hướng dẫn cài đặt cho Claude Code) là **Superpowers** — một **framework kỹ năng (agentic skills framework) kiêm quy trình phát triển phần mềm chuẩn hóa** dành cho các AI Coding Agent, do tác giả Jesse Vincent (`obra`) khởi xướng (phát hành theo giấy phép **MIT**, hiện đạt hơn **317 nghìn sao** trên GitHub).

Nếu như bình thường khi bạn yêu cầu **Claude Code**, **Cursor** hay **Antigravity** viết một tính năng, AI thường lập tức nhảy vào sinh code ngay (dễ dẫn đến lệch ý tưởng, thiếu kiểm thử hoặc phá vỡ kiến trúc cũ), thì **Superpowers** gắn thêm một "bộ khung kỷ luật kỹ thuật" (hooks + skills) ép AI phải làm việc bài bản như một kỹ sư phần mềm thực thụ.

## Cơ chế hoạt động cốt lõi (Quy trình 4 bước tự động)

Điểm đặc biệt của Superpowers là các kỹ năng trong thư mục `skills/` và `hooks/` **tự động kích hoạt (auto-trigger)** ngay khi bạn bắt đầu phiên làm việc mà không cần gọi lệnh thủ công:

1. **Khơi gợi yêu cầu và chốt thiết kế (`brainstorming`):** Ngay khi thấy bạn muốn xây dựng tính năng mới, agent sẽ không viết code ngay mà lùi lại đặt câu hỏi để làm rõ mục tiêu thực sự. Sau đó, nó chia nhỏ bản đặc tả kỹ thuật (spec) thành từng phần ngắn gọn để bạn duyệt và tạo tài liệu thiết kế.
2. **Lập kế hoạch thực thi chi tiết (Implementation Planning):** Sau khi bạn chốt thiết kế, agent lập một bản kế hoạch chi tiết đến mức một lập trình viên ít kinh nghiệm nhất cũng có thể làm theo, tuân thủ nghiêm ngặt 3 nguyên tắc: **Red/Green TDD** (viết test báo đỏ trước rồi mới viết code cho xanh), **YAGNI** (không viết thừa những thứ chưa cần) và **DRY** (tránh lặp code), kết hợp quản lý nhánh độc lập qua `using-git-worktrees`.
3. **Phát triển dựa trên các tác nhân con (`subagent-driven-development` - SDD):** Khi bạn ra lệnh bắt đầu ("go"), agent chính sẽ đóng vai trò điều phối, giao từng tác vụ nhỏ trong kế hoạch cho các **subagents** thực hiện độc lập, sau đó tự động kiểm tra và đánh giá chéo kết quả (code review) trước khi chuyển sang bước tiếp theo.
4. **Tự hành dài hạn bám sát kế hoạch:** Nhờ cơ chế chia nhỏ và kiểm soát qua subagent, coding agent có thể tự chạy liên tục hàng giờ liền để hoàn thiện tính năng lớn mà không bị trượt khỏi thiết kế ban đầu hay tràn ngữ cảnh (context window).

## Cách cài đặt cho Claude Code (Mục `#claude-code`)

Phần `#claude-code` trong liên kết bạn gửi hướng dẫn 2 cách cài đặt Superpowers trực tiếp vào **Claude Code** thông qua hệ thống plugin:

* **Cách 1 — Cài từ chợ ứng dụng chính thức của Anthropic (Official Marketplace):**
Chạy lệnh sau trực tiếp trong cửa sổ dòng lệnh của Claude Code:
```text
/plugin install superpowers@claude-plugins-official

```


* **Cách 2 — Cài từ chợ riêng của tác giả (`superpowers-marketplace`):**
Đăng ký kho plugin và cài đặt bằng 2 lệnh:
```text
/plugin marketplace add obra/superpowers-marketplace
/plugin install superpowers@superpowers-marketplace

```



## Khả năng tương thích đa nền tảng

Bên cạnh Claude Code (`.claude-plugin`), kho mã này đã được đóng gói sẵn cấu hình mở rộng cho hầu hết các công cụ lập trình AI phổ biến hiện nay:

* **Antigravity:** `agy plugin install [https://github.com/obra/superpowers](https://github.com/obra/superpowers)`
* **Gemini CLI:** `gemini extensions install [https://github.com/obra/superpowers](https://github.com/obra/superpowers)`
* **Cursor:** Chạy lệnh `/add-plugin superpowers` trong khung chat Cursor Agent.
* **Các công cụ khác:** Hỗ trợ sẵn cho **Codex (App & CLI)**, **GitHub Copilot CLI**, **Devin CLI**, **OpenCode**, **Qwen Code**, **Kimi Code**, **Grok Build CLI**, **Factory Droid**, **Hermes Agent**, **Pi** và **Muse**.

## So sánh vị trí của `obra/superpowers` với các kho mã trước

Đặt cạnh những kho GitHub bạn vừa tìm hiểu, **Superpowers** nằm ở tầng **quy trình làm việc (Workflow & Methodology)**:

| Kho mã GitHub | Trọng tâm chính | Điểm khác biệt của `obra/superpowers` |
| --- | --- | --- |
| **`Shubhamsaboo/awesome-llm-apps`** | Tập hợp hơn 100 ứng dụng AI độc lập và các tiện ích `agent_skills` rời rạc. | `superpowers` không phải là các tiện ích lẻ tẻ, mà là **một quy trình SDLC khép kín** (Brainstorm $\rightarrow$ Spec $\rightarrow$ Plan $\rightarrow$ TDD $\rightarrow$ Subagents). |
| **`x1xhlol/system-prompts...`** | Tài liệu lưu trữ prompt gốc bị lộ của các công cụ như Cursor, Claude Code. | `superpowers` là bộ kỹ năng **cài đè thêm** vào chính Claude Code, Cursor để nâng cấp cách chúng suy nghĩ và viết code. |
| **`stitionai/devika`** | Một phần mềm AI Engineer độc lập (đang ở giai đoạn thử nghiệm cũ). | `superpowers` tận dụng sức mạnh của các CLI/IDE hiện đại nhất (Claude Code, Cursor, Antigravity) và biến chúng thành đội ngũ kỹ sư bài bản. |
