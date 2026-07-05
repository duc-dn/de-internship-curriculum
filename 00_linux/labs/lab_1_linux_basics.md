# 📝 Lab 1: Các câu lệnh Linux cơ bản & Giám sát hệ thống (40 Bài tập)

Tài liệu này bao gồm **40 bài tập thực hành chia làm 4 chủ đề chính** sắp xếp theo độ khó tăng dần từ Rất dễ đến Khó để Intern rèn luyện tư duy vận hành hệ điều hành Linux (Ubuntu/Debian) ngay trên Terminal.

---

## 🎯 Tại sao Data Engineer phải nắm được các lệnh cơ bản này?
1.  **Làm việc trực tiếp trên Terminal:** Các máy chủ chứa cơ sở dữ liệu hoặc pipeline Airflow không có giao diện đồ họa. Bạn bắt buộc phải tương tác thông qua dòng lệnh.
2.  **Giám sát RAM/CPU:** Khi viết Data Pipeline bị chạy chậm, bạn phải biết kiểm tra xem hệ thống có bị nghẽn RAM (`free -h`), đầy ổ đĩa (`df -h`), hay CPU đang bị chiếm bởi tiến trình nào (`top`/`htop`).
3.  **Xử lý Log và File thô:** Đếm số dòng dữ liệu (`wc -l`), lọc nhanh log lỗi (`grep "[ERROR]"`) mà không cần mở file lớn tránh tràn RAM.

---

## 📂 40 BÀI TẬP THỰC HÀNH (4 CHỦ ĐỀ - 10 BÀI/CHỦ ĐỀ)

---

### CHỦ ĐỀ 1: Thao tác Hệ thống File & Điều hướng CLI

*   **Bài 1.1 [Rất dễ]:** Hiển thị đường dẫn đầy đủ của thư mục hiện tại bạn đang đứng (`pwd`).
*   **Bài 1.2 [Rất dễ]:** Liệt kê toàn bộ các file trong thư mục hiện tại bao gồm cả các file ẩn (bắt đầu bằng dấu `.`) kèm theo kích thước định dạng dễ đọc như KB/MB (`ls -laH`).
*   **Bài 1.3 [Dễ]:** Tạo một cấu trúc thư mục lồng nhau dạng `data/raw/2026/07/` chỉ bằng một câu lệnh duy nhất (`mkdir -p`).
*   **Bài 1.4 [Dễ]:** Tạo một tệp tin trống tên là `README.txt` và ghi nội dung `"Data Engineering Curriculum 2026"` vào đó bằng lệnh `echo` kết hợp dẫn hướng xuất `>`.
*   **Bài 1.5 [Trung bình - Dễ]:** Sao chép tệp `README.txt` vào thư mục `data/raw/2026/07/` vừa tạo.
*   **Bài 1.6 [Trung bình - Dễ]:** Di chuyển tệp `README.txt` trong thư mục `data/raw/2026/07/` ra thư mục cha của nó (`data/raw/2026/`) và đổi tên thành `info.metadata`.
*   **Bài 1.7 [Trung bình]:** Kiểm tra dung lượng ổ đĩa còn trống của toàn bộ các phân vùng trên hệ thống (`df -h`).
*   **Bài 1.8 [Trung bình]:** Tính toán tổng dung lượng thực tế đang bị chiếm dụng bởi thư mục `data/` hiển thị định dạng tổng hợp dễ đọc (`du -sh`).
*   **Bài 1.9 [Trung bình - Khó]:** Tạo một liên kết mềm (Symbolic Link) tên là `latest_data` trỏ tới thư mục `data/raw/2026/07/` ngay tại thư mục hiện hành (`ln -s`).
*   **Bài 1.10 [Khó]:** Cập nhật thời gian sửa đổi (Modified Time) của tệp `info.metadata` lùi về chính xác thời điểm ngày 15 tháng 06 năm 2026 lúc 12 giờ 00 phút (`touch -t`).

---

### CHỦ ĐỀ 2: Phân quyền File & Quản lý Sở hữu

*   **Bài 2.1 [Rất dễ]:** Xem quyền truy cập chi tiết (Read, Write, Execute) của tệp `info.metadata` (`ls -l`).
*   **Bài 2.2 [Rất dễ]:** Cấp quyền thực thi (Execute) cho tệp `script.sh` chỉ dành cho người sở hữu tệp (Owner) (`chmod u+x`).
*   **Bài 2.3 [Dễ]:** Thu hồi (xóa bỏ) quyền ghi (Write) của nhóm Khác (Others) đối với tệp cấu hình database quan trọng `database.config` (`chmod o-w`).
*   **Bài 2.4 [Dễ]:** Thiết lập phân quyền cho tệp `secure_key.pem` sao cho chỉ người sở hữu (Owner) có quyền đọc và viết, còn tất cả các nhóm khác hoàn toàn không có quyền gì bằng số hệ bát phân (`chmod 600`).
*   **Bài 2.5 [Trung bình - Dễ]:** Chuyển đổi chủ sở hữu của tệp `app.log` thành người dùng `admin` trên hệ thống (`chown admin`).
*   **Bài 2.6 [Trung bình]:** Chuyển đổi đồng thời chủ sở hữu thành `admin` và nhóm sở hữu thành `developers` cho thư mục `data/` (`chown admin:developers`).
*   **Bài 2.7 [Trung bình]:** Áp dụng quyền đọc/ghi đệ quy (`755`) cho toàn bộ thư mục con và tệp tin nằm bên trong thư mục `data/` (`chmod -R 755`).
*   **Bài 2.8 [Trung bình - Khó]:** Giải thích tại sao không bao giờ được cấp quyền `777` cho thư mục chứa code chạy hoặc tệp tin cấu hình nhạy cảm trong thực tế.
*   **Bài 2.9 [Khó]:** Thay đổi giá trị mặt nạ phân quyền hệ thống `umask` để từ thời điểm này, mọi tệp tin mới được tạo ra bởi user hiện tại mặc định sẽ chỉ có quyền `644`.
*   **Bài 2.10 [Khó]:** Khắc phục lỗi `Permission Denied` khi chạy một file python script kết nối ghi file vào thư mục `/var/log/my_app/` mà không cần dùng quyền `sudo`.

---

### CHỦ ĐỀ 3: Tìm kiếm, Bộ lọc & Xử lý Văn bản

*   **Bài 3.1 [Rất dễ]:** Xem 15 dòng đầu tiên của file log hệ thống `app.log` (`head -n 15`).
*   **Bài 3.2 [Rất dễ]:** Xem và cập nhật liên tục thời gian thực các dòng log mới nhất được ghi vào cuối file `app.log` (`tail -f`).
*   **Bài 3.3 [Dễ]:** Đếm tổng số dòng dữ liệu hiện có trong tệp dữ liệu CSV `customers.csv` mà không cần mở file (`wc -l`).
*   **Bài 3.4 [Dễ]:** Tìm kiếm tất cả các dòng có chứa từ khóa `[ERROR]` trong tệp log hệ thống `app.log` (`grep`).
*   **Bài 3.5 [Trung bình - Dễ]:** Tìm kiếm không phân biệt chữ hoa/chữ thường từ khóa `database` và in ra kèm số thứ tự dòng xuất hiện trong tệp log (`grep -in`).
*   **Bài 3.6 [Trung bình]:** Quét và tìm kiếm tất cả các tệp tin có định dạng đuôi `.parquet` nằm trong toàn bộ thư mục `/var/data/` (`find`).
*   **Bài 3.7 [Trung bình]:** Sử dụng đường ống lệnh (Pipe `|`) kết hợp `grep` và `wc -l` để đếm tổng số lỗi `[ERROR]` xuất hiện trong file log.
*   **Bài 3.8 [Trung bình - Khó]:** Sử dụng lệnh `cut` để trích xuất riêng cột địa chỉ IP (cột thứ nhất) từ các dòng log của Web Server.
*   **Bài 3.9 [Khó]:** Đọc tệp log, tìm các dòng lỗi trùng nhau, lọc bỏ các dòng trùng lặp và sắp xếp danh sách lỗi duy nhất theo thứ tự bảng chữ cái (`sort` kết hợp `uniq`).
*   **Bài 3.10 [Khó]:** Sử dụng lệnh nâng cao `awk` để tính tổng giá trị tiền nằm tại cột số 5 trong file CSV dữ liệu thô `transactions.csv` (ngăn cách bởi dấu `,`).

---

### CHỦ ĐỀ 4: Quản lý Tiến trình & Tài nguyên Hệ thống

*   **Bài 4.1 [Rất dễ]:** Hiển thị danh sách các tiến trình đang chạy thuộc về người dùng hiện tại (`ps`).
*   **Bài 4.2 [Rất dễ]:** Kiểm tra lượng bộ nhớ RAM còn trống, đã sử dụng của máy chủ dưới định dạng dễ đọc như Gigabytes/Megabytes (`free -h`).
*   **Bài 4.3 [Dễ]:** Tìm kiếm mã định danh tiến trình (PID) của chương trình Python đang chạy ngầm trên hệ thống bằng cách lọc kết quả của lệnh `ps aux` (`ps aux | grep python`).
*   **Bài 4.4 [Dễ]:** Sử dụng lệnh tương tác `top` hoặc `htop` để xác định tiến trình nào đang chiếm dụng nhiều tài nguyên CPU nhất hiện tại.
*   **Bài 4.5 [Trung bình - Dễ]:** Tắt (Kill) một tiến trình đang bị treo thông qua mã PID (`kill`).
*   **Bài 4.6 [Trung bình]:** Ép buộc tắt ngay lập tức (Force Kill) một tiến trình đang bị treo cứng không phản hồi (`kill -9`).
*   **Bài 4.7 [Trung bình]:** Xác định tiến trình nào đang mở và lắng nghe (listening) tại cổng mạng số `5432` của database PostgreSQL (`lsof -i :5432` hoặc `ss -lntp`).
*   **Bài 4.8 [Trung bình - Khó]:** Khởi chạy một chương trình Python `ingest.py` chạy ở chế độ nền (Background) và vẫn tiếp tục chạy bình thường ngay cả khi bạn tắt Terminal hoặc ngắt kết nối SSH (`nohup ... &`).
*   **Bài 4.9 [Khó]:** Kiểm tra danh sách các tác vụ đang chạy ngầm trong background và đưa một tác vụ cụ thể quay trở lại màn hình chính foreground (`jobs` kết hợp `fg`).
*   **Bài 4.10 [Khó]:** Thiết lập lịch tự động chạy script Python `daily_etl.py` vào lúc 12:00 giờ đêm mỗi ngày thông qua tiện ích lập lịch hệ thống `crontab`.

---

## 📥 Sản phẩm bàn giao & Báo cáo của Intern
1.  Intern tạo một file `basics_answers.md` lưu tại thư mục `00_linux_labs/lab_1/`.
2.  Với mỗi bài tập trong 40 bài trên, Intern ghi rõ:
    *   **Đề bài:** (Ghi lại yêu cầu).
    *   **Lệnh thực thi:** (Ghi lại câu lệnh Linux chính xác đã gõ trên Terminal).
    *   **Ảnh minh chứng kết quả:** (Chèn ảnh chụp màn hình Terminal chạy lệnh hiển thị kết quả thành công).
