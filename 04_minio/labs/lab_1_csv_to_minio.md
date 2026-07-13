# 📝 Lab 1: Đọc dữ liệu từ file CSV và lưu vào MinIO

Bài thực hành này giúp Intern biết cách cài đặt, cấu hình và tương tác với **MinIO Object Storage (S3-compatible)** bằng Python SDK thông qua việc đọc dữ liệu từ tệp CSV và lưu trữ vào Bucket theo cấu trúc lưu trữ chuẩn.

---

## 🎯 Kiến thức Intern cần nắm được (Target Knowledge)
Sau bài lab này, Intern phải giải thích được:
1.  **Object Storage:** Sự khác biệt so với File System và Database. Các thành phần chính: Bucket, Object, Key, Metadata.
2.  **S3-Compatible API:** Tại sao MinIO lại sử dụng API của AWS S3 và lợi ích của tính tương thích này.
3.  **Docker Compose với MinIO:** Hiểu sự khác nhau giữa cổng API (`9000`) và cổng Console (`9001`).
4.  **Python Boto3 / MinIO SDK:** Cách khởi tạo client, quản lý bucket và upload/download đối tượng.
5.  **Environment Variables & Security:** Cách quản lý Access Key và Secret Key bằng biến môi trường qua tệp `.env`.

---

## 📂 Đề bài chi tiết (Detailed Requirements)

Intern cấu trúc thư mục dự án `minio_ingestion/` như sau:
```text
minio_ingestion/
├── data/
│   └── customers.csv          # File dữ liệu CSV đầu vào
├── src/
│   ├── __init__.py
│   └── ingest.py              # Script Python thực hiện đọc CSV & upload lên MinIO
├── docker-compose.yml         # File Docker Compose khởi dựng MinIO Server
├── .env                       # Lưu thông tin cấu hình và credentials bảo mật
├── requirements.txt           # Danh sách thư viện (boto3, python-dotenv)
└── README.md                  # Hướng dẫn chạy dự án
```

### 1. Dựng MinIO Server bằng Docker Compose:
Viết file `docker-compose.yml` để chạy dịch vụ MinIO:
*   Sử dụng image: `minio/minio:latest`.
*   Ánh xạ các cổng: `9000:9000` (API) và `9001:9001` (Console UI).
*   Chỉ định lệnh chạy mặc định: `minio server /data --console-address ":9001"`.
*   Cấu hình tên tài khoản và mật khẩu quản trị (`MINIO_ROOT_USER`, `MINIO_ROOT_PASSWORD`) thông qua các biến môi trường đọc từ file `.env`.
*   Sử dụng Docker Named Volume để lưu trữ dữ liệu lâu dài cho MinIO (`minio_data:/data`).

### 2. Dữ liệu đầu vào (`data/customers.csv`):
Hãy tự chuẩn bị một file CSV nhỏ chứa danh sách khách hàng mẫu để thực hành:
```csv
customer_id,name,email,country
1,Nguyen Van A,anv@gmail.com,Vietnam
2,Tran Thi B,tranthib@gmail.com,Vietnam
3,John Doe,johndoe@example.com,USA
4,Alex Smith,alex@example.com,Singapore
```

### 3. Viết script Python `src/ingest.py`:
Sử dụng thư viện `boto3` (hoặc thư viện `minio` của Python) để thực hiện các công việc sau:
*   **Đọc cấu hình:** Load các thông số từ file `.env` bao gồm:
    *   `MINIO_ENDPOINT`: URL của MinIO API (ví dụ: `http://localhost:9000`).
    *   `MINIO_ACCESS_KEY`: Tài khoản truy cập.
    *   `MINIO_SECRET_KEY`: Mật khẩu truy cập.
    *   `BUCKET_NAME`: Tên bucket muốn tạo (ví dụ: `landing-zone`).
*   **Kết nối đến MinIO:** Khởi tạo S3 Client kết nối đến MinIO Endpoint.
*   **Quản lý Bucket:**
    *   Kiểm tra xem bucket có tên trong cấu hình (`BUCKET_NAME`) đã tồn tại chưa.
    *   Nếu chưa tồn tại, tự động tạo mới bucket đó.
*   **Upload Object:**
    *   Đọc tệp `data/customers.csv`.
    *   Upload tệp lên bucket với cấu trúc Key phân vùng theo thời gian thực tế chạy để mô phỏng Landing Zone thực tế. Ví dụ: `raw/customers/year=2026/month=07/customers.csv`.
    *   Đính kèm các **Metadata tùy chỉnh** (Custom Metadata) cho object khi upload, ví dụ:
        *   `source-format`: `csv`
        *   `ingestion-type`: `batch`
        *   `author`: `tên_intern`
*   **Xử lý ngoại lệ:** Viết code bao bọc trong khối `try-except` để bắt các lỗi kết nối, sai thông tin xác thực (Credentials) hoặc lỗi đường dẫn tệp tin và ghi log báo lỗi rõ ràng.

---

## 📥 Định nghĩa đầu ra & Sản phẩm bàn giao (Expected Outputs & Deliverables)

### 1. Mã nguồn hoàn chỉnh
Thư mục dự án `minio_ingestion/` có đầy đủ mã nguồn và tệp cấu hình theo cấu trúc yêu cầu.

### 2. File `.env.example`
Tệp ví dụ cấu hình các biến môi trường cần thiết (không chứa thông tin đăng nhập thật).

### 3. Ảnh chụp kết quả kiểm thử thành công
Intern chụp ảnh màn hình đính kèm vào báo cáo nộp bài:
1.  **MinIO Console UI (Cổng 9001):** Hiển thị Bucket `landing-zone` đã được tạo.
2.  **Object details:** Hiển thị tệp `customers.csv` nằm trong đường dẫn `raw/customers/year=2026/month=07/` kèm theo các trường **Metadata** tùy chỉnh đã thiết lập ở phần Python script.

---

## 📈 Tiêu chí đánh giá của Mentor (Validation Criteria)
1.  **No Hardcoded Credentials:** Tuyệt đối không lưu Access Key/Secret Key trong code Python. Mọi thông tin xác thực phải được lấy qua biến môi trường của file `.env`.
2.  **Idempotency (Tính khả lặp):** Script chạy nhiều lần mà không bị lỗi crash do bucket đã được tạo từ trước.
3.  **Exception Handling:** Khi tắt container MinIO và chạy script, chương trình phải in ra log thông báo lỗi kết nối rõ ràng và thoát an toàn, không văng lỗi Raw Python Traceback ra màn hình.
4.  **Metadata Tagging:** Object sau khi được tải lên MinIO phải hiển thị đầy đủ thông tin metadata tùy chỉnh đã được cấu hình trong code.
