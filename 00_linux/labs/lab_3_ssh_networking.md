# 📝 Lab 3: SSH, Mạng & Truyền tải tệp tin từ xa (10 Bài tập)

Tài liệu này bao gồm **10 bài tập thực hành về Mạng, SSH và truyền tải tệp tin** sắp xếp theo độ khó tăng dần từ Rất dễ đến Khó để Intern luyện tập kỹ năng tương tác với các máy chủ từ xa, tải dữ liệu từ internet và đóng gói nén tệp tin trong Data Pipeline.

---

## 🎯 Tại sao Data Engineer bắt buộc phải biết kết nối SSH và mạng?
Data pipeline thường xuyên phải:
1.  Tải dữ liệu từ internet thông qua API hoặc các nguồn lưu trữ đám mây (`curl`, `wget`).
2.  Kết nối điều khiển máy chủ Spark/Airflow từ xa qua giao thức bảo mật (`ssh`).
3.  Truyền tải các tệp tin CSV/Parquet thô vừa crawl được lên máy chủ lưu trữ (`scp`, `rsync`).
4.  Kiểm tra tường lửa (firewall) có đang mở và kết nối được vào cổng cơ sở dữ liệu hay không (`ping`, `telnet`, `nc`).

---

## 📂 10 BÀI TẬP THỰC HÀNH (Độ khó tăng dần):

*   **Bài 3.1 [Rất dễ]:** Kiểm tra kết nối mạng (Network Connectivity) tới máy chủ Database thông qua địa chỉ IP của nó (`ping`).
*   **Bài 3.2 [Rất dễ]:** Sử dụng dòng lệnh để tải một file dữ liệu từ đường dẫn HTTP trên internet công khai về lưu ở máy cục bộ (`curl` hoặc `wget`).
*   **Bài 3.3 [Dễ]:** Sử dụng giao thức bảo mật SSH kết hợp tệp khóa riêng tư `private.key` để kết nối vào dòng lệnh của một máy chủ Ubuntu từ xa (`ssh -i`).
*   **Bài 3.4 [Dễ]:** Đóng gói và nén toàn bộ thư mục chứa các file log `logs/` thành một tệp tin lưu trữ nén duy nhất dạng `.tar.gz` để tiết kiệm không gian lưu trữ (`tar -czf`).
*   **Bài 3.5 [Trung bình - Dễ]:** Giải nén tệp tin zip dữ liệu `dataset.zip` tải từ internet vào một thư mục chỉ định `/tmp/data/` (`unzip -d`).
*   **Bài 3.6 [Trung bình]:** Sao chép một file dữ liệu từ máy tính cá nhân local lên thư mục `/home/ubuntu/data/` của máy chủ từ xa thông qua SSH (`scp`).
*   **Bài 3.7 [Trung bình]:** Sử dụng công cụ `rsync` để đồng bộ hóa dữ liệu thông minh giữa hai thư mục, đảm bảo chỉ sao chép các tệp tin mới được thay đổi để tối ưu hóa băng thông truyền tải mạng (`rsync -avz`).
*   **Bài 3.8 [Trung bình - Khó]:** Kiểm tra xem máy chủ từ xa có đang mở và kết nối được vào cổng `5432` của database hay không mà không cần đăng nhập vào máy chủ đó (`telnet` hoặc `nc -zv`).
*   **Bài 3.9 [Khó]:** Tra cứu địa chỉ IP của một tên miền dịch vụ API bằng lệnh `nslookup` hoặc `dig` (`nslookup api.open-meteo.com`).
*   **Bài 3.10 [Khó]:** Viết một câu lệnh duy nhất (sử dụng Pipes) để tải một file nén dữ liệu `.gz` từ internet về, giải nén trực tiếp trên RAM, lọc ra các dòng log chứa lỗi `[ERROR]` và ghi vào file kết quả cục bộ mà không cần ghi file nén trung gian ra đĩa cứng (kết hợp `curl`, `gunzip -c`, `grep` và `>`).

---

## 📥 Sản phẩm bàn giao & Báo cáo của Intern
1.  Intern tạo một file `ssh_answers.md` lưu tại thư mục `00_linux_labs/lab_3/`.
2.  Với mỗi bài tập trên, Intern ghi rõ:
    *   **Đề bài:** (Ghi lại yêu cầu).
    *   **Lệnh thực thi:** (Ghi lại câu lệnh Linux chính xác đã gõ trên Terminal).
    *   **Ảnh minh chứng kết quả:** (Chèn ảnh chụp màn hình Terminal chạy lệnh hiển thị kết quả thành công).
