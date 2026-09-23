# Hướng dẫn sử dụng trang cá nhân

Trang được xây trên **Jekyll + theme [al-folio](https://github.com/alshedivat/al-folio)**,
host miễn phí trên **GitHub Pages**. Anh không cần cài gì trên máy: mỗi lần sửa nội dung và
đẩy (push) lên GitHub, GitHub Actions sẽ tự build lại và trang cập nhật sau 2–4 phút.

---

## 1. Đưa trang lên mạng

Mã nguồn nằm trong repo **`HOAHCE.github.io`**, nhánh `main`. Tên repo đặt đúng
dạng `<tên tài khoản>.github.io` nên GitHub xem đây là **user site** — địa chỉ mặc
định là `https://hoahce.github.io`, không có phần đuôi nào phía sau.

### Bước 1 — Bật GitHub Pages

Vào repo → **Settings** → **Pages** → mục **Build and deployment**:

- **Source**: `Deploy from a branch`
- **Branch**: `gh-pages` / thư mục `/ (root)` → **Save**

Nhánh `gh-pages` do workflow tự tạo sau lần build đầu tiên. Nếu chưa thấy trong
danh sách, vào tab **Actions** đợi job *Deploy site* chạy xong rồi quay lại.

### Bước 2 — Trỏ tên miền `tranthaihoa.id.vn`

File `CNAME` đã có sẵn trong mã nguồn và `_config.yml` đã đặt
`url: https://tranthaihoa.id.vn`, nên GitHub sẽ tự nhận tên miền. Việc còn lại
là cấu hình DNS.

**a) Tại Tenten.vn** — đăng nhập https://tenten.vn → **Quản lý tên miền** → chọn
`tranthaihoa.id.vn` → **Quản lý DNS**. Xoá các bản ghi A đang trỏ tới trang chờ
(parking) của Tenten, rồi thêm:

| Loại | Host / Tên | Giá trị / Trỏ tới | TTL |
| --- | --- | --- | --- |
| A | `@` | `185.199.108.153` | mặc định |
| A | `@` | `185.199.109.153` | mặc định |
| A | `@` | `185.199.110.153` | mặc định |
| A | `@` | `185.199.111.153` | mặc định |
| CNAME | `www` | `hoahce.github.io.` | mặc định |

> Cả 4 bản ghi A đều dùng host `@` — đó là cách GitHub cân bằng tải, không phải
> khai trùng. Nếu ô Host của Tenten không nhận ký tự `@`, hãy để **trống** hoặc
> điền `tranthaihoa.id.vn`. Giá trị CNAME nhớ giữ **dấu chấm ở cuối**.

**b) Tại GitHub** — Settings → Pages → **Custom domain** điền `tranthaihoa.id.vn`
→ **Save**. Đợi DNS lan truyền (thường 15–60 phút, tối đa 24 giờ). Khi GitHub báo
*DNS check successful*, tích **Enforce HTTPS**. Chứng chỉ SSL do GitHub cấp miễn
phí và tự gia hạn.

Kiểm tra DNS đã cập nhật chưa:

```bash
nslookup tranthaihoa.id.vn
```

Kết quả đúng là 4 địa chỉ `185.199.10x.153` ở trên.

---

### Lấy mã nguồn về máy để sửa

```bash
git clone https://github.com/HOAHCE/HOAHCE.github.io.git
cd HOAHCE.github.io
```

Sửa xong thì đẩy lên:

```bash
git add .
git commit -m "Cap nhat noi dung"
git push
```

Với thay đổi nhỏ, anh có thể sửa thẳng trên giao diện web của GitHub
(mở file → biểu tượng bút chì → sửa → *Commit changes*), không cần dùng terminal.

---

## 2. Cập nhật nội dung hằng ngày

Với thay đổi nhỏ, anh có thể sửa **trực tiếp trên giao diện web của GitHub**
(mở file → biểu tượng bút chì → sửa → *Commit changes*). Không cần dùng terminal.

### 2.1. Thêm một bài blog

Tạo file mới trong `_posts/` theo đúng mẫu tên `NĂM-THÁNG-NGÀY-tieu-de.md`:

```markdown
---
layout: post
title: Tiêu đề bài viết
date: 2025-11-20 09:00:00 +0700
description: Mô tả ngắn hiện ở danh sách bài viết.
tags: [du-bao, python]
categories: [nghien-cuu]
---

Nội dung viết bằng Markdown.
```

- `tags` và `categories` nên viết **không dấu**, vì chúng trở thành đường dẫn
  (`/blog/tag/du-bao/`). Muốn chúng hiện ở đầu trang blog thì khai báo thêm trong
  `_config.yml` (mục `display_tags`, `display_categories`).
- Chèn ảnh: đặt ảnh vào `assets/img/`, rồi dùng `![Chú thích](/assets/img/ten-anh.jpg)`.

### 2.2. Thêm một công bố khoa học

Danh mục chia làm **hai file**:

| File | Chứa gì |
| --- | --- |
| `_bibliography/papers.bib` | Bài báo tạp chí và báo cáo hội thảo |
| `_bibliography/books.bib` | Sách và giáo trình |

Cách viết **tên tác giả** — quan trọng, viết sai là tên người ta bị đảo lộn.

**Nguyên tắc: bài viết bằng ngôn ngữ nào thì ghi tên theo ngôn ngữ đó.**

Bài tiếng Anh:

```bibtex
author = {Tran Thai, Hoa and {Thanh Manh Le} and {Cuong Hoa Nguyen-Dinh}}
```

Bài tiếng Việt:

```bibtex
author = {Thái Hòa, Trần and {Lê Mạnh Thạnh} and {Nguyễn Đình Hoa Cương}}
```

- **Đồng tác giả** luôn bọc trong dấu `{}` để giữ nguyên thứ tự họ-tên.
  Không bọc thì BibTeX đảo thành "Thạnh Lê Mạnh", sai tên người ta.
- **Tên anh** thì ngược lại: viết dạng `Họ, Tên` và **không bọc `{}`**, vì theme chỉ
  in đậm được tên tác giả khi tách rời được phần họ và phần tên riêng.
  Hai giá trị đó phải khớp với `scholar.last_name` và `scholar.first_name`
  trong `_config.yml`:

  | | Viết trong .bib | Hiện ra trên trang |
  | --- | --- | --- |
  | Bài tiếng Anh | `Tran Thai, Hoa` | Hoa Tran Thai |
  | Bài tiếng Việt | `Thái Hòa, Trần` | Trần Thái Hòa |

Bảng chuyển tên đồng tác giả sang dạng Latin nằm ở đầu file `papers.bib`.

**Tình trạng công bố:**

- Bài **đã xuất bản và có DOI** — thêm hai dòng, tên bài sẽ có nút bấm sang bài gốc:

  ```bibtex
    doi    = {10.1007/s41870-025-02472-6},
    html   = {https://doi.org/10.1007/s41870-025-02472-6},
  ```

- Bài **đã nhận đăng nhưng chưa xuất bản** — thêm một dòng:

  ```bibtex
    status = {Accepted},
  ```

  Trang sẽ hiện nhãn *Accepted* viền xanh bên cạnh tên tạp chí. Khi bài lên trang
  chính thức thì **xoá dòng `status`** và điền `volume`, `number`, `pages`, `doi`.


Mở `_bibliography/papers.bib`, dán thêm một khối BibTeX (lấy sẵn từ nút "Cite" trên
Google Scholar hoặc trang tạp chí):

```bibtex
@article{tran2025forecasting,
  abbr        = {JTR},
  title       = {Tên bài báo},
  author      = {Tran, Thai Hoa and Nguyen, Van A},
  journal     = {Tên tạp chí},
  volume      = {12},
  number      = {3},
  pages       = {45--60},
  year        = {2025},
  doi         = {10.xxxx/yyyy},
  html        = {https://doi.org/10.xxxx/yyyy},
  pdf         = {ten-file.pdf},
  selected    = {true},
  bibtex_show = {true},
  abstract    = {Tóm tắt bài báo.}
}
```

- Trang **research** tự sắp xếp theo năm, tự in đậm tên anh.
- `selected = {true}` → bài đó hiện luôn ở trang chủ.
- `pdf = {ten-file.pdf}` → đặt file trong `assets/pdf/`.

### 2.3. Thêm một đề tài nghiên cứu

Mở `_data/grants.yml`, thêm một khối `- title:` mới theo đúng mẫu có sẵn.
Không cần sửa code trang.

### 2.4. Thêm một học phần

Tạo file mới trong `_teachings/`, ví dụ `_teachings/phan-tich-du-lieu.md`.
Chép nguyên cấu trúc của `_teachings/kinh-te-luong.md` rồi sửa lại.
Mỗi buổi học là một khối `- week:`; mỗi tài liệu là một dòng trong `materials`.

### 2.5. Tải tài liệu bài giảng lên

Đặt file PDF/slide vào thư mục `assets/pdf/`, rồi trỏ đường dẫn:

```yaml
materials:
  - name: Slide chương 3
    url: /assets/pdf/slide-chuong-3.pdf
```

> GitHub giới hạn 100 MB mỗi file. Với video hoặc file rất lớn, nên để trên
> Google Drive / OneDrive và dán link vào `url`.

### 2.6. Thêm một dự án

Tạo file mới trong `_projects/`, chép cấu trúc từ `_projects/1_hue_heritage.md`.
`importance` quyết định thứ tự hiển thị (số nhỏ đứng trước), `category` phải nằm trong
danh sách `display_categories` khai báo ở đầu `_pages/projects.md`.

### 2.7. Thêm một tin ngắn (hiện ở trang chủ)

Tạo file trong `_news/`, ví dụ `_news/2025-11-20-hoi-thao.md`:

```markdown
---
layout: post
date: 2025-11-20
inline: true
related_posts: false
---

Báo cáo tại Hội thảo ABC, Đà Nẵng.
```

### 2.8. Sửa thông tin chung / CV

| Muốn sửa gì | Mở file nào |
| --- | --- |
| Banner + giới thiệu + học tập + liên hệ (tiếng Anh) | `_pages/about.md` |
| Banner + giới thiệu + học tập + liên hệ (tiếng Việt) | `_pages/vi/about.md` |
| Ảnh chân dung | thay file `assets/img/prof_pic.png` (ảnh đã tách nền) |
| Liên kết ORCID / Scholar / GitHub / email | `_data/socials.yml` |
| Tên hiển thị, tên miền, chân trang | `_config.yml` |

> Phần **Học tập** nằm ngay trong trang giới thiệu, không dùng trang CV riêng.
> Trang `/cv/` hiện đang tắt khỏi menu (`nav: false` trong `_pages/cv.md`);
> muốn dùng lại thì bật `nav: true` và điền `_data/cv.yml`.

---

## 3. Cấu trúc song ngữ

Trang có hai phiên bản **tách bạch hoàn toàn**, mỗi bên một ngôn ngữ:

| | Tiếng Anh | Tiếng Việt |
| --- | --- | --- |
| Trang chủ | `/` | `/vi/` |
| Nghiên cứu | `/publications/` | `/vi/nghien-cuu/` |
| Giảng dạy | `/teaching/` | `/vi/giang-day/` |
| Dự án | `/projects/` | `/vi/du-an/` |
| Blog | `/blog/` (dùng chung — chữ "Blog" giống nhau ở cả hai thứ tiếng) | |

**Nút chuyển ngôn ngữ EN / VI** nằm ở góc trên bên phải, cạnh ô tìm kiếm. Mỗi trang
tự khai báo trang tương ứng bên kia trong front matter:

```yaml
lang: vi              # ngôn ngữ của trang: en hoặc vi
lang_alt: /teaching/  # địa chỉ trang cùng nội dung ở ngôn ngữ kia
nav: true             # có hiện trên menu không
nav_order: 2          # thứ tự trên menu
nav_title: Nghiên cứu # tên ngắn hiện trên menu (nếu muốn khác tiêu đề trang)
```

Thanh menu **tự lọc theo ngôn ngữ**: trang tiếng Việt chỉ hiện các mục tiếng Việt và
ngược lại. Trang muốn hiện ở cả hai menu (như Blog) khai báo thêm `nav_lang: both`.

### Nội dung dùng chung cho hai ngôn ngữ

Học phần, dự án và tin ngắn chỉ lưu **một bản**, kèm phần dịch, nên không phải nhập
hai lần:

| Nội dung | Bản tiếng Việt | Bản tiếng Anh |
| --- | --- | --- |
| Học phần (`_teachings/`) | `title`, `description`, `term` | `title_en`, `description_en`, `term_en` |
| Dự án (`_projects/`) | `description_vi` | `title`, `description` |
| Tin ngắn (`_news/`) | phần thân bài | `text_en` |
| Đề tài (`_data/grants.yml`) | `title_vi`, `level_vi`, `role_vi`… | `title`, `level`, `role`… |

Thiếu bản dịch thì trang tự dùng bản còn lại, không bị trống.

**Hai chỗ cố tình giữ nguyên ngữ:**

- **Tên các công bố** trong `_bibliography/papers.bib` giữ đúng ngôn ngữ gốc của bài
  báo — đây là thông lệ học thuật, dịch tên bài báo đã xuất bản là sai.
- **Trang chi tiết từng học phần** (`/teachings/...`) viết bằng tiếng Việt, vì đó là
  tài liệu cho sinh viên. Muốn dịch sang tiếng Anh thì sửa trực tiếp trong
  `_teachings/*.md`.

---

## 4. Các file tuỳ biến theme

Theme al-folio được cài dưới dạng thư viện (gem), nên gần như toàn bộ giao diện nằm
ngoài repo này. Trong repo chỉ có mấy file sau là phần tuỳ biến:

| File | Việc của nó |
| --- | --- |
| `_sass/_custom.scss` | Bảng màu, font, kiểu dáng banner và nút chuyển ngôn ngữ — **file cần sửa khi muốn đổi giao diện** |
| `_includes/hero.liquid` | Khung HTML của banner trang chủ |
| `assets/css/main.scss` | Bản sao file gốc của theme, chỉ thêm dòng `@use "custom"` ở cuối |
| `_includes/header.liquid` | Bản sao; sửa để menu hiện theo ngôn ngữ, thêm nút EN/VI |
| `_includes/footer.liquid` | Bản sao; sửa để tên và câu bản quyền hiện theo ngôn ngữ |
| `_includes/courses.liquid` | Bản sao; sửa để tên học phần hiện theo ngôn ngữ |
| `_includes/projects.liquid` | Bản sao; sửa để mô tả dự án hiện theo ngôn ngữ |
| `_includes/news.liquid` | Bản sao; sửa để tin ngắn hiện theo ngôn ngữ |
| `_layouts/about.liquid` | Bản sao; sửa để bật banner và dịch được 3 tiêu đề mục |
| `_includes/hook/bib.liquid` | **Không phải bản sao** — đây là điểm mở rộng theme cung cấp sẵn; dùng để in thêm nhà xuất bản của sách, tập(số), trang và vai trò tác giả |

Mỗi file bản sao đều có ghi chú ở đầu nói rõ đã sửa những gì. Khi nâng cấp theme,
nếu giao diện có chỗ lạ thì chép lại file tương ứng từ bản mới của gem rồi áp dụng
lại đúng phần sửa đã ghi chú. Riêng `_custom.scss` và `hero.liquid` là của riêng anh,
không bị ảnh hưởng khi nâng cấp.

---

## 5. Sửa banner trang chủ

Toàn bộ chữ trên banner nằm trong khối `hero:` ở đầu file
`_pages/about.md` (bản tiếng Anh) và `_pages/vi/about.md` (bản tiếng Việt):

```yaml
hero:
  badge: Xin chào!                  # nhãn nhỏ phía trên
  title: Tôi là Trần Thái Hòa       # dòng tên
  role: Nghiên cứu sinh &amp; Giảng viên  # dòng thứ hai, chữ mảnh hơn
  description: >                    # đoạn mô tả ngắn
    ...
  image: prof_pic.jpg               # ảnh trong assets/img/
  buttons:                          # các nút bấm
    - label: Nghiên cứu khoa học
      url: /vi/nghien-cuu/
      style: primary                # primary = nút đậm, ghost = nút viền
  links:                            # hàng liên kết cuối banner
    - label: Google Scholar
      url: https://...
```

Thêm nút hay liên kết = thêm một dòng `- label:` nữa. Muốn **bỏ banner** và quay lại
kiểu ảnh đại diện bên phải như cũ thì xoá cả khối `hero:` — khối `profile:` bên dưới
sẽ tự được dùng lại.

Đổi màu nền banner: sửa `$hero-from` và `$hero-to` trong `_sass/_custom.scss`.

> Lưu ý dấu `&` trong YAML: viết `&amp;` như ví dụ trên, đừng viết `&` trần.

---

## 6. Đổi màu và font

Toàn bộ màu sắc và font nằm trong **một file duy nhất**: `_sass/_custom.scss`.

- Đổi **màu chủ đạo** (màu của liên kết, tiêu đề mục): sửa `$c-blue` cho nền sáng và
  `$c-blue-dark` cho nền tối.
- Đổi **font**: sửa biến `$font-main`, rồi cập nhật đường dẫn Google Fonts trong
  `_config.yml` (mục `third_party_libraries.google_fonts.url.fonts`) cho khớp.

> **Quan trọng với font tiếng Việt:** đường dẫn Google Fonts phải có
> `&subset=latin,latin-ext,vietnamese`. Thiếu tham số này, Google chỉ trả về bộ ký tự
> latin và mọi chữ có dấu sẽ rơi sang font hệ thống, nhìn lệch hẳn so với phần còn lại.
> Cũng nên kiểm tra font mới có hỗ trợ tiếng Việt hay không trước khi đổi — nhiều font
> monospace phổ biến thì không.

Trang có sẵn chế độ nền sáng / nền tối; nút chuyển nằm ở góc trên bên phải.

### Đổi favicon (biểu tượng hiện ở thanh tab trình duyệt)

Favicon hiện dùng logo Trường Đại học Kinh tế, Đại học Huế. Có hai file trong
`assets/img/`:

| File | Dùng ở đâu | Yêu cầu |
|---|---|---|
| `favicon.ico` | thanh tab trình duyệt, bookmark | file .ico chứa sẵn 4 cỡ 16/32/48/64 px |
| `apple-touch-icon.png` | khi lưu trang ra màn hình chính iPhone/iPad | PNG **180x180**, **nền trắng** (iOS không hiểu nền trong suốt, sẽ tô đen) |

Hai file này được khai báo ở `_config.yml`, dòng `icon:` và `apple_touch_icon:`.
Muốn thay logo khác thì tạo lại hai file đúng tên, đúng kích thước rồi ghi đè —
không phải sửa gì trong `_config.yml`.

Nếu chỉ muốn quay lại dùng emoji cho nhanh, sửa `icon: favicon.ico` thành một emoji
bất kỳ (ví dụ `icon: 🏯`) và để trống `apple_touch_icon:`.

> Lưu ý về cỡ 16 px: logo dạng con dấu tròn có vòng chữ bao quanh thì ở 16 px vòng chữ
> chắc chắn nhoè, chỉ còn nhận ra vòng xanh và mái vòm vàng — đây là giới hạn chung của
> mọi logo dạng này, không phải lỗi file. Màn hình độ phân giải cao dùng cỡ 32 px nên
> nhìn rõ chữ HUE.

---

## 7. Xem thử trên máy trước khi đẩy lên (tuỳ chọn)

Không bắt buộc, nhưng tiện khi sửa nhiều. Cần Ruby ≥ 3.0:

```bash
bundle install
bundle exec jekyll serve
```

Rồi mở http://localhost:4000. Cách khác, nếu có Docker:

```bash
docker compose up
```

---

## 8. Danh sách việc cần làm (TODO)

Tất cả chỗ cần điền đều được đánh dấu `TODO` trong mã nguồn. Tìm nhanh bằng lệnh:

```bash
grep -rn "TODO" _pages _data _projects _teachings _posts _news _bibliography _config.yml
```

Đã xong:

- [x] Tên miền `tranthaihoa.id.vn` (`_config.yml` + `CNAME`)
- [x] Banner trang chủ kiểu tối giản, ảnh tròn nền trùng màu banner
- [x] Hai phiên bản Anh / Việt tách bạch, có nút chuyển EN–VI ở góc trên phải
- [x] Tên hiển thị: **Trần Thái Hòa** (tiếng Việt) / **Hoa Tran Thai** (tiếng Anh)
- [x] Font Inter, bảng màu xám nhạt – đen – xanh nhạt
- [x] Quá trình học tập (cử nhân, thạc sĩ, nghiên cứu sinh)
- [x] **22 bài báo & kỷ yếu** (2013–2026), **8 sách, giáo trình**, **11 đề tài**
      — lấy từ ba file kết xuất CSDL Khoa học ĐH Huế, đủ đồng tác giả và tập/số/trang
- [x] Tên tác giả thống nhất theo ngôn ngữ từng bài (Anh / Việt)
- [x] 5 học phần giảng dạy

Còn lại:

- [ ] **Bốn bài đang để nhãn `Accepted`** — tôi suy ra từ chỗ file CSDL bỏ trống
      tập/số/trang, trong khi LNNS và LNICST là tùng thư Springer vốn luôn có số
      trang khi đã in. Bài nào thực ra đã xuất bản thì xoá dòng `status` và điền
      số trang.
- [ ] **DOI — mới có 1/22 bài** (bài IJIT 2025). Crossref, doi.org và OpenAlex
      đều bị chặn trong môi trường tôi làm việc nên không tra tự động được.
- [ ] **Nhà xuất bản ghi sai trong file CSDL** — bài CCIS 2026 ghi "Marcel Dekker
      Inc." và bài LNICST 2026 ghi "Nickan Research Institute", trong khi cả hai
      tùng thư đều của Springer. Tôi để trống thay vì chép lại.
- [ ] **Vài bài chỉ có trang bắt đầu** (GRU 2026 trang 15, ba bài logistics 2023,
      ICYREB 2025 trang 379). Bổ sung trang kết thúc.
- [ ] **Bảng chuyển tên đồng tác giả sang dạng Latin** ở đầu `papers.bib` — ba tên
      đối chiếu được với Springer và Scholar, phần còn lại suy theo cùng quy tắc.
      Anh đối chiếu với bản in thật của từng bài.
- [ ] `_teachings/*.md` — khi vào học kỳ thì mở phần `term`, `location`, `time`,
      `schedule` (đang để dạng ghi chú) và tải slide/đề cương lên `assets/pdf/`
- [ ] `_data/resources.yml` — thay bằng tài liệu thật, hoặc xoá hết nội dung
- [ ] `_news/2025-10-02-cong-bo-moi.md` và `_posts/2025-10-01-chao-mung.md` — mẫu, sửa hoặc xoá
- [ ] `_projects/*.md` — viết mô tả chi tiết cho 3 dự án GitHub
- [ ] `assets/pdf/example_pdf.pdf` — xoá sau khi đã thay bằng tài liệu thật

---

## 9. Khi gặp lỗi

- Vào tab **Actions** trên GitHub, mở job *Deploy site* gần nhất để xem log lỗi.
- Lỗi hay gặp nhất là **sai thụt lề trong file YAML** (`.yml` và phần đầu file `.md`).
  YAML dùng dấu cách, **không dùng tab**.
- Tài liệu gốc của theme nằm trong thư mục `docs/`, đặc biệt là `docs/FAQ.md`
  và `docs/TROUBLESHOOTING.md`.
