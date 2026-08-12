# Project context

Shared vocabulary and architecture map between humans and Claude. A precise term
used consistently beats a paragraph of description — this file is what makes short
prompts land correctly.

Maintained by `/onboard` and updated whenever vocabulary or architecture changes.
Keep entries short; delete stale ones.

## What this project is

Marketing website for **Nhà Bạt Nguyễn Bình** — a Vietnamese company (operating
since 2009, southern Vietnam) that rents large tent/canopy structures for events,
exhibitions, and industry, plus temporary warehousing and industrial cooling.
Audience: event organizers, exhibition operators (e.g. SECC), and businesses
needing temporary covered space. All content is in Vietnamese. The repo is a
redesign effort: multiple full-page design iterations coexist as separate files.

## Domain vocabulary

| Term | Meaning |
| --- | --- |
| `Nhà bạt` | Tent / large fabric canopy structure — the core product |
| `Khẩu độ` | Span (clear width) of a tent. "Bảng khẩu độ tiêu chuẩn" = standard-span table; a key spec buyers pick by |
| `Nhà bạt sự kiện` | Event tents (concerts, festivals, brand activations) |
| `Nhà bạt triển lãm` | Exhibition tents (international trade-fair standard, e.g. SECC) |
| `Kho tạm` | Temporary warehouse rental, billed monthly |
| `Máy lạnh công nghiệp` | Industrial air conditioning / cooling, offered as a package |
| `Hồ sơ năng lực` | Company capability profile, used for `hồ sơ thầu` (bid/tender submissions) |
| `SECC` | Saigon Exhibition & Convention Center — a flagship client cited on the site |
| `Nhà Thi Đấu Phú Thọ` | Phú Thọ Stadium — another named client |
| `vX` (v2…v10) | A numbered full-page design iteration; each is a standalone live page |

## Architecture map

- **Pages**: `index.html`, `v2.html` … `v10.html` at repo root. Each is one
  **self-contained** static HTML file — one inline `<style>`, inline `<script>`,
  no external local CSS/JS. `index.html` and every `v*.html` are live/deployed.
- **New iterations**: copy the latest version to the next number (`v8.html`); do
  not overwrite an existing version.
- **Shared assets**: `img/` (logos, hero images, project photos). Fonts load from
  Google Fonts CDN. No other external dependencies.
- **Deploy**: GitHub Pages from `main`; `.nojekyll` disables Jekyll processing.
- **No build/test/lint** — verification is visual, in a browser.

## Key decisions & gotchas

- Single-file-per-page is intentional. Don't refactor shared CSS/JS into separate
  files or add a bundler/framework — it breaks the "open the file, see the page"
  workflow.
- Versions are kept, not replaced. Editing `v7.html` in place changes that live
  page; start a new `v8.html` when iterating on the design.
- Content is Vietnamese with diacritics — preserve exact wording, phone numbers
  (`0937 327 777`), Zalo links (`zalo.me/0937327777`), and the embedded Google
  Maps address when editing.
