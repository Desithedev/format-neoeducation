# Watermark cho bản xuất NEO

Watermark được chèn trong LaTeX trước khi biên dịch thành PDF. Không cần một chương trình đóng dấu PDF riêng.

## Mặc định và ưu tiên

| Thuộc tính | Mặc định |
| --- | --- |
| Nội dung | Ảnh `assets/watermark/neo-watermark.png` đã được người dùng chọn |
| Vị trí | Giữa trang, phía sau nội dung |
| Trang áp dụng | Đúng một watermark trên mỗi trang, kể cả trang cuối |
| Độ mờ / opacity | `0.15` trên thang `0`–`1` |
| Góc xoay | `0` độ |
| Kích thước | Rộng `\textwidth` theo template người dùng cung cấp, giữ tỷ lệ ảnh |

Yêu cầu cụ thể của người dùng ưu tiên hơn mặc định. Opacity `0.15` là độ đậm hiển thị 15%; số nhỏ hơn thì nhạt hơn. Giữ ảnh ngay ngắn, không xoay trừ khi người dùng yêu cầu.

Khi không có chỉ định mới, dùng ảnh mặc định đã chọn, dù thư mục có thêm ảnh khác. Nếu người dùng yêu cầu đổi ảnh nhưng chưa chọn file, hỏi trước. Nếu ảnh mặc định bị thiếu, báo thiếu và yêu cầu bổ sung, không tự thay bằng chữ. Không lấy watermark từ PDF nguồn làm ảnh mặc định.

Kiểm tra mẫu đã có watermark chưa. Nếu đã có, chỉnh hoặc thay cơ chế đó để đáp ứng yêu cầu; không giữ hai hook cùng đóng dấu mọi trang. Nếu người dùng tắt watermark, bỏ hook hoặc không nạp `watermark.tex`.

## Chèn vào nguồn của một đề

1. Sao chép `assets/watermark/watermark.tex` sang `output/latex/<ten-de>/watermark.tex` và ảnh `assets/watermark/neo-watermark.png` sang `output/latex/<ten-de>/assets/watermark.png`.
2. Nạp trong preamble của file đề, sau cấu hình font và trước `\begin{document}`:

```latex
\input{watermark.tex}
```

Fragment dùng lớp nền `\AddToShipoutPictureBG` không có dấu `*`, đăng ký **một lần** ở preamble, áp dụng mọi trang. Không nạp trong vòng lặp câu hỏi và không thêm một hook cho từng trang. Nó chỉ là cấu hình watermark, **không phải mẫu đề NEO hoàn chỉnh**. Tôn trọng các gói và tùy chọn đã được nạp trong mẫu; nếu gặp xung đột, tích hợp vào preamble hiện có thay vì nạp trùng tùy chọn.

Muốn đổi độ mờ/góc xoay cho riêng đề, đặt ngay sau `\input{watermark.tex}`:

```latex
\renewcommand{\NEOWatermarkOpacity}{0.15}
\renewcommand{\NEOWatermarkAngle}{0}
```

Muốn đổi vị trí, sửa tọa độ `(current page.center)` và thêm `xshift`/`yshift` trong node của bản sao `watermark.tex`. Không đổi cấu hình dùng chung cho một yêu cầu riêng của một đề.

Chỉ khi người dùng yêu cầu watermark chữ, thay nội dung bằng `\renewcommand{\NEOWatermarkContent}{\sffamily\bfseries\fontsize{48}{58}\selectfont NEO EDUCATION}`.

## Dùng ảnh được chọn

Sao chép ảnh người dùng đã chọn vào `output/latex/<ten-de>/assets/watermark.png` (hoặc `.jpg`/`.pdf` đúng định dạng thật), rồi thay nội dung/góc sau khi nạp fragment:

Ví dụ bên dưới dùng PNG; với JPG/PDF phải đổi đường dẫn trong `\includegraphics` theo đúng tên file đã sao chép.

```latex
\renewcommand{\NEOWatermarkAngle}{0}
\renewcommand{\NEOWatermarkContent}{\includegraphics[width=\textwidth]{assets/watermark.png}}
```

Ưu tiên PNG trong suốt hoặc PDF một trang. Với PDF phải chọn đúng trang nếu nguồn có nhiều trang. SVG cần chuyển sang PNG/PDF bằng công cụ sẵn có; đổi đuôi file không phải chuyển đổi. Font và file ảnh phải tồn tại trước khi biên dịch.

## Biên dịch và kiểm tra

Chạy từ `output/latex/<ten-de>/`, với tên file thực tế:

```bash
xelatex -interaction=nonstopmode -halt-on-error -no-shell-escape de-01.tex
xelatex -interaction=nonstopmode -halt-on-error -no-shell-escape de-01.tex
```

Có thể thay `xelatex` bằng `lualatex`. Không dùng PDFLaTeX cho mẫu có `fontspec`. Giữ log để xử lý lỗi, không sao chép PDF sinh dở sau lần biên dịch lỗi sang thư mục giao file. Lần hai giúp ổn định vị trí TikZ và số trang; đọc log nếu còn yêu cầu biên dịch lại.

Sau khi thành công, xem tất cả các trang: watermark có mặt ở trang đầu/giữa/cuối, không nhân đôi, đúng góc/vị trí, chữ và code vẫn đọc rõ. Nền trắng đặc của bảng/hình có thể che watermark phía sau; giữ khả năng đọc, không chuyển watermark lên trên nội dung để ép nó xuyên qua hình.

Chỉ sao chép PDF đã kiểm tra sang `output/pdf/<ten-de>.pdf`. Thiếu compiler, gói, font, ảnh hoặc công cụ xem trang thì nêu chính xác trạng thái; không nhận là đã biên dịch/kiểm tra.
