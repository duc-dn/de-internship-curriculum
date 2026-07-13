# 📋 Checklist Kiến thức cần nắm được - Python Lập trình nâng cao

Dưới đây là danh sách các câu hỏi tự đánh giá giúp Intern kiểm tra mức độ hiểu biết của mình sau khi hoàn thành Giai đoạn 1 (Python). Intern cần tự tin giải thích được các câu hỏi này và demo giải pháp với Mentor trong buổi Code Review.

---

## 🏗️ 1. Lập trình Hướng đối tượng (OOP) & PEP 8
- [ ] Phân biệt sự khác nhau giữa Class Attribute (thuộc tính của lớp) và Instance Attribute (thuộc tính của đối tượng).
- [ ] Lợi ích của kế thừa (Inheritance) và đa hình (Polymorphism) là gì? Lấy ví dụ thực tế liên quan đến Data Ingestion.
- [ ] `@property` decorator dùng để làm gì? Cho ví dụ về cách bảo vệ/validate một thuộc tính private.
- [ ] Type Hinting trong Python có giúp kiểm soát kiểu dữ liệu ở thời điểm chạy (Runtime) hay không? Tại sao vẫn nên viết Type Hinting?
- [ ] Kể tên ít nhất 3 quy chuẩn đặt tên biến, hàm, lớp và hằng số theo chuẩn PEP 8.

---

## 🐛 2. Exception Handling & Logging
- [ ] Tại sao ta nên tránh việc viết block `except Exception: pass`? Tác hại của nó là gì?
- [ ] Custom Exception dùng để làm gì? Khi nào ta nên tạo một Custom Exception thay vì dùng các Exception có sẵn của Python?
- [ ] Tại sao trong dự án thực tế bắt buộc phải dùng thư viện `logging` thay thế hoàn toàn cho lệnh `print`?
- [ ] Giải thích ý nghĩa của các mức độ log (Log Levels): `DEBUG`, `INFO`, `WARNING`, `ERROR`, `CRITICAL`.
- [ ] Làm thế nào để log vừa được in ra màn hình Console, vừa được lưu vào tệp tin `.log`? (Giải thích khái niệm Handlers).

---

## ⚡ 3. Generators & Decorators (Nâng cao)
- [ ] Cơ chế hoạt động của từ khóa `yield` khác gì so với `return`?
- [ ] Tại sao sử dụng Generator lại giúp tối ưu hóa bộ nhớ RAM khi đọc một tệp dữ liệu cực kỳ lớn (ví dụ file CSV hàng chục triệu dòng)?
- [ ] Bản chất của một Decorator trong Python là gì? Hãy giải thích cách viết một decorator nhận tham số và thay đổi hành vi của một hàm.
- [ ] Context Manager (câu lệnh `with`) giúp giải quyết vấn đề gì? Để một class hoạt động được với `with`, nó cần định nghĩa 2 phương thức đặc biệt (dunder methods) nào?

---

## 📊 4. Xử lý Dữ liệu với Pandas & NumPy
- [ ] Vectorization trong NumPy/Pandas là gì? Tại sao tính toán vector hóa lại nhanh hơn hàng chục lần so với dùng vòng lặp `for` thông thường trên Python?
- [ ] Phân biệt sự khác nhau giữa `loc` và `iloc` trong Pandas.
- [ ] Phân biệt các phương thức liên kết dữ liệu trong Pandas: `merge()`, `join()`, và `concat()`.
- [ ] Tại sao nên tránh sử dụng các hàm lặp như `iterrows()` khi thao tác với DataFrames? Thay thế bằng cách nào?

---

## 🧵 5. Xử lý Song song (Concurrency & Parallelism)
- [ ] Giải thích khái niệm GIL (Global Interpreter Lock) trong Python. Nó ảnh hưởng thế nào đến hiệu năng đa luồng?
- [ ] Khi nào nên sử dụng `ThreadPoolExecutor` (Đa luồng) và khi nào nên sử dụng `ProcessPoolExecutor` (Đa tiến trình)? Cho ví dụ cụ thể đối với công việc của một Data Engineer.
- [ ] Làm thế nào để xây dựng một data pipeline chịu lỗi tốt (Fault-tolerant) khi xử lý hàng trăm tệp dữ liệu đầu vào?

---

## 🧪 6. Unit Testing & Mocking (Pytest)
- [ ] Lợi ích của `@pytest.fixture` là gì? Khi nào ta cần sử dụng fixtures?
- [ ] Khái niệm Mocking trong Unit Test dùng để làm gì?
- [ ] Tại sao khi viết test case cho một hàm gọi API ngoài (hoặc kết nối Database), ta bắt buộc phải sử dụng Mocking?
