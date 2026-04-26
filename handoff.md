# Handoff Document — Onboarding Flow Brainstorm
**Project:** Web Builder / Store Builder SaaS
**Ngày cập nhật:** 2026-04-26
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

### `signup.html` ← **Live implementation**
Màn hình Sign up đã implement. Split layout: form trái + animated template gallery phải.
Assets dùng: `assets/signup-thumbs/` (9 thumbnails), `assets/figma-graphic-cropped/`.

### `onboarding-step1.html` ← **Live implementation**
Màn hình Step 1 — chọn Business Model. Đã implement đầy đủ:
- 4 cards: General Dropship, Niche Store, Print-on-Demand, Other
- Selection state với checkmark animation
- Continue button disabled cho đến khi chọn
- Assets: `assets/business-model-thumbs/` (4 ảnh: general-dropship.png, niche-dropship.png, print-on-demand.png, other.png)

**Copy đã được confirm:**

| Card | Tagline | Description |
|------|---------|-------------|
| General Dropship | Sell anything. Move fast. | Sell across any category, built for speed. |
| Niche Store | Go deep. Build a brand. | One niche, all in. Built for brand loyalty. |
| Print-on-Demand | Your design. Printed on demand. | Your designs on products — no inventory needed. |
| Other | Something else in mind. | Not sure which model fits? We'll keep it flexible. |

### `loading-screen-spec.md`
Spec đầy đủ cho loading screen, bao gồm layout suggestion. Dùng khi muốn có reference layout cụ thể.

### `loading-screen-spec-v2.md` ← **Dùng cái này để handoff cho AI designer**
Spec layout-free — chỉ có goal, content, behavior, script. Dùng khi muốn AI designer tự quyết định layout.

Nội dung v2 gồm:
- Mục đích màn hình
- 4 thành phần bắt buộc (Brand context, Tiến trình tổng thể, Activity log, Reveal State)
- Script 13 items đầy đủ cho Dropship
- Micro-copy guidelines + bảng "tránh/dùng"
- Timing table
- Điều không làm
- Edge cases (loading nhanh/chậm/lỗi)

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

## 8. Onboarding Step 2 & 3 — Copy đã quyết định

### Step 2 — Store Name

**Title:** `What is your store name?`

**Description:** `We'll use this to brand your store — logo, header, and more.`

**Mục đích:** Hệ thống dùng tên này để cá nhân hóa logo và branding của store được gen ra.

---

### Step 3 — Store Goal

**Title:** `What is your goal for your store?`

**Description:** `We'll set up your store's layout and features to match this.`

**Mục đích:** Hệ thống dùng goal này để config template, features, và defaults phù hợp.

**UI:** Free-text input + 10 suggestion chips bên dưới (multi-select). User có thể gõ tự do hoặc click chọn nhanh.

**10 Suggestion chips (theo thứ tự):**
```
Test and find winning products
Launch product pages faster
Turn followers into buyers
Capture more sales when posts go viral
Increase conversion rate
Increase average order value
Build a brand people come back to
Look and convert like a Shopify store
Automate product content at scale
Scale revenue without platform limits
```

**Nguồn gốc chips:** Distill từ 4 target segment goals — mỗi chip chạm đúng 1 job-to-be-done thực tế của ít nhất 1 segment.

---

## 9. Phần chưa làm — cần tiếp tục

### A. Implement Step 2 — Store Name (`onboarding-step2.html`) ← **Tiếp theo**
Copy đã có. Cần implement HTML theo design system hiện tại.
- Text input với placeholder gợi ý (e.g. "e.g. Trendie, PawShop...")
- Validation: không để trống
- Continue button disabled khi input trống

### B. Implement Step 3 — Store Goal (`onboarding-step3.html`) ← **Tiếp theo**
Copy và 10 chips đã có. Cần implement HTML:
- Free-text input (optional hay required — cần quyết định)
- 10 chips dạng toggle, multi-select
- Chips được chọn → append vào input hoặc lưu riêng

### C. CR Analysis cho Product Page (PDP) — ưu tiên cao
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
