# 📝 Lab 6: Tối ưu bộ nhớ với Generators & Data Streaming

Bài thực hành nâng cao này giải quyết vấn đề kinh điển trong Data Engineering: Làm thế nào để xử lý tập dữ liệu dung lượng lớn (hàng triệu dòng) mà không làm tràn bộ nhớ RAM (lỗi Out-Of-Memory - OOM).

---

## 🎯 Kiến thức Intern cần nắm được (Target Knowledge)
Sau bài lab này, Intern phải giải thích được:
1.  **Memory Management (Quản lý bộ nhớ):** Cơ chế Python nạp dữ liệu vào RAM và tại sao việc dùng `.read()` hoặc `list(data)` trên file dung lượng lớn là hành vi nguy hiểm.
2.  **Generators & Yield:** Cơ chế hoạt động của hàm Generator, từ khóa `yield`, và trạng thái "lazy evaluation" (chỉ tính toán khi cần thiết).
3.  **Memory Profiling:** Cách sử dụng thư viện `tracemalloc` hoặc `sys.getsizeof` để đo lường lượng RAM tiêu thụ thực tế của chương trình.
4.  **Streaming Pipeline:** Thiết lập luồng đọc file theo dòng (line-by-line) kết hợp xử lý dữ liệu liên tục (streaming).

---

## 📂 Đề bài chi tiết (Detailed Requirements)

### 1. Kịch bản chuẩn bị dữ liệu (Data Setup)
Intern viết một script nhỏ để tự động tạo một tệp tin CSV lớn tên là `huge_logs.csv` (~500,000 dòng log giả lập) chứa cấu trúc:
`log_id, timestamp, level, message`
*   *Lưu ý:* File này có dung lượng đủ lớn để thấy rõ sự chênh lệch về RAM khi đọc.

### 2. Yêu cầu kỹ thuật thực thi:
Intern viết mã nguồn trong file `memory_optimized_stream.py` thực hiện so sánh 2 phương pháp:

#### Phương pháp 1: Đọc tuần tự tải toàn bộ (Naive Loading)
*   Viết hàm `load_all_logs(file_path: str) -> list`: Đọc toàn bộ file CSV cùng lúc, đưa tất cả các dòng vào một List lớn trong RAM, lọc ra các log có `level == "ERROR"`, sau đó trả về List đó.
*   Sử dụng thư viện `tracemalloc` để đo và in ra lượng bộ nhớ đỉnh điểm (Peak Memory Usage) của hàm này.

#### Phương pháp 2: Đọc dạng dòng chảy (Generator Ingestion)
*   Viết hàm `stream_logs_generator(file_path: str)`: Sử dụng từ khóa `yield` để đọc và trả về từng dòng log một (line-by-line) từ file CSV.
*   Viết hàm xử lý: Duyệt qua generator trên, lọc các dòng log có `level == "ERROR"`, chỉ đếm số lượng dòng lỗi mà không lưu trữ toàn bộ danh sách dòng lỗi vào RAM.
*   Sử dụng `tracemalloc` để đo lượng bộ nhớ đỉnh điểm của phương pháp này.

---

## 🖥️ 3. Yêu cầu làm Slide Thuyết trình (Presentation Requirement)

Trước khi gửi PR nộp bài Lab này, **Intern bắt buộc phải chuẩn bị một slide thuyết trình (5-7 trang)** trình bày trước Mentor và đội ngũ Data Engineer với các nội dung sau:
*   **Trang 1:** Tiêu đề & Giới thiệu bài toán OOM (Out Of Memory) trong ETL.
*   **Trang 2:** Cơ chế nạp bộ nhớ của List/Collection vs Iterator/Generator trong Python.
*   **Trang 3:** Giải thích chi tiết cơ chế hoạt động của từ khóa `yield` và vòng đời của một Generator Object.
*   **Trang 4:** Biểu đồ so sánh bộ nhớ tiêu thụ thực tế (Peak Memory) giữa Phương pháp 1 và Phương pháp 2 thu được từ bài Lab.
*   **Trang 5:** Kết luận: Khi nào nên sử dụng Generator và các ứng dụng thực tế trong Data Pipeline (Ví dụ: Đọc dữ liệu từ S3, gọi API Cursor-based).

---

## 📥 Định nghĩa đầu ra & Sản phẩm bàn giao (Expected Outputs & Deliverables)

### 1. File mã nguồn `memory_optimized_stream.py`
Code xử lý hoàn chỉnh kèm kết quả đo lường ghi lại trong log Console:
```text
--- Đọc Naive (Load All to List) ---
Số lượng log lỗi: 12453
Peak Memory: 45.24 MB

--- Đọc Stream (Generator yield) ---
Số lượng log lỗi: 12453
Peak Memory: 0.12 MB

=> Kết quả: Generator tiết kiệm 99.7% bộ nhớ RAM!
```

### 2. Slide thuyết trình định dạng PDF/PPTX
Được lưu trong thư mục `01_python_labs/lab_6_memory/presentation.pdf`.

---

## 📈 Tiêu chí đánh giá của Mentor (Validation Criteria)
1.  **Generator Integrity:** Hàm đọc stream phải sử dụng từ khóa `yield`, không được append dữ liệu vào bất kỳ list nào trước khi trả về.
2.  **Memory Profiling accuracy:** Intern giải thích đúng cách hoạt động của `tracemalloc` (sử dụng `get_traced_memory()`).
3.  **Slide Quality:** Slide trình bày rõ ràng, mạch lạc, giải thích đúng cơ chế Lazy Evaluation của Python.
