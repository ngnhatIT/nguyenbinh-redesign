# SPEC: v9.html — tinh chỉnh mobile trang home (từ v5.html)

## Goal

Tạo `v9.html` bằng cách **copy nguyên `v5.html`** (không sửa v5 — trang live), rồi
tinh chỉnh trang home:

1. **Mobile — mục "Dự án tiêu biểu"**: bỏ slideshow trên mobile, thay bằng 5 card
   full-ảnh xếp dọc kiểu PICO (ảnh nền phủ card, tiêu đề trắng đè lên đáy ảnh,
   nút CTA pill bên dưới tiêu đề). **PC giữ nguyên slideshow hiện tại.**
2. **Mobile — 4 card dịch vụ (`.scard`)**: description gọn hơn (font nhỏ hơn,
   clamp 2 dòng), **ẩn hoàn toàn hàng tag** ("Chuẩn SECC", "Chống dột 100%", …).
   Chỉ áp dụng trên mobile — PC giữ nguyên.
3. **Cả PC + Mobile — màu 4 card dịch vụ**: 4 sắc độ xanh dương nhạt → hơi đậm dần
   theo palette sẵn có, chữ vẫn tối. KHÔNG dùng nền navy đậm chữ trắng như v4.
4. **Menu mobile xổ xuống**: nền đỡ trắng hơn (tint xanh nhạt).
5. **Animation nhẹ nhàng, chuyên nghiệp** bổ sung trên nền hệ reveal sẵn có.

Kết quả quan sát được: mở `v9.html` ở width mobile (~390px) thấy dự án tiêu biểu
là 5 card dọc kiểu PICO, card dịch vụ gọn không còn tag, 4 card có 4 sắc xanh phân
biệt nhẹ, menu burger mở ra nền xanh nhạt; ở desktop mọi thứ giống v5 trừ màu card
dịch vụ và animation mượt hơn.

## Bối cảnh v5.html (từ khảo sát — line number theo v5.html)

- Single-file SPA ~2300 dòng, hash router, các section render bằng JS inline.
- `:root` (dòng 25–58): `--bg #FFFFFF`, `--bg2 #EDF2F9`, `--navy #1B2F8A`,
  `--sky #2E9BD6`, `--skyd #1479B8`, `--sky3 #8FCBEF`, `--ink #0E1A36`,
  `--mut #54648A`, `--line`, `--e: cubic-bezier(.22,1,.36,1)`, v.v.
- **Dự án tiêu biểu** (markup dòng 998–1024): `.show` slideshow — `.show-slides`
  (`#show-slides`, JS đổ `.sslide`, một cái `.on` tại một thời điểm, dòng ~2075),
  `.show-bar` progress (`@keyframes sbar`), `.show-count`, `.show-info`, `.show-nav`
  prev/next, swipe handler dòng ~2094. CSS dòng 363–392; breakpoint mobile của mục
  này là `@media(max-width:768px)` dòng ~389.
- **Dịch vụ** (markup dòng 983–995): `.stack-zone#home-services` được JS đổ card
  qua hàm `svcCard(key)` (dòng 1971–1983), data trong object `SERVICES` (dòng 1734+,
  keys: `su-kien`, `trien-lam`, `kho-tam`, `may-lanh`). Cấu trúc card:
  `article.scard > .info (.num, h3, p, .tags > span.tag, a.btn) + .media > img`.
  - `.scard p`: `font-size:15.5px; line-height:1.75` (dòng ~346).
  - `.tag`: pill 12px (dòng ~348).
  - Nền card hiện tại: gradient trắng→xanh rất nhạt, phân biệt bằng `:nth-child(2/3/4)`
    (dòng ~340, 354–359) — đã có sẵn cơ chế nth-child để đổi màu.
  - Breakpoint mobile của mục này: `@media(max-width:860px)` dòng ~360.
  - **Lưu ý**: `svcCard()` và markup `.scard` được tái dùng cho trang `/dich-vu`
    (`#all-services`, dòng ~1351) — thay đổi CSS `.scard` sẽ ảnh hưởng cả trang đó;
    chấp nhận (đồng bộ là tốt). Mega-menu `.mg-it` là markup khác, không đụng.
- **Menu mobile**: chính `ul.nav#nav` biến thành drawer tại `@media(max-width:1180px)`
  (dòng ~235), `background:var(--bg)` = trắng đặc, toggle class `.open` bởi `#burger`
  (dòng ~2245).
- **Animation sẵn có**: IntersectionObserver `io` (dòng ~2211) bật `.in` trên
  `.reveal` / `.reveal.stagger`; keyframes `grain, blink, pulse2, spin, sbar, sup,
  pgin, tmarq`; hover scale ảnh `.scard`. Mobile đã có block giảm blur/rút ngắn
  transition (~dòng 650–664).

## Files & interfaces

- **Tạo mới**: `v9.html` (copy từ `v5.html`, sau đó sửa trong file này).
- Không sửa file nào khác. Không thêm asset — dùng lại ảnh dự án đã có trong data JS.

## Approach

### 0. Tạo file

```bash
cp v5.html v9.html
```

Đổi `<title>` nếu có hậu tố version (kiểm tra và đồng bộ như cách các v khác làm).

### 1. Card dự án kiểu PICO trên mobile

Cách ít xâm lấn nhất: **không đụng slideshow/JS hiện có**. Thêm một container mới
`#prj-cards` (class `.prj-cards`) ngay sau `.show` trong markup section dự án
(dòng ~998–1024), đổ card bằng chính mảng data dự án mà slideshow đang dùng
(cùng vòng render JS đổ `#show-slides` — thêm vài dòng đổ luôn `#prj-cards`).

Card PICO (theo ảnh mẫu):

```html
<a class="pcard reveal" href="#/du-an">
  <img src="..." alt="..." loading="lazy">
  <div class="pcard-info">
    <h3>Tên dự án</h3>
    <span class="btn btn--sky">Xem dự án →</span>
  </div>
</a>
```

CSS:
- `.pcard`: `position:relative; border-radius:var(--r); overflow:hidden;
  aspect-ratio:3/4` (dọc như PICO), gradient overlay tối dần về đáy
  (`linear-gradient(180deg,transparent 40%,rgba(14,26,54,.78))` — dùng tông `--ink`),
  h3 trắng đậm, nút pill `--sky`.
- Hiển thị responsive: mặc định `.prj-cards{display:none}`; trong
  `@media(max-width:768px)` (cùng breakpoint mục này): `.show{display:none}`,
  `.prj-cards{display:grid; gap:16px}`.
- 5 card xếp dọc, mỗi card 1 cột.

### 2. Card dịch vụ gọn trên mobile

Trong block `@media(max-width:860px)` sẵn có (dòng ~360), thêm:

```css
.scard p{font-size:13.5px;line-height:1.6;display:-webkit-box;
  -webkit-line-clamp:2;-webkit-box-orient:vertical;overflow:hidden;margin:10px 0 18px}
.scard .tags{display:none}
```

Chỉ CSS — không sửa data `SERVICES` hay `svcCard()` (PC vẫn cần tags).

### 3. Màu 4 card dịch vụ (PC + Mobile)

Sửa các rule `:nth-child` sẵn có của `.scard` (dòng ~354–359): 4 sắc độ xanh nhạt
đậm dần, chữ giữ nguyên tối. Gợi ý thang (tinh chỉnh bằng mắt khi xem browser):

| Card | Gradient nền (135deg) | Viền |
| --- | --- | --- |
| 1 (sự kiện) | `#FFFFFF → #EAF4FC` | `rgba(46,155,214,.14)` |
| 2 (triển lãm) | `#FAFDFF → #DEEDF9` | `rgba(46,155,214,.20)` |
| 3 (kho tạm) | `#F6FBFF → #D2E6F6` | `rgba(46,155,214,.26)` |
| 4 (máy lạnh) | `#F2F9FF → #C6DFF3` | `rgba(46,155,214,.32)` |

Tiêu chí "không đậm như v4": card đậm nhất vẫn phải là nền sáng, text `--txt`/`--mut`
đạt contrast; tuyệt đối không nền navy chữ trắng.

### 4. Nền menu mobile

Tại rule `.nav` trong `@media(max-width:1180px)` (dòng ~235): đổi
`background:var(--bg)` → `background:var(--bg2)` (`#EDF2F9`) hoặc gradient
`linear-gradient(180deg,#F4F8FC,#E7EFF8)`. Vẫn đặc (không blur — giữ hành vi
`backdrop-filter:none` sẵn có cho perf mobile).

### 5. Animation nhẹ

Tận dụng hệ `.reveal` + `io` observer sẵn có — KHÔNG thêm thư viện:

- Gắn class `reveal` (+ stagger delay qua `transition-delay` nth-child) cho
  `.pcard` mới và các `.scard` nếu chưa có.
- `.scard` hover (PC): nâng nhẹ `translateY(-4px)` + shadow `--sh-md`,
  transition `.45s var(--e)` — hiện chỉ có scale ảnh.
- `.pcard` active (mobile tap): `transform:scale(.98)` transition ngắn.
- Nút CTA hover: hiệu ứng sẵn có của `.btn`, không thêm.
- Menu mobile mở: drawer đã có `transform:translateX(100%)` transition — thêm
  stagger fade-in cho các `li` (delay nth-child 40ms) là đủ.
- Tôn trọng block giảm-motion mobile sẵn có (dòng ~650–664): mọi hiệu ứng mới trên
  mobile chỉ dùng opacity/transform, không blur, thời lượng ≤ .5s.
- Thêm `@media(prefers-reduced-motion:reduce)` tắt transition mới nếu v5 chưa có.

## Out of scope

- KHÔNG sửa `v5.html` hay bất kỳ version cũ nào.
- KHÔNG đổi slideshow dự án trên desktop.
- KHÔNG sửa nội dung tiếng Việt, số điện thoại `0937 327 777`, link Zalo, Maps.
- KHÔNG đổi data `SERVICES` (tags vẫn tồn tại, chỉ ẩn bằng CSS trên mobile).
- KHÔNG đụng mega-menu `.mg-it`, các trang con (`/dich-vu`, `/du-an`, …) ngoài
  hiệu ứng phụ chấp nhận được từ CSS `.scard` dùng chung.
- KHÔNG thêm thư viện animation, framework, file CSS/JS ngoài — giữ single-file.

## Verification

Không có test tự động — verify bằng browser (mở `v9.html` trực tiếp):

1. **Mobile 390×844** (DevTools hoặc browser tool, resize mobile):
   - Mục dự án tiêu biểu: 5 card dọc full-ảnh, tiêu đề trắng đè đáy ảnh, nút CTA;
     slideshow không hiện.
   - Card dịch vụ: description tối đa 2 dòng, không còn pill tag nào.
   - Mở menu burger: nền xanh nhạt (không trắng đặc), item fade-in nhẹ.
   - Cuộn trang: reveal mượt, không giật.
2. **Desktop 1280+**:
   - Mục dự án là slideshow y như v5 (progress bar, prev/next, swipe).
   - 4 card dịch vụ: 4 sắc xanh nhạt phân biệt được, chữ tối đọc rõ, tag còn nguyên.
   - Hover card dịch vụ: nâng nhẹ + shadow.
3. **Trang `/dich-vu`**: card `.scard` vẫn render đúng với màu mới (hiệu ứng dùng
   chung CSS).
4. So sánh nhanh v5 vs v9 desktop: ngoài màu card dịch vụ + hover, layout không đổi.
5. Chụp screenshot mobile + desktop làm bằng chứng trước khi `/ship`.
