https://github.com/microsoft/autogen

**[microsoft/autogen](https://github.com/microsoft/autogen)** là một framework lập trình mã nguồn mở (hỗ trợ **Python** và **.NET/C#**) do **Microsoft Research** khởi xướng, dùng để xây dựng các ứng dụng **Multi-Agent AI điều phối thông qua hội thoại (conversation-driven)** và kiến trúc hướng sự kiện (event-driven) ([IBM](https://www.ibm.com/think/topics/autogen), [Atlan](https://atlan.com/know/ai-agent/what-is-autogen/)).

Điểm khác biệt cốt lõi của [AutoGen](https://galileo.ai/blog/autogen-framework-multi-agents) so với [CrewAI](https://github.com/crewaiinc/crewai) ở câu trước nằm ở tư duy thiết kế: nếu **CrewAI** chia công việc theo *Vai trò & Nhiệm vụ tuần tự/phân cấp (Role & Task pipeline)* như một dây chuyền phòng ban, thì **AutoGen** coi mọi tương tác là một **cuộc hội thoại đa chiều (Multi-Agent Conversation / GroupChat)** hoặc hệ thống truyền thông điệp bất đồng bộ (asynchronous message passing) — nơi các agent tự thảo luận, phản biện, viết code, chạy thử trong sandbox và sửa lỗi cho nhau đến khi đạt kết quả ([Atlan](https://atlan.com/know/ai-agent/what-is-autogen/), [Galileo AI](https://galileo.ai/blog/autogen-framework-multi-agents)).

![](https://encrypted-tbn0.gstatic.com/images?q=tbn:ANd9GcQvorQece_6B31jh40VuS6H7_1idj_PWwdpaXFaHKbM_172Gc2bP5nsWvqw&s=10)

### Kiến trúc 3 tầng và Bộ công cụ trong Repository

Từ phiên bản 0.4 trở đi, mã nguồn của **[microsoft/autogen](https://github.com/microsoft/autogen)** (trong hai thư mục `python/` và `dotnet/`) được thiết kế lại hoàn toàn theo kiến trúc phân tầng rõ ràng như trong sơ đồ trên ([IBM](https://www.ibm.com/think/topics/autogen), [GitHub - microsoft/autogen](https://github.com/microsoft/autogen)):

| Thành phần / Package | Tên gói cài đặt (PyPI / NuGet) | Chức năng chi tiết |
| --- | --- | --- |
| **1. Core API** | `autogen-core` / `Microsoft.AutoGen.Core` | Tầng nền tảng thấp nhất xử lý cơ chế truyền thông điệp (message passing), quản lý vòng đời agent hướng sự kiện (event-driven) và hỗ trợ chạy phân tán (distributed runtime) xuyên ngôn ngữ giữa Python và .NET qua gRPC ([GitHub](https://github.com/microsoft/autogen), [IBM](https://www.ibm.com/think/topics/autogen)). |
| **2. AgentChat API** | `autogen-agentchat` | Tầng API bậc cao xây trên Core để lập trình nhanh các mẫu đa tác tử quen thuộc: chat 2 chiều (`AssistantAgent` + `UserProxyAgent`), thảo luận nhóm (`GroupChat`), hoặc biến một agent chuyên gia thành tool (`AgentTool`) cho agent khác gọi ([GitHub](https://github.com/microsoft/autogen), [Galileo AI](https://galileo.ai/blog/autogen-framework-multi-agents)). |
| **3. Extensions API** | `autogen-ext` | Kho mở rộng kết nối với các LLM client (OpenAI, Azure OpenAI, Anthropic...), bộ thực thi code cô lập trong Docker sandbox, công cụ duyệt web tự động (`MultimodalWebSurfer`) và tích hợp trực tiếp **MCP Server** (`McpWorkbench`) ([GitHub](https://github.com/microsoft/autogen), [IBM](https://www.ibm.com/think/topics/autogen)). |
| **AutoGen Studio** | `autogenstudio` | Giao diện web low-code/no-code (chạy tại cổng `8080`) gồm *Team Builder* kéo-thả, *Playground* xem trực tiếp luồng tin nhắn giữa các agent và *Gallery* để dựng prototype nhanh mà không cần viết code ([AutoGen Studio Docs](https://microsoft.github.io/autogen/dev/user-guide/autogenstudio-user-guide/index.html)). |
| **AutoGen Bench & Magentic-One** | Đi kèm trong hệ sinh thái repo | `AutoGen Bench` dùng để đo kiểm (benchmark) hiệu năng của agent; còn `Magentic-One` là một đội ngũ multi-agent mẫu hoàn chỉnh có khả năng tự lướt web, đọc file và viết/chạy code giải quyết tác vụ mở ([GitHub](https://github.com/microsoft/autogen), [Microsoft DevBlogs](https://devblogs.microsoft.com/autogen/microsofts-agentic-frameworks-autogen-and-semantic-kernel/)). |

### Lưu ý quan trọng: Trạng thái Lưu trữ (Archived) & Các nhánh kế nhiệm

Một thông tin rất quan trọng khi bạn quyết định có dùng mã nguồn này cho dự án mới hay không: hiện tại, Microsoft đã chính thức chuyển repo **[microsoft/autogen](https://github.com/microsoft/autogen)** sang trạng thái **Archived (Lưu trữ / Ngừng bảo trì tính năng mới)** để hợp nhất lực lượng vào sản phẩm kế nhiệm ([GitHub - microsoft/autogen](https://github.com/microsoft/autogen), [Atlan](https://atlan.com/know/ai-agent/what-is-autogen/)):

* **Microsoft Agent Framework (MAF):** Là hậu duệ chính thức cấp độ Enterprise (đã đạt bản 1.0 GA cho Python và .NET), hợp nhất sức mạnh điều phối đa tác tử của AutoGen với nền tảng **Semantic Kernel** của Microsoft, bổ sung hỗ trợ dài hạn và tương thích chuẩn MCP/A2A ([GitHub - microsoft/autogen](https://github.com/microsoft/autogen), [Microsoft DevBlogs](https://devblogs.microsoft.com/autogen/microsofts-agentic-frameworks-autogen-and-semantic-kernel/)).
* **AG2 (trước đây là AutoGen 0.2):** Là nhánh fork mã nguồn mở độc lập (Apache 2.0) do chính hai tác giả khai sinh ra AutoGen ban đầu là Chi Wang và Qingyun Wu tách ra tiếp tục phát triển cùng cộng đồng ([IBM](https://www.ibm.com/think/topics/autogen), [Atlan](https://atlan.com/know/ai-agent/what-is-autogen/)).

### Cách cài đặt và chạy thử nhanh

Nếu bạn muốn trải nghiệm kiến trúc của AutoGen hoặc chạy thử giao diện **AutoGen Studio** trên máy (yêu cầu Python 3.10+):

```bash
# 1. Cài đặt thư viện lập trình AgentChat và Extensions
pip install -U "autogen-agentchat" "autogen-ext[openai]"

# 2. Hoặc cài đặt và mở giao diện kéo-thả AutoGen Studio tại http://localhost:8080
pip install -U "autogenstudio"
autogenstudio ui --port 8080 --appdir ./my-app

```
