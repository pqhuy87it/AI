https://github.com/Shubhamsaboo/awesome-llm-apps

Source code tại `[https://github.com/Shubhamsaboo/awesome-llm-apps](https://github.com/Shubhamsaboo/awesome-llm-apps)` là **một bộ sưu tập hơn 100 ứng dụng mẫu hoàn chỉnh về AI Agents, Agent Skills và RAG (Retrieval-Augmented Generation)** do tác giả Shubham Saboo (Unwind AI) phát triển và tổng hợp (hiện đạt khoảng **144,6 nghìn sao** trên GitHub, phát hành dưới giấy phép **Apache-2.0**).

Khác với nhiều kho lưu trữ mang tên `awesome-...` trên GitHub vốn chỉ liệt kê đường dẫn bài viết hay thư viện bên ngoài, mỗi thư mục con trong kho này đều chứa **mã nguồn Python chạy được trực tiếp từ đầu đến cuối (end-to-end)** để bạn tải về thử nghiệm, học kiến trúc hoặc tùy biến thành sản phẩm riêng.

## Các nhóm mã nguồn chính bên trong kho lưu trữ

Kho mã được chia thành nhiều thư mục chuyên biệt, đi từ mức độ cơ bản đến các hệ thống đa tác nhân (multi-agent) phức tạp:

* **`agent_skills/` (Kỹ năng mở rộng cho Coding Agents):** Tập hợp các gói kỹ năng cắm trực tiếp vào công cụ lập trình AI như **Claude Code, Cursor, Codex** thông qua một dòng lệnh (`npx skills add ...`). Ví dụ tiêu biểu gồm *Commit Archaeologist* (truy vết lịch sử và lý do tồn tại của một đoạn code qua lịch sử commit), *Scope Creep Detector* (kiểm tra xem `git diff` có vượt quá phạm vi yêu cầu ban đầu hay không), *Dependency Doctor* (rà soát lỗi thư viện phụ thuộc) hay *Project Graveyard*.
* **`starter_ai_agents/` (Agents khởi đầu gọn nhẹ):** Các ứng dụng gói gọn trong một vài tệp mã nguồn, chỉ cần điền API key là chạy được ngay. Ví dụ: agent lên lịch trình du lịch, phân tích dữ liệu file CSV/Excel bằng ngôn ngữ tự nhiên, chuyển bài blog thành podcast có giọng đọc, hoặc tự động thu thập dữ liệu web (web scraping).
* **`advanced_ai_agents/` (Agents nâng cao & Đội ngũ Multi-Agent):** Các hệ thống kết hợp bộ nhớ (memory), suy luận nhiều bước và nhiều agent phối hợp theo vai trò. Ví dụ: *AI Deep Research Agent* (nghiên cứu chuyên sâu kết hợp Firecrawl), *AI System Architect Agent* (đánh giá kiến trúc hệ thống kết hợp DeepSeek R1 và Claude), đội ngũ phân tích tài chính, thẩm định đầu tư, tuyển dụng hoặc thiết kế UI/UX đa phương thức.
* **`mcp_ai_agents/` (Agents tích hợp Model Context Protocol):** Các ví dụ ứng dụng chuẩn giao tiếp **MCP** để kết nối mô hình AI với công cụ và nguồn dữ liệu bên ngoài, chẳng hạn như *Dependency Upgrade Advisor*.
* **`rag_tutorials/` (Hướng dẫn và ứng dụng RAG):** Các mẫu triển khai hệ thống hỏi đáp trên tài liệu riêng, kết hợp cơ sở dữ liệu vector với các mô hình đám mây lẫn chạy cục bộ.
* **`voice_ai_agents/` & `always_on_agents/`:** Bao gồm các agent giao tiếp giọng nói thời gian thực (tổng đài chăm sóc khách hàng, hướng dẫn viên du lịch bằng âm thanh) và các agent chạy ngầm theo lịch trình để tự động giám sát tin tức (Hacker News) hoặc theo dõi phiên bản thư viện mới (*Release Radar*).
* **`generative_ui_agents/` & `ai_agent_framework_crash_course/`:** Các mẫu agent có khả năng tự tạo giao diện tương tác động và loạt bài thực hành nhanh về các framework xây dựng agent phổ biến.

## Công nghệ sử dụng và cách khai thác thực tế

* **Ngăn xếp công nghệ (Tech Stack):** Phần lớn ứng dụng được viết bằng **Python**, đóng gói giao diện web nhanh bằng **Streamlit** và hỗ trợ linh hoạt hầu hết các mô hình hiện nay từ **Claude, Gemini, OpenAI GPT, DeepSeek** cho đến các mô hình mã nguồn mở chạy cục bộ như **Llama, Qwen**.
* **Cách chạy từng dự án:** Bạn không cần cài đặt toàn bộ kho mã khổng lồ này. Chỉ cần sao chép (clone) kho về máy, di chuyển vào đúng thư mục của ứng dụng bạn quan tâm, cài đặt thư viện trong tệp `requirements.txt` của riêng thư mục đó và khởi chạy bằng lệnh `streamlit run`.
* **Quyền sử dụng thương mại:** Nhờ giấy phép **Apache-2.0**, lập trình viên được phép sao chép, chỉnh sửa, đóng gói và sử dụng mã nguồn từ các mẫu này cho cả dự án cá nhân lẫn sản phẩm thương mại.

## Tổng hợp bức tranh 5 kho mã GitHub bạn đã xem

Đến thời điểm này, 5 kho lưu trữ bạn tìm hiểu tạo thành một bộ tài liệu tham khảo khá đầy đủ từ lý thuyết nền tảng đến ứng dụng thực chiến trong mảng AI Agent:

| Kho mã GitHub | Bản chất | Giá trị sử dụng chính |
| --- | --- | --- |
| **`how-to-build-a-coding-agent`** | **Bài hướng dẫn (Tutorial)** | Hiểu cơ chế vòng lặp lõi (LLM + Tool Calling) bằng code thuần từ con số 0. |
| **`langchain-ai/langchain`** | **Thư viện nền tảng (Framework)** | Bộ SDK chuẩn hóa để kết nối đa mô hình AI, dữ liệu RAG và công cụ. |
| **`x1xhlol/system-prompts-and-models-of-ai-tools`** | **Kho câu lệnh (Prompts & Schemas)** | Tham khảo cách các sản phẩm lớn viết System Prompt và định nghĩa hàm. |
| **`stitionai/devika`** | **Sản phẩm đơn lẻ (End-user App)** | Xem kiến trúc một ứng dụng AI tự lập trình có cả Frontend lẫn Backend. |
| **`Shubhamsaboo/awesome-llm-apps`** | **Kho ứng dụng mẫu (Templates & Skills)** | Lấy code mẫu chạy sẵn cho hơn 100 kịch bản thực tế và cài thêm `agent_skills`. |
