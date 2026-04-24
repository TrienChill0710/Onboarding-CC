# Onboarding Step 1 — Business Model Selection
**Screen:** Chọn loại hình kinh doanh
**Position trong flow:** Sign up → **[Màn này]** → Step 2 → Step 3 → Loading → Store
**Version:** 1.0 — Layout-free (dành cho AI designer tự quyết định layout)

---

## 1. Mục đích màn hình

Màn hình đầu tiên user thấy sau khi tạo tài khoản. Hệ thống cần biết business model để chọn đúng template — đây là quyết định ảnh hưởng đến toàn bộ store.

**Mục tiêu chính:** User chọn được business model phù hợp mà không phải nghĩ nhiều. Mỗi option phải tự giải thích được: user đọc xong biết ngay cái nào là mình, không cần compare lâu.

**Nguyên tắc thiết kế:** Recognition over recall — image preview của template cho user nhìn thấy kết quả trước khi chọn. Quyết định bằng mắt, không bằng suy luận.

---

## 2. Step indicator

Màn hình này là bước 1 trong chuỗi 3 bước nhập thông tin. Hiển thị step progress ở trên cùng:

- Dạng: `Step 1 of 3` hoặc progress dots/bar — designer tự chọn visual
- Mục đích: user biết còn bao nhiêu bước, không bị cảm giác form vô tận
- Không cần label cho step 2, 3 — chỉ cần biết vị trí hiện tại

---

## 3. Headline & Subtext

**Headline:** `What kind of store are you building?`

**Subtext:** `We'll set up the right template and tools for your business model.`

Nguyên tắc micro-copy:
- Headline: hỏi thẳng, không giải thích dài
- Subtext: giải thích TẠI SAO câu hỏi này quan trọng — user hiểu đây không phải câu hỏi thừa

---

## 4. Các lựa chọn

4 option, hiển thị dạng card. Thứ tự cố định như sau:

### Option 1 — General Dropship
- **Label:** General Dropship
- **Tagline:** Sell anything. Move fast.
- **Description:** Phù hợp để test sản phẩm nhiều ngành hàng, tối ưu cho tốc độ ra thị trường.
- **Image:** Preview screenshot của General Dropship template (template Trendie)
- **Tag:** `Most popular` — hiển thị badge góc trên card

### Option 2 — Niche Store
- **Label:** Niche Store
- **Tagline:** Go deep. Build a brand.
- **Description:** Tập trung một ngách chuyên sâu. Thích hợp để xây thương hiệu lâu dài và loyal customer.
- **Image:** Preview screenshot của Niche Store template
- **Tag:** `Best for branding`

### Option 3 — Print-on-Demand
- **Label:** Print-on-Demand
- **Tagline:** Your design. Printed on demand.
- **Description:** Bán sản phẩm in ấn (áo, mug, poster...) mà không cần giữ hàng tồn kho.
- **Image:** Preview screenshot của POD template
- **Tag:** `No inventory needed`

### Option 4 — Other
- **Label:** Other
- **Tagline:** Something else in mind.
- **Description:** Chưa chắc về model? Chọn cái này — chúng tôi sẽ setup store linh hoạt nhất.
- **Image:** Placeholder generic / abstract visual — không dùng template cụ thể
- **Tag:** *(không có tag)*

---

## 5. Card design — yêu cầu bắt buộc

Mỗi card phải có đủ 4 thành phần sau (layout tự chọn):

1. **Image area (trên cùng):** Preview của template website — đây là thứ user nhìn đầu tiên. Tỉ lệ 16:10 hoặc 4:3, crop từ phần hero/above-the-fold của template. Không dùng mockup device (không bọc trong laptop/phone) — crop thẳng vào UI.

2. **Label + Tag:** Tên option (font lớn, bold) + badge tag nếu có. Tag không phải marketing fluff — phải là thông tin thực sự giúp user phân biệt.

3. **Tagline:** 1 câu ngắn, dưới 6 chữ, capture essence. Dùng tone active và direct.

4. **Description:** 1-2 câu. Nói rõ phù hợp với ai và dùng để làm gì. Không dùng buzzword.

---

## 6. Selection state

**Unselected (default):** Border neutral, background trắng/subtle
**Hover:** Border highlight nhẹ + subtle shadow lift — signal interactivity
**Selected:** Border màu primary rõ ràng + checkmark icon ở góc + background tint nhẹ

Chỉ chọn được 1 option tại một thời điểm (single select). Khi chọn option mới, option cũ tự deselect.

---

## 7. Next button

**Label:** `Continue →`

**Trạng thái disabled:** Khi chưa chọn option nào — button mờ, không clickable. Không cần tooltip giải thích — visual đủ rõ.

**Trạng thái active:** Ngay khi user chọn 1 option, button bật sáng và clickable — không cần user scroll xuống tìm button nên đặt button ở vị trí cố định (sticky bottom hoặc luôn visible).

**Vị trí:** Dưới grid card, căn phải hoặc căn giữa — designer quyết định.

---

## 8. Behavior khi click Continue

1. Ghi nhận business model đã chọn vào session
2. Transition sang Step 2
3. Không có loading state — transition phải instant (< 200ms)

---

## 9. Điều không làm

- **Không dùng radio button hoặc dropdown** — card visual là cần thiết để user recognition, không thể thay bằng text list
- **Không để user tiếp tục mà không chọn** — business model là required, toàn bộ store phụ thuộc vào đây
- **Không giải thích quá dài** — mỗi card description tối đa 2 câu; nếu user cần đọc nhiều hơn là design đang fail
- **Không dùng jargon** — "General Dropship" là ngành hiểu được, nhưng description phải dùng ngôn ngữ user, không phải operator
- **Không ẩn option "Other"** — dù ít người dùng, nó giảm anxiety cho người không chắc chắn và tránh bounce

---

## 10. Edge cases

**User chưa chọn và muốn bỏ qua:** Không có skip button — không được bỏ qua bước này.

**Mobile:** Grid card chuyển sang single column. Image area giữ nguyên tỉ lệ. Card phải đủ lớn để tap thoải mái (min height 180px/card). Next button sticky bottom.

**Image chưa load xong:** Hiển thị skeleton placeholder trong image area — không để blank trắng.

---

## 11. Yêu cầu Auto Layout khi AI gen thiết kế

Khi AI (Stitch, Figma AI, hoặc agent tương tự) tạo màn hình này, **bắt buộc áp dụng Auto Layout** cho toàn bộ cấu trúc. Không được dùng absolute positioning trừ trường hợp được ghi rõ bên dưới.

### 11.1 Cấu trúc frame tổng thể

```
Screen Frame (Vertical Auto Layout)
├── Step Indicator (Horizontal Auto Layout)
├── Header Group (Vertical Auto Layout)
│   ├── Headline
│   └── Subtext
├── Card Grid (2-column Grid hoặc Horizontal Wrap Auto Layout)
│   ├── Card 1 — General Dropship
│   ├── Card 2 — Niche Store
│   ├── Card 3 — Print-on-Demand
│   └── Card 4 — Other
└── Footer / CTA Area (Horizontal Auto Layout)
    └── Continue Button
```

### 11.2 Quy tắc Auto Layout cho từng tầng

**Screen Frame (root):**
- Direction: Vertical
- Padding: 24px top/bottom, 20px left/right (mobile); 40px top/bottom, 48px left/right (desktop)
- Gap: 32px giữa các section chính
- Width: Fill container / 100vw
- Height: Hug contents (không fixed height — tránh overflow ẩn)

**Card Grid:**
- Desktop: 2-column grid, gap 16px horizontal / 16px vertical
- Mobile: 1-column, gap 12px
- Không dùng absolute positioning để xếp card

**Mỗi Card (Vertical Auto Layout):**
- Direction: Vertical
- Padding: 0 (image full-bleed trên cùng) — padding 16px cho phần text bên dưới
- Gap: 8px giữa label+tag / tagline / description
- Width: Fill container
- Height: Hug contents — không fixed height card
- Corner radius: 12px

**Image Area trong Card:**
- Chiều rộng: Fill container (stretch 100% theo chiều ngang card)
- Chiều cao: Fixed — giữ tỉ lệ 16:10 theo width thực tế (dùng constraint hoặc aspect ratio)
- Clip content: ON — không để image tràn ra ngoài

**Label + Tag row (Horizontal Auto Layout):**
- Direction: Horizontal
- Alignment: Space between (label bên trái, tag bên phải)
- Gap: 8px
- Width: Fill container

**Footer / CTA Area:**
- Sticky bottom trên mobile: position fixed, background blur hoặc solid
- Desktop: cuối luồng, không sticky
- Padding: 16px top, 24px bottom (safe area aware)
- Continue button: căn phải trên desktop, full-width trên mobile

### 11.3 Spacing tokens — AI phải dùng đúng giá trị này

| Token | Giá trị | Dùng ở đâu |
|-------|---------|------------|
| `spacing-xs` | 4px | Gap giữa tag và text nhỏ |
| `spacing-sm` | 8px | Gap trong card text area |
| `spacing-md` | 16px | Padding trong card, gap giữa cards |
| `spacing-lg` | 24px | Padding screen horizontal |
| `spacing-xl` | 32px | Gap giữa sections |
| `spacing-2xl` | 48px | Padding screen desktop |

### 11.4 Resize behavior — bắt buộc test

AI phải verify rằng layout không vỡ khi:
- Thay đổi chiều rộng frame từ 375px → 1440px
- Text trong description dài hơn hoặc ngắn hơn 20%
- Badge tag bị ẩn (option "Other" không có tag) — row phải collapse đúng cách
- Image chưa load (skeleton placeholder giữ đúng kích thước, không collapse về 0px)

### 11.5 Những lỗi Auto Layout phổ biến AI hay mắc — phải tránh

- **Dùng fixed height cho card** → vỡ khi text wrap nhiều dòng
- **Absolute position badge tag** → tag tràn ra ngoài card khi resize
- **Không set "Fill container" cho image width** → image không stretch đúng
- **Quên clip content trên image** → image overflow khi aspect ratio không khớp
- **Footer không sticky trên mobile** → user không thấy nút Continue khi scroll
- **Gap và padding không dùng token** → spacing không nhất quán khi design system update
