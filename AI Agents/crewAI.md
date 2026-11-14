https://github.com/crewAIInc/crewAI

**[crewAIInc/crewAI](https://github.com/crewaiinc/crewai)** là một **Python framework độc lập (code-first) chuyên dùng để điều phối nhiều AI Agent đóng vai (role-playing, autonomous multi-agent) phối hợp làm việc nhóm** nhằm giải quyết các quy trình tự động hóa phức tạp ([OSSInsight](https://ossinsight.io/analyze/crewAIInc/crewAI), [CrewAI Docs](https://docs.crewai.com/v1.15.39/en/introduction)).

Nếu đặt vào bức tranh tổng thể cùng các repo bạn vừa xem:

* **[agency-agents](https://github.com/msitarzewski/agency-agents)** là bộ sưu tập các file hồ sơ nhân vật (markdown prompts) nạp vào IDE để bạn gọi khi ngồi code.
* **[Langflow](https://github.com/langflow-ai/langflow)** và **[Dify](https://github.com/langgenius/dify)** là các nền tảng giao diện web kéo-thả node (low-code/no-code UI) để thiết kế luồng AI.
* **[CrewAI](https://github.com/crewaiinc/crewai)** là **thư viện lập trình Python thuần (không phụ thuộc LangChain)** dành cho developer muốn viết code hoặc cấu hình JSON/YAML để lập ra một "biệt đội" AI tự động phân công việc, trao đổi kết quả với nhau và nhúng thẳng vào backend ứng dụng ([GitHub - crewAIInc/crewAI](https://github.com/crewaiinc/crewai), [Run-as-Root Guide](https://run-as-root.sh/magento-kb/general/crewai-framework-guide)).

![](https://encrypted-tbn0.gstatic.com/images?q=tbn:ANd9GcQ8Rn1ZzpdiR6UGmwe0SgRAamM9JUOrDrd1EAHU7E6syyX7nrm6pRgCKZlu&s=10)

### Hai trụ cột kiến trúc của CrewAI: Crews và Flows

Điểm mạnh cốt lõi của [CrewAI](https://github.com/crewaiinc/crewai) là kết hợp giữa tính tự chủ của AI và tính kiểm soát chặt chẽ bằng code thông qua 2 thành phần bổ trợ nhau ([CrewAI Docs](https://docs.crewai.com/v1.15.39/en/introduction)):

* **CrewAI Crews (Tối ưu cho sự tự chủ & trí tuệ tập thể):** Cho phép bạn định nghĩa một nhóm các `Agent` (mỗi agent có `role`, `goal`, `backstory`, bộ `tools` và `llm` riêng) cùng danh sách các `Task` cụ thể ([GitHub - crewAIInc/crewAI](https://github.com/crewaiinc/crewai)). Các agent trong một Crew có thể chạy tuần tự (*Sequential Process* — người trước làm xong truyền kết quả cho người sau như trong sơ đồ trên) hoặc theo cấu trúc phân cấp (*Hierarchical Process* — có một Manager Agent tự động chia việc và nghiệm thu kết quả của các thành viên) ([GitHub - crewAIInc/crewAI](https://github.com/crewaiinc/crewai), [Run-as-Root Guide](https://run-as-root.sh/magento-kb/general/crewai-framework-guide)).
* **CrewAI Flows (Tối ưu cho kiểm soát luồng sự kiện & trạng thái):** Cung cấp các decorator trong Python (`@start`, `@listen`, `@router` cùng toán tử điều kiện `and_`, `or_`) và quản lý trạng thái có kiểu dữ liệu chặt chẽ qua Pydantic `BaseModel` ([GitHub - crewAIInc/crewAI](https://github.com/crewaiinc/crewai)). Nhờ đó, bạn dùng `Flow` để quản lý khung xương logic nghiệp vụ (if/else, vòng lặp, gọi API thường) và chỉ gọi một `Crew` vào đúng bước nào thực sự cần nhiều AI bàn bạc xử lý ([CrewAI Docs](https://docs.crewai.com/v1.15.39/en/introduction)).

### Các thành phần kỹ thuật bên trong Repository

Mã nguồn trong thư mục `lib/` của **[crewAIInc/crewAI](https://github.com/crewaiinc/crewai)** cung cấp sẵn toàn bộ các khối xây dựng (primitives) cho hệ thống Multi-Agent ở cấp độ Production ([CrewAI Docs](https://docs.crewai.com/v1.15.39/en/introduction)):

| Thành phần (Primitive) | Chức năng chi tiết |
| --- | --- |
| **Agents & Tasks** (`agents/*.jsonc`, `crew.jsonc`) | Định nghĩa vai trò, mục tiêu, ràng buộc đầu ra chuẩn hóa (ép kiểu đầu ra bằng `output_pydantic` hoặc `output_json`) và cơ chế *Human-in-the-loop* chờ con người duyệt ([GitHub - crewAIInc/crewAI](https://github.com/crewaiinc/crewai)). |
| **Sistem Memory đa tầng** | Tích hợp sẵn bộ nhớ ngắn hạn (Short-Term), dài hạn (Long-Term), bộ nhớ thực thể (Entity Memory) và ngữ cảnh để các agent nhớ thông tin xuyên suốt quá trình phối hợp ([GitHub - crewAIInc/crewAI](https://github.com/crewaiinc/crewai)). |
| **Knowledge & RAG tích hợp** (`knowledge/`) | Hỗ trợ nạp trực tiếp tài liệu PDF, CSV, Excel, JSONL, bảng tính và Vector Database làm nguồn tri thức chung cho cả đội agent tra cứu ([GitHub - crewAIInc/crewAI](https://github.com/crewaiinc/crewai)). |
| **Tools, MCP & A2A** (`tools/`) | Kết nối với kho công cụ có sẵn (tìm kiếm web Serper, đọc file, cào dữ liệu), tự viết custom tool bằng Python hoặc kết nối thẳng với các **MCP Server** và giao thức **A2A (Agent-to-Agent)** ([GitHub - crewAIInc/crewAI](https://github.com/crewaiinc/crewai)). |
| **Đa mô hình (LiteLLM & Local LLMs)** | Kết nối linh hoạt với OpenAI, Anthropic Claude, Google Gemini, hoặc chạy hoàn toàn local miễn phí qua **Ollama** và **LM Studio** ([GitHub - crewAIInc/crewAI](https://github.com/crewaiinc/crewai)). |
| **Agent Skills đi kèm** (`crewaiinc/skills`) | Cung cấp bộ skill chính chủ (cài qua `npx skills add crewaiinc/skills` hoặc plugin Claude Code) để dạy chính các AI coding agent trong IDE của bạn cách thiết kế Crew, Flow và Task đúng chuẩn nhất ([GitHub - crewAIInc/crewAI](https://github.com/crewaiinc/crewai)). |

### Cách khởi tạo và chạy thử một dự án CrewAI

Ở các phiên bản mới, [CrewAI](https://github.com/crewaiinc/crewai) sử dụng trình quản lý gói `uv` và hỗ trợ cấu trúc dự án **JSON-first** (`crew.jsonc`) cực gọn bên cạnh kiểu Python/YAML truyền thống (`--classic`):

```bash
# 1. Cài đặt CrewAI CLI toàn cục bằng uv (yêu cầu Python >= 3.10 và < 3.14)
uv tool install crewai

# 2. Khởi tạo một dự án Crew mới (tự sinh thư mục agents/, crew.jsonc, tools/, knowledge/)
crewai create crew my-research-team
cd my-research-team

# 3. Cài đặt dependencies và kích hoạt đội ngũ AI chạy việc
crewai install
crewai run

```

Khi chạy `crewai run`, các agent (ví dụ *Senior Data Researcher* và *Reporting Analyst*) sẽ tự động tìm kiếm thông tin, tổng hợp chéo và xuất báo cáo hoàn chỉnh ra file `output/report.md` ([GitHub - crewAIInc/crewAI](https://github.com/crewaiinc/crewai)).
