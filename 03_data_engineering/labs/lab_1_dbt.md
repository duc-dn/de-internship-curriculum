# 📝 Lab 1: Data Modeling & dbt (Data Build Tool)

Bài thực hành này giúp Intern tiếp cận tư duy Analytics Engineering, biết cách chuyển đổi dữ liệu thô (Raw) thành mô hình dữ liệu đa chiều Star Schema tối ưu cho báo cáo phân tích bằng công cụ **dbt**.

---

## 🎯 Kiến thức Intern cần nắm được (Target Knowledge)
Sau bài lab này, Intern phải giải thích được:
1.  **Star Schema (Mô hình hình sao):** Định nghĩa bảng Fact, bảng Dimension và các khóa ngoại liên kết.
2.  **dbt Project Structure:** Cách cấu hình file `dbt_project.yml`, thư mục `models/` và quản lý nguồn dữ liệu `sources`.
3.  **dbt Materializations:** Sự khác nhau giữa `table`, `view`, `incremental` và `ephemeral`. Khi nào nên dùng loại nào?
4.  **Jinja & Macros:** Cách viết SQL động trong dbt để tái sử dụng mã nguồn (hàm `ref()`, `source()`).
5.  **Data Quality Testing:** Tầm quan trọng của schema tests (`unique`, `not_null`, `relationships`) đối với độ tin cậy của báo cáo.

---

## 📂 Nguồn dữ liệu thực hành (Internet Data Sources)

Để Intern có thể thực hành nhanh chóng và đồng bộ dữ liệu, ta sử dụng nguồn dữ liệu công khai trên Internet dưới đây:

### 1. Dữ liệu thời tiết thô (Weather History):
Dữ liệu thời tiết thô được lưu trữ công khai trên GitHub dưới định dạng CSV (khoảng hơn 1,400 dòng lịch sử nhiệt độ, lượng mưa hàng ngày):
*   **Link tải trực tiếp:** [Seattle Weather Dataset (CSV)](https://raw.githubusercontent.com/seattle-code-academy/weather-data/main/seattle-weather.csv)
*   *Cấu trúc file:* `date, precipitation, temp_max, temp_min, wind, weather`
*   *Cách chuẩn bị:* Intern tải file này về và import trực tiếp vào bảng `raw_weather` trong cơ sở dữ liệu PostgreSQL của mình để làm bảng nguồn (source table) hoặc sử dụng tính năng `dbt seed` để tải lên DB.

### 2. Dữ liệu danh mục thành phố (City Lookup Seed):
Bảng thông tin tọa độ các thành phố lớn để làm bảng Dimension:
*   **Link tải trực tiếp:** [World Cities Latitude & Longitude (CSV)](https://raw.githubusercontent.com/lutangar/cities.json/master/cities.json) (hoặc Intern tự tạo một file seed CSV nhỏ chứa danh mục các thành phố Việt Nam: Hà Nội, TP.HCM, Đà Nẵng).

---

## 📂 Đề bài chi tiết (Detailed Requirements)

### 1. Thiết kế mô hình dữ liệu (Star Schema):
Intern cần tổ chức lại bảng dữ liệu phẳng thô `raw_weather` thành mô hình Dimensional Model gồm:
*   `fact_weather_metrics` (khóa chính, khóa ngoại liên kết, các chỉ số đo lường: `temp_max`, `temp_min`, `precipitation`, `wind`).
*   `dim_weather_condition` (khóa chính `condition_key`, tên trạng thái thời tiết: `drizzle`, `rain`, `sun`, `snow`, `fog`).
*   `dim_date` (khóa ngày `date_key`, `date`, `year`, `month`, `quarter`, `day_of_week`).

### 2. Cấu hình dự án dbt:
*   Khởi tạo dự án dbt kết nối tới PostgreSQL database.
*   Thiết lập file `profiles.yml` chuẩn xác để kết nối.
*   Tổ chức các tầng thư mục trong thư mục `models/`:
    *   `staging/`: Chứa các model làm sạch dữ liệu ban đầu, ép kiểu và đổi tên cột dễ hiểu.
    *   `marts/` (hoặc `core/`): Chứa các model liên kết tạo bảng Dimension và Fact.

### 3. Viết dbt Models:
*   **Staging layer:** Viết `stg_weather.sql` đọc dữ liệu thô từ database.
*   **Core layer:**
    *   `dim_weather_condition.sql`: Trích xuất danh sách trạng thái thời tiết duy nhất từ dữ liệu nguồn.
    *   `dim_date.sql`: Tạo ra danh mục ngày tháng từ khoảng thời gian của dữ liệu thời tiết.
    *   `fact_weather_metrics.sql`: Liên kết dữ liệu thời tiết staging với các bảng dim bằng các khóa tương ứng.
*   *Yêu cầu:* Sử dụng hàm `ref()` và `source()` thay vì hard-code tên schema/bảng trong câu lệnh SQL.

### 4. Cấu hình Data Quality Tests:
Tạo file `schema.yml` khai báo kiểm tra:
*   `unique` và `not_null` cho các khóa chính của `dim_weather_condition`, `dim_date`, và `fact_weather_metrics`.
*   `relationships` (Foreign Key test) để đảm bảo khóa ngoại trong Fact tồn tại trong bảng Dim tương ứng.

---

## 📥 Định nghĩa đầu ra & Sản phẩm bàn giao (Expected Outputs & Deliverables)

### 1. Mã nguồn dbt Project
Thư mục dự án dbt có đầy đủ models, cấu hình YAML và profiles mẫu.

### 2. Kết quả kiểm thử thành công
Báo cáo log khi chạy lệnh:
*   `dbt run`: Thực thi biến đổi dữ liệu thành công, tạo các bảng trong database.
*   `dbt test`: Vượt qua tất cả các bài Data Quality Test không có lỗi.
*   `dbt docs generate`: Tạo thành công tài liệu lineage graph trực quan của dự án.

---

## 📈 Tiêu chí đánh giá của Mentor (Validation Criteria)
1.  **dbt Best Practices:** Mentor kiểm tra nếu Intern dùng câu lệnh `FROM schema.table` trực tiếp thay vì dùng macros `ref()` hoặc `source()` -> Đánh giá Không Đạt.
2.  **Star Schema Integrity:** Các bảng Fact và Dim phải được thiết lập đúng khóa liên kết. Bảng Fact không chứa các thông tin mô tả phi cấu trúc của bảng Dim (như tên trạng thái...).
3.  **Data Quality Cover:** Bắt buộc có cấu hình kiểm tra ràng buộc `relationships` (khóa ngoại) giữa Fact và Dim.
