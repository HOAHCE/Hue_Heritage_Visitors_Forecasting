# Hướng dẫn sử dụng trang cá nhân

Trang được xây trên **Jekyll + theme [al-folio](https://github.com/alshedivat/al-folio)**,
host miễn phí trên **GitHub Pages**. Anh không cần cài gì trên máy: mỗi lần sửa nội dung và
đẩy (push) lên GitHub, GitHub Actions sẽ tự build lại và trang cập nhật sau 2–4 phút.

---

## 1. Đưa trang lên mạng lần đầu

### Bước 1 — Tạo repository

Vào https://github.com/new và tạo repo với tên **chính xác** là:

```
HOAHCE.github.io
```

Chọn **Public**, **không** tích "Add a README file".

### Bước 2 — Đẩy mã nguồn lên

Trên máy anh, mở terminal (hoặc Git Bash trên Windows):

```bash
# Lấy mã nguồn về (thư mục personal-website trong repo hiện tại)
git clone -b claude/compassionate-ride-mdc6s1 \
  https://github.com/HOAHCE/Hue_Heritage_Visitors_Forecasting.git tam
cp -r tam/personal-website HOAHCE.github.io
rm -rf tam

cd HOAHCE.github.io
git init
git add .
git commit -m "Khoi tao trang ca nhan"
git branch -M main
git remote add origin https://github.com/HOAHCE/HOAHCE.github.io.git
git push -u origin main
```

### Bước 3 — Bật GitHub Pages

Vào repo `HOAHCE.github.io` → **Settings** → **Pages** → mục **Build and deployment**:

- **Source**: `Deploy from a branch`
- **Branch**: `gh-pages` / thư mục `/ (root)` → **Save**

Nhánh `gh-pages` được workflow tự tạo sau lần build đầu tiên. Nếu chưa thấy, vào tab
**Actions** đợi job *Deploy site* chạy xong rồi quay lại.

Sau đó trang chạy tại `https://HOAHCE.github.io`.

### Bước 4 — Trỏ tên miền `tranthaihoa.id.vn`

Mã nguồn đã có sẵn file `CNAME` chứa `tranthaihoa.id.vn`, và `_config.yml` đã đặt
`url: https://tranthaihoa.id.vn`. Chỉ còn hai việc:

**a) Tại Tenten.vn** — đăng nhập https://tenten.vn → **Quản lý tên miền** → chọn
`tranthaihoa.id.vn` → **Quản lý DNS** (hoặc *Cấu hình bản ghi DNS*). Xoá các bản ghi A
đang trỏ tới trang chờ (parking) của Tenten, rồi thêm:

| Loại | Host / Tên | Giá trị / Trỏ tới | TTL |
| --- | --- | --- | --- |
| A | `@` | `185.199.108.153` | mặc định |
| A | `@` | `185.199.109.153` | mặc định |
| A | `@` | `185.199.110.153` | mặc định |
| A | `@` | `185.199.111.153` | mặc định |
| CNAME | `www` | `hoahce.github.io.` | mặc định |

> Cả 4 bản ghi A đều dùng host `@` — đó là cách GitHub cân bằng tải, không phải khai trùng.
> Nếu ô Host của Tenten không nhận ký tự `@`, hãy để **trống** hoặc điền
> `tranthaihoa.id.vn`. Giá trị CNAME nhớ giữ **dấu chấm ở cuối**.

**b) Tại GitHub** — repo `HOAHCE.github.io` → **Settings** → **Pages** →
mục **Custom domain** điền `tranthaihoa.id.vn` → **Save**.

Đợi DNS lan truyền (thường 15–60 phút, tối đa 24 giờ). Khi GitHub báo
*DNS check successful*, quay lại tích **Enforce HTTPS** để trang chạy `https://`.
Chứng chỉ SSL do GitHub cấp miễn phí, tự gia hạn.

Kiểm tra DNS đã cập nhật chưa:

```bash
nslookup tranthaihoa.id.vn
```

Kết quả đúng là 4 địa chỉ `185.199.10x.153` ở trên.

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
| Giới thiệu bản thân (bản tiếng Anh) | `_pages/about.md` |
| Giới thiệu bản thân (bản tiếng Việt) | `_pages/vi/about.md` |
| Ảnh chân dung | thay file `assets/img/prof_pic.jpg` |
| Học tập, kinh nghiệm, kỹ năng (trang CV) | `_data/cv.yml` |
| Liên kết ORCID / Scholar / GitHub / email | `_data/socials.yml` |
| Tên trang, tên miền, tiêu đề blog | `_config.yml` |

---

## 3. Cấu trúc song ngữ

- **Tiếng Anh** là bản mặc định, nằm ở đường dẫn gốc: `/`, `/publications/`, `/teaching/`, `/projects/`.
- **Tiếng Việt** nằm dưới `/vi/`: `/vi/`, `/vi/nghien-cuu/`, `/vi/giang-day/`, `/vi/du-an/`.
- Nút **"Tiếng Việt"** trên thanh menu dẫn sang bản tiếng Việt; trong bản tiếng Việt có
  thanh điều hướng riêng kèm nút **English** để quay lại.
- Blog, CV và danh mục công bố **dùng chung** cho cả hai ngôn ngữ (mỗi bài viết tự nhiên
  đã ở một ngôn ngữ), nên anh không phải viết hai lần.
- Muốn sửa thanh điều hướng tiếng Việt: mở `_includes/vi_nav.liquid`.
- Muốn thêm một trang tiếng Việt mới: tạo file trong `_pages/vi/`, đặt `permalink: /vi/ten-trang/`,
  `nav: false`, và thêm `{% raw %}{% include vi_nav.liquid %}{% endraw %}` ở đầu nội dung.

---

## 3b. Ba file ghi đè theme

Theme al-folio được cài dưới dạng thư viện (gem), nên gần như toàn bộ giao diện nằm
ngoài repo này. Chỉ có **ba file** trong repo là bản ghi đè:

| File | Ghi đè cái gì |
| --- | --- |
| `assets/css/main.scss` | Bản sao file gốc của theme, chỉ thêm dòng `@use "custom"` ở cuối |
| `_sass/_custom.scss` | Bảng màu và font riêng — **đây là file cần sửa khi muốn đổi giao diện** |
| `_layouts/about.liquid` | Bản sao layout trang chủ, sửa để ba tiêu đề mục dịch được sang tiếng Việt |

Khi nâng cấp theme lên phiên bản mới, nếu giao diện có chỗ lạ thì chép lại hai file
`main.scss` và `about.liquid` từ bản mới của theme rồi áp dụng lại đúng phần sửa nói trên.
File `_custom.scss` là của riêng anh, không cần đụng tới.

---

## 3c. Đổi màu và font

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

---

## 4. Xem thử trên máy trước khi đẩy lên (tuỳ chọn)

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

## 5. Danh sách việc cần làm (TODO)

Tất cả chỗ cần điền đều được đánh dấu `TODO` trong mã nguồn. Tìm nhanh bằng lệnh:

```bash
grep -rn "TODO" _pages _data _projects _teachings _posts _news _bibliography _config.yml
```

Đã xong:

- [x] `_config.yml` — `url` đã trỏ về `https://tranthaihoa.id.vn`
- [x] `CNAME` — đã tạo
- [x] `assets/img/prof_pic.jpg` — đã dùng ảnh chân dung của anh
- [x] `_data/socials.yml` — email `tranthaihoa@hueuni.edu.vn`
- [x] Phần giới thiệu ở `_pages/about.md` và `_pages/vi/about.md`
- [x] Bảng màu xám nhạt – đen – xanh nhạt, chữ monospace (`_sass/_custom.scss`)

Còn lại:

- [ ] `_bibliography/papers.bib` — thay khối `TODO_thay_bang_cong_bo_that` bằng danh mục
      công bố thật (lấy từ CSDL Khoa học Đại học Huế)
- [ ] `_data/cv.yml` — điền các bậc học (nghiên cứu sinh, thạc sĩ, cử nhân) và năm công tác
- [ ] `_data/grants.yml` — thay ví dụ bằng đề tài thật
- [ ] `_teachings/kinh-te-luong.md` — sửa thành học phần thật, hoặc xoá
- [ ] `_data/resources.yml` — thay bằng tài liệu thật, hoặc xoá hết nội dung
- [ ] `_news/2025-10-02-tai-lieu-hoc-phan.md` — sửa hoặc xoá
- [ ] `_posts/2025-10-01-chao-mung.md` — sửa hoặc xoá
- [ ] `assets/pdf/example_pdf.pdf` — xoá sau khi đã thay bằng tài liệu thật

---

## 6. Khi gặp lỗi

- Vào tab **Actions** trên GitHub, mở job *Deploy site* gần nhất để xem log lỗi.
- Lỗi hay gặp nhất là **sai thụt lề trong file YAML** (`.yml` và phần đầu file `.md`).
  YAML dùng dấu cách, **không dùng tab**.
- Tài liệu gốc của theme nằm trong thư mục `docs/`, đặc biệt là `docs/FAQ.md`
  và `docs/TROUBLESHOOTING.md`.
