# Handoff Document — Onboarding Flow Brainstorm
**Project:** Web Builder / Store Builder SaaS
**Ngày cập nhật:** 2026-04-26 (session 3)
**Mục đích file:** Tóm tắt toàn bộ cuộc trò chuyện để agent mới tiếp tục mà không cần đọc lại lịch sử

---

## 1. Sản phẩm đang xây dựng là gì

Một **web builder** giúp merchant tạo store ecommerce nhanh, tập trung vào 3 business model:
- **Print on Demand (POD)**
- **General Dropship**
- **Niche Dropship**

**USP của sản phẩm:** AI giúp tăng CR (Conversion Rate) và AOV (Average Order Value) cho store.

**User goal:** Có một store chuyên nghiệp, CR/AOV tốt, nhanh chóng.

---

## 2. User Journey đã được thiết kế

Luồng đã thống nhất theo thứ tự:

1. **Landing Page** — User visit lần đầu
2. **Sign up** — Tạo tài khoản
3. **Onboarding 3 màn** — Nhập thông tin để gen store
   - Step 1: Chọn Business Model *(spec tại `onboarding-step1-business-model-spec.md`, HTML tại `onboarding-step1.html`)*
   - Step 2: Nhập Store Name *(copy đã được quyết định — xem Section 9)*
   - Step 3: Nhập Store Goal *(copy đã được quyết định — xem Section 9)*
4. **Loading Screen** — Hệ thống gen store *(spec chi tiết tại `loading-screen-spec-v2.md`)*
5. **Store Screen** — User xem store đã được gen, với CR highlights + hover tooltip *(spec tại `store-screen-requirements.md`)*
6. **Guided Refinement** — AI đề xuất 3 việc tiếp theo có impact cao nhất
7. **Publish** — CTA duy nhất "Publish store", sau đó có dashboard AI monitoring

---

## 3. Quyết định quan trọng đã được thống nhất

### Về template vs AI gen
- Backend thực tế dùng **template matching theo business model**, không AI gen thật
- Ngôn ngữ màn hình loading phải **trung thực**: nói thẳng "chọn template" thay vì "AI đang tạo store"
- Template IS the intelligence — trí tuệ nằm ở việc biết template nào phù hợp với business model nào

### Về trust problem
- Giải pháp trust không phải claim số liệu chung chung mà là **CR highlights cụ thể** trực tiếp trên store
- Mỗi element được optimize có highlight ring + badge "⚡ CR" + hover tooltip giải thích WHY + số liệu benchmark
- Format tooltip: *[Con số/nguyên lý] — [Hệ quả với buyer behavior]*
- Trust được build qua recognition: user đã nghe ở loading screen → vào store nhìn thấy đúng element đó

### Về Store Screen (thay thế "Preview Store + CR Audit Panel")
- Quyết định cuối: **không có sidebar panel**. Chỉ có store đầy đủ + highlights + tooltip on hover
- Đơn giản hơn, ít cognitive load hơn, buyer journey không bị interrupt
- Tooltip xuất hiện 150ms sau hover, dark background, 280–320px width
- 3 tab navigation: Homepage | Product Page | Checkout (theo happy flow mua hàng)

### Về Loading Screen
- **Không phải màn hình chờ** — là màn hình chứng minh năng lực
- Mỗi item trong loading phải: (1) nói hệ thống làm gì cụ thể + (2) giải thích tại sao quan trọng với CR
- Script 13 items, chia 6 giai đoạn, dropship-specific

---

## 4. Template đang dùng — Figma Reference

**File:** [Ecom] Trendie • General Dropship  
**Figma URL:** `https://www.figma.com/design/znvtVRAgOucsCoYMUBDHHS/-Ecom--Trendie-%E2%80%A2-General-Dropship`  
**Node Homepage:** `4002:1836` (HD Home frame)

### CR Analysis đã thực hiện — Homepage

Đã đọc và audit toàn bộ homepage của template. Kết quả:

**Điểm mạnh hiện có (giữ nguyên làm CR highlights):**
- Deal of the Week: countdown timer + anchor price ($90 → $45, 50% OFF) + scarcity ("Only 10 items left") + dual CTA (Add to cart + Buy now) → section CR tốt nhất trang
- Benefits bar: 3 trust signals ngay dưới hero (free shipping, international delivery, return policy)
- "Best Selling" label trên section → social proof signal
- Star rating + review count visible trên product cards
- SALES badge trên product cards

**Vấn đề cần fix trước khi dùng template (theo thứ tự ưu tiên):**

| # | Vấn đề | Severity |
|---|--------|----------|
| 1 | Review text không khớp product title trong "Customer Love Us" — placeholder data sai hoàn toàn | Destroy trust |
| 2 | Product cards không có anchor price (không có giá gốc gạch ngang) — mất perceived value của discount | High |
| 3 | "Deal of the Week" section đặt quá sâu — sau 4 collection sections, section CR tốt nhất bị chôn vùi | High |
| 4 | Hero CTA "SHOP NOW" quá generic — không có context value | Medium |
| 5 | Free shipping threshold $150 quá cao cho average Dropship order ($30–$80) | Medium |
| 6 | Announcement bar bị ẩn (visible: false) — real estate trust signal đang bỏ trống | Medium |
| 7 | "SALES" badge không show % discount — kém hiệu quả hơn "30% OFF" | Low |

**Chưa audit:** Product Page (PDP) và Checkout — cần làm tiếp ở session sau.

---

## 5. Files đã tạo

### `signup.html` ← **Live implementation — TẤT CẢ 4 SCREENS trong 1 file**

Toàn bộ onboarding flow đã được consolidate vào file này. Navigation giữa các screen bằng CSS opacity transition (không reload trang). Thứ tự:

**Screen 1 — Sign Up** (`#sign-up-screen`)
- Split layout: form trái + animated template gallery phải (rotate -20deg, 3 cột scroll độc lập)
- Prototype auto-fill: focus vào email/password → điền sẵn `rashford99@gmail.com / Rashford99@`
- Sign up button → chuyển sang Business Model screen

**Screen 2 — Business Model** (`#business-model-screen`, STEP 1 OF 3, progress 33%)
- 4 cards: General Dropship (Most Popular badge), Niche Dropship, Print-on-Demand, Other
- Radio group với keyboard navigation (Arrow keys)
- Continue → chuyển sang Store Name, lưu vào `sessionStorage['convercore.selectedBusinessModel']`

**Screen 3 — Store Name** (`#store-name-screen`, STEP 2 OF 3, progress 67%)
- Input: placeholder "Enter your store name", maxlength 64
- AI suggestion chips (pill shape, `border-radius: 100px`) — 5 chips, content thay đổi theo business model đã chọn
  - General Dropship: "Shop siêu rẻ", "Cửa hàng thông minh", "Cửa hàng tốt", "Thế giới 247", "Đồ gia dụng thông minh"
  - Chip click → fill input (replace)
- Validate ≥ 2 ký tự. Continue → `sessionStorage['convercore.storeName']`, chuyển sang Business Goal

**Screen 4 — Business Goal** (`#business-goal-screen`, STEP 3 OF 3, progress 90%)
- Textarea height 88px (không resize), placeholder "Enter your business goal"
- AI suggestion chips (rounded rectangle, `border-radius: 12px`) — 6 chips trong grid 3 cột:
  - "Launch product pages faster", "Test and find winning products", "Turn followers into buyers"
  - "Build a brand people come back to", "Increase conversion rate", "Increase average order value"
- **Multi-select logic:** Click chip → append ", " vào textarea, chip hiện `is-selected` state (xanh). Click lại → remove. User gõ tay → clear chip selection. Cmd+Enter để submit.
- Continue → `sessionStorage['convercore.businessGoal']` *(chưa có screen tiếp theo)*

**JS architecture:**
- `setActiveScreen(screenName)` — central navigator, nhận `'sign-up' | 'business-model' | 'store-name' | 'business-goal'`
- `setupPrototypeFill()` — auto-fill signup form, cleanup `prototype-filled` class khi user edit
- `setupBusinessModelSelection()` — radio card selection + keyboard nav
- `renderSuggestionChips()` — re-render chips mỗi lần store-name screen active (vì chips phụ thuộc selectedBusinessModel)
- `setupStoreNameScreen()` — input validation, chip fill
- `setupBusinessGoalScreen()` — textarea + multi-select Set logic, chips rendered once at init

### `onboarding-step1.html` ← **Deprecated — dùng signup.html thay thế**
File cũ chỉ có Screen 2 (Business Model). Không xóa để tham khảo nếu cần tách file.

### `loading-screen-spec-v3.md` ← **Spec mới nhất cho Loading Screen**
Layout VEED-inspired: 3-card carousel + headline/subtitle dưới. Đây là v3 (v2 là activity log style).

**3 phase labels đã confirm:**
| Phase | Icon | Label |
|-------|------|-------|
| 1 | 🔍 | `Finding your template` |
| 2 | ✏️ | `Customizing your store` |
| 3 | ⚡ | `Boosting CR & AOV` |

**Headline copy đã confirm:**
- Headline: `We're creating your store`
- Description: `Setting up your pages, layout, and conversion features — tailored to your goals.`
- Subtitle: `This takes just a few seconds`

**Completion state:** CR Score card xuất hiện sau phase 3. Score khác nhau theo business model (General Dropship: 84/100, Niche: 79/100, POD: 81/100, Other: 76/100). CTA: `View your store →`

### `loading-screen-spec.md` / `loading-screen-spec-v2.md`
Spec cũ hơn — v2 dùng activity log 13 items. Giữ để tham khảo.

### `store-screen-requirements.md` ← **Spec màn hình sau loading**
Spec cho Store Screen — màn hình user xem store với CR highlights và hover tooltips.

Nội dung gồm:
- Layout: full-width store + tab bar (Homepage | Product Page | Checkout)
- Highlight system: border ring + glow + badge "⚡ CR" trên mỗi element được optimize
- Tooltip spec: visual, vị trí, timing, animation
- 13 tooltip contents đầy đủ (map 1:1 với 13 items trong loading script)
- Behavior: staggered animation khi load, bidirectional hover, page switching
- Điều không làm

### `onboarding-step1-business-model-spec.md`
Spec chi tiết cho Step 1. Bao gồm Auto Layout spec cho AI designer (section 11).

---

## 6. Codebase & Assets

**GitHub repo:** `https://github.com/TrienChill0710/Onboarding-CC.git`

**Cấu trúc assets:**
```
assets/
├── signup-thumbs/        — 9 thumbnails dùng cho animated gallery ở signup screen
├── figma-graphic-cropped/ — graphic assets mới (dùng cho signup hoặc onboarding)
└── business-model-thumbs/ — 4 ảnh preview cho Step 1 cards
    ├── general-dropship.png
    ├── niche-dropship.png
    ├── print-on-demand.png
    └── other.png
```

**Design system (từ signup.html):**
- Font: Inter
- Primary: `#0088ff`
- Text primary: `#0f172a` / `#1e293b`
- Text secondary: `#64748b`
- Border: `#e2e8f0`
- Background: `#f8fafc`
- Border radius card: 12px

**Figma access:** Token đã config trong `~/.claude/settings.json` (figma-console MCP). Dùng Figma REST API trực tiếp: `curl -H "X-Figma-Token: <token>" https://api.figma.com/v1/files/<filekey>/nodes?ids=<nodeids>`

---

## 7. Target Segments — Goals Analysis

Đã đọc 4 Figma board nodes (FigJam: `SXEdFC21sjPRzCBTBJ0svv`) qua REST API. Kết quả:

### General Store Dropshipping
1. Launch product pages fast — dựng page chuyên nghiệp sẵn sàng chạy ads, không mất công setup
2. Find winners, cut losers quickly — spy → copy → chạy ads → tìm winning product
3. Keep overhead low — tránh app bloat, giữ store gọn, đủ convert

### Social Community Seller
1. Convert followers into buyers — funnel liền mạch từ social post → checkout
2. Capture revenue when traffic spikes — store sẵn sàng với offer khi viral post kéo traffic
3. Maximize AOV from organic audience — upsell/bundle vì không chạy paid ads

### Niche / Brand Builder
1. Build a store that looks and converts like Shopify — đủ đẹp, nhanh, branded để tăng CR
2. Launch campaigns fast — landing page và campaign page nhanh cho holidays, drops, trends
3. Scale on a stable, extensible platform — stable + app ecosystem đủ rộng

### Niche / Brand Builder Specific ($400k–$1M/month)
1. Automate content production — AI lo descriptions, images, landing pages
2. Advanced upsell / AOV optimization — cấu hình upsell linh hoạt theo từng sản phẩm
3. Zero downtime at scale — stability là ưu tiên số 1, bất kỳ lỗi nào = mất dòng tiền

---

## 8. Onboarding Step 2 & 3 — Copy đã implement

### Step 2 — Store Name ✅ DONE

**Title:** `What is your store name?`
**Description:** `We'll use this to brand your store — logo, header, and more.`
**Input placeholder:** `Enter your store name`
**Figma nodes:** Header 1008-35094 | Main content 1008-35141 | Actions 1008-35105

---

### Step 3 — Business Goal ✅ DONE

**Title:** `What is your main goal?`
**Description:** `We'll set up your store's layout and features to match this.`
**Input placeholder:** `Enter your business goal`
**Figma nodes:** Step indicator 1020-15017 | Header 1020-15023 | Frame 1020-15028 | Actions 1020-15063

**6 Suggestion chips implemented (grid 3×2):**
```
Launch product pages faster     | Test and find winning products  | Turn followers into buyers
Build a brand people come back  | Increase conversion rate        | Increase average order value
```

*Lưu ý: Ban đầu spec có 10 chips, đã thu gọn còn 6 chip cốt lõi nhất để match Figma design và Hick's Law.*

---

## 9. Phần chưa làm — cần tiếp tục

### A. Implement Loading Screen ← **TIẾP THEO — ưu tiên cao nhất**
Spec đầy đủ tại `loading-screen-spec-v3.md`. Implement vào `signup.html` như Screen 5.

Key implementation notes:
- 3-card carousel: active card scale 1.0 + shadow, side cards scale 0.85 + opacity 0.3-0.4
- Slide animation: right → left, 400-500ms, `cubic-bezier(0.4, 0, 0.2, 1)`
- Progress % per card: realtime count-up trong phase active, hiển thị 100% khi done
- Timing: Phase 1 ~2s, Phase 2 ~3s, Phase 3 ~2.5s, tổng minimum 6s
- Completion state: carousel fade out → CR Score card scale-in → count-up 0→84 trong 900ms → benchmark badge → CTA "View your store →"
- Dynamic tagline khi Phase 3 active: *"Cài đặt checkout tối ưu, upsell logic, và trust signals..."*
- Score theo business model: General 84/100, Niche 79/100, POD 81/100, Other 76/100
- Business goal screen Continue button chưa có navigation → cần trỏ tới loading screen

### B. CR Analysis cho Product Page (PDP) — ưu tiên cao
Đã audit Homepage xong. Cần tiếp tục với PDP (node `1:2` trong Figma file).
Các element cần chú ý theo spec:
- Price anchoring
- Sticky ATC bar
- Stock indicator
- Review proximity to price

### D. CR Analysis cho Checkout — ưu tiên cao
Node `2489:4915` trong Figma file.
Các element cần chú ý:
- Form fields count
- Progress bar
- Trust badges gần payment button

### E. Store Screen — tooltip content cho PDP và Checkout
`store-screen-requirements.md` đã có đủ 13 tooltips. Nhưng các tooltips hiện dùng placeholder element positions — cần verify lại vị trí badge sau khi xem PDP và Checkout thực tế trong Figma.

### F. Loading Screen spec cho POD và Niche Dropship
Hiện chỉ có spec cho General Dropship. POD và Niche Dropship cần script riêng vì pain point khác:
- POD: nhấn vào quality proof, mockup, return policy
- Niche Dropship: nhấn vào expertise signals, community proof

---

## 10. Context bổ sung

- Business model được chọn ở sign up là: Print on Demand / General Dropship / Niche Dropship — không đi sâu vào ngành hàng cụ thể
- Benchmark CR tham khảo: POD 1.5–2.5% | General Dropship 1.8–3.2% | Niche Dropship 2.0–4.0%
- Giai đoạn đầu chưa có internal data → dùng industry benchmark (Shopify, Baymard Institute, Klaviyo) và cite source rõ ràng
- CLAUDE.md của project chứa các skill UX: Hick's Law, 10 Usability Heuristics, Don't Make Me Think, Judge UI, Frontend Design — agent mới nên đọc file đó trước khi làm bất kỳ task design nào
- Figma board target segments: `https://www.figma.com/board/SXEdFC21sjPRzCBTBJ0svv/ConvertCore-Concept---Plan`
  - General Store Dropshipping: node `2979-12427`
  - Social Community Seller: node `2979-12333`
  - Niche/Brand Builder: node `2979-12371`
  - Niche/Brand Builder Specific: node `2979-12404`
