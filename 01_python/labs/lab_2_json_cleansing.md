# 📝 Lab 2: Python Scripting & Data Cleansing

Đây là bài thực hành khởi động (Warm-up Lab) giúp Intern làm quen với lập trình Python thực tế trước khi học các kiến thức nâng cao hơn.

---

## 🎯 Kiến thức Intern cần nắm được (Target Knowledge)
Sau bài lab này, Intern phải giải thích được:
1.  **Môi trường ảo (Virtual Environment):** Tại sao cần dùng `venv`, cách kích hoạt và quản lý thư viện bằng `requirements.txt`.
2.  **Đọc/Ghi File:** Cách làm việc với tệp tin JSON và CSV bằng Python chuẩn.
3.  **Xử lý Ngoại lệ (Exception Handling):** Cách dùng try-except bắt các lỗi phổ biến (`FileNotFoundError`, `JSONDecodeError`) mà không làm chương trình bị crash đột ngột.
4.  **Kiểm tra và chuẩn hóa dữ liệu (Data Cleansing):** Tư duy loại bỏ dữ liệu rác, xử lý giá trị khuyết (null/None) và định dạng chuẩn (email, số điện thoại).

---

## 📂 Đề bài chi tiết (Detailed Requirements)

### 1. Dữ liệu đầu vào (Input Data)
Mentor hãy tạo sẵn file `raw_customers.json` ở thư mục chạy chương trình với nội dung:
```json
[
  {"id": 1, "name": "Nguyen Van A", "email": "anv@gmail.com", "phone": "0912345678", "age": 25},
  {"id": 2, "name": "Tran Thi B", "email": "invalid_email_format", "phone": "0987654321", "age": null},
  {"id": 3, "name": "Le Van C", "email": "c.le@outlook.com", "phone": "O901234567", "age": 17},
  {"id": 4, "name": "Pham Van D", "email": "dpham@yahoo.com", "phone": "092-345-6789", "age": 30},
  {"id": 5, "name": "Hoang Thi E", "email": null, "phone": "0934567890", "age": 45}
]
```

### 2. Logic cần thực thi
Viết script `main.py` thực hiện:
*   Đọc file `raw_customers.json`.
*   Lọc bỏ các bản ghi không hợp lệ:
    *   `email` bị null/None hoặc không chứa ký tự `@` và `.`.
    *   `id` bị trùng hoặc không tồn tại (nếu có).
*   Chuẩn hóa dữ liệu:
    *   Chuyển toàn bộ email về chữ thường (lowercase).
    *   SĐT phải được dọn dẹp chỉ giữ lại chữ số (ví dụ: chữ `O` viết nhầm ở ID 3 phải đổi thành số `0`, các dấu gạch ngang `-` ở ID 4 phải loại bỏ).
    *   Trường `age` bị null thì điền giá trị mặc định là `0`.
*   Xuất dữ liệu sạch ra tệp tin CSV.

---

## 📥 Định nghĩa đầu ra & Sản phẩm bàn giao (Expected Outputs & Deliverables)

### 1. File dữ liệu sạch `cleaned_customers.csv`
Nội dung file mong muốn:
```csv
customer_id,full_name,email,phone_number,age
1,Nguyen Van A,anv@gmail.com,0912345678,25
3,Le Van C,c.le@outlook.com,0901234567,17
4,Pham Van D,dpham@yahoo.com,0923456789,30
```
*(ID 2 và 5 bị loại bỏ do email sai định dạng hoặc bị null)*

### 2. File nhật ký log `cleansing.log`
Chứa các dòng log do thư viện `logging` sinh ra:
```text
2026-07-05 10:00:00 [INFO] main.py: Loaded 5 raw customer records.
2026-07-05 10:00:01 [WARNING] main.py: Customer ID 2 removed due to invalid email format: invalid_email_format
2026-07-05 10:00:01 [WARNING] main.py: Customer ID 5 removed due to missing email.
2026-07-05 10:00:01 [INFO] main.py: Successfully wrote 3 cleaned records to cleaned_customers.csv.
```

---

## 📈 Tiêu chí đánh giá của Mentor (Validation Criteria)
1.  **Isolation:** Intern có sử dụng môi trường ảo `.venv` độc lập không? Thư viện được quản lý trong file `requirements.txt` không?
2.  **Safety:** Khi xóa file `raw_customers.json`, chạy script có bị văng lỗi crash màn hình hay chương trình in ra log báo lỗi `FileNotFoundError` và thoát an toàn?
3.  **Logs:** Không dùng bất kỳ lệnh `print` nào để log. Bắt buộc dùng `logging` với format chuẩn có kèm timestamp và level log.
