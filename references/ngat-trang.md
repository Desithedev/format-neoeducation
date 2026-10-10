# Dàn trang gọn — Phần I và Phần II có quy tắc riêng

Theo yêu cầu mới và PDF tham chiếu, **chỉ Phần I giữ cả câu trên một trang**. Phần II được nối trang giữa các ý để tận dụng phần trang còn lại, giữ từng ý và nhãn đáp án nguyên khối.

| Phần | Đơn vị không tách trang | Môi trường |
| --- | --- | --- |
| I — 24 câu trắc nghiệm của đề này | Đề bài, code/hình/bảng, tất cả A/B/C/D của **một câu** | `NEOQuestion` |
| II — đúng/sai | Nội dung **một ý** a/b/c/d, danh sách con/code/bảng và nhãn cuối của ý đó | `NEOStatement` trong `NEOTrueFalse` |

## Phần I

- Bọc từng câu từ `\cau` đến phương án D trong `NEOQuestion`. Nếu cả khối không đủ chỗ, chuyển cả câu xuống trang/cột kế tiếp; không để phương án lẻ ở trang sau.
- Không gom 24 câu thành một khối, không ép mỗi trang chỉ có một câu. Không dùng `\newpage` cố định sau câu 8/18 hoặc mọi câu.
- Giữ số cột của từng khối nguồn. Hình/code nằm đúng câu; không dùng float tự đẩy sang câu khác.

## Phần II

- Dùng `NEOTFQuestion` cho đoạn dẫn và các ý. Đây là luồng văn bản **có thể tách trang**, không phải minipage cho cả câu.
- Trong `NEOTrueFalse`, thay mỗi `\item` cấp a/b/c/d bằng `\begin{NEOStatement}` ... `\end{NEOStatement}`. Môi trường tự sinh nhãn a)/b)/c)/d); không thêm `\item` lần nữa.
- Phải giữ toàn bộ một ý, kể cả code/danh sách con và `\NEOTrue`/`\NEOFalse`, bên trong `NEOStatement`. Danh sách con vẫn dùng `\item` bình thường.
- **Quy tắc ngắt trang 2-2 ("Đi chung hoặc 2 - 2"):** Mọi câu Phần II được phân chia thành 2 khối cố định: `NEOPairAB` (gồm ý a và b trong minipage) và `NEOPairCD` (gồm ý c và d trong minipage). Nhờ đó:
  - Nếu trang còn đủ chỗ: cả 4 ý a, b, c, d sẽ đi cùng nhau trên một trang ("đi chung").
  - Nếu thiếu chỗ: khối c, d sẽ cùng nhau nhảy sang trang sau ("2 - 2").
  - **Tuyệt đối 100% không thể xảy ra hiện tượng ý d lẻ loi một mình** (vì d luôn gắn chặt với c trong `NEOPairCD`), và ý a không bao giờ lẻ loi (vì a luôn gắn chặt với b trong `NEOPairAB`).
- Đoạn dẫn dài và tiêu đề mục: Tiêu đề `B. Phần riêng` + `Định hướng Khoa học máy tính` phải đi liền sát với Câu 3 (dùng `\Needspace{14\baselineskip}` trước tiêu đề và `\nopagebreak`), không bao giờ để tiêu đề mục nằm đơn độc ở đáy trang trong khi Câu 3 bị đẩy sang trang sau. Bảng code hai cột và hình giữ nguyên từng khối.

Ví dụ cấu trúc (nội dung thực phải lấy từ đề, không thêm câu minh họa này):

```latex
\begin{NEOTFQuestion}
\cau{1} Xét các phát biểu sau:
\begin{NEOTrueFalse}
  \begin{NEOStatement}
    Nội dung ý a) từ đề. \NEOTrue
  \end{NEOStatement}
  \begin{NEOStatement}
    Nội dung ý b) từ đề. \NEOFalse
  \end{NEOStatement}
\end{NEOTrueFalse}
\end{NEOTFQuestion}
```

## Stem đứng trước khối không phá được

Bảng code Python/C++ và khung SQL là hộp không phá trang. Nếu chỉ cần vài dòng cuối trang, stem câu đó sẽ bị bỏ lại một mình. Đặt `\Needspace{18\baselineskip}` ngay trước dòng `\cau{n}` của câu TF có bảng code để stem đi cùng bảng. `\NEOCodePair` trong template đã có `\Needspace{12\baselineskip}`; macro tùy biến của đề phải có guard tương tự.

Khoảng trắng đuôi trang do bảng code này là chấp nhận được. Rút xuống dưới khoảng trắng do stem lẻ vẫn xấu hơn.

## Tiết kiệm giấy, vẫn dễ đọc

- A4, lề 1.5cm; `includefoot` và `footskip=1cm` dành chỗ cho footer. Thân Times New Roman 12pt, baseline 14.5pt; code Courier New 11pt theo template. Không thu chữ tới mức khó đọc để ép số trang.
- Khoảng cách giữa câu MC 0.2cm, giữa câu TF 0.1cm; `itemsep=2pt`, `topsep=3pt`, `parsep=0pt` cho danh sách. Không chèn dòng trắng thừa giữa các ý.
- Không dùng `\vfill` hoặc nhiều `\vspace` để kéo đầy trang. `\raggedbottom` giữ khoảng cách thực ổn định; giảm khoảng trắng bằng việc cho TF nối giữa ý, không kéo giãn chữ. Cần ngắt giữa các ý thì dùng `midpenalty=0`; `itempenalty` không tồn tại trong `enumitem`.
- Khoảng trắng còn lại do một câu MC, một ý TF hoặc bảng code chưa vừa là chấp nhận được. Một khối **riêng lẻ** cao hơn vùng in phải báo và xử lý bố cục, không cắt nội dung hay tràn footer.

## Kiểm tra PDF

1. Phần I: tất cả câu và A/B/C/D nằm trọn trang; đối chiếu cuối trang–đầu trang kế tiếp.
2. Phần II: có thể nối giữa ý; mỗi ý và nhãn cuối nằm trọn trang, thứ tự a/b/c/d không khởi động lại, nội dung không thiếu.
3. Không có tiêu đề lẻ, nhãn đáp án bị bỏ sang trang khác, khoảng trắng giãn bất thường hoặc chữ chạm footer. Kiểm tra cả code, hình và danh sách con.
4. Palette, font, icon Facebook và một watermark opacity 0.15 mỗi trang đúng mẫu. Đếm số trang cuối và cập nhật thông tin đầu đề trực tiếp.
