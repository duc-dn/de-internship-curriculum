# 📝 Lab 4: Phân tích dữ liệu doanh thu cửa hàng (Pandas & NumPy)

Bài thực hành này giúp Intern thành thạo kỹ năng xử lý dữ liệu dạng bảng bằng thư viện Pandas và NumPy, rèn luyện tư duy tối ưu hóa hiệu năng thay vì sử dụng vòng lặp Python thuần.

---

## 🎯 Kiến thức Intern cần nắm được (Target Knowledge)
Sau bài lab này, Intern phải giải thích được:
1.  **DataFrame & Series:** Cấu trúc dữ liệu dạng bảng của Pandas và cách truy xuất hàng/cột (`loc`, `iloc`).
2.  **Vectorization (Vectơ hóa):** Cơ chế tính toán song song trên mảng của NumPy/Pandas, tại sao phép toán mảng `df['a'] * df['b']` lại nhanh hơn việc duyệt vòng lặp `for`.
3.  **Xử lý dữ liệu khuyết (Handling Missing Data):** Sự khác nhau giữa việc xóa dòng (`dropna`) và điền giá trị thay thế (`fillna`) tùy theo ý nghĩa nghiệp vụ.
4.  **Gộp nhóm & Tổng hợp (Aggregation):** Cách dùng `groupby` kết hợp `agg` để tính toán nhiều chỉ số thống kê cùng lúc.
5.  **Parquet Format:** Tại sao định dạng lưu trữ cột (Columnar format) này lại tối ưu cho Data Warehouse và BI tools so với định dạng dòng (CSV, JSON).

---

## 📂 Đề bài chi tiết (Detailed Requirements)

### 1. Dữ liệu đầu vào (Input Data)
Mentor hãy chuẩn bị một tệp tin CSV mẫu tên là `sales_raw.csv` (~50,000 dòng dữ liệu mô phỏng giao dịch cửa hàng bán lẻ) có cấu trúc cột như sau:
`transaction_id, customer_id, product_name, category, quantity, unit_price, discount, purchase_date`
*   *Các lỗi dữ liệu giả lập cần có để Intern xử lý:*
    *   Cột `discount` chứa nhiều giá trị trống (null).
    *   Cột `unit_price` và `quantity` có một vài dòng bị giá trị <= 0 hoặc null.
    *   Cột `purchase_date` không đồng bộ định dạng (ví dụ: có dòng là `YYYY-MM-DD`, dòng khác là `DD/MM/YYYY`, hoặc `YYYY/MM/DD`).

### 2. Logic cần thực thi trong script `sales_analysis.py`
*   **Đọc và làm sạch dữ liệu:**
    *   Đọc file CSV vào Pandas DataFrame.
    *   Loại bỏ các giao dịch có `quantity <= 0` hoặc `unit_price <= 0`.
    *   Xử lý giá trị trống ở cột `discount`: Điền giá trị mặc định là `0.0`.
    *   Đồng bộ định dạng cột `purchase_date` về kiểu `datetime64`.
*   **Tính toán doanh thu thực tế:**
    *   Tính toán cột mới `revenue` = `quantity` * `unit_price` * (1 - `discount`).
    *   *Yêu cầu bắt buộc:* Phải sử dụng phép tính Vectorized của Pandas/NumPy, tuyệt đối không dùng vòng lặp `for` hay `.iterrows()`.
*   **Tổng hợp dữ liệu (Aggregation):**
    *   Tính tổng doanh thu và tổng số lượng hàng bán được theo từng danh mục sản phẩm (`category`).
    *   Tìm top 5 khách hàng (`customer_id`) mang lại doanh thu lớn nhất.
    *   Tính tỷ trọng doanh thu của mỗi danh mục so với tổng doanh thu toàn cửa hàng.
*   **Xuất kết quả:** Ghi bảng tổng hợp doanh thu danh mục sản phẩm ra file `category_report.parquet`.

---

## 📥 Định nghĩa đầu ra & Sản phẩm bàn giao (Expected Outputs & Deliverables)

### 1. Script `sales_analysis.py`
Mã nguồn xử lý dữ liệu hoàn chỉnh, viết rõ ràng, có chú thích các bước giải quyết.

### 2. File kết quả `category_report.parquet`
Nội dung file sau khi đọc lại bằng Pandas phải có dạng bảng giống như thế này:
```text
      category  total_revenue  total_quantity  revenue_percentage
0  Electronics      1502400.5            3400               45.24
1     Clothing       890500.2            5200               26.81
2         Home       930000.3            1800               27.95
```

---

## 📈 Tiêu chí đánh giá của Mentor (Validation Criteria)
1.  **No Loops for Ingestion/Math:** Mentor kiểm tra nếu Intern sử dụng bất kỳ vòng lặp `for` nào để tính toán cột `revenue` hoặc để groupby -> Đánh giá Không Đạt. Yêu cầu làm lại bằng Vectorized operations.
2.  **Date Standard:** Kiểm tra kiểu dữ liệu của cột ngày tháng bằng `df.dtypes` phải hiển thị là `datetime64[ns]`.
3.  **Parquet Schema:** File kết quả phải được ghi đúng bằng lệnh `df.to_parquet(...)`. Kiểm tra dung lượng file parquet so với CSV gốc để Intern thấy sự khác biệt về lưu trữ.
