# Code trong đề — bảo toàn nguyên văn và thụt lề

Code là nội dung đề, không phải mã để “sửa cho chạy”. Giữ nguyên mọi dòng, dấu nháy, dấu ngoặc, toán tử, tên biến, chuỗi, thụt lề và thứ tự. Không đổi thuật toán, sửa SQL, thêm dòng, dịch Python sang C++ hoặc tự đổi tab/space có ảnh hưởng nội dung.

## Cơ chế chuẩn

Template dùng **Courier New** và `fvextra`/`\BVerbatimInput` để đọc file code nguyên văn thành hộp. Không đặt môi trường verbatim thô trong thân `tabularx` hoặc trong tham số macro: bảng đo lại nội dung và có thể làm mất cơ chế catcode. Đọc file bằng `\NEOCode` không gặp vấn đề đó.

1. Trích code từng khối thành UTF-8 trong `output/latex/<ten-de>/assets/code/`, ví dụ `p2-q4.py`, `p2-q4.cpp`, `p2-q6.sql`.
2. Đối chiếu từng dòng với nguồn, đặc biệt nhánh/indent Python ở B1. Khoa học máy tính. Nếu OCR không xác định được dấu hoặc thụt lề, hỏi/báo chỗ chưa rõ; không tự đoán.
3. File code chứa ký tự lập trình thật: `%`, `_`, `#`, `{}`, `<>`, `'`, `"`, `\`. Không thêm escape LaTeX vào file code. Ví dụ C++ giữ `{`, không ghi `\{`; phép chia dư Python giữ `%`, không ghi `\%`.
4. Đặt code đúng vị trí trước nhận xét/đáp án như nguồn. Phần I: code cùng toàn câu MC trong `NEOQuestion`. Phần II: code đoạn dẫn nằm trong luồng `NEOTFQuestion`; code thuộc một ý nằm trong `NEOStatement` của ý đó. Giữ một cột nếu nguồn có một ngôn ngữ; nguồn có bảng Python/C++ hai cột thì dùng `\NEOCodePair` để giữ đường viền, tiêu đề và hai cột ngang nhau, căn đầu.
5. Chép kèm mọi file code khi giao bộ LaTeX; không để đường dẫn tới file ngoài bộ xuất. Các file `.py`/`.cpp`/`.sql` chỉ là tài nguyên hiển thị, không được tự chạy.

Ví dụ khi **hai file thật** đã được trích:

```latex
\begin{NEOTFQuestion}
\cau{4} Cho hàm sau đây được viết bằng hai ngôn ngữ lập trình Python và C++:
\NEOCodePair{assets/code/p2-q4.py}{assets/code/p2-q4.cpp}
\end{NEOTFQuestion}
```

Đặt thêm đoạn dẫn và danh sách `NEOTrueFalse` trước `\end{NEOTFQuestion}`; từng ý dùng `NEOStatement`. Không giữ cả đoạn dẫn, bảng code và mọi ý trong một minipage lớn. Ví dụ chỉ minh họa lệnh chèn code, không phải câu hoàn chỉnh để giao.

## Dòng dài và kiểu chữ

- Code màu đen, không tự thêm syntax highlighting hoặc số dòng khi mẫu/nguồn không có.
- Mặc định `\small`; khối vượt chiều rộng được thu **đồng tỷ lệ** tới chiều rộng cột bằng `\resizebox`, không wrap hay thêm mũi tên/ngắt dòng mới. Bảng Python/C++ lấy độ rộng lớn nhất của hai khối để dùng **cùng tỷ lệ/cỡ chữ** cho cả hai cột, không thu riêng khiến một cột to một cột nhỏ. Khối ngắn không bị phóng to. Kiểm tra font sau thu vẫn đọc rõ (khoảng 9pt trở lên); nếu quá nhỏ thì báo khối cần xử lý bố cục, không âm thầm thu chữ tới mức khó đọc.
- Có khoảng đệm dưới khối code để ký tự dòng cuối không chạm/đè đường viền bảng.
- Không ghép các dòng bằng `\newline` hay giả lập indent bằng `\hspace*{1em}` khi đã có code nguyên văn. Chỉ dùng kiểu gõ tay trong bảng nếu môi trường không hỗ trợ verbatim và đã được kiểm tra từng ký tự/thụt lề.
- `upquote` và Courier New giữ nháy thẳng. Không chuyển nháy đơn/kép ASCII thành nháy cong; không gọi `\nd`/`\bd` bên trong file code.
- Khối code/bảng không vượt `\linewidth` hoặc sang câu khác. Phần I giữ cùng toàn câu; Phần II được nối trang giữa đoạn dẫn và các ý, nhưng code của từng ý phải đi cùng ý/nhãn đó. Khối riêng lẻ quá cao xử lý theo [ngắt trang](ngat-trang.md), không thu code xuống cỡ khó đọc hoặc bỏ dòng.

## Phần II và SQL

Nhận xét a)/b)/c)/d) dùng `NEOStatement` trong `NEOTrueFalse`, nhãn `\NEOTrue`/`\NEOFalse` đặt ở **cuối ý** khi làm đáp án. Với ý d) có SQL hoặc danh sách con, giữ toàn bộ ý và nhãn trong cùng `NEOStatement`; không gắn nhãn vào từng dòng SQL hoặc mỗi mục con.

Đoạn SQL trong khung giữ `tcolorbox` của mẫu. Đóng `itemize` danh sách bảng dữ liệu trước đoạn dẫn nhận xét; không để `enumerate` nhận xét nằm nhầm bên trong danh sách bảng.

## Kiểm tra

So sánh code tài nguyên với nguồn; xem toàn bộ bảng trên PDF ở kích thước đọc được: Python đúng indent, C++ đủ ngoặc/`;`, SQL không tự sửa trường, cột đủ rộng, nháy thẳng, không thiếu font/glyph, không tràn bảng và không mất dòng cuối.
