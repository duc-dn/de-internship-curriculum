# 📝 Lab 1: Docker & PostgreSQL Containerization

Đây là bài thực hành khởi động (Warm-up Lab) giúp Intern làm quen với Docker CLI, hiểu về Port Mapping, Mount Volume và vòng đời của Container trước khi làm việc với Docker-Compose.

---

## 🎯 Kiến thức Intern cần nắm được (Target Knowledge)
Sau bài lab này, Intern phải giải thích được:
1.  **Container vs Image:** Sự khác nhau giữa một Image tĩnh đóng vai trò như template và một Container động là thực thể đang chạy.
2.  **Port Mapping:** Cơ chế ánh xạ cổng mạng (`-p host_port:container_port`) để các dịch vụ ngoài có thể truy cập cổng mạng nội bộ của Container.
3.  **Data Persistence (Tính bền vững dữ liệu):** Tại sao khi xóa container thì dữ liệu bị mất, và cơ chế giải quyết bằng **Docker Volumes**.
4.  **Docker Volumes vs Bind Mounts:** Phân biệt cơ chế Docker tự quản lý volume (Named Volume) với việc map thư mục local trực tiếp (Bind Mount).

---

## 📂 Đề bài chi tiết (Detailed Requirements)

### 1. Thí nghiệm mất dữ liệu (Bước 1 & 2)
Intern chạy lệnh tải image và khởi động một PostgreSQL container tạm thời:
```bash
docker run -d --name pg_temp -p 5439:5432 -e POSTGRES_PASSWORD=mysecretpassword postgres:15-alpine
```
*   Dùng công cụ DBeaver hoặc pgAdmin kết nối vào database này thông qua Host: `localhost`, Port: `5439`, User: `postgres`, Password: `mysecretpassword`.
*   Tạo cơ sở dữ liệu `learning_db`. Tạo bảng `users` chứa 2 cột `id` và `name`.
*   Insert 2 dòng dữ liệu vào bảng.
*   Xóa container bằng lệnh: `docker rm -f pg_temp`.
*   Chạy lại đúng câu lệnh khởi tạo ban đầu, kết nối lại database và kiểm tra xem bảng `users` còn tồn tại không. Giải thích hiện tượng.

### 2. Thiết lập dữ liệu bền vững (Bước 3 & 4)
*   Tạo một Docker Named Volume: `docker volume create pg_data_local`.
*   Khởi chạy container PostgreSQL mới kết hợp mount volume này vào thư mục lưu trữ dữ liệu của Postgres:
    ```bash
    docker run -d --name pg_persistent -p 5439:5432 -e POSTGRES_PASSWORD=mysecretpassword -v pg_data_local:/var/lib/postgresql/data postgres:15-alpine
    ```
*   Kết nối lại qua DBeaver, tạo lại bảng `users` và insert dữ liệu.
*   Xóa container `pg_persistent` đi bằng lệnh `docker rm -f`.
*   Chạy lại container bằng đúng câu lệnh ở Bước 3. Truy cập DBeaver kiểm tra để xác nhận dữ liệu đã được bảo toàn thành công.

---

## 📥 Định nghĩa đầu ra & Sản phẩm bàn giao (Expected Outputs & Deliverables)

### 1. Báo cáo thực hành `report.txt`
Intern viết báo cáo ghi lại:
*   Các câu lệnh Docker CLI đã sử dụng.
*   Lời giải thích tại sao ở thí nghiệm 1 dữ liệu lại bị mất khi xóa container, và tại sao mount volume ở thí nghiệm 2 lại giúp bảo toàn dữ liệu.

---

## 📈 Tiêu chí đánh giá của Mentor (Validation Criteria)
1.  **CLI Command Accuracy:** Lệnh chạy container phải chỉ định chính xác biến môi trường `POSTGRES_PASSWORD` và volume mount path `/var/lib/postgresql/data`.
2.  **DBeaver Connection:** Intern thực hiện kết nối thành công từ client bên ngoài vào database chạy trong container.
3.  **Conceptual Understanding:** Giải thích đúng bản chất ghi dữ liệu tạm thời của container writable layer.
