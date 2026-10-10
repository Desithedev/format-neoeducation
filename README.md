# Format đề Tin NEO

Skill nằm tại `.opencode/skills/format-de-tin-neo/` trong dự án. Đây là bộ hướng dẫn để trợ lý xử lý đề, không phải ứng dụng tự động chuyển đổi file.

## Đặt tài liệu vào đâu?

```text
format-de-tin-neo/
├── SKILL.md
├── README.md
├── references/
│   ├── quy-cach-neo.md
│   ├── template-neo.md
│   ├── code-trong-de.md
│   ├── prompt-thao-tac.md
│   ├── hinh-trong-de.md
│   ├── ngat-trang.md
│   ├── pdf-tham-chieu.md
│   └── watermark.md
├── assets/
│   ├── templates/         # Đã có neo-template.tex chuẩn
│   └── watermark/         # Ảnh watermark và cấu hình LaTeX
├── input/
│   └── de/                # Đề Word, PDF, ảnh, văn bản
└── output/
    ├── latex/             # Mỗi đề một thư mục .tex + tài nguyên
    └── pdf/               # PDF đã format, có watermark
```

1. Đặt đề vào `input/de/`. Có thể chỉ định một file ở ngoài thư mục này.
2. Dùng `assets/templates/neo-template.tex` đã chuẩn hóa từ mẫu LaTeX anh cung cấp. Template khóa màu, font, header/footer có icon Facebook và cách trình bày code; không cần bổ sung các DOCX mẫu cũ. Xem [quy cách thiết kế](references/template-neo.md).
3. Ảnh chim cánh cụt anh chọn đã được lưu ở `assets/watermark/neo-watermark.png`. Mặc định mỗi trang có đúng một watermark ảnh này, opacity **0.15**, giữa trang, phía sau nội dung và không xoay. Muốn đổi ảnh, đặt PNG trong suốt, JPG hoặc PDF một trang vào `assets/watermark/` và chỉ rõ tên file. SVG cần chuyển sang PNG/PDF trước khi dùng với LaTeX.
4. Gọi trợ lý trong dự án bằng tên skill hoặc đường dẫn `SKILL.md`.

## Ví dụ gọi

> Dùng skill format-de-tin-neo tại .opencode/skills/format-de-tin-neo/SKILL.md. Format input/de/de-01.pdf theo mẫu NEO, giữ nguyên nội dung/số cột, chưa làm đáp án. Phần I giữ mỗi câu cùng A/B/C/D trên một trang; Phần II cho nối trang giữa ý, từng ý và nhãn nguyên khối. Dàn gọn, lề 1.5cm, thân 13pt, mỗi trang một watermark ảnh opacity 0.15.

> Format input/de/de-02.docx, dùng ảnh assets/watermark/neo-watermark.png làm watermark phía sau nội dung, độ mờ 0.15, đặt giữa mọi trang; xuất tex và PDF.

> Tạo bản đáp án từ đề và bảng đáp án đính kèm, giữ watermark ảnh mặc định. Phần I tô vàng ký hiệu đáp án đúng; Phần II thêm [ĐÚNG]/[SAI] in đậm cuối mỗi ý.

Đường dẫn rút gọn trong các ví dụ tính từ thư mục skill. Nếu trợ lý chưa tìm thấy skill, chỉ rõ đường dẫn đầy đủ tới `SKILL.md`.

## Đầu ra

Ví dụ với đề `de-01.pdf`:

```text
output/latex/de-01/
├── de-01.tex
├── watermark.tex
└── assets/
    ├── watermark.png      # Bản sao ảnh watermark đã chọn
    ├── figures/           # Hình được trích/cắt từ đề
    └── code/              # Code nguyên văn đọc vào LaTeX
output/pdf/de-01.pdf
```

Tên đã tồn tại sẽ dùng `de-01-v2`, rồi `de-01-v3` cho cả thư mục LaTeX và PDF. File `.tex` dùng đường dẫn tương đối để chuyển cả thư mục sang máy khác vẫn sử dụng được.

Hình trong đề được trích/cắt rồi chèn bằng `\includegraphics`, giữ vị trí, nhãn và chú thích. **Phần I:** `NEOQuestion` giữ từng câu và A/B/C/D nguyên khối. **Phần II:** `NEOTFQuestion` cho nối trang giữa các ý; từng ý a/b/c/d dùng `NEOStatement` cùng nhãn cuối. Không kéo cả câu TF lên trang mới. Xem [ngắt trang](references/ngat-trang.md).

Dàn gọn theo [PDF anh chọn](references/pdf-tham-chieu.md): lề 1.5cm, thân Times New Roman 13pt, danh sách cách 2pt, có khoảng dành cho footer. Giữ màu và code rõ, hạn chế khoảng trắng bằng phân trang hợp lý.

Code dùng Courier New, giữ nguyên nội dung/thụt lề/nháy, không tự sửa chương trình. Nguồn có Python/C++ hai cột thì dùng bảng hai cột của template, không chuyển thành một khối hoặc thêm syntax highlighting. Xem [code trong đề](references/code-trong-de.md).

Logic “làm đáp án”, trích đề sạn/làm sạch, giữ cột, cấm comment/citation và cú pháp `\nd`/`\bd` được tổng hợp tại [prompt thao tác](references/prompt-thao-tac.md).

`input/de/`, `output/` và ảnh watermark bổ sung được bỏ qua bởi Git; ảnh mặc định đã chọn, cấu hình và tài liệu hướng dẫn được giữ. Việc thêm file vào `input/de/` không tự chạy format.

## Điều kiện xuất PDF

Cần XeLaTeX hoặc LuaLaTeX, font theo mẫu và các gói LaTeX mà mẫu sử dụng. Watermark kèm theo dùng `eso-pic`, `tikz` và `graphicx`. Cách biên dịch ở [reference watermark](references/watermark.md).

PDF chỉ được coi là hoàn tất sau khi biên dịch thành công và xem mọi trang. Nếu thiếu môi trường biên dịch, trợ lý vẫn có thể giao bộ `.tex` và tài nguyên, ghi rõ chưa xác minh PDF.
