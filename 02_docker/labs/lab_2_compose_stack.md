# 📝 Lab 2: Containerize Python App & PostgreSQL Stack

Bài thực hành này giúp Intern biết cách đóng gói ứng dụng Python hoàn chỉnh vào một Docker Image và sử dụng Docker Compose để tự động liên kết ứng dụng đó với PostgreSQL database chạy song song.

---

## 🎯 Kiến thức Intern cần nắm được (Target Knowledge)
Sau bài lab này, Intern phải giải thích được:
1.  **Dockerfile Syntax:** Vai trò của các chỉ thị `FROM`, `WORKDIR`, `COPY`, `RUN`, `CMD` và `ENTRYPOINT`.
2.  **Layer Caching (Tối ưu hóa build image):** Cách tổ chức file để Docker tận dụng tối đa cache layer cũ, giảm thời gian build image.
3.  **Docker Compose:** Tại sao cần compose để quản lý multi-container và cách cấu hình mạng nội bộ để các container gọi được nhau bằng tên service.
4.  **Startup Order & Healthcheck:** Tại sao thuộc tính `depends_on` đơn thuần không đủ để chống lỗi kết nối database, và cách dùng `healthcheck` kiểm tra trạng thái DB sẵn sàng.
5.  **Environment Configuration:** Cách truyền biến môi trường từ file `.env` ngoài vào trong container để bảo mật thông tin.

---

## 📂 Đề bài chi tiết (Detailed Requirements)

### 1. Cấu trúc thư mục tích hợp:
Intern cấu trúc thư mục dự án `weather_pipeline/` như sau:
```text
weather_pipeline/
├── weather_client/         # Thư mục mã nguồn từ Lab 1 Python
│   ├── src/
│   │   ├── client.py       # Cập nhật code để đọc cấu hình DB từ env
│   │   ├── exceptions.py
│   │   └── models.py
│   ├── tests/
│   ├── Dockerfile          # [NEW] File định nghĩa build image cho Python App
│   └── requirements.txt    # Thư viện sử dụng (requests, psycopg2-binary, pytest)
├── init_db/
│   └── init.sql            # [NEW] File DDL SQL khởi tạo bảng cơ sở dữ liệu
├── docker-compose.yml      # [NEW] File Docker Compose cấu hình toàn bộ stack
├── .env                    # [NEW] Lưu thông tin bảo mật môi trường
└── README.md
```

### 2. Yêu cầu chi tiết các phần:
*   **Dockerfile của Weather Client:**
    *   Sử dụng base image `python:3.9-slim`.
    *   Đảm bảo tối ưu hóa Layer Cache: copy `requirements.txt` và chạy `pip install` trước khi copy mã nguồn.
    *   Chỉ định lệnh chạy mặc định: `CMD ["python", "src/client.py"]` (hoặc script thực thi run ingest).
*   **File `init_db/init.sql`:**
    *   Viết mã DDL tạo bảng `weather_history` (chứa các cột tương ứng với đối tượng `WeatherReport` thu được từ API thời tiết).
*   **File `docker-compose.yml`:**
    *   **Service `db`:**
        *   Sử dụng image `postgres:15-alpine`.
        *   Khai báo biến môi trường `POSTGRES_USER`, `POSTGRES_PASSWORD`, `POSTGRES_DB` truyền từ file `.env`.
        *   Volume: Mount named volume lưu trữ database và mount file `init.sql` vào thư mục `/docker-entrypoint-initdb.d/` trong container để tự chạy script tạo bảng.
        *   Cấu hình `healthcheck`: Kiểm tra Postgres thực sự chạy bằng lệnh `pg_isready`.
    *   **Service `ingest_job`:**
        *   Build từ thư mục `./weather_client/`.
        *   Khai báo biến kết nối DB trỏ host về tên service `db` (`DB_HOST=db`).
        *   Cấu hình `depends_on`:
            ```yaml
            depends_on:
              db:
                condition: service_healthy
            ```
*   **Cập nhật code Python Ingest:**
    *   Sử dụng `os.environ.get()` để lấy thông số kết nối DB. Tuyệt đối không hard-code host/password.
    *   Sau khi API lấy dữ liệu thành công, dùng module DB connector ghi dữ liệu vào bảng `weather_history` trong PostgreSQL.

---

## 📥 Định nghĩa đầu ra & Sản phẩm bàn giao (Expected Outputs & Deliverables)

### 1. Mã nguồn tích hợp đầy đủ
Thư mục dự án có đầy đủ mã nguồn và cấu hình theo mẫu cấu trúc.

### 2. Log chạy Docker Compose thành công
Khi gõ lệnh `docker-compose up --build`, log terminal phải thể hiện đúng thứ tự:
1.  Container `db` khởi chạy và chạy script `init.sql`.
2.  Container `db` báo trạng thái `healthy`.
3.  Container `ingest_job` khởi chạy, gọi API thời tiết, kết nối thành công tới Database `db` và ghi dữ liệu, sau đó tắt (exit code 0) an toàn.

---

## 📈 Tiêu chí đánh giá của Mentor (Validation Criteria)
1.  **Dockerfile Layer Cache:** Mentor kiểm tra nếu Intern thay đổi một dòng code trong thư mục `src/` và build lại, Docker không được cài đặt lại các thư viện python từ đầu mà phải lấy từ cache.
2.  **No Hardcoded Secrets:** Tuyệt đối không lưu password hay IP database trong code Python. Mọi cấu hình phải lấy qua biến môi trường.
3.  **Startup Sequence:** Khi chạy hệ thống lần đầu từ số 0, container Python không được văng lỗi "Connection refused" do Postgres chưa khởi động kịp.
