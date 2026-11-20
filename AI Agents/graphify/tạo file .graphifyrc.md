Ran inspect full graphify/rcfile.py and check .h/.m files in s-relay
Ran check existing .graphifyrc and viz_node_limit in hooks.py
Created .graphifyrc
Ran verify .graphifyrc parsing and query budget behavior

Tôi đã tạo file [.graphifyrc](file:///Users/phamhuy/Syntra%20AI/Projects/s-relay/.graphifyrc) tại thư mục gốc của `s-relay` với nội dung sau:

* `query_budget=4000` (nâng từ mặc định `2000` để hạn chế cắt cụt kết quả trên đồ thị ~7,500 nodes của `s-relay`).
* `query_depth=2` (độ sâu duyệt mặc định; khi cần lần chuỗi sâu hơn vẫn truyền `--depth 3` hoặc `--depth 4` để ghi đè).
* Kèm dòng gợi ý `# viz_node_limit=0` (đang comment) phòng trường hợp sau này bạn cài `graphify hook install` và muốn bỏ qua bước xuất `graph.html` nặng mỗi lần commit.

*(Do `s-relay` là codebase Swift thuần — không có file `.h` / `.m` / `.mm` — nên không cần khai báo thêm `language.<ext>`).*

### Kết quả kiểm tra thực tế
* `load_graphifyrc(Path('.'))` đọc thành công không lỗi/cảnh báo: `{'query_budget': 4000, 'query_depth': 2}`.
* Chạy thử `graphify query "LinkDetailViewModel"` cho thấy ngân sách mặc định đã tăng lên **`~4000-token budget`** (trả về `100 / 110 nodes`, thay vì `51 / 110 nodes` như trước).

Bạn có muốn tôi tạo luôn `.graphifyrc` (và `.graphifyignore`) cho các repo khác như `s-ecom-fe`, `s-ecom-arm`, `s-ecom-bff` không?
