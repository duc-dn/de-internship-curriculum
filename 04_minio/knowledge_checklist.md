# 📋 Checklist Kiến thức cần nắm được - Object Storage & MinIO

Dưới đây là danh sách các câu hỏi tự đánh giá giúp Intern kiểm tra mức độ hiểu biết của mình sau khi hoàn thành Giai đoạn 4 (MinIO & Object Storage). Intern cần tự tin giải thích được các câu hỏi này và demo trực tiếp với Mentor trong buổi Code Review.

---

## 📦 1. Tổng quan về Object Storage & MinIO
- [ ] So sánh sự khác nhau cơ bản về cách tổ chức dữ liệu giữa Object Storage (flat namespace) và File Storage (hierarchical folder structure).
- [ ] Tại sao Object Storage là lựa chọn hàng đầu làm Data Lake để lưu trữ dữ liệu thô (Raw Data) trong các hệ thống Big Data?
- [ ] S3-Compatible API là gì? Tại sao việc MinIO tương thích với S3 API lại là một lợi thế lớn?

---

## 🐍 2. Lập trình MinIO với Python SDK
- [ ] Trình bày các tham số bắt buộc phải truyền vào khi khởi tạo một S3 Client sử dụng thư viện `boto3` để kết nối đến MinIO Server local.
- [ ] Làm thế nào để kiểm tra một Bucket có tồn tại hay không, và nếu chưa có thì tự động tạo mới thông qua code Python?
- [ ] Khi upload một đối tượng lên Object Storage, Object Key là gì? Tại sao ta nên thiết lập Object Key theo dạng đường dẫn chứa thời gian (ví dụ: `raw/customers/year=2026/month=07/data.csv`)?
- [ ] Custom Metadata của Object dùng để làm gì? Nêu một số ví dụ về việc đính kèm custom metadata có ích cho quá trình quản lý dữ liệu (Data Governance).
- [ ] Làm thế nào để bắt các lỗi phổ biến (như sai tài khoản đăng nhập hoặc server MinIO chưa khởi động) trong code Python để chương trình không bị crash?

---

## 🐚 3. Công cụ dòng lệnh MinIO Client (`mc`) & Quản trị
- [ ] Làm thế nào để cấu hình kết nối từ máy local tới MinIO Server bằng lệnh `mc alias set`?
- [ ] Kể tên các lệnh tương ứng trên `mc` CLI cho các thao tác: Liệt kê đối tượng, tạo bucket, copy tệp tin và đồng bộ thư mục.
- [ ] Presigned URL là gì? Khi nào ta cần tạo Presigned URL cho một Object? Lệnh nào trên `mc` thực hiện việc này?
- [ ] Phân biệt các chế độ truy cập (Bucket Policies) mặc định: `private`, `public`, `download`.
