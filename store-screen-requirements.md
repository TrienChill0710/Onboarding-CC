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
│  │  13 optimizations applied  ·  [See full report ↓]    │   │   (compact, sticky top)
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
| Count | `13 optimizations applied` | Prove thoroughness |
| CTA phụ | `See full report ↓` | Expand detail nếu cần |

**Expand state (khi click "See full report"):**
Drawer hoặc panel mở ra phía dưới, show breakdown theo từng page:

```
Homepage         ████████░░  8/10   5 optimizations
Product Detail   ███████░░░  7/10   4 optimizations
Cart             ████████░░  8/10   2 optimizations
Checkout         █████████░  9/10   2 optimizations
```

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

#### State 1 — Default (Quiet Pins)

Store hiển thị gần như bình thường. Chỉ có **chấm số nhỏ** ①②③ đặt ở cạnh mỗi element được optimize:

```
┌──────────────────────────────┐
│  [Element nội dung store]  ① │  ← Số nhỏ 16px, góc element
│                              │
└──────────────────────────────┘
```

- **Kích thước:** 16-18px diameter
- **Màu:** Neutral subtle (không dùng màu accent mạnh) — chỉ đủ để nhận ra, không đủ để distract
- **Không có:** border ring, badge ⚡CR, glow — store visual không bị chôn vùi
- **Mục đích:** User cảm nhận được scope ("có 5 thứ được optimize trên trang này") mà không bị overwhelm

#### State 2 — Spotlight (khi hover pin hoặc element)

Khi user hover vào pin số hoặc element tương ứng:

```
[Store bị dim 40% opacity — toàn bộ]

┌──────────────────────────────────┐  ← Border ring sáng lên
│  [Element] — 100% opacity, nổi  ①│  ← Số chuyển màu accent
│                                  │
└──────────────────────────────────┘
         ↓
┌─────────────────────────────────────┐
│  ⚡ [Tên optimization]              │  ← Tooltip xuất hiện
│  [Con số] — [Hệ quả buyer behavior] │
└─────────────────────────────────────┘
```

- **Dim background:** Toàn bộ store (trừ element active) → opacity 40%, blur nhẹ 1-2px
- **Element active:** opacity 100% + border ring + subtle glow
- **Pin số:** Đổi màu accent, scale lên nhẹ 1.1x
- **Transition:** 200ms ease-out
- **Chỉ 1 spotlight tại 1 thời điểm** — hover element mới tắt spotlight cũ ngay

#### State 3 — Tour Mode (guided walkthrough)

Khi user click `Take a tour` trên assessment bar — hệ thống tự động dẫn qua từng optimization:

```
Assessment bar: [CR Score 84/100] · [13 optimizations] · [Take a tour →]

Trong tour:
┌────────────────────────────────────────────┐
│  ① / 13        [Skip]        [Next →]      │  ← Tour controls (bottom bar)
└────────────────────────────────────────────┘
```

- Auto-scroll store đến element tiếp theo
- Spotlight effect giống State 2
- Progress: `② / 13` hiển thị trên tour control bar
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
- Tổng thời gian reveal: ~800-1000ms cho 13 pins

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

### 7.2 Product Detail Page (4 highlights)

**Highlight 6 — Price Anchoring**
- Element: Giá gốc gạch ngang cạnh giá sale
- Tooltip: `Giá gốc gạch ngang tạo reference point — buyer không cần so sánh bên ngoài để cảm thấy đang mua được giá tốt`

**Highlight 7 — Stock / Scarcity Indicator**
- Element: "Only X left in stock" gần CTA
- Tooltip: `Scarcity signal tăng add-to-cart rate ~12% — đặc biệt hiệu quả với impulse purchase. Phải đặt gần CTA, không phải cuối trang`

**Highlight 8 — Sticky Add-to-Cart Bar**
- Element: ATC bar cố định khi scroll
- Tooltip: `68% quyết định mua xảy ra sau khi đọc reviews — sticky ATC đảm bảo CTA luôn trong tầm với tại thời điểm quyết định`

**Highlight 9 — Review Stars gần Price**
- Element: Star rating đặt cạnh giá, không phải cuối trang
- Tooltip: `Proximity của social proof và price giảm price objection — buyer nhìn giá, thấy ngay trust signal. Đặt cuối trang mất 80% hiệu quả`

---

### 7.3 Cart (2 highlights)

**Highlight 10 — Free Shipping Progress**
- Element: Thanh tiến trình "Add $X more for free shipping"
- Tooltip: `Free shipping threshold progress tăng AOV ~15% — buyer chủ động thêm sản phẩm để đạt ngưỡng miễn phí vận chuyển`

**Highlight 11 — Upsell / Cross-sell**
- Element: Section "You may also like" hoặc "Frequently bought together"
- Tooltip: `Post-cart upsell tăng AOV mà không interrupt purchase flow — buyer đã commit mua, đây là thời điểm đề xuất hiệu quả nhất`

---

### 7.4 Checkout (2 highlights)

**Highlight 12 — Rút gọn Form Fields**
- Element: Checkout form với số fields tối thiểu
- Tooltip: `Mỗi field thừa tăng abandon rate ~10% — form này chỉ giữ 7 fields thiết yếu, loại bỏ mọi thứ không cần để tính phí`

**Highlight 13 — Trust Badges gần Payment Button**
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

Assessment bar luôn có nút `Take a tour (13) →` ở bên phải:
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
- **Không show border ring + badge ⚡ trên mọi element cùng lúc** — default chỉ có quiet pins số
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
