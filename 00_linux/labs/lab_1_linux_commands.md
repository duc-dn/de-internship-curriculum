# 📝 Lab 1: Giám sát log và Tự động hóa sao lưu bằng Bash Script

Bài thực hành này giúp Intern làm quen với hệ điều hành Linux (Ubuntu/Debian), thành thạo các câu lệnh CLI quản lý hệ thống tệp tin, phân quyền, tìm kiếm văn bản và viết kịch bản tự động hóa (Bash Script) phục vụ công việc quản trị dữ liệu hàng ngày.

---

## 🎯 Kiến thức Intern cần nắm được (Target Knowledge)
Sau bài lab này, Intern phải giải thích được:
1.  **Dòng lệnh điều hướng (Navigation & File Ops):** Cách sao chép, di chuyển, xóa tệp tin và tạo cấu trúc thư mục an toàn trên Linux.
2.  **Bộ lọc văn bản (Text Filtering):** Cơ chế hoạt động của `grep` và `find` để định vị dữ liệu lỗi hoặc tìm tệp tin theo điều kiện.
3.  **Dẫn hướng & Đường ống (Pipes & Redirection):** Cách kết hợp nhiều lệnh Linux đơn lẻ thành một chuỗi xử lý phức tạp qua toán tử `|` và `>>`.
4.  **Phân quyền (Permissions):** Tầm quan trọng của quyền thực thi (`chmod +x`) và quyền sở hữu trong bảo mật thư mục dữ liệu.
5.  **Bash Scripting:** Cách khai báo biến, kiểm tra thư mục tồn tại, nén file (`tar`) và xóa file tự động theo ngày tháng.

---

## 📂 Đề bài chi tiết (Detailed Requirements)

Intern thực hành trực tiếp trên Terminal của máy tính macOS/Linux, hoặc khởi chạy một container Ubuntu thông qua Docker để làm bài.

### 1. Chuẩn bị cấu trúc thư mục thực hành:
Intern chạy các lệnh hệ thống để tạo cấu trúc thư mục sau tại thư mục nhà:
```text
linux_practice/
├── logs/            # Chứa các file log thô hàng ngày
├── backups/         # Chứa các file nén sao lưu thành công
└── scripts/
    └── backup_logs.sh  # Script tự động hóa sao lưu Intern sẽ viết
```

Intern tự tạo ra **5 file log giả lập** trong thư mục `logs/`:
*   `app_20260701.log`
*   `app_20260702.log`
*   `app_20260703.log`
*   `app_20260704.log`
*   `app_20260705.log`
*   *Nội dung các file log:* Mỗi file chứa khoảng 20 dòng log ghi nhận hoạt động hệ thống. Trong đó cố tình chèn một vài dòng log chứa từ khóa `[ERROR]` hoặc `[CRITICAL]` để thực hành tìm kiếm.

### 2. Yêu cầu viết Script tự động hóa `backup_logs.sh`:
Script phải bắt đầu bằng dòng chỉ thị shebang: `#!/bin/bash`. Logic thực hiện của script gồm:

*   **Bước 2.1: Định nghĩa các biến**
    *   `LOG_DIR`: Đường dẫn tuyệt đối tới thư mục `logs/`.
    *   `BACKUP_DIR`: Đường dẫn tuyệt đối tới thư mục `backups/`.
    *   `DATE`: Ngày tháng hiện tại theo định dạng `YYYYMMDD_HHMMSS`.
*   **Bước 2.2: Kiểm tra thư mục**
    *   Sử dụng lệnh điều kiện `if [ ! -d "$BACKUP_DIR" ]` để kiểm tra thư mục backup đã tồn tại chưa. Nếu chưa, tự động tạo mới (`mkdir -p`).
*   **Bước 2.3: Tìm kiếm log lỗi và ghi nhận**
    *   Quét qua toàn bộ file trong thư mục `LOG_DIR`, tìm các dòng chứa từ khóa `[ERROR]` và ghi nối tiếp (append) tất cả các dòng lỗi này vào một file tổng hợp lỗi đặt tên là `error_report_$DATE.txt` lưu tại `BACKUP_DIR`.
*   **Bước 2.4: Đóng gói và Nén sao lưu**
    *   Sử dụng lệnh `tar -czf` để đóng gói toàn bộ thư mục `logs/` thành file nén dạng `logs_backup_$DATE.tar.gz` lưu trữ tại thư mục `BACKUP_DIR`.
*   **Bước 2.5: Dọn dẹp dữ liệu cũ (Retention Policy)**
    *   Tìm và xóa toàn bộ các tệp tin nén sao lưu cũ trong thư mục `BACKUP_DIR` đã được tạo **quá 7 ngày** sử dụng lệnh `find` kết hợp `-mtime` và `-delete` (hoặc `-exec rm`).

---

## 📥 Định nghĩa đầu ra & Sản phẩm bàn giao (Expected Outputs)

### 1. Script `backup_logs.sh` hoàn chỉnh
*   File script được định dạng viết sạch sẽ, có giải thích comment các bước.
*   File script đã được cấp quyền thực thi: `chmod +x backup_logs.sh`.

### 2. Log chạy thử thành công
Khi gõ lệnh chạy `./backup_logs.sh`, chương trình tự động thực thi và sinh ra trong thư mục `backups/`:
*   Một file nén `logs_backup_YYYYMMDD_HHMMSS.tar.gz` chứa toàn bộ log cũ.
*   Một file báo cáo lỗi `error_report_YYYYMMDD_HHMMSS.txt` chứa danh sách dòng log bị lỗi trích xuất từ tất cả các file log thô.

---

## 🏆 Các bài luyện tập bổ sung trên LeetCode / HackerRank:
*   [HackerRank: Cut #1 (Bash text filtering)](https://www.hackerrank.com/challenges/text-processing-cut-1/problem) (Cấp độ: Dễ - Cắt chuỗi văn bản bằng dòng lệnh)
*   [HackerRank: Grep #1 (Searching text patterns)](https://www.hackerrank.com/challenges/text-processing-in-bash-the-grep-command-1/problem) (Cấp độ: Dễ - Tìm kiếm với lệnh grep)
*   [HackerRank: Awk #1 (Structured log analysis)](https://www.hackerrank.com/challenges/text-processing-in-bash-the-awk-1/problem) (Cấp độ: Trung bình - Xử lý cột dữ liệu nâng cao với lệnh awk)

---

## 📈 Tiêu chí đánh giá của Mentor (Validation Criteria)
1.  **Shebang & Permissions:** File script phải có dòng `#!/bin/bash` ngay dòng 1 và chạy được trực tiếp bằng lệnh `./backup_logs.sh` (không cần gõ `bash backup_logs.sh`).
2.  **No Hardcoded Paths:** Các đường dẫn thư mục phải dùng biến môi trường hoặc biến tự định nghĩa ở đầu file để dễ cấu hình lại khi sang máy tính khác.
3.  **Error Handling (Redirection):** Kiểm tra xem file báo cáo lỗi được ghi chính xác bằng lệnh `grep` kết hợp redirection `>>` hay không.
