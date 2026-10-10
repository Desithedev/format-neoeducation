# Trích/cắt hình của đề vào LaTeX

Mọi hình thuộc đề phải được trích/cắt và chèn vào nguồn LaTeX, đúng câu, đúng thứ tự và bố cục. Không thay hình bằng lời mô tả, không bỏ hình vì OCR không đọc được, không giao đường dẫn ảnh không tồn tại.

## Quy trình

1. Xem trang gốc, lập danh sách hình theo trang/câu, gồm biểu đồ, sơ đồ, ảnh chụp màn hình và hình có nhãn. Ghi vị trí và chú thích; phân biệt với watermark/trang trí nền.
2. Ưu tiên lấy tài nguyên gốc khi chúng đúng với vùng hình **đang hiển thị**. Nếu hình là một phần của ảnh scan toàn trang hoặc có crop/ghép nhiều đối tượng, render trang rồi cắt riêng vùng hình.
3. Giữ đầy đủ hình, trục, đơn vị, nhãn, mũi tên, chú giải và chữ bên trong. Chừa viền nhỏ để không cắt sát nét; không lấy câu lân cận, cột bên cạnh, header/footer hoặc số trang.
4. Chú thích ngoài hình: chép đúng thành văn bản LaTeX tại vị trí tương ứng và loại khỏi crop. Nếu chữ gắn liền với hình không thể tách an toàn, giữ trong crop và không chép lại để tránh lặp. Không tự thêm số hình hoặc dùng `\caption` sinh nhãn mới khi nguồn không có.
5. Lưu vào `output/latex/<ten-de>/assets/figures/`; đặt tên theo nguồn, ví dụ `p002-q008-fig01.png`. Nếu chưa xác định số câu, dùng trang và thứ tự hình, không đoán. Giữ file gốc; chỉ thao tác trên bản sao.
6. Chèn bằng đường dẫn tương đối đúng vị trí. Phần I: hình cùng toàn câu và A/B/C/D trong `NEOQuestion`. Phần II: hình đoạn dẫn nằm trong luồng `NEOTFQuestion`; hình thuộc một ý ở trong `NEOStatement` cùng ý/nhãn đó. Giữ tỷ lệ ảnh và số cột nguồn. Không dùng float `figure` tự đẩy hình sang câu khác, không ép cả câu TF vào một minipage lớn.
7. Đối chiếu tất cả hình đã liệt kê với nguồn LaTeX và PDF: đúng câu, đủ số hình/nhãn/chú thích, không mờ hoặc tràn lề.

## Theo loại đầu vào

| Nguồn | Cách lấy hình |
| --- | --- |
| PDF có ảnh nhúng | Trích ảnh gốc nếu đủ nhãn và đúng vùng hiển thị; nếu nhãn/vector/crop nằm ngoài ảnh nhúng, render và crop vùng hoàn chỉnh hoặc xuất vùng vector bằng công cụ hỗ trợ |
| PDF scan / ảnh toàn trang | Render PDF khoảng 300 DPI rồi crop theo tọa độ vùng hình; giữ độ phân giải ảnh đầu vào nếu nguồn đã là raster |
| DOCX | Ánh xạ `r:embed` trong `word/document.xml` qua `word/_rels/document.xml.rels` tới `word/media`; lấy cả hình trong bảng. `r:link` có thể trỏ tới ảnh ngoài tài liệu: kiểm tra target thực tế, không giả định nằm trong media; thiếu file thì báo và yêu cầu bổ sung |
| DOCX có crop/xoay/ghép | Áp dụng vùng hiển thị và biến đổi của nguồn; nếu không tái hiện chắc chắn, render DOCX rồi crop từ bản hiển thị |

Nếu PDF có vector/nhãn rất nhỏ, ưu tiên vector; raster có thể render 450–600 DPI khi cần để đọc rõ. Không upscale ảnh gốc mờ rồi nhận là đã khôi phục chi tiết. Nếu không có công cụ render/trích ảnh, báo phần thiếu và yêu cầu ảnh/trang rõ hơn; không bịa hoặc tự vẽ lại hình.

### Crop DOCX

Ảnh trong `word/media` có thể chứa phần đã bị cắt khi hiển thị. Đọc `a:srcRect` của đối tượng hình: `l`, `t`, `r`, `b` là tỷ lệ cắt trên thang `100000`, trường thiếu là `0`. Với ảnh rộng `W`, cao `H`, vùng còn lại trước xoay/lật là:

```text
x0 = W * l / 100000
y0 = H * t / 100000
x1 = W * (1 - r / 100000)
y1 = H * (1 - b / 100000)
```

Đối chiếu thêm kích thước hiển thị, xoay/lật, clipping và nhóm đối tượng. Nếu không có vùng hợp lệ hoặc ảnh thuộc nhóm phức tạp, dùng bản render để giữ đúng hình nhìn thấy; không chỉ lấy nguyên file media rồi bỏ qua crop.

## Chèn hình

Mẫu dưới đây minh họa ảnh đã thực sự được tạo, đặt trực tiếp trong khối câu tương ứng. Chiều rộng phải điều chỉnh theo nguồn và không vượt `\linewidth` của cột; không tự phóng hình nhỏ thành cả cột.

```latex
\begin{center}
  \includegraphics[width=0.8\linewidth]{assets/figures/p002-q008-fig01.png}
\end{center}
```

Khi hình có watermark nguồn dính trong ảnh, báo riêng; không tự xóa chi tiết. Watermark của PDF đầu ra vẫn dùng ảnh mặc định của skill ở lớp nền trang.
