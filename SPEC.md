# SPEC — v10.html (phát triển từ v9.html)

## Goal

Tạo `v10.html` bằng cách **copy `v9.html`** (không sửa v9 — quy tắc "versions are kept,
not replaced"), rồi thực hiện 6 cải tiến: giảm khoảng trống dọc giữa các section,
band "CAM KẾT NGUYỄN BÌNH" full-bleed, showcase "Công trình nói thay năng lực"
tự chạy với tabs tên dự án (bỏ nút prev/next và label 01/05), card `/du-an` trên
mobile theo phong cách v7, animation mượt/chuyên nghiệp hơn, và mobile menu dịu
màu hơn (gradient sáng pha xanh, không trắng chói). Kết quả là một file HTML
tự chứa duy nhất, tiếng Việt, chạy bằng cách mở trực tiếp trong browser.

Line numbers dưới đây tham chiếu **v9.html** (2342 dòng) — trong v10 chúng là điểm
xuất phát tương ứng.

## Files & interfaces

- **Tạo mới**: `v10.html` — copy từ `v9.html`, mọi thay đổi làm trong file này.
- Không sửa file nào khác. Data objects (`PROJECTS`, `FAQS`…), router, `IMG` giữ nguyên.
- Tham khảo (chỉ đọc): `v7.html:394-413` (CSS `.pcard/.ptag/.pyear/.pbtn`) và
  `v7.html:2003-2013` (hàm `prjCard()`).

## Approach

### 1. Giảm padding dọc section

- `:root` dòng 50: đổi `--sec:clamp(68px,8vw,116px)` → `--sec:clamp(48px,6vw,84px)`.
- Chỉ một chỗ tiêu thụ token: `.sec{padding:var(--sec) 0}` (dòng 100) — không cần sửa gì thêm.
- Kiểm tra các inline override (vd. CEO band dòng 1136 dùng `clamp(48px,6vw,80px)`)
  vẫn hài hoà với nhịp mới; nếu chỗ nào giờ lớn hơn `--sec` mới thì hạ tương ứng.

### 2. Band "CAM KẾT NGUYỄN BÌNH" full-bleed

- Markup hiện tại (dòng 1030-1046): `.sec.sec--line > .wrap > .pledge`.
- **Bỏ `.wrap`** quanh `.pledge` (chuyển `.pledge` thành con trực tiếp của `.sec`).
  KHÔNG sửa class `.wrap` (dòng 99) — nó là chokepoint dùng chung toàn trang.
- Trong CSS `.pledge` (dòng 539): bỏ `border-radius:var(--r)` (full-bleed thì không
  bo góc). Nội dung bên trong (`.pl-in`, dòng 542) đã có padding riêng bằng `clamp()`
  nên chữ không dính mép màn hình — giữ nguyên, chỉ cân nhắc thêm `max-width` cho
  khối text nếu quá rộng trên desktop lớn.

### 3. Showcase "Công trình nói thay năng lực" — auto-slide + tabs tên dự án

Component dùng chung desktop + mobile (một DOM `#prj-show`, một bộ JS — sửa một lần).

- **Xoá**:
  - Markup 2 nút `#show-prev`/`#show-next` (`.snav`, trong dòng 1048-1075) + CSS `.snav`.
  - Markup label `.show-count` (dòng 1060: `01 / 05`) + CSS `.show-count` (dòng 382-383).
  - Trong JS: listener click của `#show-prev`/`#show-next`; các update `#show-cur`/`#show-total` trong `showGo()` (dòng 2131-2150).
- **Thêm tabs tên dự án** (indicator mới, theo lựa chọn của user):
  - Một hàng tabs (vd. `#show-tabs`) render từ `SHOW` (= `PROJECTS.slice(0,5)`, dòng 2128):
    mỗi tab là `<button>` chứa tên dự án (`p.t`, rút gọn nếu dài), đặt dưới slider
    (trên `.show-meta` hoặc thay vị trí hàng nav cũ).
  - Tab active được highlight + có **fill progress** chạy 6s đồng bộ autoplay
    (tái dùng cơ chế `@keyframes sbar` / class `.run` của `.show-bar` dòng 384-386 —
    có thể chuyển progress bar hiện có vào trong tab active thay vì bar rời).
  - Click tab → `showGo(index)` + `showAuto()` (reset timer).
  - Mobile (`@media(max-width:700px)`, dòng 394-398): tabs thu gọn — cho phép
    scroll ngang (`overflow-x:auto`, ẩn scrollbar) hoặc chỉ hiện số thứ tự/tên ngắn.
- **Autoplay đã có sẵn** — `showAuto()` dòng 2151 (interval 6s, guard `offsetParent`).
  Giữ nguyên, giữ touch-swipe (dòng 2154-2163). Thêm: dừng autoplay khi hover vào
  `#show-slides` (desktop), chạy lại khi rời chuột.
- Cập nhật `showGo()` để sync trạng thái active của tabs mỗi lần chuyển slide.

### 4. Card `/du-an` trên mobile theo v7

- Thay body hàm `prjCard()` (v9 dòng 2038-2045) bằng bản v7 (v7 dòng 2003-2013):
  thêm `.ptag` (pill loại dự án), `.pyear` (năm watermark outline), `.pbtn`
  (nút pill "xem dự án"). Data shape (`p.type/p.year/p.size/p.t/p.img`) khớp sẵn.
- Copy CSS v7 dòng 394-413 (`.pcard::after` vignette 2 đầu, `.ptag`, `.pyear`, `.pbtn`).
- **Scope theo yêu cầu (chỉ mobile)**: desktop giữ diện mạo card v9 hiện tại —
  đặt các rule v7 (aspect-ratio 4/4.35, `.ptag`, `.pyear`, `.pbtn` hiển thị) trong
  `@media(max-width:560px)` (breakpoint 1 cột hiện có, dòng 411); ngoài breakpoint đó
  ẩn `.ptag/.pyear/.pbtn` và giữ `aspect-ratio:4/3` + `.pd` như v9.
  (Markup v7 là superset nên render một markup cho mọi width, chỉ CSS quyết định.)
- Lưu ý giữ text tiếng Việt qua `t('tb_event')/t('tb_expo')/t('pj_view')` —
  kiểm tra các key này tồn tại trong v9; nếu thiếu key `pj_view` thì thêm vào dict.

### 5. Animation + màu sắc (tinh chỉnh palette hiện tại)

Màu — giữ nhận diện v9, chỉ cân lại:

- Giảm độ chói accent: `--glow:rgba(46,155,214,.28)` → hạ alpha (~.18).
- Thống nhất accent: mọi CTA/hover dùng `--grad`/`--sky`; rà những chỗ hardcode
  mã hex xanh lệch tông (vd. gradient trong `.pbtn` v7 là `#3AA6DF,#1F84BE` —
  đổi sang `var(--grad)` khi port).
- Tăng nhẹ contrast text phụ: `--mut:#54648A` → tối hơn một nấc (vd. `#4A5A80`)
  để đỡ "bạc" trên nền sáng.
- Không đổi `--navy/--sky` gốc, không đổi font.

Animation — nâng chất, không thêm thư viện:

- `.reveal` (dòng 680-682): giảm blur `5px` → `3px`, giảm translateY `40px` → `28px`,
  thêm `scale(.985)` → mượt hơn, đỡ "nặng". Giữ nguyên 3 khối media
  motion-safety (dòng 685, 690, 697).
- Dùng `.reveal.stagger` (dòng 797-805) cho các grid chưa có stagger
  (services trên home, `.prj-grid`).
- Slider showcase: easing chuyển slide dùng `--e` và tăng duration nhẹ
  (vd. .9s → 1.05s) cho cảm giác cinematic; ảnh trong slide thêm hiệu ứng
  scale nhẹ (Ken Burns rất nhẹ, ~1.0→1.04 trong 6s autoplay).
- Micro-interaction: hover card/nút dùng transform + shadow qua `--e`,
  transition đồng nhất ~.45s (rà những transition lệch duration).

### 6. Mobile menu — dịu màu, chuyên nghiệp hơn

Theo user: **không phải nền tối**, cũng đừng trắng chói — gradient trắng chủ đạo
pha chút xanh của app.

- `.nav` mobile (dòng 231): đổi `linear-gradient(180deg,#F4F8FC,#E7EFF8)` sang
  gradient dịu hơn có tint xanh thương hiệu, vd.
  `linear-gradient(180deg,#EFF5FB 0%,#DEEAF6 55%,#D3E3F3 100%)` — vẫn sáng nhưng
  không trắng gắt, ngả tông `--sky`.
- `body.nav-open` header (dòng ~234): đổi `#FAFCFF` cho khớp tông đầu gradient mới.
- Nav items: tăng cỡ chữ item chính, thêm chỉ số thứ tự nhỏ màu `--sky` trước mỗi
  item (kiểu editorial), giữ stagger hiện có (dòng 245-251) nhưng tăng delay bước
  0.06s → 0.07s và thêm translateY lớn hơn chút cho rõ nhịp.
- `.nav-cta` (dòng 253-258): giữ, chỉnh nền/viền theo tông gradient mới.
- **Gotcha**: khối override `body.home-on.nav-open` (dòng 239-242) reset màu
  logo/lang/burger cho drawer sáng — vẫn hợp lệ vì drawer vẫn sáng, chỉ kiểm tra
  contrast với tông mới.

## Out of scope

- Không sửa `v9.html` hay bất kỳ version cũ nào; không sửa `index.html`.
- Không đổi hướng palette (accent mới / đơn sắc) — chỉ tinh chỉnh palette v9.
- Không thêm testimonials (v9 không có, không port từ v2/v3).
- Không đổi card `/du-an` trên **desktop** — v7-style chỉ áp cho mobile (≤560px).
- Không thêm thư viện, không tách CSS/JS ra file riêng, không build step.
- Không đổi nội dung tiếng Việt, số điện thoại, link Zalo, Google Maps.

## Verification

Không có test tự động — verify bằng browser (mở trực tiếp `v10.html`):

1. **Desktop (~1440px)**:
   - Các section sít lại rõ rệt so với v9 (mở v9 cạnh bên để so).
   - Band "CAM KẾT NGUYỄN BÌNH" chạm cả 2 mép màn hình, không bo góc, chữ không dính mép.
   - Showcase: không còn nút prev/next, không còn "01/05"; slide tự chuyển mỗi 6s;
     hàng tabs tên dự án hiện dưới slider, tab active highlight + progress fill chạy;
     click tab bất kỳ → nhảy đúng slide và timer reset; hover vào ảnh → tạm dừng.
   - `/du-an` desktop: card giữ nguyên diện mạo v9 (không ptag/pyear/pbtn).
2. **Mobile (≤560px, DevTools responsive)**:
   - `/du-an`: card kiểu v7 — pill loại dự án góc trên trái, năm watermark outline,
     nút pill "xem dự án" gradient.
   - Showcase mobile vẫn swipe được, tabs không tràn layout (scroll ngang được).
   - Mở burger menu: gradient sáng dịu pha xanh (không trắng chói), items stagger
     mượt, CTA điện thoại/Zalo hoạt động.
3. **Chung**: chuyển qua đủ các hash route (`#/`, `#/dich-vu/...`, `#/du-an`,
   `#/lien-he`…) — reveal animation chạy lại bình thường, không lỗi console.
4. Bật "Emulate CSS prefers-reduced-motion" → mọi thứ hiện tức thì, không animation.

Sau khi verify: chạy `/review-diff`, rồi `/ship` (commit `feat: v10 — ...`,
GitHub Pages tự deploy từ `main`).
