# 📋 Checklist Kiến thức cần nắm được - Docker & Containerization

Dưới đây là danh sách các câu hỏi tự đánh giá giúp Intern kiểm tra mức độ hiểu biết của mình sau khi hoàn thành Giai đoạn 2 (Docker). Intern cần tự tin giải thích được các câu hỏi này và demo trực tiếp với Mentor trong buổi Code Review.

---

## 🐳 1. Khái niệm cốt lõi về Docker
- [ ] Phân biệt sự khác nhau giữa Docker Image (tĩnh) và Docker Container (động).
- [ ] Docker Hub đóng vai trò gì trong vòng đời phát triển ứng dụng sử dụng Docker?
- [ ] Kể tên 3 lệnh Docker CLI cơ bản dùng để quản lý vòng đời của một Container.

---

## 🔌 2. Docker Networking & Port Mapping
- [ ] Giải thích tham số `-p 5439:5432` khi khởi chạy container PostgreSQL. Cổng nào ở máy host (local machine) và cổng nào ở trong container?
- [ ] Tại sao trong custom network của Docker, các container lại có thể tự gọi cho nhau bằng tên service/tên container thay vì dùng địa chỉ IP tĩnh?
- [ ] Driver mặc định cho network của Docker trên máy đơn (local master) là gì?

---

## 💾 3. Docker Volume & Persistence
- [ ] Tại sao khi ta xóa một container (không dùng volumes) thì dữ liệu ghi trong container đó lại bị mất? Ghi dữ liệu vào container layer ảnh hưởng gì đến hiệu năng?
- [ ] Phân biệt sự khác nhau giữa **Named Volume** (Docker quản lý) và **Bind Mount** (Ánh xạ trực tiếp thư mục local). Khi nào nên dùng loại nào?
- [ ] Các tệp tin cấu hình môi trường nhạy cảm (như mật khẩu Database) nên được truyền vào container bằng cách nào để đảm bảo tính an toàn bảo mật?

---

## 🛠️ 4. Dockerfile & Tối ưu hóa Image
- [ ] Giải thích vai trò của các chỉ thị trong Dockerfile: `FROM`, `WORKDIR`, `COPY`, `RUN`, `CMD`, `ENTRYPOINT`.
- [ ] Phân biệt sự khác nhau giữa `CMD` và `ENTRYPOINT`.
- [ ] Cơ chế Layer Caching của Docker hoạt động như thế nào?
- [ ] Làm thế nào để viết Dockerfile cho một ứng dụng Python tối ưu nhất (Gợi ý: Tách biệt COPY `requirements.txt` và COPY source code `src/`).

---

## 🐙 5. Docker Compose & Multi-Container
- [ ] Vai trò của file `docker-compose.yml` là gì? Tại sao nên dùng Compose thay vì chạy các lệnh `docker run` thủ công?
- [ ] Thuộc tính `depends_on` trong Docker Compose có đảm bảo chắc chắn container ứng dụng sẽ kết nối thành công tới Database ngay khi khởi chạy không? Tại sao?
- [ ] Giải thích cách dùng `healthcheck` (ví dụ `pg_isready` đối với Postgres) kết hợp với `depends_on (condition: service_healthy)` để giải quyết triệt để lỗi kết nối database khi khởi động hệ thống.
