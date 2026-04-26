# Loading Screen — Content & Behavior Specification v3
**Screen:** Store Generation Loading Screen
**Version:** 3.0 — VEED-inspired card carousel layout
**Reference:** VEED Clips loading screen (3-card carousel + progress bar)

---

## 1. Mục đích màn hình

Màn hình xuất hiện sau khi user hoàn tất 3 bước onboarding (business model, brand, goals). Hệ thống đang build store.

**Mục tiêu:** Trình diễn 3 phase công việc chính một cách gọn gàng, có nhịp điệu — user hiểu ngay hệ thống đang làm gì mà không phải đọc nhiều.

**Khác biệt so với v2:** v2 dùng activity log chi tiết 13 items để build trust sâu. v3 gọn hơn — 3 phase lớn với visual carousel — phù hợp khi muốn pacing nhanh, không overload user với detail.

---

## 2. Layout tổng thể

Tham chiếu layout VEED Clips (3 card carousel) — cấu trúc từ trên xuống:

```
┌─────────────────────────────────────────┐
│  [Logo]  [Back to Dashboard]            │ ← Top nav
│                                         │
│                                         │
│   ┌───┐    ┌─────────┐    ┌───┐         │
│   │ ░ │    │  [ICON] │    │ ░ │         │ ← 3-card carousel
│   │prev│   │ Label   │    │next│         │
│   │100%│   │  xx%    │    │... │         │
│   └───┘    └─────────┘    └───┘         │
│                                         │
│                                         │
│     We're creating your store           │ ← Main headline
│   This takes just a few seconds         │ ← Subtitle (always visible)
│                                         │
│   [Phase tagline — chỉ hiện Phase 3]    │ ← Dynamic tagline (USP)
│                                         │
└─────────────────────────────────────────┘
```

**Thành phần:**
1. **Top nav:** Logo trái + "Back to Dashboard" (optional, fallback exit)
2. **3-card carousel:** Center card = active phase; left = completed (fade); right = upcoming (fade)
3. **Main headline:** `We're creating your store`
4. **Subtitle:** `This takes just a few seconds` — luôn hiển thị, giảm anxiety khi chờ
5. **Dynamic tagline:** Xuất hiện dưới carousel, thay đổi theo từng phase active — mỗi phase có 1 tagline riêng (xem Section 3)

---

## 3. Card content — 3 phase

Mỗi card có **3 thành phần** để match với card size nhỏ:
1. **Icon** (trên cùng, visual chính)
2. **Label** (dưới icon, 2-3 từ tối đa)
3. **Progress %** (dưới label, thực time)

### Phase 1 — Template matching

| Element | Content |
|---------|---------|
| Icon | 🔍 Search / magnifying glass icon |
| Label | `Finding your template` |
| Progress | `0% → 100%` |

**Dynamic tagline khi Phase 1 active** (hiển thị dưới carousel):
> `Tìm kiếm template phù hợp nhất với mô hình [General Dropship] của bạn`

*Backup label nếu card quá nhỏ:* `Finding template` · `Searching template`

---

### Phase 2 — Brand & Goals

| Element | Content |
|---------|---------|
| Icon | ✏️ Pen / edit icon |
| Label | `Customizing your store` |
| Progress | `0% → 100%` |

**Dynamic tagline khi Phase 2 active** (hiển thị dưới carousel):
> `Áp dụng tên thương hiệu và mục tiêu kinh doanh của bạn vào store`

*Backup label nếu card quá nhỏ:* `Customizing` · `Your store`

---

### Phase 3 — CR & AOV Optimization

| Element | Content |
|---------|---------|
| Icon | ⚡ Lightning / chart-up icon |
| Label | `Boosting CR & AOV` |
| Progress | `0% → 100%` |

*Backup label nếu card quá nhỏ:* `CR & AOV` · `Boosting sales`

**Dynamic tagline — chỉ xuất hiện khi Phase 3 active:**
Hiển thị ngay dưới carousel, fade in khi card 3 activate, fade out khi completion state vào:
> *"Cài đặt checkout tối ưu, upsell logic, và trust signals — những yếu tố trực tiếp tăng CR và AOV"*

*Lý do cần tagline:* Label card 2-3 từ không đủ để truyền tải USP. Tagline là khoảng không gian để hệ thống nói rõ *tại sao* Phase 3 quan trọng hơn 2 phase trước — đây là lúc user nhận ra điểm khác biệt của sản phẩm.

---

## 4. Main headline, Subtitle & Progress bar

**Headline:** `We're creating your store`
- Font: Bold, large (display size)
- Color: Primary dark
- Không có emoji/sparkle — giữ clean, confident

**Description:** `Setting up your pages, layout, and conversion features — tailored to your goals.`
- Vị trí: ngay dưới headline, trước subtitle
- Font: Regular, nhỏ hơn headline ~30%, lớn hơn subtitle ~15%
- Color: Secondary — đậm hơn subtitle, nhạt hơn headline
- Luôn hiển thị, không thay đổi theo phase
- Mục đích: trả lời ngay câu hỏi "đang làm gì?" — bridging giữa headline trừu tượng và 3 card cụ thể bên dưới
- Không được dài quá 12 từ — loading screen, user không đọc nhiều
- **Không dùng:** "AI-powered", "magic", "amazing" — giữ tone factual, confident

**Variants (nếu cần personalize theo business model):**
| Business Model | Description |
|----------------|-------------|
| General Dropship | `Setting up your pages, layout, and conversion features — tailored to your goals.` |
| Niche Dropship | `Building your niche store with layouts and features matched to your brand.` |
| Print-on-Demand | `Assembling your store with print-ready pages and conversion features.` |
| Other | `Configuring your store's pages, layout, and conversion tools.` |

*Default (khi không có business model):* Dùng variant General Dropship.

**Subtitle:** `This takes just a few seconds`
- Font: Regular, nhỏ hơn headline ~40%
- Color: Muted/secondary — không cạnh tranh với headline
- Luôn hiển thị trong suốt loading — không thay đổi
- Mục đích: giảm anxiety, set expectation về thời gian chờ

**Progress bar — đặt trong card carousel, không dưới headline:**
- Mỗi card hiển thị progress % riêng của phase đó
- Không có progress bar tổng dưới headline
- Card active hiển thị progress realtime; card completed hiển thị `100%`

---

## 5. Carousel animation

**State transitions giữa 3 card:**

### State 1: Phase 1 active
```
[  empty  ]  [PHASE 1 active]  [phase 2 preview]
                 ●●●●○○           (faded right)
```

### State 2: Phase 1 done → Phase 2 active
```
[phase 1 done]  [PHASE 2 active]  [phase 3 preview]
  (faded left)      ●●●○○○           (faded right)
     100%
```

### State 3: Phase 2 done → Phase 3 active
```
[phase 2 done]  [PHASE 3 active]  [  empty  ]
  (faded left)      ●●○○○○
     100%
```

**Animation specs:**
- Slide direction: Right → Left (next card slides in from right, active slides to left)
- Duration: 400-500ms
- Easing: `cubic-bezier(0.4, 0, 0.2, 1)` (material standard)
- Side cards: opacity 0.3-0.4, scale 0.85
- Active card: opacity 1, scale 1, shadow subtle elevated

---

## 6. Timing

Tổng thời lượng: **6-8 giây** (gọn, snappy — không phải trust builder dài như v2).

| Phase | Duration | Progress tổng |
|-------|----------|---------------|
| Phase 1 — Matching template | 1.5-2s | 0% → 33% |
| Phase 2 — Applying brand | 2.5-3s | 33% → 70% |
| Phase 3 — Optimizing store | 2-2.5s | 70% → 100% |
| Transition giữa card | 400-500ms | (overlap) |
| Completion (hold) | 0.5s | 100% |

**Minimum floor:** 6s dù backend xong sớm — tránh fake loading cảm giác.
**Maximum ceiling:** 10s. Nếu backend > 10s, thêm sub-messages trong phase 3.

---

## 7. Completion state — CR Score Reveal

Khi Phase 3 hoàn thành, màn hình chuyển sang Reveal State. Đây là moment quan trọng nhất để nhấn mạnh USP về CR & AOV.

### 7.1 Sequence animation (theo thứ tự)

```
Timeline:
  0ms    — Phase 3 card icon đổi sang ✓ checkmark
  300ms  — Carousel fade down + slide out
  500ms  — Headline đổi: "We're creating your store" → "Your store is ready"
  600ms  — Subtitle đổi: "This takes just a few seconds" → "Conversion-optimized and ready to sell"
  700ms  — CR Score card scale-in (từ 0.85 → 1.0, ease-out)
  700ms  — CR Score count-up bắt đầu: 0 → 84 trong 900ms
 1600ms  — Score dừng, benchmark tag fade in
 1900ms  — CTA button fade in
```

### 7.2 CR Score card layout

```
┌─────────────────────────────────┐
│                                 │
│          84  /100               │  ← Count-up animation, số lớn
│       CR Score                  │  ← Label nhỏ dưới số
│                                 │
│  ┌─────────────────────────┐    │
│  │ Top 20% Dropship stores │    │  ← Benchmark badge
│  └─────────────────────────┘    │
│                                 │
│  13 CR & AOV optimizations      │  ← Small supporting text
│  applied to your store          │
│                                 │
└─────────────────────────────────┘
```

**Copy chi tiết:**
- **Số chính:** `84` (count-up từ 0)
- **Đơn vị:** `/100`
- **Label:** `CR Score`
- **Benchmark badge:** `Top 20% Dropship stores`
- **Supporting text:** `13 CR & AOV optimizations applied to your store`

### 7.3 CTA (xuất hiện sau khi score dừng)

**Label:** `View your store →`
**Style:** Primary button, full-width max 320px, căn giữa
**Delay:** 300ms sau khi score count-up dừng — không xuất hiện đồng thời với số

### 7.4 Lý do chọn pattern CR Score

| Pattern | Điểm mạnh | Điểm yếu |
|---------|-----------|----------|
| **CR Score (đang dùng)** | Quantified — user rời màn hình với con số cụ thể; benchmark tạo status/pride; dễ nhớ | Score cần có logic thực để không bị hollow |
| Text summary bullets | Honest, không cần số | Không memorable, không emotional impact |
| Auto-transition không reveal | Nhanh | Bỏ lỡ toàn bộ moment USP |

**Con số 84/100 phải có cơ sở:** Nên tính từ số optimization thực sự được apply. Không dùng số cố định — nếu business model khác nhau thì score khác nhau (Niche Store có thể 79/100, POD có thể 81/100).

### 7.5 Variant theo business model

| Business Model | Score | Benchmark |
|----------------|-------|-----------|
| General Dropship | 84/100 | Top 20% Dropship stores |
| Niche Store | 79/100 | Top 25% Niche stores |
| Print-on-Demand | 81/100 | Top 22% POD stores |
| Other | 76/100 | Top 30% among new stores |

---

## 8. Edge cases

**Backend nhanh hơn 6s:**
- Dùng artificial minimum, chạy đủ 6s animation rồi mới transition.

**Backend chậm hơn 10s:**
- Stuck tại phase 3 không đẹp. Thêm rotating sub-labels trong phase 3:
  - `Setting up checkout...`
  - `Installing trust badges...`
  - `Configuring upsells...`
- Progress bar đi chậm lại nhưng không dừng hẳn (creep từ 95% → 99%).

**Backend fail:**
- Active card chuyển sang error state (icon ⚠, border red subtle)
- Headline: `Hmm, something went wrong`
- Progress bar subtext: `Retry automatically...` hoặc CTA `Try again`

**User bấm "Back to Dashboard" giữa chừng:**
- Dialog: `Store của bạn sắp xong. Chắc chắn muốn thoát?`
- Actions: `Stay` (primary) · `Leave anyway` (ghost)

---

## 9. Điều không làm

- **Không dùng subtext dưới headline** — progress bar đã thay thế
- **Không hiển thị cả 3 card cùng size** — phải có visual hierarchy (active vs side cards)
- **Không fake progress** — nếu backend chưa xong, slow creep thay vì nhảy số giả
- **Không dùng spinner vô tận** — có carousel + progress bar rồi, không cần thêm loader
- **Không để card content dài** — label phải fit card size, dài quá sẽ truncate xấu
- **Không dùng ngôn ngữ marketing fluff** — "magic", "AI genius", "amazing" đều ra

---

## 10. Responsive

**Desktop (>1024px):**
- Carousel 3 card hiển thị đầy đủ
- Side cards partial visible (cắt mép)
- Progress bar max-width 480px, center

**Tablet (768-1024px):**
- Carousel thu nhỏ, side cards hiển thị ~60% width
- Progress bar max-width 400px

**Mobile (<768px):**
- Side cards ẩn hoặc chỉ hiển thị ~20% (peek preview)
- Active card full-focus
- Progress bar full-width với padding 24px hai bên
- Headline size giảm xuống (vẫn bold, vẫn dominant)
