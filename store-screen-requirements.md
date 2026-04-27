# Store Preview Screen — Requirements Document
**Screen:** Store Preview & CR Trust Screen
**Position trong flow:** Loading Screen → **[Màn này]** → Guided Refinement → Publish
**Version:** 1.0
**Mục đích file:** Requirements document để AI gen UI — layout-free, goal-driven

---

## 1. Bài toán cần giải quyết

**Trust problem:** User vừa xem 3 phase gen store trên loading screen. Họ đã nghe về CR & AOV optimization — nhưng chưa tin. Chưa tin vì chưa thấy bằng chứng cụ thể.

**Giải pháp của màn hình này:** Cho user xem chính store của họ, với từng element CR/AOV được chỉ ra và giải thích bằng số liệu cụ thể. Trust được build qua **recognition** — user thấy đúng element đã được nhắc đến, không phải qua claim chung chung.

**Kết quả mong đợi sau khi user xem xong màn này:**
> *"Store này không chỉ đẹp — nó được setup đúng cách để bán hàng. Tôi tin điều này vì tôi tự thấy từng chi tiết."*

---

## 2. Hai mục tiêu song song

| Mục tiêu | Cách đạt được |
|----------|---------------|
| **Prove CR/AOV** | Highlight từng element, tooltip giải thích WHY bằng số liệu |
| **Give ownership** | User navigate store như một buyer thật — họ *trải nghiệm* store, không chỉ nhìn screenshot |

---

## 3. Layout tổng thể

```
┌──────────────────────────────────────────────────────────────┐
│  [Logo]                               [Back]  [Publish →]    │ ← Top bar
├──────────────────────────────────────────────────────────────┤
│                                                              │
│  ┌──────────────────────────────────────────────────────┐   │
│  │  CR Score: 84/100  ·  Top 20% Dropship  ·            │   │ ← Assessment bar
│  │  19 optimizations applied  ·  [See full report ↓]    │   │   (compact, sticky top)
│  └──────────────────────────────────────────────────────┘   │
│                                                              │
│  Homepage  │  Product Detail  │  Cart  │  Checkout           │ ← Page tabs
│  ──────────                                                  │
│                                                              │
│  ┌──────────────────────────────────────────────────────┐   │
│  │                                                      │   │
│  │              STORE PREVIEW                           │   │ ← Store preview
│  │         (full buyer experience)                      │   │   (dominant area)
│  │                                                      │   │
│  │   [⚡CR] highlighted elements overlaid              │   │
│  │                                                      │   │
│  └──────────────────────────────────────────────────────┘   │
│                                                              │
└──────────────────────────────────────────────────────────────┘
```

**Nguyên tắc layout:**
- Store preview chiếm **tối thiểu 75% màn hình** — đây là nhân vật chính
- Assessment bar compact, không chiếm quá 48-56px chiều cao
- Tabs điều hướng giữa các trang theo buyer journey
- Không có sidebar — highlights nằm trực tiếp trên store

---

## 4. Assessment Bar (đánh giá tổng quan)

Luôn visible, sticky top ngay dưới top bar. Compact và scannable trong 3 giây.

**Các thông tin hiển thị:**

| Element | Content | Mục đích |
|---------|---------|----------|
| CR Score | `CR Score: 84/100` | Số hóa giá trị USP |
| Benchmark | `Top 20% Dropship stores` | Context để số có nghĩa |
| Count | `19 optimizations applied` | Prove thoroughness |
| CTA phụ | `See full report ↓` | Expand detail nếu cần |

**Expand state (khi click "See full report"):**
Drawer hoặc panel mở ra phía dưới, show breakdown theo từng page:

```
Homepage         ████████░░  8/10    5 optimizations  ← có score
Cart             ████████░░  8/10    2 optimizations  ← có score
Product Detail                      10 optimizations  ← applied, không score
Checkout                             2 optimizations  ← applied, không score
```

**Lý do không tính score cho Product Detail và Checkout:**
CR Score phản ánh chất lượng thiết kế mà seller đánh giá được ngay — Homepage và Cart là 2 trang buyer nhìn nhiều nhất và dễ so sánh nhất. Product Detail và Checkout có optimizations nhưng mang tính kỹ thuật sâu hơn (bundle logic, form field count) — không phù hợp để quy ra điểm trực quan.

**Score thay đổi theo business model:**

| Business Model | Score | Benchmark |
|----------------|-------|-----------|
| General Dropship | 84/100 | Top 20% Dropship stores |
| Niche Store | 79/100 | Top 25% Niche stores |
| Print-on-Demand | 81/100 | Top 22% POD stores |
| Other | 76/100 | Top 30% among new stores |

---

## 5. Page Tabs — Buyer Flow Navigation

User điều hướng giữa các trang theo **thứ tự buyer journey**, không phải theo cấu trúc website:

```
Homepage  →  Product Detail  →  Cart  →  Checkout
```

**Behavior:**
- Tab active: underline hoặc background highlight rõ
- Chuyển tab: transition mượt, không reload
- Mỗi tab load đúng page preview tương ứng
- Highlights tự động refresh khi chuyển tab — chỉ show highlights của page hiện tại

---

## 6. CR Highlight System

**Nguyên tắc cốt lõi:** Không bao giờ show nhiều hơn 1 highlight đang active cùng lúc. Store phải luôn readable như store thật — highlights chỉ nổi lên khi được chủ động trigger.

### 6.1 Ba trạng thái của highlight system

#### State 1 — Default (CR Badges)

Store hiển thị gần như bình thường. Mỗi element được optimize có một **badge nhỏ** gồm icon ⚡ (thunder/flash) kèm chữ "CR" đặt ở góc element:

```
┌──────────────────────────────────┐
│  [Element nội dung store]        │
│                        [⚡ CR]   │  ← Badge góc phải, compact
└──────────────────────────────────┘
```

**Badge anatomy:**
```
┌──────────┐
│ ⚡  CR   │  ← Icon thunder (12px) + label "CR" (10px, semibold)
└──────────┘
  Pill shape, height 20px, padding 4px 6px
```

- **Kích thước:** Height 20px, pill-shaped (border-radius 10px)
- **Icon:** Thunder/flash ⚡ 12px — thể hiện boost, energy, optimization
- **Label:** Chữ "CR" 10px semibold — meaningful, user hiểu ngay đây là conversion rate
- **Màu default:** Subtle — ví dụ amber/yellow mờ (#F59E0B ở opacity 60-70%) với text tối, không dùng màu accent full strength
- **Không có:** border ring, glow, số thứ tự — badge tự nói lên ý nghĩa mà không cần đánh số
- **Mục đích:** User nhận ra ngay "đây là điểm được optimize cho CR" — meaningful hơn số thứ tự trung tính

#### State 2 — Spotlight (khi hover pin hoặc element)

Khi user hover vào badge hoặc element tương ứng:

```
[Store bị dim 40% opacity — toàn bộ]

┌──────────────────────────────────┐  ← Border ring sáng lên
│  [Element] — 100% opacity, nổi  │
│                       [⚡ CR] ▲ │  ← Badge đổi màu full accent, scale 1.1x
└──────────────────────────────────┘
         ↓
┌─────────────────────────────────────┐
│  ⚡ [Tên optimization]              │  ← Tooltip xuất hiện
│  [Con số] — [Hệ quả buyer behavior] │
└─────────────────────────────────────┘
```

- **Dim background:** Toàn bộ store (trừ element active) → opacity 40%, blur nhẹ 1-2px
- **Element active:** opacity 100% + border ring + subtle glow
- **Badge:** Đổi từ subtle → full accent color (#F59E0B 100%), scale lên 1.1x
- **Transition:** 200ms ease-out
- **Chỉ 1 spotlight tại 1 thời điểm** — hover element mới tắt spotlight cũ ngay

#### State 3 — Tour Mode (guided walkthrough)

Khi user click `Take a tour` trên assessment bar — hệ thống tự động dẫn qua từng optimization:

```
Assessment bar: [CR Score 84/100] · [19 optimizations] · [Take a tour →]

Trong tour:
┌────────────────────────────────────────────┐
│  ① / 19        [Skip]        [Next →]      │  ← Tour controls (bottom bar)
└────────────────────────────────────────────┘
```

- Auto-scroll store đến element tiếp theo
- Spotlight effect giống State 2
- Progress: `② / 19` hiển thị trên tour control bar
- `Skip` thoát khỏi tour về State 1
- `Next →` chuyển spotlight sang optimization tiếp theo
- Tour không loop — kết thúc ở optimization cuối, controls ẩn đi

---

### 6.2 Tooltip spec (áp dụng cho cả Spotlight và Tour)

- **Trigger:** Hover vào pin số hoặc element — delay 150ms để tránh flicker
- **Position:** Bên cạnh element, không che khuất content — tự động flip nếu gần edge
- **Size:** Width 280–320px, height hug content
- **Style:** Dark background (#1a1a1a), text trắng, corner radius 8px, shadow elevation
- **Dismiss:** Mouse out khỏi element + tooltip vùng (hover leave)

**Cấu trúc nội dung tooltip:**

```
┌─────────────────────────────────────┐
│  ⚡ [Tên optimization]              │  ← Title, bold
│                                     │
│  [Con số/nguyên lý] — [Hệ quả      │  ← Explanation, 1-2 câu
│   với buyer behavior]               │
└─────────────────────────────────────┘
```

**Ví dụ:**
```
⚡ Price Anchoring
$90 → $45 anchor tạo perceived value — buyer
cảm nhận "đang được deal" ngay cả khi không
so sánh với sản phẩm khác.
```

---

### 6.3 Reveal animation khi vào màn hình

Pins xuất hiện lần lượt sau khi store load xong — không cùng lúc:
- Delay: 400ms sau khi store preview load
- Stagger: Mỗi pin fade in cách nhau 60ms, từ trên xuống dưới
- Animation: Fade in + scale 0.5 → 1.0 (pop in nhẹ)
- Tổng thời gian reveal: ~1200-1500ms cho 19 pins

---

## 7. CR Highlight Content — theo từng page

### 7.1 Homepage (5 highlights)

**Highlight 1 — Hero Value Proposition**
- Element: Hero section text/headline
- Tooltip: `68% buyer quyết định ở lại hay rời đi trong 5 giây đầu — value proposition hiện ngay above-the-fold, không cần scroll`

**Highlight 2 — Trust Badges Bar**
- Element: Dải trust badges ngay dưới hero (Free Shipping · Secure Payment · Free Return · Tracked Shipping)
- Tooltip: `4 trust signals above-the-fold giải quyết lo ngại trước khi buyer scroll xuống — giảm bounce rate tại điểm cao nhất`

**Highlight 3 — Deal Section với Anchor Price**
- Element: Section Deal/Sale với giá gốc gạch ngang + giá sale + countdown
- Tooltip: `Anchor price + scarcity + countdown — 3 conversion triggers kết hợp. Anchor price tăng perceived value; scarcity tăng urgency; countdown kích hoạt loss aversion`

**Highlight 4 — CTA Button**
- Element: Button Add to Cart / Shop Now chính
- Tooltip: `Contrast ratio 5.2:1 với background — đạt ngưỡng tối ưu click-through. Button mờ là nguyên nhân phổ biến nhất của low CTR`

**Highlight 5 — Social Proof**
- Element: Review stars hoặc "Best Selling" label
- Tooltip: `Social proof signal trên homepage giảm cold-start anxiety — buyer mới thấy người khác đã mua trước khi vào product page`

---

### 7.2 Product Detail Page (10 highlights — applied, không tính vào CR Score)

**Highlight 6 — Anchor Price + % OFF Badge**
- Element: Giá sale ($22.5) + giá gốc gạch ngang ($45.99) + badge "50% OFF"
- Tooltip: `Giá gốc gạch ngang tạo reference point — buyer cảm thấy đang được deal mà không cần so sánh bên ngoài. Conversion trigger mạnh nhất với impulse purchase`

**Highlight 7 — Discount Countdown**
- Element: Countdown timer "Discount only available in 30m : 40s" ngay dưới giá
- Tooltip: `Đồng hồ đếm ngược tại điểm quyết định kích hoạt loss aversion — buyer phải mua ngay, không thể trì hoãn. Tăng add-to-cart rate 10-15%`

**Highlight 8 — Stock Scarcity**
- Element: "X items left in stock" đặt ngay cạnh CTA
- Tooltip: `Số stock cụ thể cạnh CTA cắt giai đoạn so sánh — buyer mua ngay vì sợ hết hàng. Hiệu quả nhất khi đặt gần nút mua, không phải cuối trang`

**Highlight 9 — Review Stars gần Price**
- Element: "(4.5) 200 reviews" đặt ngay trên block giá
- Tooltip: `Social proof đặt trên giá = buyer thấy trust signal trước khi đọc giá — giảm price objection. Đặt cuối trang mất 80% hiệu quả`

**Highlight 10 — Dual CTA (Add to Cart + Buy Now)**
- Element: Hai button "ADD TO CART" và "BUY NOW" cạnh nhau
- Tooltip: `Add to Cart cho buyer muốn tiếp tục xem; Buy Now cho buyer impulse muốn checkout ngay. Hai path song song tăng CR cho cả 2 loại buyer`

**Highlight 11 — Estimated Delivery Date**
- Element: "Arrive in [date range] — Delivery to [country]" ngay dưới CTA
- Tooltip: `Ngày giao hàng cụ thể xóa lo ngại "không biết bao giờ nhận" — giảm câu hỏi support và checkout abandon ~6%`

**Highlight 12 — Accessories Cross-sell**
- Element: Section "Accessories" với sản phẩm bổ sung + nút "Add" 1-click
- Tooltip: `Buyer đã quyết mua product chính là thời điểm sẵn lòng nhất để add thêm. Add 1-click không break flow — accessories tăng AOV trung bình 8-12%`

**Highlight 13 — Volume Discount Tiers**
- Element: Section "Buy more Save more" với 3 tier: Buy 2 / Buy 3 / Buy 4
- Tooltip: `3 tier giá rõ ràng để buyer tự tính lợi và chủ động tăng số lượng. Không cần khuyến mãi — buyer tự đẩy AOV lên 1.5-2x`

**Highlight 14 — Frequently Bought Together**
- Element: Section "Frequently bought together" — bundle pre-selected + Total price hiển thị sẵn
- Tooltip: `Bundle pre-selected với total price = buyer thấy ngay tiết kiệm được bao nhiêu khi mua cùng. Bundle acceptance rate 25-35% — leverage AOV cao nhất trên product page`

**Highlight 15 — Rating Breakdown + Reviews có ảnh**
- Element: Section "Our Customers Love Us" — bar chart % từng sao + reviews kèm ảnh buyer thật
- Tooltip: `Bar chart 92% five-star + review có ảnh = bằng chứng mạnh nhất trước decision cuối. Review có ảnh được tin gấp 3x review chỉ có text`

---

### 7.3 Cart (2 highlights)

**Highlight 16 — Free Shipping Progress**
- Element: Thanh tiến trình "Add $X more for free shipping"
- Tooltip: `Free shipping threshold progress tăng AOV ~15% — buyer chủ động thêm sản phẩm để đạt ngưỡng miễn phí vận chuyển`

**Highlight 17 — Upsell / Cross-sell**
- Element: Section "You may also like" hoặc "Frequently bought together"
- Tooltip: `Post-cart upsell tăng AOV mà không interrupt purchase flow — buyer đã commit mua, đây là thời điểm đề xuất hiệu quả nhất`

---

### 7.4 Checkout (2 highlights — applied, không tính vào CR Score)

**Highlight 18 — Rút gọn Form Fields**
- Element: Checkout form với số fields tối thiểu
- Tooltip: `Mỗi field thừa tăng abandon rate ~10% — form này chỉ giữ 7 fields thiết yếu, loại bỏ mọi thứ không cần để tính phí`

**Highlight 19 — Trust Badges gần Payment Button**
- Element: Trust badges và payment logos ngay cạnh nút thanh toán
- Tooltip: `High-anxiety moment — buyer lo nhất về bảo mật thanh toán tại đây. Trust signals đặt đúng vị trí này giảm checkout abandon ~15%`

---

## 8. Interaction Design

### 8.1 Ba cách user khám phá highlights

| Cách | Mô tả | Phù hợp với |
|------|-------|------------|
| **Passive** | Thấy pins số rải trên store, tự tò mò hover | User chủ động, muốn tự khám phá |
| **Active hover** | Hover trực tiếp vào pin hoặc element → spotlight | User biết mình muốn xem gì |
| **Guided tour** | Click `Take a tour` → hệ thống dẫn qua từng cái | User muốn được dẫn dắt, không muốn tự scan |

Không ép user chọn — cả 3 cách đều hoạt động song song.

### 8.2 Assessment bar + Tour entry point

Assessment bar luôn có nút `Take a tour (19) →` ở bên phải:
- Click → tour bắt đầu từ optimization đầu tiên của page đang active
- Nếu đang ở Product Detail tab → tour bắt đầu từ optimization #6
- Tour control bar xuất hiện ở bottom của store preview (không che nội dung store)

### 8.3 Navigation trong store preview
- User có thể click link/button trong store preview để navigate như buyer thật
- Hoặc dùng tab bar phía trên để jump thẳng đến page cần xem
- Scroll trong preview: scroll riêng, không scroll cả màn hình
- Khi chuyển tab: pins của tab mới fade in với stagger animation, spotlight reset về inactive

### 8.4 Mobile behavior
- Assessment bar collapse xuống còn CR Score + benchmark + icon tour
- Tabs: scroll horizontally nếu không đủ chỗ
- Store preview: scale down, có thể swipe giữa pages thay vì tab click
- Pins: kích thước giữ nguyên 16-18px (không scale nhỏ hơn — tap target)
- Spotlight + tooltip: trigger bằng tap thay vì hover; tap ngoài vùng để dismiss

---

## 9. Top Bar

**Left:** Logo sản phẩm

**Right (2 actions):**
- `← Back` — quay lại để chỉnh sửa (ghost button)
- `Publish store →` — CTA primary, màu nổi bật

**Lý do Publish ở đây:**
User vừa được thuyết phục bởi highlights. Đây là "peak motivation moment" — CTA phải có mặt ngay lúc này. Không bắt user scroll xuống hay tìm nút ở chỗ khác.

---

## 10. Điều không làm

- **Không dùng sidebar panel** — highlights phải inline trên store, không tách rời
- **Không show border ring trên mọi element cùng lúc** — default chỉ có CR badge subtle, border ring chỉ xuất hiện khi spotlight active
- **Không spotlight nhiều hơn 1 element cùng lúc** — khi hover element mới, spotlight cũ tắt ngay
- **Không dùng màu accent mạnh cho pins ở default state** — pins phải subtle, không tranh attention với store
- **Không chặn store navigation** — user phải click được link trong preview như buyer thật
- **Không dùng số CR Score cố định** — score phải vary theo business model và số optimization thực tế
- **Không dùng ngôn ngữ marketing trong tooltip** — chỉ dùng số liệu và nguyên lý cụ thể
- **Không ép user vào tour** — tour là optional, không phải required walkthrough
- **Không ẩn Publish button** — phải visible ngay khi user vào màn hình, không chỉ xuất hiện sau khi scroll xong

---

## 11. Auto Layout — yêu cầu khi AI gen UI

### Cấu trúc frame

```
Screen Frame (Vertical Auto Layout)
├── Top Bar (Horizontal Auto Layout, space-between)
├── Assessment Bar (Horizontal Auto Layout, sticky)
├── Tab Row (Horizontal Auto Layout)
└── Store Preview Container (Fill remaining height)
    ├── Store iframe / preview frame
    └── Highlights Layer (absolute, overlay on preview)
        ├── Highlight Ring (absolute per element)
        ├── CR Badge (absolute, top-right of element)
        └── Tooltip (absolute, position-aware)
```

### Quy tắc bắt buộc

**Top Bar:**
- Height: Fixed 56px
- Padding: 16px horizontal
- Direction: Horizontal, space-between

**Assessment Bar:**
- Height: Fixed 48px
- Background: Surface/subtle — phân biệt với top bar nhưng không nặng
- Padding: 16px horizontal
- Direction: Horizontal, gap 16px giữa items
- Sticky: top = 56px (ngay dưới top bar)

**Tab Row:**
- Height: Fixed 44px
- Direction: Horizontal, gap 0 — tabs fill đều hoặc hug content với padding 16px
- Underline active tab: 2px, primary color

**Store Preview Container:**
- Width: Fill container
- Height: Fill remaining screen height (screen height - 56px - 48px - 44px)
- Overflow: hidden (scroll within preview only)
- No padding — store preview edge-to-edge

**Highlights Layer:**
- Position: Absolute, 0/0/0/0 (cover store preview)
- Pointer events: None trên layer, chỉ enable trên từng badge/ring
- Z-index: Above store content, below tooltip

**Tooltip:**
- Position: Absolute, calculated từ badge position
- Width: Fixed 280-320px
- Height: Hug content
- Padding: 12px 16px
- Z-index: Highest

---

## 12. Edge Cases

**Store chưa load xong:**
Hiển thị skeleton của store preview — giữ đúng proportions. Badges chỉ xuất hiện sau khi preview fully loaded.

**User tắt highlights rồi muốn bật lại:**
Toggle button luôn visible, không ẩn theo state.

**Tooltip gần edge màn hình:**
Auto-flip direction — nếu tooltip sẽ bị cắt bên phải, render sang trái của badge thay vì phải.

**User click Publish trước khi xem hết:**
Không chặn — không có forced walkthrough. User tự quyết định khi nào sẵn sàng.

**Mobile — hover không có:**
Tooltip trigger bằng tap vào badge. Tap lần 2 hoặc tap ra ngoài để dismiss.
