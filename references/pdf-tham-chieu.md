# PDF tham chiếu người dùng chọn

Người dùng cung cấp PDF 6 trang của đề Tin học Trung Văn, mã 1301, làm chuẩn **độ gọn và thẩm mỹ**. Nội dung, mã đề, cơ quan và metadata của PDF này không trở thành mặc định cho đề khác.

## Đặc trưng cần bám

- Chữ dễ đọc, các câu xếp liên tục với khoảng cách ngắn, không có vùng trắng lớn do đẩy cả câu đúng/sai.
- Chữ Câu và số xanh đậm, `[NEO]` đỏ đậm; A/B/C/D có vòng tròn xanh, đáp án đúng nền vàng trong bản giáo viên. a)/b)/c)/d) và nhãn `[ĐÚNG]`/`[SAI]` cuối ý in đậm.
- Màu dùng đúng palette trong [template chuẩn](template-neo.md); footer đường kẻ xanh, icon Facebook và câu chữ NEO, số trang bên phải.
- Bảng code Python/C++ hai cột ngang nhau, viền mảnh, tiêu đề đậm, Courier New và thụt lề đúng. HTML/CSS/SQL dạng nguyên văn; khối có khung khi nguồn/mẫu có khung, không tự sửa chương trình.
- Câu đúng/sai có thể nối giữa a)/b)/c)/d) từ trang này sang trang sau. Ví dụ câu 5 Phần II có a) ở trang 5, b)–d) ở trang 6: đây là cách nối trang được chấp nhận. Giữ từng ý trọn vẹn cùng nhãn cuối.
- Tiêu đề mục đi với vài dòng tiếp theo, không giữ cùng cả câu dài. Lề mặc định 1.5cm và thân 13pt theo yêu cầu mới; không sao chép khoảng trắng hoặc vị trí `\newpage` cố định.

## Yêu cầu riêng ưu tiên khi dàn lại

1. Phần I vẫn giữ **mỗi câu cùng tất cả A/B/C/D trên một trang** theo chỉ định riêng của người dùng. Phần II chỉ giữ từng ý, không cả câu.
2. Số trang ghi đầu đề phải bằng PDF cuối. Đếm sau biên dịch và cập nhật trực tiếp, không lấy cố định “05 trang” từ PDF tham chiếu.
3. Watermark theo lựa chọn đã chốt: một ảnh chim cánh cụt NEO mỗi trang, opacity 0.15, phía sau nội dung, không xoay.
4. Khi chuyển đề mới, đối chiếu nội dung với **đề nguồn**, dùng PDF này để kiểm tra trình bày. Không lấy bản text OCR của PDF tham chiếu làm bằng chứng cho ký tự/code chưa rõ ở đề nguồn.

## Kiểm tra nhìn thực tế

Xem mọi trang ở kích thước đọc được: mật độ vừa phải, code không mất/cắt dòng, mỗi câu MC đầy đủ, mỗi ý TF/nhãn đầy đủ, không tràn footer. Khoảng trắng cuối trang chỉ còn khi một khối cần giữ nguyên thật sự không vừa. Không hạ cỡ chữ khó đọc chỉ để bắt buộc cùng số trang với PDF tham chiếu.
