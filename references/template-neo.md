# Template NEO chuẩn — thiết kế đã được người dùng cung cấp

[neo-template.tex](../assets/templates/neo-template.tex) là nguồn thiết kế dùng chung, chuẩn hóa từ LaTeX người dùng gửi. Khi format, sao chép template vào bộ LaTeX của đề rồi chèn nội dung thật giữa `\begin{document}` và `\end{document}`. Không tự viết lại preamble, đổi màu, bỏ icon hoặc đổi bố cục header/footer. Template không chứa đề minh họa để tránh nhập nhầm nội dung.

## Các thành phần cố định

| Token | Giá trị | Vai trò |
| --- | --- | --- |
| `headerbg` | HTML `EBF5FB` | Nền và viền khung tiêu đề, bo 10pt |
| `neotitle` | HTML `0D47A1` | Màu navy thương hiệu của mẫu; không tự thêm thành phần mới chỉ để sử dụng token |
| `tealtext` | HTML `1976D2` | Chữ Câu, tiêu đề phần, đường kẻ header/footer |
| `neored` | HTML `FF0000` | Chỉ dành cho `[NEO]` |
| `neoblue` | HTML `0288D1` | Viền/chữ vòng tròn đáp án |
| `answerhighlight` | HTML `FFFF00` | Nền vàng của ký hiệu đáp án đúng |
| `navyblue` | RGB `25,45,95` | Chữ footer |
| `fbblue` | RGB `24,119,242` | Icon Facebook |

- Font nội dung **Times New Roman**, code **Courier New**; không âm thầm thay bằng font khác. Thiếu font thì báo và cài/cung cấp font trước khi nhận khớp mẫu.
- Khổ A4, lề trên/dưới/trái/phải **1.5cm**, `includefoot`, `footskip=1cm`. Khai báo trực tiếp `\documentclass[13.5pt,a4paper]{extarticle}` theo chuẩn thực tế chạy với XeLaTeX (kể cả trên Overleaf). Do `extarticle` mặc định không có file cỡ 13.5pt (sẽ fallback về 10pt khiến `\large` và `\Large` bị teo nhỏ hơn hoặc bằng thân chữ), **bắt buộc** định nghĩa lại toàn bộ thang cỡ chữ trong `\makeatletter`:
  - `\normalsize`: 13pt / 15.8pt (thân chữ câu hỏi và phương án)
  - `\small`: 10.5pt / 12.8pt (code trong đề)
  - `\footnotesize`: 9.5pt / 11.5pt
  - `\large`: 14.5pt / 17.5pt (tiêu đề mục A, B, Định hướng KHMT, THƯD)
  - `\Large`: 17pt / 20.5pt (tiêu đề PHẦN I, Phần II, MÔN THI, Mã đề)
  - `\LARGE`: 20pt / 24pt
  `footskip` phải ≥ 26.27pt, nếu nhỏ hơn fancyhdr cảnh báo và footer dính sát vùng in.
- Header giữ nền xanh nhạt, bo góc, hai nhóm thông tin 40%/60%, dòng họ tên, Số báo danh và ô mã đề bo góc bên dưới. Hai dòng gạch chân dài được `NEOFitLine` thu vừa chiều rộng nếu cần để không đè nhóm bên cạnh; không phóng dòng ngắn lên quá cỡ. Nội dung metadata lấy từ đề hoặc chỉ định của người dùng. Số trang hiển thị phải khớp PDF cuối.
- Footer bắt buộc có `\usepackage{fontawesome5}` và **`\faFacebook`** màu `fbblue`; chữ `NEO - Cùng nhau Thủ Khoa Tin Học THPT` màu `navyblue`, cỡ `\large`. Bên phải `Trang \thepage`; đường kẻ `tealtext` dày **1.5pt**. Không thay icon bằng chữ “Facebook”, không bỏ icon hoặc thu cả footer thành `\footnotesize`.
- Watermark lấy ảnh đã chọn, đúng một ảnh nền mỗi trang, opacity **0.15**, không xoay, rộng **`\textwidth`** như mẫu mới. Sao chép `watermark.tex` và `assets/watermark.png` cùng nguồn đề; không để đường dẫn `linhvatneo.JPEG` chưa tồn tại.
- `\cau{n}` in đậm riêng chữ Câu và số; `[NEO]:` đỏ đậm. `\opt{A}`/B/C/D là chữ đậm trong vòng tròn xanh. Phần II a)/b)/c)/d) đậm bằng `NEOTrueFalse`.

## Dùng macro

- `\NEOHeader{co-quan}{dia-phuong}{loai-de}{so-trang}{ky-thi}{dot-thi}{mon}{thoi-gian}{ma-de}` nhận chín giá trị **thật**. Đếm trang sau biên dịch rồi cập nhật số trang trực tiếp; không dùng `\label`/`\pageref`. Thiếu metadata thì hỏi hoặc bỏ riêng trường không có, không đưa tên tham số làm nội dung đề.
- **Phần I:** bọc toàn bộ một câu và A/B/C/D bằng `NEOQuestion`. **Phần II:** dùng `NEOTFQuestion` cho luồng câu, `NEOTrueFalse` cho danh sách và `NEOStatement` cho từng ý nguyên khối; không bọc cả bốn ý trong `NEOQuestion`. Header ở ngoài; tiêu đề mục dùng `NEOSectionHeading`, đi với vài dòng tiếp theo thay vì kéo cả câu TF sang trang mới.
- `\NEOOptions{1}{...}`, `\NEOOptions{2}{...}`, `\NEOOptions{4}{...}` theo đúng số cột nguồn. Dùng `&` ngăn cột, `\\` ngăn hàng; một cột dùng `1`, không tạo bảng `XX` rồi bỏ trống cột thứ hai.
- `\NEOCode{duong-dan-file-code}` cho một khối code; `\NEOCodePair{file-python}{file-cpp}` chỉ khi nguồn có hai cột Python/C++. Xem [quy tắc code](code-trong-de.md).
- `\nd{b}` hiển thị `'b'`; ví dụ `id = \nd{b}`. `\bd{Đăng}` in đậm Đăng. Không dùng cú pháp sai `\nd = {b}` hoặc `\bd = {Đăng}`. Trong code nguyên văn dùng nháy thật, không chèn `\nd`.

## Quy cách tiêu đề các phần và cỡ chữ chuẩn

- **Tiêu đề Phần I & Phần II:** Dùng màu `tealtext`, in đậm cỡ `\Large`, dòng hướng dẫn thí sinh cỡ `{\normalsize\textit{...}}`:
  ```latex
  \noindent\textcolor{tealtext}{\Large\textbf{PHẦN I. Câu hỏi trắc nghiệm nhiều phương án lựa chọn.}}\\[1pt]
  {\normalsize\textit{Thí sinh trả lời từ câu 1 đến câu 24. Mỗi câu hỏi thí sinh chỉ chọn một phương án.}}
  ```
  ```latex
  \noindent\textcolor{tealtext}{\Large\textbf{Phần II. Câu hỏi trắc nghiệm đúng sai.}}\\[1pt]
  {\normalsize\textit{Thí sinh trả lời 4 câu hỏi. Trong mỗi ý a), b), c), d) ở mỗi câu, thí sinh chọn đúng hoặc sai.}}
  ```
- **Tiêu đề mục con A, B:** Dùng cỡ `\textbf{\large ...}`:
  ```latex
  \noindent\textbf{\large A. Phần chung cho tất cả các thí sinh}
  ```
  ```latex
  \noindent\textbf{\large B. Phần riêng}\\[2pt]
  \textit{Thí sinh chỉ chọn một trong hai phần sau: ...}
  ```
- **Tiêu đề phân ban (Định hướng KHMT / THƯD hoặc B1, B2):** Dùng cỡ `\textbf{\large ...}`:
  ```latex
  \noindent\textbf{\large Định hướng Khoa học máy tính}
  ```
  ```latex
  \noindent\textbf{\large Định hướng Tin học ứng dụng}
  ```

## Các sửa kỹ thuật so với mã minh họa

1. Cỡ thân 13.5pt (khai báo `\documentclass[13.5pt,a4paper]{extarticle}`) và lề 1.5cm được áp dụng chuẩn hóa.
2. Mọi môi trường `itemize`, `enumerate`, bảng và box phải đóng đúng; ở câu cuối của mã minh họa, đóng `itemize` danh sách bảng trước đoạn “Sau khi tìm hiểu…” và `enumerate` nhận xét.
3. Code đọc nguyên văn từ tài nguyên riêng, thay cách gõ `\ttfamily` + `\newline` + `\hspace*` thủ công vốn dễ sai thụt lề/ký tự.
4. Câu MC giữ nguyên khối; câu TF nối giữa các ý, giữ từng ý và nhãn cuối. Không dùng `\newpage` cố định sau câu 8/18 của mẫu cho mọi đề.
6. Trong `enumitem` chỉ có key phạt ngắt trang `beginpenalty`, `midpenalty`, `endpenalty`. Viết `itempenalty` sẽ dừng biên dịch với `Package enumitem Error: itempenalty undefined`; cần cho phép ngắt giữa các ý thì đặt `midpenalty=0`.
5. Bỏ comment và `%` chặn khoảng trắng. Các cặp minipage dùng `\end{minipage}\hfill\begin{minipage}` trên cùng dòng để không sinh khoảng trắng dư, không cần `%`.

Các sửa trên thay cơ chế dàn trang, không sửa câu hỏi/đáp án/code. Nếu người dùng yêu cầu sửa nội dung thì xử lý riêng và nêu thay đổi đó.
