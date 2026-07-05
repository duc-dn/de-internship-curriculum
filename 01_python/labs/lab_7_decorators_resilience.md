# 📝 Lab 7: Custom Decorators & Cơ chế Chịu lỗi (Resilience Ingestion)

Bài thực hành này giúp Intern nâng cao kỹ năng thiết kế phần mềm, viết mã nguồn tái sử dụng cao và xây dựng cơ chế chịu lỗi (Fault-tolerance/Resilience) khi thu thập dữ liệu bằng cách tự viết các **Custom Decorators**.

---

## 🎯 Kiến thức Intern cần nắm được (Target Knowledge)
Sau bài lab này, Intern phải giải thích được:
1.  **First-Class Functions (Hàm hạng nhất):** Khái niệm coi hàm như một biến thông thường (truyền hàm vào hàm khác, trả về hàm).
2.  **Closures (Bao đóng):** Cơ chế hàm con truy xuất và nhớ được giá trị của biến ở phạm vi hàm cha ngay cả khi hàm cha đã chạy xong.
3.  **Decorators:** Cách hoạt động của ký hiệu `@` và cơ chế thay đổi hành vi của một hàm mà không sửa đổi trực tiếp mã nguồn của hàm đó.
4.  **Decorator Factories:** Viết decorator có thể nhận tham số đầu vào (như số lần retry, thời gian sleep).
5.  **Exponential Backoff:** Thuật toán tăng dần thời gian chờ giữa các lần thử lại để tránh làm nghẽn hệ thống bên thứ ba.

---

## 📂 Đề bài chi tiết (Detailed Requirements)

Intern cần viết mã nguồn trong thư mục dự án `resilience_ingestion/` gồm các file:
- `decorators.py` (Chứa code decorators)
- `ingest_job.py` (Chứa logic gọi API giả lập)

### 1. Viết Decorator đo lường hiệu năng `@timer`
*   Viết decorator `@timer` dùng để đo thời gian chạy thực tế của một hàm bất kỳ.
*   Khi hàm thực thi xong, decorator tự động in ra log thông tin: `[PERF] Function '{tên_hàm}' took {thời_gian} seconds to execute.`

### 2. Viết Decorator Retry với Backoff `@retry(max_attempts=3, backoff_seconds=2)`
*   Viết decorator factory `@retry` có cấu hình số lần thử lại tối đa (`max_attempts`) và thời gian chờ cơ bản (`backoff_seconds`).
*   **Thuật toán Exponential Backoff:** Thời gian chờ giữa các lần thử lại tiếp theo phải tăng dần theo cấp số nhân: $T = \text{backoff\_seconds} \times 2^{\text{attempt} - 1}$.
*   **Logic thực thi:**
    *   Nếu hàm thực thi bị lỗi (quăng ra ngoại lệ), decorator phải bắt ngoại lệ, ghi log mức `WARNING` báo lỗi và số lần thử lại tiếp theo.
    *   Thực hiện sleep theo thời gian tính toán và chạy lại hàm.
    *   Nếu hết số lần `max_attempts` vẫn lỗi, decorator ném ngoại lệ gốc ra ngoài để hệ thống lớn xử lý.

### 3. Giả lập công việc Ingestion (`ingest_job.py`)
*   Viết hàm `fetch_api_data(url: str) -> dict` giả lập việc gọi API: dùng hàm `random` để mô phỏng 70% cuộc gọi bị thất bại (ném ra lỗi `ConnectionError`) và 30% thành công.
*   Áp dụng đồng thời cả 2 decorators trên cho hàm:
    ```python
    @timer
    @retry(max_attempts=4, backoff_seconds=1)
    def fetch_api_data(url: str) -> dict:
        # Code logic giả lập lỗi ở đây
    ```

---

## 🖥️ 3. Yêu cầu làm Slide Thuyết trình (Presentation Requirement)

Trước khi gửi PR nộp bài Lab này, **Intern bắt buộc phải chuẩn bị một slide thuyết trình (5-7 trang)** trình bày các nội dung sau:
*   **Trang 1:** Khái niệm Closure và cơ chế hoạt động của Decorator trong Python.
*   **Trang 2:** Cú pháp Decorator Factory (Decorator nhận đối số) hoạt động như thế nào? (Giải thích luồng chạy qua 3 tầng hàm lồng nhau).
*   **Trang 3:** Tầm quan trọng của cơ chế Tự động Thử lại (Retry Mechanism) kết hợp Exponential Backoff khi viết pipeline thu thập dữ liệu (ingestion).
*   **Trang 4:** Phân tích luồng chạy thực tế của bài Lab khi có sự cố API xảy ra (hiển thị thời gian sleep tăng dần).
*   **Trang 5:** Cách bảo toàn siêu dữ liệu của hàm gốc (như tên hàm, docstring) sử dụng `@functools.wraps`.

---

## 📥 Định nghĩa đầu ra & Sản phẩm bàn giao (Expected Outputs & Deliverables)

### 1. Mã nguồn Python hoàn chỉnh
Thư mục `resilience_ingestion/` chứa đầy đủ module.

### 2. File log chạy Terminal thành công
Khi chạy `python ingest_job.py`, log in ra phải thể hiện rõ:
```text
2026-07-05 11:30:00 [WARNING] Function 'fetch_api_data' failed on attempt 1. Retrying in 1s...
2026-07-05 11:30:01 [WARNING] Function 'fetch_api_data' failed on attempt 2. Retrying in 2s...
2026-07-05 11:30:03 [WARNING] Function 'fetch_api_data' failed on attempt 3. Retrying in 4s...
2026-07-05 11:30:07 [INFO] Function 'fetch_api_data' succeeded on attempt 4.
[PERF] Function 'fetch_api_data' took 7.12 seconds to execute.
```

---

## 📈 Tiêu chí đánh giá của Mentor (Validation Criteria)
1.  **Correct Exponential Backoff:** Mentor kiểm tra log thời gian chạy. Thời gian chờ giữa các lần retry phải tăng chính xác theo lũy thừa cơ số 2 (1s, 2s, 4s, 8s...).
2.  **Metadata Preservation:** Code trong decorator phải sử dụng `@functools.wraps` để bảo toàn thuộc tính `__name__` và `__doc__` của hàm gốc.
3.  **Decorator Nesting Order:** Intern giải thích đúng cơ chế chạy lồng nhau của Decorators: `@timer` nằm ngoài `@retry` sẽ đo cả thời gian chờ retry, trong khi `@retry` nằm ngoài `@timer` chỉ đo thời gian chạy của nỗ lực cuối cùng thành công.
