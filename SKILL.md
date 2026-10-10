---
name: format-de-tin-neo
description: Use when người dùng yêu cầu format đề thi Tin học theo mẫu NEO từ Word, PDF, ảnh hoặc văn bản; chuyển đề sang LaTeX NEO, xuất PDF có watermark NEO hoặc làm bản đáp án NEO.
---

# Format đề Tin NEO

Giữ nguyên nội dung đề, định dạng theo mẫu NEO và xuất file mới. Không tự soạn câu hỏi, sửa đáp án hoặc lấy thông tin kỳ thi từ câu hỏi minh họa.

## Tra nhanh

Mọi đường dẫn bên dưới tính từ **thư mục chứa SKILL.md này**, không phải thư mục làm việc hiện tại. Đọc [template chuẩn](references/template-neo.md) và [quy cách](references/quy-cach-neo.md) trước khi format. Khi có code, đọc [code nguyên văn](references/code-trong-de.md); khi có ảnh, đọc [xử lý hình](references/hinh-trong-de.md). Áp dụng [ngắt trang](references/ngat-trang.md), [watermark](references/watermark.md) và [logic prompt](references/prompt-thao-tac.md) theo chế độ.

| Mục | Đường dẫn |
| --- | --- |
| Đề gốc | `input/de/` hoặc file người dùng chỉ định |
| Template NEO chuẩn | `assets/templates/neo-template.tex` |
| Watermark | `assets/watermark/` |
| LaTeX và ảnh đi kèm | `output/latex/<ten-de>/` |
| PDF đã format | `output/pdf/<ten-de>.pdf` |

PDF mặc định có **đúng một ảnh chim cánh cụt NEO** từ `assets/watermark/neo-watermark.png` phía sau nội dung **mỗi trang**, opacity **0.15**, không xoay. Chỉ đổi ảnh, dùng chữ hoặc tắt watermark khi người dùng yêu cầu. Watermark cũ của PDF nguồn không phải watermark cần sao chép sang bản xuất.

## Chế độ

- **Format đề học sinh:** giữ câu hỏi, phương án, phát biểu, code, bảng, hình và số cột từng khối; không đánh dấu đáp án.
- **Bản đáp án:** chỉ khi được yêu cầu; Phần I dùng `\trueopt` tô vàng ô tròn ký hiệu đúng; Phần II thêm `\textbf{[ĐÚNG]}` hoặc `\textbf{[SAI]}` cuối từng ý, sau cả code/danh sách con nếu có. Giữ nguyên phát biểu sai; chỉ thêm lời giải khi được yêu cầu.
- **Trích PDF/ảnh / đề sạn:** xem trang gốc để nhận diện cột trước khi format. Chỉ làm sạch vùng nhiễu/chữ chèn thừa được yêu cầu, không xóa chữ đen có ý nghĩa trong hình. Xuất Word/text khi được yêu cầu và công cụ hỗ trợ.

## Quy trình

1. Xác định đề và chế độ. Một đề thì xử lý đề đó; nhiều đề mà chưa chỉ rõ phạm vi thì hỏi chọn đề hay tất cả. Chưa có đề thì đề nghị cung cấp đề, không dùng câu minh họa làm đầu vào.
2. Đọc toàn bộ nguồn; DOCX gồm bảng trong `word/document.xml` và hình trong `word/media`. Ghi nhận số phần/câu/ý, cột, metadata, code, bảng, hình; báo riêng chỗ mờ, thiếu hoặc mâu thuẫn.
3. Sao chép template `.tex` đã có, giữ nguyên palette, font, header và footer **có `\faFacebook`**; không tự viết lại thành bản đơn sắc hoặc bỏ icon. Không cần đòi DOCX mẫu cũ. Metadata lấy từ đề hoặc chỉ định rõ, không lấy từ câu minh họa; xác nhận khi chỉ định xung đột với nguồn.
4. Chọn tên không dấu, an toàn cho đường dẫn. Kiểm tra **cả** thư mục LaTeX và tên PDF; nếu đã tồn tại, tăng hậu tố `-v2`, `-v3` đồng bộ. Tạo `.tex` UTF-8 hoàn chỉnh. Trích hình vào `assets/figures/`, code nguyên văn vào `assets/code/` trong bộ LaTeX; giữ cột và dùng `\NEOCodePair` chỉ khi nguồn có hai cột Python/C++. Đường dẫn tương đối, giữ nguyên nguồn.
5. Áp dụng watermark theo reference, sao chép cấu hình và ảnh vào bộ LaTeX; chỉ giữ một cơ chế cho mọi trang. **Phần I:** bọc từng câu và tất cả A/B/C/D bằng `NEOQuestion`; không đủ chỗ thì chuyển cả câu, không ép mỗi trang một câu. **Phần II:** dùng `NEOTFQuestion`, cho nối trang giữa các ý; mỗi ý a/b/c/d cùng code/bảng/danh sách con và nhãn cuối dùng `NEOStatement` trong `NEOTrueFalse`. Không bọc cả câu đúng/sai bằng `NEOQuestion`. Dùng lề 1.5cm, thân chữ 13pt và khoảng cách gọn của template để hạn chế khoảng trắng mà vẫn đọc rõ.
6. Đối chiếu đáp án cung cấp; tự giải chỉ khi được yêu cầu. Kiểm tra code bằng mô phỏng khi cần, không chạy trực tiếp mã chưa tin cậy. Thiếu dữ kiện thì báo câu chưa xác định, không đoán hoặc đưa ghi chú nội bộ vào đề học sinh.
7. Kiểm tra nội dung, palette, icon Facebook, indent/nháy code, môi trường đóng đủ, nhãn đáp án và không có comment/citation theo reference. Biên dịch XeLaTeX hoặc LuaLaTeX từ thư mục đề, xem **mọi trang** PDF rồi mới giao. Thiếu compiler/font/tài nguyên: giao `.tex` và tài nguyên có sẵn, nêu phần chưa kiểm chứng; không gọi PDF cũ là bản xuất mới.

## Giao file

Trả đường dẫn `.tex`, thư mục ảnh và PDF thực sự tạo được; nêu trạng thái biên dịch/kiểm tra trang, câu còn thiếu và cấu hình watermark đã dùng. Không khẳng định đã xem PDF khi chỉ đọc mã.

Ví dụ: “Dùng skill `format-de-tin-neo`, format `input/de/de-01.pdf` sang mẫu NEO, giữ nguyên nội dung và số cột, chưa làm đáp án; Phần I giữ từng câu cùng A/B/C/D trong một trang, Phần II nối trang giữa các ý nhưng mỗi ý nguyên khối; dàn gọn theo PDF tham chiếu, xuất LaTeX/PDF mỗi trang một watermark ảnh NEO opacity 0.15.”
