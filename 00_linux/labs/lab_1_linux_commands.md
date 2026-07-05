# 📝 Lab 1: Danh sách 60 Bài tập Dòng lệnh Linux & Trình soạn thảo Vim từ Dễ đến Khó

Hệ điều hành Linux (đặc biệt là bản phân phối Ubuntu) là nền tảng cốt lõi để vận hành các hệ thống Big Data, cơ sở dữ liệu, Docker Containers và các máy chủ đám mây (AWS, GCP, Azure). 

Tài liệu này cung cấp lời giải thích **tại sao Data Engineer phải học Linux và Vim**, bảng tra cứu lệnh nhanh, và **60 bài tập thực hành chia làm 6 chủ đề** sắp xếp theo độ khó tăng dần từ Rất dễ đến Khó để Intern luyện tập hàng ngày trên Terminal.

---

## 🎯 Tại sao Data Engineer bắt buộc phải thành thạo Linux & Trình soạn thảo Vim?

1.  **Môi trường chạy Production là Linux:** 99% các công cụ dữ liệu như PostgreSQL, Apache Spark, Kafka, Airflow, Docker đều chạy tối ưu nhất trên hệ điều hành Linux.
2.  **Sử dụng Vim để chỉnh sửa file trực tiếp trên Server:** Khi bạn kết nối SSH vào một máy chủ từ xa (Remote Server) để cấu hình hệ thống hoặc sửa nhanh một dòng code, bạn sẽ **không có giao diện đồ họa (GUI) hay VS Code**. Vim là trình soạn thảo văn bản phổ biến nhất, nhẹ nhất và có sẵn trên mọi server Linux giúp bạn xử lý công việc ngay tại Terminal.
3.  **Làm việc với Containers (Docker/Kubernetes):** Việc vào trực tiếp container đang chạy để kiểm tra file cấu hình bắt buộc phải dùng các lệnh Linux cơ bản và các công cụ soạn thảo CLI như Vim/Vi.
4.  **Điều phối luồng công việc (Orchestration):** Các công cụ điều phối như Apache Airflow thường xuyên gọi các lệnh shell (`BashOperator`) để di chuyển file, gọi API, kích hoạt job Spark, hoặc kiểm tra kết nối mạng.
5.  **Xử lý File dữ liệu thô cực nhanh:** Thay vì viết code Python dài dòng để đếm số dòng của file CSV nặng 10GB, bạn chỉ cần gõ đúng 1 dòng lệnh Linux `wc -l file.csv` chạy trong 1 giây mà không tốn RAM.
6.  **Giám sát phần cứng và tài nguyên:** Kỹ năng kiểm tra xem RAM có bị tràn không (`free -h`), ổ đĩa có bị đầy không (`df -h`), hay CPU đang bị chiếm bởi tiến trình nào (`top`/`htop`).

---

## 📚 Bảng tra cứu nhanh các lệnh Linux & Vim cốt lõi

### 1. Phím tắt Vim quan trọng:
*   `i`: Chuyển sang **Insert Mode** (để bắt đầu gõ chữ).
*   `Esc`: Quay về **Normal Mode** (chế độ dòng lệnh để di chuyển/tìm kiếm/xóa).
*   `:wq` hoặc `:x`: Lưu thay đổi và Thoát khỏi Vim.
*   `:q!`: Thoát khỏi Vim và **không lưu** các thay đổi.
*   `dd`: Xóa (Cut) dòng hiện tại.
*   `yy`: Sao chép (Copy) dòng hiện tại.
*   `p`: Dán (Paste) nội dung xuống dòng bên dưới.
*   `u`: Hoàn tác (Undo) hành động trước đó.
*   `/từ_khóa`: Tìm kiếm từ khóa (ấn `n` để nhảy đến kết quả tiếp theo, `N` quay lại).

### 2. Lệnh Linux cơ bản:
*   **Thao tác File & Thư mục:** `pwd`, `ls`, `cd`, `mkdir`, `cp`, `mv`, `rm`, `touch`, `du`, `df`
*   **Phân quyền & Sở hữu:** `chmod`, `chown`, `chgrp`, `sudo`
*   **Xử lý & Tìm kiếm Văn bản:** `cat`, `head`, `tail`, `wc`, `grep`, `find`, `sort`, `uniq`, `cut`, `awk`, `sed`
*   **Tiến trình & Hệ thống:** `ps`, `top`, `htop`, `kill`, `free`, `lsof`, `ss`, `nohup`, `crontab`
*   **Mạng & Truyền dữ liệu:** `ping`, `curl`, `wget`, `ssh`, `scp`, `rsync`, `tar`, `gzip`, `unzip`

---

## 📂 60 BÀI TẬP THỰC HÀNH (6 CHỦ ĐỀ - 10 BÀI/CHỦ ĐỀ)

Intern mở Terminal trên Ubuntu (hoặc macOS/Git Bash) và thực hiện các yêu cầu dưới đây.

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
*   **Bài 1.9 [Trung bình - Khó]:** Tạo một liên kết mềm (Symbolic Link / Shortcut) tên là `latest_data` trỏ tới thư mục `data/raw/2026/07/` ngay tại thư mục hiện hành (`ln -s`).
*   **Bài 1.10 [Khó]:** Cập nhật thời gian sửa đổi (Modified Time) của tệp `info.metadata` lùi về chính xác thời điểm ngày 15 tháng 06 năm 2026 lúc 12 giờ 00 phút để giả lập file cũ phục vụ kiểm thử hệ thống (`touch -t`).

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
*   **Bài 2.9 [Khó]:** Thay đổi giá trị mặt nạ phân quyền hệ thống `umask` để từ thời điểm này, mọi tệp tin mới được tạo ra bởi user hiện tại mặc định sẽ chỉ có quyền `644` (Owner đọc/ghi, nhóm và người khác chỉ đọc).
*   **Bài 2.10 [Khó]:** Khắc phục lỗi `Permission Denied` khi chạy một file python script kết nối ghi file vào thư mục `/var/log/my_app/` mà không cần dùng quyền `sudo` (hướng dẫn: dùng `chown` đổi sở hữu thư mục đích về user hiện tại).

---

### CHỦ ĐỀ 3: Tìm kiếm, Bộ lọc & Xử lý Văn bản

*   **Bài 3.1 [Rất dễ]:** Xem 15 dòng đầu tiên của file log hệ thống `app.log` (`head -n 15`).
*   **Bài 3.2 [Rất dễ]:** Xem và cập nhật liên tục thời gian thực (real-time stream) các dòng log mới nhất được ghi vào cuối file `app.log` (`tail -f`).
*   **Bài 3.3 [Dễ]:** Đếm tổng số dòng dữ liệu hiện có trong tệp dữ liệu CSV `customers.csv` mà không cần mở file (`wc -l`).
*   **Bài 3.4 [Dễ]:** Tìm kiếm tất cả các dòng có chứa từ khóa `[ERROR]` trong tệp log hệ thống `app.log` (`grep`).
*   **Bài 3.5 [Trung bình - Dễ]:** Tìm kiếm không phân biệt chữ hoa/chữ thường từ khóa `database` và in ra kèm số thứ tự dòng xuất hiện trong tệp log (`grep -in`).
*   **Bài 3.6 [Trung bình]:** Quét và tìm kiếm tất cả các tệp tin có định dạng đuôi `.parquet` nằm trong toàn bộ thư mục `/var/data/` (`find`).
*   **Bài 3.7 [Trung bình]:** Sử dụng đường ống lệnh (Pipe `|`) kết hợp `grep` và `wc -l` để đếm tổng số lỗi `[ERROR]` xuất hiện trong file log.
*   **Bài 3.8 [Trung bình - Khó]:** Sử dụng lệnh `cut` để trích xuất riêng cột địa chỉ IP (cột thứ nhất) từ các dòng log của Web Server (các trường ngăn cách bởi khoảng trắng).
*   **Bài 3.9 [Khó]:** Đọc tệp log, tìm các dòng lỗi trùng nhau, lọc bỏ các dòng trùng lặp và sắp xếp danh sách lỗi duy nhất theo thứ tự bảng chữ cái (`sort` kết hợp `uniq`).
*   **Bài 3.10 [Khó]:** Sử dụng lệnh nâng cao `awk` để tính tổng giá trị tiền nằm tại cột số 5 trong file CSV dữ liệu thô `transactions.csv` (các cột ngăn cách bởi dấu phẩy `,`).

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

### CHỦ ĐỀ 5: Mạng, Tải file & Giao tiếp File từ xa

*   **Bài 5.1 [Rất dễ]:** Kiểm tra kết nối mạng (Network Connectivity) tới máy chủ Database thông qua địa chỉ IP của nó (`ping`).
*   **Bài 5.2 [Rất dễ]:** Sử dụng dòng lệnh để tải một file dữ liệu từ đường dẫn HTTP trên internet công khai về lưu ở máy cục bộ (`curl` hoặc `wget`).
*   **Bài 5.3 [Dễ]:** Sử dụng giao thức bảo mật SSH kết hợp tệp khóa riêng tư `private.key` để kết nối vào dòng lệnh của một máy chủ Ubuntu từ xa (`ssh -i`).
*   **Bài 5.4 [Dễ]:** Đóng gói và nén toàn bộ thư mục chứa các file log `logs/` thành một tệp tin lưu trữ nén duy nhất dạng `.tar.gz` để tiết kiệm không gian lưu trữ (`tar -czf`).
*   **Bài 5.5 [Trung bình - Dễ]:** Giải nén tệp tin zip dữ liệu `dataset.zip` tải từ internet vào một thư mục chỉ định `/tmp/data/` (`unzip -d`).
*   **Bài 5.6 [Trung bình]:** Sao chép một file dữ liệu từ máy tính cá nhân local lên thư mục `/home/ubuntu/data/` của máy chủ từ xa thông qua SSH (`scp`).
*   **Bài 5.7 [Trung bình]:** Sử dụng công cụ `rsync` để đồng bộ hóa dữ liệu thông minh giữa hai thư mục, đảm bảo chỉ sao chép các tệp tin mới được thay đổi để tối ưu hóa băng thông truyền tải mạng (`rsync -avz`).
*   **Bài 5.8 [Trung bình - Khó]:** Kiểm tra xem máy chủ từ xa có đang mở và kết nối được vào cổng `5432` của database hay không mà không cần đăng nhập vào máy chủ đó (`telnet` hoặc `nc -zv`).
*   **Bài 5.9 [Khó]:** Tra cứu địa chỉ IP của một tên miền dịch vụ API bằng lệnh `nslookup` hoặc `dig` (`nslookup api.open-meteo.com`).
*   **Bài 5.10 [Khó]:** Viết một câu lệnh duy nhất (sử dụng Pipes) để tải một file nén dữ liệu `.gz` từ internet về, giải nén trực tiếp trên RAM, lọc ra các dòng log chứa lỗi `[ERROR]` và ghi vào file kết quả cục bộ mà không cần ghi file nén trung gian ra đĩa cứng (kết hợp `curl`, `gunzip -c`, `grep` và `>`).

---

### CHỦ ĐỀ 6: Trình soạn thảo văn bản Vim

*Chủ đề này giúp Intern thành thạo kỹ năng thao tác và chỉnh sửa tệp tin trực tiếp trên máy chủ bằng phím tắt của Vim.*

*   **Bài 6.1 [Rất dễ]:** Sử dụng Vim để mở một tệp tin cấu hình mới tên là `database.ini` (`vim database.ini`).
*   **Bài 6.2 [Rất dễ]:** Chuyển sang chế độ soạn thảo (Insert Mode), nhập vào nội dung kết nối database (host, port, user), sau đó quay về chế độ lệnh (Normal Mode) bằng phím `Esc`.
*   **Bài 6.3 [Dễ]:** Lưu lại toàn bộ nội dung đã viết và thoát khỏi trình soạn thảo Vim để quay lại terminal (`:wq` hoặc `:x`).
*   **Bài 6.4 [Dễ]:** Mở lại tệp `database.ini` bằng Vim, sửa đổi nội dung lung tung, sau đó thực hiện thoát hoàn toàn ra ngoài Terminal mà **không lưu** bất kỳ sửa đổi mới nào (`:q!`).
*   **Bài 6.5 [Trung bình - Dễ]:** Viết một tệp cấu hình dài 50 dòng log giả lập. Thực hiện phím tắt để nhảy nhanh con trỏ xuống dòng cuối cùng của tệp (`G`), sau đó nhảy ngược về dòng đầu tiên của tệp (`gg`).
*   **Bài 6.6 [Trung bình]:** Ở chế độ Normal Mode, thực hiện xóa (Cut) nhanh đúng 3 dòng dữ liệu liên tục trong tệp mà không dùng nút Backspace xóa từng chữ (sử dụng lệnh `3dd` hoặc gõ `dd` 3 lần).
*   **Bài 6.7 [Trung bình]:** Thực hiện sao chép một dòng hiện tại (Copy line - `yy`) và dán dòng đó xuống bên dưới (Paste - `p`) 5 lần liên tục.
*   **Bài 6.8 [Trung bình - Khó]:** Tìm kiếm từ khóa `port` trong tệp cấu hình và nhảy qua các kết quả trùng khớp tiếp theo xuôi chiều và ngược chiều (sử dụng `/port` kết hợp phím `n` và `N`).
*   **Bài 6.9 [Khó]:** Sử dụng phím tắt để hoàn tác (Undo - `u`) một thao tác vừa xóa nhầm dòng, sau đó làm ngược lại khôi phục (Redo - `Ctrl + R`).
*   **Bài 6.10 [Khó]:** Thực hiện thay thế tự động (Find and Replace) tất cả các từ khóa `localhost` thành địa chỉ IP `192.168.1.15` trên toàn bộ tệp tin bằng một câu lệnh duy nhất ở chế độ Command-line (`:%s/localhost/192.168.1.15/g`).

---

## 📥 Sản phẩm bàn giao & Báo cáo của Intern

1.  Intern tạo một file `linux_answers.md` lưu tại thư mục `00_linux_labs/lab_1/`.
2.  Với mỗi bài tập trong 60 bài trên, Intern ghi rõ:
    *   **Đề bài:** (Ghi lại yêu cầu).
    *   **Lệnh/Phím tắt thực thi:** (Ghi lại câu lệnh Linux hoặc tổ hợp phím tắt Vim đã sử dụng).
    *   **Ảnh minh chứng kết quả:** (Chèn ảnh chụp màn hình Terminal chạy lệnh hiển thị kết quả thành công).
