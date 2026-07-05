# 📝 Lab 2: Big Data Processing với PySpark

Bài thực hành này giúp Intern làm quen với công nghệ xử lý dữ liệu lớn bằng **Apache Spark (PySpark)**, hiểu được nguyên lý tính toán phân tán và cách tối ưu hóa hiệu năng truy vấn trên dữ liệu hàng triệu bản ghi.

---

## 🎯 Kiến thức Intern cần nắm được (Target Knowledge)
Sau bài lab này, Intern phải giải thích được:
1.  **Spark Architecture:** Sự phối hợp giữa Driver Program và các Executors chạy phân tán.
2.  **Lazy Evaluation:** Tại sao Spark chia các lệnh thành Transformations (không chạy ngay) và Actions (chạy thực sự để trả kết quả).
3.  **Parquet Storage:** Cơ chế nén dữ liệu và đọc cột tối ưu của Parquet cho xử lý dữ liệu lớn.
4.  **Broadcast Join:** Sự khác biệt về hiệu năng giữa Shuffle Join (chuyển đổi dữ liệu qua mạng) và Broadcast Join (phân phối bảng nhỏ sang tất cả executors) khi join bảng lớn với bảng nhỏ.
5.  **Partitioning (Phân vùng dữ liệu):** Cách lưu trữ phân vùng trên đĩa đệm giúp tối ưu hóa thao tác đọc dữ liệu sau này.

---

## 📂 Nguồn dữ liệu thực hành (Internet Data Sources)

Intern tải trực tiếp nguồn dữ liệu thật quy mô lớn từ AWS CloudFront của Ủy ban Taxi và Xe limousine Thành phố New York (NYC TLC):

### 1. Tập dữ liệu giao dịch chính (Yellow Taxi Trip Records):
*   **Tháng thực hành:** Tháng 1 năm 2024 (~2.9 triệu dòng dữ liệu thô, định dạng Parquet).
*   **Link tải trực tiếp:** [NYC Yellow Taxi Jan 2024 Parquet (50 MB)](https://d37ci6vzurychx.cloudfront.net/trip-data/yellow_tripdata_2024-01.parquet)
*   *Lệnh tải nhanh:* `curl -o yellow_tripdata_2024-01.parquet https://d37ci6vzurychx.cloudfront.net/trip-data/yellow_tripdata_2024-01.parquet`

### 2. Tập dữ liệu phụ tra cứu (Taxi Zone Lookup Table):
*   **Nội dung:** Danh mục ánh xạ ID khu vực sang tên Phố/Quận của NYC (CSV).
*   **Link tải trực tiếp:** [Taxi Zone Lookup (CSV)](https://d37ci6vzurychx.cloudfront.net/misc/taxi_zone_lookup.csv)
*   *Lệnh tải nhanh:* `curl -o taxi_zone_lookup.csv https://d37ci6vzurychx.cloudfront.net/misc/taxi_zone_lookup.csv`

---

## 📂 Đề bài chi tiết (Detailed Requirements)

### 1. Yêu cầu thiết lập môi trường:
*   Intern cấu hình chạy Spark Local trên máy tính cá nhân hoặc trong container Docker.
*   Đặt 2 file dữ liệu tải về ở trên vào thư mục dữ liệu đầu vào:
    ```text
    spark_project/
    ├── data/
    │   ├── yellow_tripdata_2024-01.parquet
    │   └── taxi_zone_lookup.csv
    ├── src/
    │   └── taxi_processor.py
    └── output_taxi_data/ (Thư mục kết quả tự động sinh ra)
    ```

### 2. Logic thực thi trong PySpark script `taxi_processor.py`:
*   **Đọc dữ liệu thô:**
    *   Khởi tạo `SparkSession` tối ưu cấu hình chạy local.
    *   Đọc tệp dữ liệu Parquet lớn và kiểm tra cấu trúc schema.
*   **Làm sạch & Biến đổi dữ liệu:**
    *   Kiểm tra định dạng các cột thời gian đón khách (`tpep_pickup_datetime`) và trả khách (`tpep_dropoff_datetime`), thực hiện cast types về `TimestampType` nếu cần.
    *   Lọc bỏ các bản ghi không hợp lệ: Quãng đường di chuyển (`trip_distance`) <= 0, số hành khách (`passenger_count`) <= 0, tổng tiền thanh toán (`total_amount`) <= 0.
    *   Tạo cột mới: Tính toán thời gian chuyến đi (duration = dropoff - pickup, tính theo đơn vị phút) và tính vận tốc di chuyển trung bình (dặm/giờ).
*   **Tối ưu hóa Join:**
    *   Đọc tệp danh mục `taxi_zone_lookup.csv` thành Spark DataFrame.
    *   Thực hiện **Broadcast Join** để join bảng danh mục này vào bảng dữ liệu taxi chính nhằm gán tên khu vực đón (`PULocationID` -> `pickup_zone_name`) thông qua ID khu vực tương ứng.
*   **Xuất dữ liệu:**
    *   Ghi dữ liệu kết quả ra thư mục cục bộ dưới định dạng **Parquet**.
    *   Áp dụng kỹ thuật **Partitioning** theo cột quận đón khách (`Borough` của địa điểm đón) khi ghi file.

---

## 📥 Định nghĩa đầu ra & Sản phẩm bàn giao (Expected Outputs & Deliverables)

### 1. Script PySpark `taxi_processor.py` (hoặc file Jupyter Notebook `.ipynb`)
Mã nguồn xử lý dữ liệu Spark hoàn chỉnh. Có phần chú thích giải thích rõ:
*   Tại sao lại sử dụng Broadcast Join trong trường hợp này.
*   Cách xác định số lượng partition ghi ra đĩa.

### 2. Thư mục đầu ra dữ liệu Parquet
Thư mục lưu trữ kết quả phân vùng dạng:
`output_taxi_data/Borough=Manhattan/part-xxxx.snappy.parquet`  
`output_taxi_data/Borough=Brooklyn/part-xxxx.snappy.parquet`

---

## 📈 Tiêu chí đánh giá của Mentor (Validation Criteria)
1.  **Spark session optimization:** Intern khởi tạo session đúng cách, tắt ứng dụng (`spark.stop()`) sau khi chạy xong để giải phóng tài nguyên.
2.  **Broadcast Join implementation:** Code phải sử dụng rõ ràng hàm `broadcast()` từ thư viện `pyspark.sql.functions` cho bảng zone lookup.
3.  **Data Quality check:** Không có dòng dữ liệu nào có giá trị âm hoặc quãng đường bằng 0 tồn tại ở tệp kết quả.
