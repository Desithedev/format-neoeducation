# Logic prompt của người dùng

Các yêu cầu dưới đây được tích hợp vào skill theo **chế độ được yêu cầu**, không tự chạy đồng thời tất cả chế độ.

## Format/chuyển sang LaTeX

- Giữ thiết kế NEO theo [template chuẩn](template-neo.md): màu, header, footer có icon Facebook, font, vòng tròn đáp án và watermark.
- Chỉ thay nội dung minh họa bằng nội dung đề được cung cấp. Không thêm/xóa/sửa câu, phương án, nhận xét, code, hình hoặc bảng.
- Giữ số cột từng khối của nguồn: 2 cột vẫn 2, 1 cột vẫn 1; 4 cột giữ 4. Không suy ra số cột toàn đề từ bố cục phương án.
- Phần I giữ cả câu và A/B/C/D trên một trang. Phần II cho nối trang giữa ý, giữ từng ý cùng nhãn cuối; không kéo cả câu TF sang trang mới. Lề 1.5cm, thân 12pt, `footskip=1cm` theo yêu cầu tiết kiệm giấy, vẫn đủ chỗ footer và đọc rõ. Câu TF có bảng code cần `\Needspace` trước stem để không bỏ stem lẻ cuối trang.
- Không thêm `\cite`, `\ref`, `\label`, `\footnote`, `\pageref`, `\autoref`, `\eqref` hoặc lệnh tham chiếu tương tự. Không đưa dấu `[cite]` của OCR/model vào đề.
- Không thêm comment LaTeX hoặc `%` cuối dòng để chặn khoảng trắng. Dấu phần trăm thật trong văn bản dùng `\%`; toán tử/comment là nội dung của chương trình giữ nguyên trong file code riêng. Không xóa nội dung thật chỉ để tránh ký tự `%`.
- In đậm chữ **Câu và số câu** bằng `\cau`; ký hiệu A/B/C/D bằng `\opt`/`\trueopt`, a)/b)/c)/d) bằng label đậm của `NEOTrueFalse`. Không tô đậm toàn câu khi chỉ yêu cầu tô đậm nhãn.

## Làm đáp án

Chỉ khi người dùng yêu cầu “làm đáp án”, “bản giáo viên” hoặc cung cấp yêu cầu tương đương:

1. Phần I: chọn đáp án theo bảng đáp án hoặc tự giải nếu được yêu cầu; dùng `\trueopt` tô vàng **ô tròn ký hiệu đúng**. Các lựa chọn còn lại vẫn `\opt`. Không tô toàn dòng hoặc đổi màu chữ thành màu khác.
2. Phần II: cuối từng ý thêm đúng một `\textbf{[ĐÚNG]}` hoặc `\textbf{[SAI]}`; có thể dùng `\NEOTrue`/`\NEOFalse` tương đương. Giữ nguyên nội dung phát biểu dù nó sai.
3. Tự giải phải kiểm tra từng đáp án; điểm thiếu dữ kiện báo ngoài đề. Không thêm lời giải nếu chỉ yêu cầu đánh dấu.

## “Format đề sạn” / trích xuất và làm sạch

- Trích toàn bộ đề ra text khi được yêu cầu, giữ thứ tự, bảng, code, hình và chú thích. Xuất Word/PDF chỉ khi được yêu cầu và công cụ hỗ trợ; render kiểm tra trước khi nhận hoàn tất.
- Khi được yêu cầu loại chữ đen/dấu vết trong ảnh, xác định phần đó là **nhiễu, chữ chèn thừa hoặc dấu vết cần xóa** rồi làm trên bản sao. Chữ đen có ý nghĩa trong sơ đồ, code, trục, nhãn và chú thích vẫn là nội dung đề; nếu chưa phân biệt được vùng nhiễu, hỏi chỉ rõ vùng cần xóa.
- Không tự làm sạch trong chế độ format thông thường; không sửa trực tiếp đề gốc.

## Lệnh dùng đúng

| Ý định | Cú pháp |
| --- | --- |
| Ảnh giữa khối | `\begin{center}\includegraphics[width=0.4\linewidth]{assets/figures/ten-anh.png}\end{center}` |
| Ảnh bên trái | `\noindent\includegraphics[width=5cm]{assets/figures/ten-anh.png}` |
| Nháy đơn quanh b trong văn bản | `id = \nd{b}` |
| In đậm Đăng | `\bd{Đăng}` |
| Khoảng cách giữa các câu | MC theo `NEOQuestion`; TF theo `NEOTFQuestion`, mỗi ý `NEOStatement`; `\vspace` chỉ điều chỉnh có chủ đích |
| Trang mới có chủ đích | `\newpage`, không đặt sau mọi câu |
| Ngắt dòng văn bản | `\\` khi có dòng để kết thúc; đoạn mới dùng `\par` |

Ví dụ tên ảnh phải thay bằng file thực đã tạo. Giới hạn kích thước theo `\linewidth` của cột; không dùng `\textwidth` để làm ảnh/bảng tràn khỏi một cột nhỏ.
