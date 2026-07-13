# 📋 Checklist Kiến thức cần nắm được - Linux & Bash Shell

Dưới đây là danh sách các câu hỏi tự đánh giá giúp Intern kiểm tra mức độ hiểu biết của mình sau khi hoàn thành Giai đoạn 0 (Linux & Bash). Intern cần tự tin giải thích được các câu hỏi này và demo trực tiếp trên Terminal với Mentor trong buổi Code Review.

---

## 📁 1. Hệ thống Tệp tin & CLI
- [ ] Giải thích sự khác biệt giữa đường dẫn tuyệt đối (Absolute Path) và đường dẫn tương đối (Relative Path). Cho ví dụ.
- [ ] Phân biệt công dụng của các lệnh xem file: `cat`, `less`, `head`, `tail`.
- [ ] Làm thế nào để theo dõi một file nhật ký log đang liên tục cập nhật dòng mới theo thời gian thực (Real-time monitoring)? Lệnh nào thực hiện điều đó và tùy chọn gì?

---

## 🔒 2. Phân quyền File & Thư mục (Permissions)
- [ ] Ý nghĩa của các ký tự `r`, `w`, `x` đối với **File** khác gì so với đối với **Thư mục (Directory)**? (Gợi ý: Tại sao quyền `x` trên thư mục lại tối quan trọng?).
- [ ] Giải thích ý nghĩa phân quyền bằng số, ví dụ: `chmod 755` và `chmod 600`. Nhóm người dùng nào được hưởng quyền gì ở mỗi trường hợp?
- [ ] Làm thế nào để thay đổi chủ sở hữu (Owner) và nhóm (Group) sở hữu của một tệp tin?

---

## 🔍 3. Tìm kiếm & Xử lý Văn bản
- [ ] Sử dụng lệnh `find` như thế nào để tìm tất cả các file có đuôi `.csv` có kích thước lớn hơn 10MB trong thư mục `/data`?
- [ ] Phân biệt công dụng của lệnh `find` và lệnh `grep`.
- [ ] Ý nghĩa của ký tự đường ống Pipe (`|`) là gì? Cho ví dụ về việc kết hợp lệnh `find` hoặc `cat` với `grep` thông qua Pipe.
- [ ] Làm thế nào để đếm số dòng trong kết quả trả về của một lệnh?

---

## 📝 4. Trình soạn thảo Vim
- [ ] Vim có mấy chế độ hoạt động chính (Modes)? Cách chuyển đổi qua lại giữa các chế độ đó là gì?
- [ ] Trình bày các phím tắt nhanh để:
  - [ ] Di chuyển con trỏ lên, xuống, trái, phải trong Normal Mode.
  - [ ] Xóa một dòng hiện tại (`dd`), sao chép một dòng (`yy`), dán dòng (`p`).
  - [ ] Tìm kiếm một từ khóa bất kỳ trong file.
  - [ ] Thoát và lưu file (`:wq`), thoát và không lưu thay đổi (`:q!`).

---

## 🌐 5. Biến môi trường & Điều hướng luồng (Redirection)
- [ ] Biến môi trường là gì? Làm thế nào để kiểm tra danh sách các biến môi trường hiện có?
- [ ] Làm thế nào để khai báo một biến môi trường tạm thời trong session hiện tại, và làm thế nào để khai báo vĩnh viễn (mỗi khi mở terminal mới đều có)?
- [ ] Phân biệt sự khác nhau giữa ký tự điều hướng ghi đè (`>`) và ghi nối tiếp (`>>`).
- [ ] Giải thích ý nghĩa của cụm ký tự `2>&1` ở cuối lệnh. Khi nào chúng ta cần dùng nó?

---

## 🐚 6. Kịch bản tự động hóa Bash Script
- [ ] Dòng đầu tiên `#!/bin/bash` (Shebang) trong một file script dùng để làm gì? Điều gì xảy ra nếu thiếu dòng này?
- [ ] Trong Bash script, các biến đặc biệt như `$0`, `$1`, `$2`, `$#`, và `$?` đại diện cho giá trị nào?
- [ ] Làm thế nào để kiểm tra xem một lệnh trước đó chạy thành công hay thất bại trong Bash Script?
- [ ] Cấu trúc rẽ nhánh `if [ ... ]` cần lưu ý khoảng trắng ở các vị trí nào để không bị báo lỗi cú pháp?
