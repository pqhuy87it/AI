https://github.com/DietrichGebert/ponytail

**[DietrichGebert/ponytail](https://github.com/dietrichgebert/ponytail)** là bộ **skill và ruleset** (tập quy tắc điều hướng prompt) dành cho các AI coding agent, được thiết kế để buộc AI tư duy như một "Senior Developer lười biếng nhưng dày dạn kinh nghiệm" theo triết lý **YAGNI** (*"The best code is the code you never wrote"* — code tốt nhất là code bạn không cần phải viết).

Thay vì để AI tự động sinh hàng trăm dòng boilerplate, cài thêm thư viện ngoài hoặc over-engineer các class phức tạp, [Ponytail](https://deepwiki.com/DietrichGebert/ponytail) ép agent giải quyết bài toán với lượng code mới tối thiểu nhưng vẫn giữ nguyên các chốt chặn quan trọng.

### Triết lý hoạt động và Ví dụ thực tế

* **Ưu tiên tận dụng cái có sẵn:** Kiểm tra xem yêu cầu có thực sự cần viết code không, ưu tiên dùng hàm có sẵn trong codebase, standard library hoặc tính năng native của nền tảng trước khi cài thêm dependency hay tự viết mới ([Awesome MCP Servers](https://mcpservers.org/agent-skills/dietrichgebert/ponytail)).
* **Tối giản nhưng không cẩu thả:** Giảm tối đa số dòng code (benchmark thực tế của repo ghi nhận giảm **53% lượng code**, **45% lượng token** và **41% thời gian chạy**), nhưng tuyệt đối không cắt bỏ bước validate dữ liệu đầu vào, xử lý lỗi, bảo mật hay accessibility ([GitHub - DietrichGebert/ponytail](https://github.com/dietrichgebert/ponytail)).
* **Tự động kèm test cho logic nhạy cảm:** Bất kỳ đoạn logic nào có rẽ nhánh, vòng lặp, parser, xử lý tiền tệ hoặc bảo mật đều được viết kèm một bài test nhỏ gọn (98% logic rủi ro có test đi kèm so với 68% khi không dùng skill) ([GitHub - DietrichGebert/ponytail](https://github.com/dietrichgebert/ponytail)).
* **Ví dụ đối chiếu trực quan:** Khi được yêu cầu thêm một bộ chọn ngày (date picker) vào frontend, một AI thông thường có thể cài thêm thư viện lịch hoặc tự dựng component dài 335 dòng code; trong khi đó, [Ponytail](https://github.com/dietrichgebert/ponytail) kiểm tra repo thấy đã có sẵn `Input` component và kết hợp trực tiếp với date picker mặc định của trình duyệt chỉ trong **10 dòng code**.

### Cấu trúc mã nguồn và Khả năng tương thích

Thực chất lõi của repository này là một bộ prompt điều hướng nằm tại file [`skills/ponytail/SKILL.md`](https://github.com/DietrichGebert/ponytail/blob/main/skills/ponytail/SKILL.md) và bản rút gọn [`AGENTS.md`](https://github.com/DietrichGebert/ponytail/blob/main/AGENTS.md) ([Pi Packages](https://pi.dev/packages/@dietrichgebert/ponytail)). Toàn bộ các thư mục và script còn lại trong repo (JavaScript, Python, TypeScript) đóng vai trò là **adapter, plugin và lifecycle hooks** để nạp bộ quy tắc này vào khoảng 20 công cụ lập trình AI phổ biến như Claude Code, Codex, Cursor, Windsurf, Cline, GitHub Copilot, OpenCode, Gemini và Antigravity CLI ([GitHub - DietrichGebert/ponytail](https://github.com/dietrichgebert/ponytail), [DeepWiki](https://deepwiki.com/DietrichGebert/ponytail)).

### Các lệnh và chế độ chính trong bộ công cụ

Khi cài đặt dưới dạng skill hoặc plugin (ví dụ qua `npx skills add dietrichgebert/ponytail` hoặc plugin marketplace), bộ công cụ cung cấp các chế độ làm việc và kiểm tra độc lập ([Collective Brain](https://collectivebrain.de/en/skills/ponytail/), [Pi Packages](https://pi.dev/packages/@dietrichgebert/ponytail)):

| Lệnh / Chế độ | Vai trò | Cơ chế hoạt động chi tiết |
| --- | --- | --- |
| `/ponytail [lite | full | ultra]` | Chế độ code tối giản | Điều chỉnh mức độ khắt khe: `lite` làm theo yêu cầu và gợi ý cách gọn hơn 1 dòng; `full` (mặc định) viết thay đổi nhỏ nhất và báo cáo phần lược bỏ/rủi ro; `ultra` phản biện thẳng xem yêu cầu có thực sự cần làm hay không trước khi viết code ([Collective Brain](https://collectivebrain.de/en/skills/ponytail/)). |
| `/ponytail-review` | Review thay đổi (PR/Diff) | Đọc cả phần code liên đới chứ không chỉ nhìn diff để bắt lỗi logic, lỗ hổng bảo mật, hiệu năng chịu tải thực tế, thiếu test và code thừa; chỉ đọc và xuất báo cáo chứ không tự sửa code ([QAI](https://qai.io/skills/github/dietrichgebert/ponytail/ponytail-review)). |
| `/ponytail-audit` | Audit toàn bộ repo | Lập bản đồ entry points và luồng dữ liệu của cả dự án, sau đó xếp hạng tối đa 20 vấn đề theo mức độ ưu tiên (*Must fix*, *Should fix*, *Nice to have*) kèm cách sửa gọn nhất ([Collective Brain](https://collectivebrain.de/en/skills/ponytail/), [GitHub](https://github.com/dietrichgebert/ponytail)). |
| `/ponytail-debt` | Quản lý nợ kỹ thuật | Quét toàn bộ các comment đánh dấu `shortcut:` (những chỗ cố tình làm tắt để code gọn) và gom thành sổ cái (debt ledger) kèm điều kiện khi nào cần quay lại xử lý ([Pi Packages](https://pi.dev/packages/@dietrichgebert/ponytail)). |
| `/ponytail band` | Thanh theo dõi chi phí | Hiển thị thanh trạng thái ngay trên khung nhập lệnh (dành cho Claude Code) thống kê chi phí task hiện tại, số token, thời gian chạy, tỷ lệ tái sử dụng code và trạng thái pass/fail của test ([GitHub - DietrichGebert/ponytail](https://github.com/dietrichgebert/ponytail)). |
| `/caveman` | Phản hồi siêu nén | Yêu cầu AI trả lời đi thẳng vào vấn đề, cắt bỏ toàn bộ lời chào hỏi hay câu chữ rườm rà (nhưng tự động quay về ngôn ngữ rõ ràng khi cảnh báo bảo mật hoặc thao tác không thể hoàn tác) ([Collective Brain](https://collectivebrain.de/en/skills/ponytail/)). |
