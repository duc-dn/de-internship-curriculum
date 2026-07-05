# 📝 Lab 5: Pipeline xử lý dữ liệu song song (Multi-file Parallel Processing)

Bài thực hành nâng cao này hướng dẫn Intern cách xây dựng một Data Pipeline thu thập và làm sạch hàng chục file dữ liệu thô hàng ngày một cách tối ưu bằng phương pháp lập trình song song, đồng thời thiết kế cơ chế chịu lỗi cho hệ thống.

---

## 🎯 Kiến thức Intern cần nắm được (Target Knowledge)
Sau bài lab này, Intern phải giải thích được:
1.  **GIL (Global Interpreter Lock):** Tại sao Python thuần không thể tận dụng đa nhân CPU cho các tác vụ tính toán, và cách giải quyết bằng cơ chế Multi-processing.
2.  **ThreadPool vs ProcessPool:** Khi nào dùng Thread (I/O-bound: tải file, gọi API) và khi nào dùng Process (CPU-bound: phân tích dữ liệu, tính toán lớn).
3.  **Pathlib Library:** Cách sử dụng lập trình hướng đối tượng để tương tác với File System thay vì thư viện `os` kiểu cũ.
4.  **Pipeline Chịu lỗi (Fault-tolerance):** Thiết kế hệ thống tự động xử lý khi một phần luồng dữ liệu bị lỗi cấu trúc mà không làm gián đoạn toàn bộ tiến trình lớn.
5.  **Dịch chuyển trạng thái File (Directory Shifting):** Kỹ thuật quản lý luồng dữ liệu thô chuyển qua các trạng thái: Đang xử lý -> Thành công (Processed) hoặc Thất bại (Failed).

---

## 📂 Đề bài chi tiết (Detailed Requirements)

### 1. Kịch bản chuẩn bị dữ liệu (Data Setup)
Intern cần viết một script helper `generate_raw_data.py` để tự động tạo ra thư mục `data/raw/` chứa **30 tệp tin CSV** dạng `transactions_YYYYMMDD.csv` đại diện cho 30 ngày giao dịch.
*   Mỗi file chứa ngẫu nhiên ~5,000 dòng dữ liệu bán hàng.
*   *Lưu ý:* Cố tình tạo ra 2 file lỗi (ví dụ: một file rỗng dung lượng 0 bytes, một file chứa ký tự nhị phân rác không thể parse).

### 2. Yêu cầu thiết lập cấu trúc thư mục Pipeline:
```text
parallel_pipeline/
├── data/
│   ├── raw/         # Chứa 30 file CSV ban đầu
│   ├── processed/   # Lưu các file thô đã xử lý thành công
│   └── failed/      # Lưu các file bị lỗi cấu trúc để điều tra
├── src/
│   ├── __init__.py
│   ├── processor.py # Chứa logic xử lý file đơn lẻ và xử lý song song/đa luồng
│   └── utils.py
├── main.py          # Điểm chạy chương trình chính
├── run.log          # File ghi log lịch sử chạy
└── README.md
```

### 3. Yêu cầu kỹ thuật chi tiết:
*   **Logic xử lý file đơn lẻ (`processor.py`):**
    *   Hàm `process_single_file(file_path: Path) -> bool`:
        *   Đọc file CSV bằng Pandas DataFrame.
        *   Loại bỏ các bản ghi trùng lặp (`drop_duplicates`).
        *   Nếu file rỗng hoặc lỗi parser (ví dụ quăng ra lỗi `EmptyDataError`, `ParserError`), bắt exception, di chuyển file thô đó từ `data/raw/` sang `data/failed/`, ghi log lỗi mức `ERROR` kèm trackback và trả về `False`.
        *   Nếu thành công, tính tổng doanh thu của ngày đó, lưu kết quả tổng hợp vào một file kết quả trung gian, di chuyển file thô ban đầu sang `data/processed/` và trả về `True`.
*   **Logic điều phối song song (`main.py`):**
    *   Quét thư mục `data/raw/` để tìm tất cả các file CSV bằng `.glob()`.
    *   Intern viết chương trình để thực thi và đo lường 3 kịch bản chạy độc lập sau:
        *   **Kịch bản 1: Chạy Tuần tự (Sequential)**
            *   Dùng vòng lặp `for` chạy lần lượt hàm `process_single_file` cho từng file.
            *   Đo lường tổng thời gian chạy bằng thư viện `time`.
        *   **Kịch bản 2: Chạy Đa luồng (Multi-threading)**
            *   Sử dụng `concurrent.futures.ThreadPoolExecutor` với `max_workers = 4`.
            *   Phân phối danh sách file chạy song song và thu thập kết quả trả về. Đo thời gian.
        *   **Kịch bản 3: Chạy Đa tiến trình (Multi-processing)**
            *   Sử dụng `concurrent.futures.ProcessPoolExecutor` với `max_workers = 4`.
            *   Phân phối danh sách file chạy song song và thu thập kết quả trả về. Đo thời gian.
    *   In bảng so sánh thời gian chạy của cả ba kịch bản ra màn hình Console.

---

## 📥 Định nghĩa đầu ra & Sản phẩm bàn giao (Expected Outputs & Deliverables)

### 1. File log `run.log`
Chứa thông tin chi tiết:
```text
2026-07-05 11:00:00 [INFO] Scanning data/raw/ directory... Found 30 CSV files.
2026-07-05 11:00:00 [INFO] Starting execution with 4 workers.
2026-07-05 11:00:02 [ERROR] Error processing transactions_20260715.csv: File is empty. Moving to failed/.
2026-07-05 11:00:05 [INFO] Finished processing. 28 success, 2 failed.
2026-07-05 11:00:05 [INFO] Sequential execution time: 18.52 seconds.
2026-07-05 11:00:05 [INFO] Multi-threading execution time: 17.80 seconds.
2026-07-05 11:00:05 [INFO] Multi-processing execution time: 5.24 seconds.
```

### 2. Tổ chức thư mục sau khi chạy xong
*   Thư mục `data/raw/` phải trống rỗng.
*   Thư mục `data/processed/` chứa đúng 28 file.
*   Thư mục `data/failed/` chứa đúng 2 file bị lỗi.

---

## 📈 Tiêu chí đánh giá của Mentor (Validation Criteria)
1.  **GIL & Threading analysis:** Intern phải giải thích được trong phần báo cáo tại sao phương pháp Multi-threading lại không cải thiện thời gian chạy đáng kể so với chạy tuần tự (do Pandas CPU-bound bị block bởi GIL), trong khi Multi-processing lại chạy nhanh hơn rõ rệt.
2.  **No Data Loss:** Nếu chạy nửa chừng bị lỗi một file, hệ thống vẫn tiếp tục chạy xong các file còn lại và di chuyển file lỗi sang thư mục `failed/` an toàn ở cả 3 kịch bản.
3.  **Concurrency Pool Management:** Sử dụng đúng context manager `with ThreadPoolExecutor(...) as executor:` để tự động giải phóng tài nguyên.
