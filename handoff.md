# Handoff Document — Onboarding Flow Brainstorm
**Project:** Web Builder / Store Builder SaaS
**Ngày cập nhật:** 2026-04-27 (session 7)
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

### CR Analysis đã thực hiện — Homepage (Figma node `1064:20586`)

Đã đọc và audit toàn bộ homepage của template qua Figma REST API. **Session 5:** Đã viết lại toàn bộ tooltip descriptions theo ngôn ngữ seller (xem bên dưới).

**13 CR/AOV highlights đã confirm + tooltip content (ngôn ngữ seller):**

| # | Element | Title | Description |
|---|---------|-------|-------------|
| 1 | Announcement bar | ⚡ Thanh thông báo đầu trang | Cam kết ship/khuyến mãi hiện ngay khi vào — giảm bỏ đi vì lo phí ship. |
| 2 | Hero + CTA | ⚡ Banner chính + nút Shop Now | 5 giây đầu quyết định khách ở lại hay đóng tab — banner rõ + CTA nổi giúp khách bắt đầu mua trong 1 click. |
| 3 | Benefits bar | ⚡ 3 cam kết dịch vụ | Đổi trả, ship, đóng gói — 3 lo lắng lớn nhất của khách mới được giải quyết trong 3 giây. |
| 4 | Best Selling section | ⚡ Khu vực bán chạy | Khách mới chọn theo số đông — đặt hàng bán chạy lên đầu giúp khách chốt nhanh, không lướt rồi rời. |
| 5 | SALE badge | ⚡ Tag SALE đỏ | Tag đỏ kéo mắt khách dừng lại — tăng click vào sản phẩm 15-25% mà không cần đổi giá. |
| 6 | Rating + review count | ⚡ Đánh giá ngay trên card | Sản phẩm có 4★+ trên card được click nhiều gấp 2 lần sản phẩm chưa có review. |
| 7 | Category banner discount | ⚡ Banner danh mục có hình người | Ảnh người dùng thật + % giảm = khách hình dung được mình dùng sản phẩm. Hiệu quả gấp 3 lần banner sản phẩm rời. |
| 8 | Countdown timer | ⚡ Đồng hồ đếm ngược | "Còn 5 phút" → khách sợ mất deal hơn sợ tiêu tiền. Tăng chốt đơn 10-15%. |
| 9 | Anchor price | ⚡ Giá gốc gạch ngang + % giảm | $90 → $45 = khách cảm thấy lời $45 dù chưa so giá. Tăng tỷ lệ mua 30-40%. |
| 10 | Stock scarcity | ⚡ Cảnh báo sắp hết hàng | "Chỉ còn 10" cạnh nút mua = cú đẩy cuối để khách bấm thay vì trì hoãn. |
| 11 | Dual CTA | ⚡ 2 nút Add to Cart và Buy Now | Khách phân vân → Add to Cart. Khách quyết → Buy Now. Phục vụ cả 2 nhóm cùng lúc. |
| 12 | Customer reviews | ⚡ Review thật kèm tên khách | Review tên thật giải tỏa nghi ngờ cuối — giảm thoát trang ở cuối, nhất là khách lần đầu. |
| 13 | Newsletter subscribe | ⚡ Form đăng ký nhận tin | Khách chưa mua hôm nay = có email vẫn remarketing được, kéo lại 5-8% khách đã rời. |

**Vấn đề cần fix trước khi dùng template:**

| # | Vấn đề | Severity |
|---|--------|----------|
| 1 | Review text không khớp product title trong "Customer Love Us" — placeholder data sai | Destroy trust |
| 2 | Product cards không có anchor price (không có giá gốc gạch ngang) | High |
| 3 | "Deal of the Week" section đặt quá sâu — sau 4 collection sections | High |
| 4 | Hero CTA "SHOP NOW" quá generic | Medium |
| 5 | Free shipping threshold $150 quá cao cho average Dropship order ($30–$80) | Medium |
| 6 | Announcement bar bị ẩn (visible: false) | Medium |
| 7 | "SALES" badge không show % discount — kém hơn "30% OFF" | Low |

### CR Analysis đã thực hiện — Product Detail (Figma node `1065:24834`)

Product: Mini Wifi Wide Angle Camera — $22.5 (was $45.99, 50% OFF). **Session 5:** Expanded từ 10 lên 13 highlights, viết lại tooltips theo ngôn ngữ seller.

**13 CR/AOV highlights đã confirm + tooltip content (ngôn ngữ seller):**

| # | Element | Title | Description |
|---|---------|-------|-------------|
| 1 | Rating + review count gần title | ⚡ Đánh giá ngay trên giá | Khách thấy 4.5★ với 200 review trước khi đọc giá — giảm phản đối giá ngay từ đầu. |
| 2 | Anchor price + 50% OFF badge | ⚡ Giá gốc gạch ngang + % giảm | $45.99 → $22.5 = khách cảm thấy lời $23 dù chưa so giá ngoài. Cú đẩy mạnh nhất với mua hàng cảm tính. |
| 3 | Stock urgency | ⚡ Cảnh báo sắp hết hàng | Số stock cụ thể cạnh nút mua = khách sợ hết hàng, mua ngay thay vì so sánh thêm shop khác. |
| 4 | Discount countdown | ⚡ Đồng hồ đếm ngược deal | "Còn 30 phút" tại điểm quyết định = khách không trì hoãn được. Tăng add-to-cart 10-15%. |
| 5 | Dual CTA | ⚡ 2 nút Add to Cart và Buy Now | Khách phân vân → Add to Cart. Khách quyết → Buy Now. Phục vụ cả 2 nhóm trong 1 màn. |
| 6 | Delivery estimate | ⚡ Ngày giao hàng cụ thể | "Nhận hàng Oct 30 - Nov 04" xóa lo ngại "không biết bao giờ ship đến" — giảm bỏ giỏ 6%. |
| 7 | Accessories cross-sell | ⚡ Phụ kiện kèm sản phẩm | Khách đã quyết mua chính = thời điểm dễ add thêm nhất. 1-click Add tăng AOV trung bình 8-12%. |
| 8 | Buy more save more tiers | ⚡ Mua nhiều giảm thêm | 3 mức giá rõ ràng để khách tự tính lợi và chủ động mua nhiều — đẩy AOV lên 1.5-2 lần. |
| 9 | Frequently bought together | ⚡ Bundle mua kèm có sẵn | Bundle chọn sẵn + tổng giá hiện rõ = khách thấy ngay tiết kiệm bao nhiêu. Tỷ lệ chốt bundle 25-35%. |
| 10 | Rich content + lifestyle images | ⚡ Nội dung mô tả có hình ngữ cảnh | Hình thực tế cho khách hình dung được trải nghiệm khi dùng — không chỉ mô tả khô. |
| 11 | Rating breakdown bar chart | ⚡ Biểu đồ phân bổ đánh giá | 92% 5★ trên 32,000 review = bằng chứng số đông mạnh hơn rating đơn lẻ. Khách yên tâm chốt cuối. |
| 12 | Reviews kèm ảnh khách thật | ⚡ Review có ảnh khách thật | Review có ảnh được tin gấp 3 lần review chỉ có chữ — bằng chứng cao nhất trước khi khách bấm mua. |
| 13 | You may also like | ⚡ Sản phẩm liên quan cuối trang | Khách không mua sản phẩm này = giữ họ lại bằng sản phẩm tương tự, không cho rời shop. |

---

## 5. Files đã tạo

### `signup.html` ← **Live implementation — TẤT CẢ 6 SCREENS trong 1 file** (~3937 lines)

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
- Continue → `sessionStorage['convercore.businessGoal']`, chuyển sang Loading Template screen

**Screen 5 — Loading Template** (`#loading-template-screen`)
- 3-phase loading carousel: "Finding store template" → "Customizing your store" → "Boosting CR & AOV"
- Khi phase 3 đạt 100% → sau 600ms tự động `setActiveScreen('store-preview')`

**Screen 6 — Store Preview** (`#store-preview-screen`) ← **MỚI — session 6**
- Full store preview layout, scrollable, với CC overlay ở trên
- CSS prefix: `.sp-` cho toàn bộ component
- Xem chi tiết trong section "Store Preview Screen" bên dưới

**JS architecture:**
- `setActiveScreen(screenName)` — central navigator, nhận `'sign-up' | 'business-model' | 'store-name' | 'business-goal' | 'loading-template' | 'store-preview'`
- `setupPrototypeFill()` — auto-fill signup form, cleanup `prototype-filled` class khi user edit
- `setupBusinessModelSelection()` — radio card selection + keyboard nav
- `renderSuggestionChips()` — re-render chips mỗi lần store-name screen active (vì chips phụ thuộc selectedBusinessModel)
- `setupStoreNameScreen()` — input validation, chip fill
- `setupBusinessGoalScreen()` — textarea + multi-select Set logic, chips rendered once at init

---

### Store Preview Screen — Chi tiết (session 6)

**Figma source:** File `sj4QXXjuZKaEZOKcONXshG` (Convert Core Dashboard Design), node `1075-31605` "Preview store"

**Đã đọc 22 nodes từ Figma REST API** để lấy layout, spacing, colors, typography, text content chính xác.

**Cấu trúc màn hình (top → bottom, scrollable):**

| Layer | CSS class | Figma spec | Notes |
|-------|-----------|-----------|-------|
| CC Header | `.sp-cc-header` | h=56px, white, sticky | "Back" btn (border) + "Create store now" btn (#0088ff) |
| Report bar | `.sp-report-bar` | h=56px (default) / 163px (open), sticky top=56px | Orange circle badge + CR Score · Top 20% · 21 optimizations | "See full report ▾" button right |
| Browser bar | `.sp-browser-bar` | h=35px, #f5f5f5 | Mac dots (red/yellow/green) + "Trendie-store.preview" |
| Store nav | `.sp-store-nav` | h=60px, white | TRENDIE logo + 9 links + search/user/cart(0) |
| Slideshow | `.sp-slideshow` | h=400px | Dark blue gradient + "BEST SELLING" + drone title + SHOP NOW btn |
| Benefits | `.sp-benefits` | h=86px, #c6d7eb | 3 items: Buy more discount / Int'l delivery / Secured packaging |
| Best Selling | `.sp-best-selling` | w=1120px inner | 4 product cards + carousel arrows |
| Collection Electronics | `.sp-collection` | bg=#f7fbfc | Left cover 559px + right 2×2 grid |
| Beauty Banner | `.sp-banner` | bg purple gradient | "Over 30% Off Beauty and Healthy products" + 2 cards right |
| Product List Beauty | `.sp-product-list` | 4 products | 6 in Figma, 4 shown |
| Apparel Collection | `.sp-collection-apparel` | bg=#f7f9fc | Same structure as Electronics |
| Garden Banner | `.sp-banner` | bg green gradient | "Over 40% Off Garden products" + 2 cards right |
| Product List Garden | `.sp-product-list` | 4 products | 6 in Figma, 4 shown |
| Feature Product | `.sp-feature-product` | Deal of week | Timer (05m:22s) + product info + Add to cart + Buy now |
| Customer Love Us | `.sp-customer-love` | 3 reviews | Julio / Gerardo / Gerardo review cards |
| Subscribe | `.sp-subscribe` | image left + form right | Email input + Sign up btn |
| Footer | `.sp-footer` | 4 columns | Store info / Quick shop / Policies / Customer support |

**Key design tokens dùng trong store preview:**
- Primary blue: `#3b71dc`
- Dark text: `#0d0a1e`
- Star gold: `#eebd29`
- SALES badge: `#dc3b3b`
- Benefits bg: `#c6d7eb`
- Electronics section bg: `#f7fbfc`
- CR tag: bg `#fff7ed`, text `#c2410c`
- Font: Inter (headings/body) + Roboto Flex (buttons/labels)

**Images:** Placeholder từ `picsum.photos/seed/[keyword]/[w]/[h]` — mỗi seed deterministic nên consistent giữa các load.

**CR tags:** Mỗi section có `.sp-cr-tag` (orange badge "⚡ CR") ở góc phải — pure visual, chưa có tooltip/interaction.

**Chưa implement trên Store Preview:**
- Hover tooltips cho CR highlights (theo spec cũ trong store-screen-requirements.md)
- Actual slideshow interaction (slide on click)
- Carousel navigation (prev/next buttons không có logic)
- Tab switching (Homepage / Product Page / Checkout)
- Real product images từ Figma exports

---

### Report Bar Component — Chi tiết (session 7)

**Figma source:** Node `1125-23978` (component set, 2 states) + `1125-23979` (instance dùng trong store preview)

**2 states đã implement:**

| State | Height | Trigger |
|-------|--------|---------|
| Default | 56px | Initial render |
| Open | 163px | Click "See full report" button |

**Default state layout:**
- `justify-content: space-between`, `padding: 8px 21px`, `border-bottom: 1px solid #fef3c6`
- Left group (`.sp-report-left`, `gap: 14px`):
  - Orange circle badge (28×28, `bg: #fe9a00`, `border-radius: 50%`) + lightning bolt SVG (white)
  - "CR Score: 84/100" (fs=14, fw=600, #171717)
  - "·" separator (#d4d4d4)
  - "Top 20% Dropship stores" (fs=14, fw=400, #3f3f3f)
  - "·" separator
  - "21 optimizations applied" (fs=14, fw=400, #3f3f3f)
- Right: `.sp-report-btn` — "See full report" + chevron SVG (rotates 180° when open)

**Open state — 4 page breakdown cards** (`.sp-report-cards`, `gap: 12px`):

| Card | Score | Progress fill | Optimizations |
|------|-------|--------------|--------------|
| Homepage | 8/10 | 85% orange | 8 optimizations |
| Product Detail | 9/10 | 90% orange | 9 optimizations |
| Your cart | 8/10 | 85% orange | 2 optimizations |
| Checkout | 8/10 | 85% orange | 2 optimizations |

Each card: `bg: #fff`, `border: 1px solid #fef3c6`, `border-radius: 10px`, `padding: 13px 13px 1px 13px`
Progress bar: `bg: #f5f5f5`, fill `bg: #fe9a00`, `border-radius: 999px`, `h: 6px`

**Toggle JS:** `document.getElementById('sp-report-toggle').addEventListener('click', () => reportBar.classList.toggle('is-open'))`
Transition: `height 220ms ease` + `opacity 180ms ease 40ms` trên cards

---

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

### `store-screen-requirements.md` ← **Spec màn hình sau loading — ĐÃ CẬP NHẬT session 4**
Spec cho Store Screen — màn hình user xem store với CR highlights và hover tooltips.

Nội dung gồm:
- Layout: full-width store + tab bar (Homepage | Product Detail | Cart | Checkout)
- Highlight system: border ring + glow + badge "⚡ CR" trên mỗi element được optimize
- Tooltip spec: visual, vị trí, timing, animation
- **19 tooltip contents đầy đủ** (tăng từ 13 lên 19 — đã audit Homepage + Product Detail thực tế từ Figma)
- Behavior: staggered animation khi load, bidirectional hover, page switching
- Điều không làm

**Phân bổ 19 highlights:**

| Page | Highlights | Tính vào CR Score? |
|------|------------|-------------------|
| Homepage | #1–5 (5 highlights) | ✅ Có (8/10) |
| Product Detail | #6–15 (10 highlights) | ❌ Applied only |
| Cart | #16–17 (2 highlights) | ✅ Có (8/10) |
| Checkout | #18–19 (2 highlights) | ❌ Applied only |

**Logic CR Score:** Chỉ tính từ Homepage + Cart — 2 trang buyer nhìn nhiều nhất. Product Detail và Checkout có optimizations nhưng mang tính kỹ thuật (bundle logic, form fields), không quy ra điểm trực quan được.

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

### A. Loading Screen ← **DONE — session 5+6**
Đã implement trong `signup.html` Screen 5. Navigation flow hoàn chỉnh: Loading phase 3 (boost) 100% → 600ms → Store Preview screen.

### B. Store Preview Screen — UI ← **DONE session 6+7**
Đã implement đầy đủ 17 sections. Xem chi tiết section "Store Preview Screen". Report bar đã fix đúng Figma (session 7).

### C. Store Preview — CR Tooltips ← **TIẾP THEO — ưu tiên cao**
Store preview hiện là pure UI. Cần thêm:
1. **Hover tooltips trên mỗi CR element** — `.sp-cr-tag` đã có vị trí, cần gắn tooltip content
   - Tooltip content đã có đầy đủ trong `store-screen-requirements.md` (19 highlights)
   - Tooltip design: dark bg, 280–320px width, xuất hiện 150ms sau hover, fade in
2. **Tab bar** (Homepage / Product Page / Checkout) — hiện chỉ có Homepage
3. **Product Page tab** — cần code từ Figma node `1065:24834` (PDP)
4. **Checkout tab** — cần code từ Figma node `2489:4915`

### D. Store Preview — Interaction (lower priority)
- Slideshow arrows (prev/next) — hiện chỉ visual
- Carousel navigation trong Best Selling section
- Real product images thay thế picsum.photos placeholders

### E. Store Screen spec — apply tooltip content mới ← **Cần làm trước khi implement tooltips**
Tooltips Homepage (13) và PDP (13) đã được viết lại theo ngôn ngữ seller trong session 5. **Cần apply vào `store-screen-requirements.md`** — thay thế descriptions cũ. Checkout tooltips (#18–19) vẫn estimated.

### F. Loading Screen spec cho POD và Niche Dropship
Hiện chỉ có spec cho General Dropship. POD và Niche Dropship cần script riêng:
- POD: nhấn vào quality proof, mockup, return policy
- Niche Dropship: nhấn vào expertise signals, community proof

---

## 10. Context bổ sung

- Business model được chọn ở sign up là: Print on Demand / General Dropship / Niche Dropship — không đi sâu vào ngành hàng cụ thể
- Benchmark CR tham khảo: POD 1.5–2.5% | General Dropship 1.8–3.2% | Niche Dropship 2.0–4.0%
- Giai đoạn đầu chưa có internal data → dùng industry benchmark (Shopify, Baymard Institute, Klaviyo) và cite source rõ ràng
- CLAUDE.md của project chứa các skill UX: Hick's Law, 10 Usability Heuristics, Don't Make Me Think, Judge UI, Frontend Design — agent mới nên đọc file đó trước khi làm bất kỳ task design nào
- **Figma REST API access:** Dùng `curl -s -H "X-Figma-Token: <token>" "https://api.figma.com/v1/files/<filekey>/nodes?ids=<node-id>"`. Token đã config trong `~/.claude/settings.json` — confirmed working trong session 5.
- **figma-console-mcp ≠ Figma REST reader:** `figma-console-mcp` là Figma Desktop Bridge — cần Figma Desktop app mở + plugin "Figma Desktop Bridge" active. Không dùng được để đọc file URL. Để đọc design data, dùng Figma REST API trực tiếp (xem trên).
- **Tooltip language direction (session 5):** Tooltips phải viết theo ngôn ngữ seller — mô tả bằng hành vi khách và kết quả tiền/đơn hàng cho seller, không dùng jargon UX kỹ thuật (không nói "loss aversion", "cognitive load", "above-the-fold"). Descriptions ngắn 1 dòng.
- **Convert Core Dashboard Design file:** `sj4QXXjuZKaEZOKcONXshG` — file chứa store pages (Homepage, PDP, Cart, Checkout)
  - Homepage node: `1064:20586`
  - Product Detail node: `1065:24834`
  - Checkout node: `2489:4915` (chưa audit)
- Figma board target segments: `https://www.figma.com/board/SXEdFC21sjPRzCBTBJ0svv/ConvertCore-Concept---Plan`
  - General Store Dropshipping: node `2979-12427`
  - Social Community Seller: node `2979-12333`
  - Niche/Brand Builder: node `2979-12371`
  - Niche/Brand Builder Specific: node `2979-12404`
