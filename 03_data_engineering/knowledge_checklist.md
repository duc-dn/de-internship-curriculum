# 📋 Checklist Kiến thức cần nắm được - Data Engineering (Nâng cao)

Dưới đây là danh sách các câu hỏi tự đánh giá giúp Intern kiểm tra mức độ hiểu biết của mình sau khi hoàn thành Giai đoạn 3 (SQL, Modeling, dbt, Spark & Airflow). Intern cần tự tin giải thích được các khái niệm này và trình bày sơ đồ kiến trúc hệ thống với Mentor trong buổi báo cáo cuối kỳ.

---

## 🗄️ 1. SQL nâng cao & Tối ưu hóa truy vấn
- [ ] Phân biệt sự khác nhau giữa các hàm xếp hạng: `ROW_NUMBER()`, `RANK()`, và `DENSE_RANK()`.
- [ ] Giải thích cách dùng các hàm lấy giá trị dòng trước/sau: `LEAD()` và `LAG()`.
- [ ] So sánh hiệu năng và độ rõ ràng giữa việc sử dụng Common Table Expressions (CTEs) và Subqueries.
- [ ] Làm thế nào để đọc và phân tích một Query Execution Plan (`EXPLAIN ANALYZE`) trong PostgreSQL? Kể tên 2 chỉ số quan trọng cần tối ưu (ví dụ: Sequential Scan vs Index Scan).

---

## 📐 2. Thiết kế Mô hình dữ liệu (Data Modeling)
- [ ] So sánh sự khác nhau giữa hệ thống OLTP (Online Transaction Processing) và OLAP (Online Analytical Processing).
- [ ] Thế nào là Fact Table và Dimension Table? Cho ví dụ thực tế từ Capstone Project.
- [ ] So sánh ưu/nhược điểm và trường hợp áp dụng của mô hình Star Schema và Snowflake Schema.
- [ ] Giải thích khái niệm Slow Changing Dimensions (SCD) Type 1 và Type 2. Cách thiết kế bảng Dim theo SCD Type 2 để lưu vết lịch sử thay đổi của dữ liệu.

---

## 🛠️ 3. Biến đổi dữ liệu với dbt (Data Build Tool)
- [ ] Triết lý dbt là ELT hay ETL? Giải thích tại sao.
- [ ] Tại sao trong dbt bắt buộc phải dùng hàm `ref()` và `source()` thay vì viết trực tiếp tên bảng (`FROM schema.table`)?
- [ ] Phân biệt sự khác nhau về cơ chế vật lý hóa dữ liệu (Materializations) trong dbt: `table`, `view`, `incremental`, và `ephemeral`. Khi nào nên chọn loại nào?
- [ ] Schema test trong dbt (`unique`, `not_null`, `relationships`) giúp đảm bảo Data Quality như thế nào?

---

## ⚡ 4. Tính toán phân tán với PySpark
- [ ] Giải thích vai trò của Driver Program và các Executors trong kiến trúc Apache Spark.
- [ ] Lazy Evaluation là gì? Phân biệt Transformations (Wide vs Narrow) và Actions trong Spark.
- [ ] Định dạng tệp tin Parquet giúp tăng tốc hiệu năng truy vấn phân tích dữ liệu lớn bằng cách nào so với tệp CSV thông thường? (Giải thích khái niệm Columnar Storage và Column Projection).
- [ ] So sánh cơ chế chạy và hiệu năng mạng của Shuffle Hash Join và Broadcast Hash Join. Khi nào ta nên áp dụng Broadcast Join?
- [ ] Phân biệt kỹ thuật Partitioning (phân vùng) và Bucketing trong Spark.

---

## 🕸️ 5. Điều phối luồng công việc với Apache Airflow
- [ ] Nêu chức năng của các thành phần chính trong Apache Airflow: Webserver, Scheduler, Executor, và Metadata Database.
- [ ] Phân biệt các khái niệm: DAG, Operator, Task, và Task Instance.
- [ ] Làm thế nào để truyền dữ liệu hoặc trạng thái nhỏ giữa các Task chạy độc lập trong Airflow? (Giải thích cơ chế XCom).
- [ ] Giải thích nguyên tắc Idempotency (tính đơn trị/khả lặp) trong thiết kế Data Pipeline. Tại sao một pipeline tốt bắt buộc phải đảm bảo Idempotency?
- [ ] Hai tính năng `catchup` và `backfill` trong Airflow giúp giải quyết bài toán gì khi hệ thống bị dừng hoạt động một thời gian dài?
