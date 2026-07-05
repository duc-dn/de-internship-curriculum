# 📖 Hướng dẫn Nộp bài & Tạo Pull Request (PR) dành cho Intern

Tài liệu này hướng dẫn chi tiết cách Intern tổ chức mã nguồn, chạy kiểm thử, chụp ảnh minh chứng và viết mô tả Pull Request (PR) khi nộp bài thực hành hàng tuần. Việc tuân thủ quy trình này giúp Intern làm quen với quy trình làm việc chuẩn (Standard Software Development Lifecycle) tại các dự án thực tế.

---

## 📂 1. Quy tắc Tạo thư mục & Đóng gói Code

Tất cả các bài Lab phải được làm trong một Git Repository duy nhất (Mono-repo) tên là `de-internship-2026`. Intern tổ chức các thư mục nộp bài tương ứng như sau:

```text
de-internship-2026/             # Thư mục gốc của Git Repository
│
├── 01_python_labs/             # Các bài tập giai đoạn Python
│   ├── lab_1_basics/           # Bài tập cơ bản (basics_practice.py)
│   ├── lab_2_json_cleansing/   # Bài tập lọc JSON (main.py, raw_customers.json)
│   ├── lab_3_weather_client/   # Dự án OOP Weather API Client (src/, tests/)
│   ├── lab_4_pandas_sales/     # Phân tích doanh thu Pandas (sales_analysis.py)
│   └── lab_5_parallel/         # Xử lý song song (main.py, data/)
│
├── 02_docker_labs/             # Các bài tập giai đoạn Docker
│   ├── lab_1_postgres/         # Chạy Postgres CLI & Named Volumes
│   └── lab_2_compose_stack/    # Stack Docker Compose hoàn chỉnh
│
├── 03_de_labs/                 # Các bài tập Data Engineering nâng cao
│   ├── lab_1_dbt/              # Mô hình dbt transformations
│   ├── lab_2_spark/            # Xử lý dữ liệu lớn PySpark
│   └── lab_3_capstone/         # Dự án Capstone tích hợp Airflow
│
├── .gitignore                  # Bắt buộc loại bỏ .venv, logs, database volumes
└── README.md                   # Hướng dẫn tổng quan cách cài đặt & chạy
```

> [!IMPORTANT]
> **Quy tắc về File rác:** Intern tuyệt đối không được push các file môi trường ảo (`.venv/`, `env/`), các file cache python (`__pycache__/`, `.pytest_cache/`), các file log cá nhân, hoặc thư mục chứa dữ liệu PostgreSQL (`pg_data/`) lên Git. Mọi file này phải được khai báo trong `.gitignore`.

---

## 💻 2. Quy trình Chạy kiểm thử & Chụp ảnh minh chứng

Khi hoàn thành mỗi bài Lab, Intern phải chuẩn bị minh chứng chạy thành công để đính kèm vào PR:

### Bước 2.1: Chụp ảnh chạy Console / Terminal
*   Mở Terminal lên, chạy file script (Ví dụ: `python basics_practice.py`).
*   Chụp ảnh toàn bộ cửa sổ Terminal hiển thị:
    *   Lệnh chạy chương trình.
    *   Kết quả in ra màn hình khớp hoàn toàn với định nghĩa kết quả của bài Lab.
*   *Lưu ý:* Font chữ Terminal rõ ràng, không bị che khuất kết quả.

### Bước 2.2: Chụp ảnh File Logs hoặc Kết quả Database (nếu có)
*   Đối với các bài Lab yêu cầu ghi Log ra file (như `cleansing.log`, `run.log`): Chụp ảnh nội dung file log được sinh ra.
*   Đối với các bài Lab làm việc với Database (như Lab Docker, dbt): Chụp giao diện DBeaver hoặc pgAdmin thể hiện các bảng dữ liệu đã được tạo và các dòng dữ liệu được truy vấn thành công.

### Bước 2.3: Lưu trữ ảnh minh chứng trong Git
*   Intern tạo thư mục `docs/screenshots/` ở thư mục gốc của Git Repo.
*   Lưu các ảnh chụp màn hình vào đây với tên gợi nhớ (Ví dụ: `lab_1_console_output.png`, `lab_2_dbeaver_query.png`).
*   Cách này giúp Intern dễ dàng nhúng link ảnh trực tiếp vào phần mô tả PR trên GitHub/GitLab.

---

## 🌿 3. Quy trình Git & Conventional Commits

Intern thực hiện nộp bài qua Git tuần tự theo các bước:

### Bước 3.1: Tạo nhánh mới (Branch)
Nhánh nộp bài phải được tạo từ nhánh `main` và đặt tên theo chuẩn: `feature/lab-{số}-{tên-bài}`.
```bash
git checkout main
git pull origin main
git checkout -b feature/lab-1-basics
```

### Bước 3.2: Viết Commit Message chuẩn
Intern phải viết commit message theo chuẩn **Conventional Commits** để dễ theo dõi lịch sử:
*   `feat: ...` (khi thêm code chức năng mới).
*   `fix: ...` (khi sửa lỗi logic).
*   `test: ...` (khi thêm unit tests).
*   `docs: ...` (khi viết tài liệu README, thêm ảnh chụp màn hình).

*Ví dụ:*
```bash
git add 01_python_labs/lab_1_basics/
git commit -m "feat: complete exercises 1 to 5 for python basics"
git commit -m "docs: add console output screenshots for lab 1"
git push origin feature/lab-1-basics
```

---

## 📝 4. Mẫu mô tả Pull Request (PR Template)

Khi tạo PR trên GitHub/GitLab, Intern phải sao chép mẫu dưới đây điền đầy đủ thông tin:

```markdown
## 📝 Mô tả bài thực hành nộp: [Tên Bài Lab - Ví dụ: Lab 1 Python Basics]

### 👤 Thông tin Intern
*   Họ và tên: [Điền họ tên Intern]
*   Tuần thực tập: Tuần [Ví dụ: Tuần 1]

---

### 📦 Các phần đã hoàn thành
- [x] Bài tập 1: Xử lý danh sách sản phẩm.
- [x] Bài tập 2: Hàm tính toán thống kê (đã xử lý lỗi chia cho 0).
- [x] Bài tập 3: Phép toán Set đối chiếu dữ liệu.
- [ ] ... [Đánh dấu x vào phần đã hoàn thành, bỏ trống phần chưa làm]

---

### 💻 Minh chứng chạy thành công (Screenshots & Logs)

#### 1. Ảnh chạy Terminal / Console:
![Console Output](../../docs/screenshots/lab_1_console_output.png)
*(Nhúng ảnh chụp màn hình terminal hiển thị kết quả chạy tại đây)*

#### 2. Ảnh truy vấn dữ liệu / File Log sinh ra (nếu có):
```text
[Dán nội dung log hoặc ảnh chụp màn hình DBeaver tại đây]
```

---

### 🧠 Kiến thức đã làm chủ được sau bài Lab này
1.  [Ví dụ: Hiểu tại sao Set lại kiểm tra phần tử tồn tại mất O(1) thay vì O(N) của List].
2.  [Ví dụ: Biết cách parse chuỗi thời gian bằng strptime thay vì so sánh string].

### ❓ Câu hỏi hoặc Khó khăn gặp phải (nếu có)
*   [Ghi nhận các blocker hoặc thắc mắc cần Mentor giải đáp].
```

---

## 📈 5. Quy trình Đánh giá của Mentor

1.  **Review trực tiếp trên PR:** Mentor sẽ kiểm tra code và để lại comment nhận xét trực tiếp tại dòng code chưa tối ưu.
2.  **Chỉnh sửa (Refactor):** Intern đọc comment, sửa trực tiếp trên máy local tại nhánh đó, sau đó commit và push tiếp lên. PR sẽ tự động cập nhật.
3.  **Merge:** Khi code đạt chất lượng, Mentor sẽ bấm **Approve** và thực hiện **Merge Pull Request** vào nhánh `main`. Bài Lab chính thức được xác nhận **Pass**.
