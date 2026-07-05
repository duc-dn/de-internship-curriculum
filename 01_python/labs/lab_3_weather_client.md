# 📝 Lab 3: Python OOP API Weather Client

Đây là bài thực hành chính thức đầu tiên, tập trung vào thiết lập một cấu trúc dự án chuẩn Software và viết mã nguồn Python hướng đối tượng, có kiểm thử hoàn chỉnh.

---

## 🎯 Kiến thức Intern cần nắm được (Target Knowledge)
Sau bài lab này, Intern phải giải thích được:
1.  **Lập trình Hướng đối tượng (OOP):** Class, instance attributes, methods, đóng gói (encapsulation) dữ liệu và tính kế thừa.
2.  **Type Hinting:** Cách chỉ định kiểu dữ liệu cho tham số đầu vào và đầu ra của hàm để tăng độ tin cậy và giúp IDE phân tích lỗi tĩnh.
3.  **Custom Exceptions:** Tại sao cần tự định nghĩa ngoại lệ cho riêng ứng dụng của mình thay vì sử dụng Exception chung chung.
4.  **Unit Testing & Mocking:** Khái niệm unit test độc lập, cơ chế mock dữ liệu bên thứ ba để đảm bảo kiểm thử chạy nhanh, ổn định và không cần internet.
5.  **Test Coverage (Độ bao phủ):** Cách đọc báo cáo coverage để biết các dòng code nào chưa được kiểm tra.

---

## 📂 Đề bài chi tiết (Detailed Requirements)

### 1. Cấu trúc thư mục dự án
Intern phải tạo cấu trúc dự án chính xác như dưới đây trong thư mục `weather_client/`:
```text
weather_client/
├── src/
│   ├── __init__.py
│   ├── client.py        # Chứa Class OpenMeteoClient
│   ├── exceptions.py    # Chứa các Custom Exceptions
│   └── models.py        # Chứa Class WeatherReport (dùng dataclasses hoặc Pydantic)
├── tests/
│   ├── __init__.py
│   └── test_client.py   # Chứa các unit tests
├── .env.example         # Định nghĩa các biến môi trường cấu hình mẫu
├── .gitignore
├── requirements.txt
└── README.md            # Tài liệu hướng dẫn sử dụng và chạy kiểm thử
```

### 2. Đặc tả kỹ thuật các Class:
*   **Class `WeatherReport` (trong `models.py`):**
    *   Sử dụng `@dataclass` (thư viện chuẩn) hoặc `BaseModel` (Pydantic).
    *   Thuộc tính bắt buộc: `latitude: float`, `longitude: float`, `temperature: float`, `windspeed: float`, `weather_code: int`, `timezone: str`.
*   **Class `OpenMeteoClient` (trong `client.py`):**
    *   Hàm khởi tạo: `def __init__(self, base_url: str = "https://api.open-meteo.com/v1/forecast", timeout: int = 10) -> None`.
    *   Hàm gọi dữ liệu: `def get_current_weather(self, lat: float, lon: float) -> WeatherReport`.
        *   Tạo URL request: `{base_url}?latitude={lat}&longitude={lon}&current_weather=true`.
        *   Sử dụng thư viện `requests` để gửi HTTP GET.
        *   Sử dụng block `try-except` bắt lỗi kết nối (`requests.exceptions.RequestException`). Nếu lỗi kết nối, quăng ra custom exception `WeatherConnectionError`.
        *   Kiểm tra mã trạng thái trả về (status code). Nếu không phải là 200, quăng ra custom exception `WeatherAPIError` kèm thông báo chi tiết lỗi từ API.
        *   Nếu thành công, trích xuất dữ liệu từ body JSON và khởi tạo đối tượng `WeatherReport` để trả về.
*   **Custom Exceptions (trong `exceptions.py`):**
    *   Định nghĩa: `class WeatherClientError(Exception)` làm lớp cha.
    *   Định nghĩa: `class WeatherConnectionError(WeatherClientError)` và `class WeatherAPIError(WeatherClientError)` kế thừa từ lớp cha.

---

## 📥 Định nghĩa đầu ra & Sản phẩm bàn giao (Expected Outputs & Deliverables)

### 1. File README.md
Tài liệu hướng dẫn Intern tự viết bao gồm các phần:
*   Cách cài đặt môi trường ảo và cài đặt thư viện (`pip install -r requirements.txt`).
*   Ví dụ code Python ngắn chứng minh cách sử dụng client để lấy thời tiết của một thành phố cụ thể và in nhiệt độ ra màn hình.
*   Lệnh chạy unit test và đo coverage.

### 2. File test_client.py
Yêu cầu tối thiểu viết 4 test cases sử dụng pytest và mock:
*   `test_get_weather_success`: Mock API trả về mã 200 kèm JSON dữ liệu hợp lệ. Xác nhận dữ liệu trả về từ hàm đúng kiểu `WeatherReport` và chính xác giá trị.
*   `test_get_weather_api_failure`: Mock API trả về mã 400 (do truyền sai tọa độ lat/lon). Xác nhận hàm quăng ra lỗi `WeatherAPIError`.
*   `test_get_weather_connection_timeout`: Mock thư viện quăng ra lỗi Timeout. Xác nhận hàm quăng ra lỗi `WeatherConnectionError`.
*   `test_get_weather_invalid_json`: Mock API trả về mã 200 nhưng dữ liệu JSON bị trống hoặc thiếu trường. Xác nhận code xử lý an toàn và ném lỗi phù hợp.

---

## 📈 Tiêu chí đánh giá của Mentor (Validation Criteria)
1.  **Style Guide:** Code có tuân thủ PEP 8 không? Có biến hoặc hàm nào không ghi type hints không? (Chạy thử `flake8` để kiểm tra).
2.  **Test Independence:** Khi ngắt mạng internet của máy tính, chạy lệnh `pytest` có thành công 100% không? (Nếu thất bại nghĩa là Intern chưa mock request API mà gọi trực tiếp lên internet).
3.  **Coverage:** Đo độ bao phủ kiểm thử bằng lệnh `pytest --cov=src tests/` đạt trên **80%** tổng số dòng code.
