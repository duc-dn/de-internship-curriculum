# 📝 Lab 2: Trình soạn thảo văn bản Vim (10 Bài tập)

Tài liệu này bao gồm **10 bài tập thực hành về Vim** sắp xếp theo độ khó tăng dần từ Rất dễ đến Khó để Intern luyện tập kỹ năng thao tác và chỉnh sửa tệp tin cấu hình trực tiếp trên máy chủ bằng phím tắt của Vim.

---

## 🎯 Tại sao Data Engineer bắt buộc phải biết Vim?
Khi kết nối SSH vào một máy chủ từ xa (Remote Server) để cấu hình hệ thống hoặc sửa nhanh một dòng code, bạn sẽ **không có giao diện đồ họa (GUI) hay VS Code**. Vim là trình soạn thảo văn bản phổ biến nhất, nhẹ nhất và có sẵn trên mọi server Linux giúp bạn xử lý công việc ngay tại Terminal.

---

## 📚 Phím tắt Vim quan trọng cần thuộc lòng:
*   `i`: Chuyển sang **Insert Mode** (để bắt đầu gõ chữ).
*   `Esc`: Quay về **Normal Mode** (chế độ dòng lệnh để di chuyển/tìm kiếm/xóa).
*   `:wq` hoặc `:x`: Lưu thay đổi và Thoát khỏi Vim.
*   `:q!`: Thoát khỏi Vim và **không lưu** các thay đổi.
*   `dd`: Xóa (Cut) dòng hiện tại.
*   `yy`: Sao chép (Copy) dòng hiện tại.
*   `p`: Dán (Paste) nội dung xuống dòng bên dưới.
*   `u`: Hoàn tác (Undo) hành động trước đó.
*   `/từ_khóa`: Tìm kiếm từ khóa (ấn `n` để nhảy đến kết quả tiếp theo, `N` quay lại).

---

## 📂 10 BÀI TẬP THỰC HÀNH (Độ khó tăng dần):

*   **Bài 2.1 [Rất dễ]:** Sử dụng Vim để mở một tệp tin cấu hình mới tên là `database.ini` (`vim database.ini`).
*   **Bài 2.2 [Rất dễ]:** Chuyển sang chế độ soạn thảo (Insert Mode), nhập vào nội dung kết nối database (host, port, user), sau đó quay về chế độ lệnh (Normal Mode) bằng phím `Esc`.
*   **Bài 2.3 [Dễ]:** Lưu lại toàn bộ nội dung đã viết và thoát khỏi trình soạn thảo Vim để quay lại terminal (`:wq` hoặc `:x`).
*   **Bài 2.4 [Dễ]:** Mở lại tệp `database.ini` bằng Vim, sửa đổi nội dung lung tung, sau đó thực hiện thoát hoàn toàn ra ngoài Terminal mà **không lưu** bất kỳ sửa đổi mới nào (`:q!`).
*   **Bài 2.5 [Trung bình - Dễ]:** Viết một tệp cấu hình dài 50 dòng log giả lập. Thực hiện phím tắt để nhảy nhanh con trỏ xuống dòng cuối cùng của tệp (`G`), sau đó nhảy ngược về dòng đầu tiên của tệp (`gg`).
*   **Bài 2.6 [Trung bình]:** Ở chế độ Normal Mode, thực hiện xóa (Cut) nhanh đúng 3 dòng dữ liệu liên tục trong tệp mà không dùng nút Backspace xóa từng chữ (sử dụng lệnh `3dd` hoặc gõ `dd` 3 lần).
*   **Bài 2.7 [Trung bình]:** Thực hiện sao chép một dòng hiện tại (Copy line - `yy`) và dán dòng đó xuống bên dưới (Paste - `p`) 5 lần liên tục.
*   **Bài 2.8 [Trung bình - Khó]:** Tìm kiếm từ khóa `port` trong tệp cấu hình và nhảy qua các kết quả trùng khớp tiếp theo xuôi chiều và ngược chiều (sử dụng `/port` kết hợp phím `n` và `N`).
*   **Bài 6.9 [Khó]:** Sử dụng phím tắt để hoàn tác (Undo - `u`) một thao tác vừa xóa nhầm dòng, sau đó làm ngược lại khôi phục (Redo - `Ctrl + R`).
*   **Bài 2.10 [Khó]:** Thực hiện thay thế tự động (Find and Replace) tất cả các từ khóa `localhost` thành địa chỉ IP `192.168.1.15` trên toàn bộ tệp tin bằng một câu lệnh duy nhất ở chế độ Command-line (`:%s/localhost/192.168.1.15/g`).

---

## 📥 Sản phẩm bàn giao & Báo cáo của Intern
1.  Intern tạo một file `vim_answers.md` lưu tại thư mục `00_linux_labs/lab_2/`.
2.  Với mỗi bài tập trên, Intern ghi rõ:
    *   **Đề bài:** (Ghi lại yêu cầu).
    *   **Lệnh/Phím tắt thực thi:** (Ghi lại tổ hợp phím tắt hoặc câu lệnh chế độ Command-line đã sử dụng).
    *   **Ảnh minh chứng kết quả:** (Chèn ảnh chụp màn hình terminal hoặc nội dung file sau khi lưu để chứng minh đã thao tác thành công).
