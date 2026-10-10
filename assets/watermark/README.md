# Tài nguyên watermark

- `neo-watermark.png`: ảnh chim cánh cụt NEO người dùng chọn, giữ nguyên file nguồn.
- `watermark.tex`: cấu hình ảnh này, opacity **0.15**, góc **0°**, giữa nền mọi trang; mỗi trang đúng một watermark.
- Đặt ảnh watermark muốn dùng vào đây: PNG trong suốt, JPG hoặc PDF một trang; tên đơn giản như `neo.png`.
- Không chỉ định ảnh khác thì dùng `neo-watermark.png`. Muốn đổi ảnh, chỉ rõ file khi gọi skill.
- Không dùng ảnh watermark trích từ đề nguồn làm watermark mới khi chưa được yêu cầu.

Cách tích hợp và tùy chỉnh nằm trong [reference watermark](../../references/watermark.md). Sao chép ảnh đã chọn vào `assets/watermark.png` trong bộ LaTeX của từng đề; fragment dùng đường dẫn đó. File `.tex` này không tự tạo PDF và không thay thế mẫu định dạng đề.
