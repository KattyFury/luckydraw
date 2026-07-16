# LuckyDraw — Design Brief / Product Spec

> Mục đích: brief để **thiết kế lại giao diện** (UI/UX).  
> Giữ nguyên **logic sản phẩm & hành vi**; làm lại **nhìn và cảm giác**.  
> App hiện tại: [github.com/KattyFury/luckydraw](https://github.com/KattyFury/luckydraw) — mobile-first web app / PWA.

---

## 1. Sản phẩm là gì?

**LuckyDraw** là app **vòng quay may rủi** (spin wheel), dùng offline trên điện thoại, chủ yếu trong bối cảnh **đi nhậu / party / nhóm bạn**.

Người dùng:
1. Chọn **tab chủ đề** (ví dụ “Ăn nhậu”)
2. Bấm **Spin** (hoặc chạm vào bánh)
3. Xem **kết quả** (ví dụ “Uống 1 ly”)
4. Có thể **sửa** số ô + nội dung từng ô cho từng tab

Không có backend, không login, không tài khoản. Dữ liệu lưu **localStorage** trên máy.

---

## 2. Đối tượng & ngữ cảnh dùng

| | |
|---|---|
| **Ai** | Nhóm bạn Việt, 18–35, dùng iPhone/Android |
| **Khi nào** | Quán nhậu, party, họp nhóm — cầm 1 máy, mọi người nhìn |
| **Môi trường** | Ồn, ánh sáng kém, một tay, màn nhỏ |
| **Tâm trạng** | Vui, nhanh, hơi “nát”, không formal |

**Insight thiết kế:** UI phải **đọc được từ xa ~1–1.5m**, bấm trúng nút to, cảm giác **game / party**, không giống form admin hay dashboard.

---

## 3. Mục tiêu redesign (vì UI hiện tại xấu)

### Pain points hiện tại
- Layout “ghép grid 10 hàng” cứng, khoảng trống vô nghĩa (hàng 2 trống)
- Màu xám + xanh Tailwind generic → **không brand, không vibe nhậu**
- Vòng quay canvas phẳng, 9 màu solid → trông **giống demo HTML**, không “đồ chơi”
- Nút Spin / Edit / tab bar **không cùng một hệ ngôn ngữ visual**
- Result popup đơn điệu, thiếu khoảnh khắc “wow / drama”
- Desktop chỉ là khung 430px giữa nền đen — tạm ổn nhưng chưa có character
- Typography Barlow đậm nhưng hierarchy yếu

### Mục tiêu sau redesign
1. **Mobile full-screen đẹp** — cảm giác native app / PWA, không “trang web”
2. **Hero = vòng quay** — bánh xe là ngôi sao màn hình
3. **Party energy** — màu, ánh sáng, motion gợi vui / may rủi / nhậu
4. **Touch-first** — hit target ≥ 44–48px, không chi li
5. **Kết quả memorable** — popup thắng cuộc rõ, vui, dễ chụp màn hình
6. **Edit vẫn đơn giản** — không thành form phức tạp
7. **Icon sẵn có** `spin.png` (512×512) — dùng làm brand mark / app icon (đã có favicon + apple-touch-icon)

### Không phải mục tiêu
- Không thêm social feed, leaderboard, auth
- Không multiplayer realtime (chưa)
- Không đổi core mechanic quay (random + easing) trừ khi design đề xuất motion tốt hơn *vẫn random công bằng*

---

## 4. Tính năng bắt buộc (MUST keep)

### 4.1 Tabs (4 tab cố định)
| ID | Label hiển thị | Mặc định nội dung |
|----|----------------|-------------------|
| `an-nhau` | Ăn nhậu | Uống 1 ly · Bên trái uống · Bên phải uống · Đối diện uống · Chọn 1 người uống cùng · Miễn lượt · Đồng khởi |
| `custom1` | Custom 1 | Mục 1…4 |
| `custom2` | Custom 2 | Mục 1…4 |
| `custom3` | Custom 3 | Mục 1…4 |

- Chuyển tab → reset góc quay, ẩn result, vẽ lại bánh theo labels tab đó
- Không đổi tab khi đang spinning
- Mỗi tab **lưu labels riêng** (persist)

### 4.2 Vòng quay (Wheel)
- Số ô: **3 → 9** (min 3, max 9)
- Mỗi ô: màu riêng (palette 9 màu distinct, không 2 màu kề nhau “đụng” nhau)
- **Pointer / kim chỉ** hiện ở **góc dưới-trái** (droplet: vỏ đen + lõi đỏ) — design có thể **restyle** nhưng **giữ vị trí bên trái / dưới-trái** (đã quen UX)
- Chữ trên ô: **đọc theo hướng mép → tâm** (dễ đọc khi pointer trái) — giữ hướng này trừ khi redesign pointer
- Chạm **vào bánh** cũng = Spin
- Khi spinning: disable Spin, không cho edit / đổi tab

### 4.3 Spin
- Random công bằng mỗi ô
- Animation ~4–5s, ease-out, nhiều vòng quay trước khi dừng
- Dừng → segment thắng nằm dưới pointer → hiện **Result modal**

### 4.4 Result modal
Copy hiện tại (có thể polish wording, giữ tinh thần):
- Label nhỏ: **“Kết quả”**
- Text lớn: **nội dung ô thắng**
- Sub: **“1 2 3 dzô”**
- CTA: **OK** (đóng; tap ngoài backdrop cũng đóng)
- Esc đóng (desktop)

### 4.5 Edit modal
- Mở bằng nút **Edit** (chỉ tab đang active)
- Chọn số ô 3–9 (picker)
- Sửa text từng ô (max ~40 ký tự / ô)
- Preview màu swatch theo index
- **Hủy** / **Lưu** / Đóng
- Validate: ≥ 3 ô có nội dung
- Lưu → persist + vẽ lại bánh + rotation = 0

### 4.6 Persistence
- Key localStorage (có thể đổi version khi ship UI mới)
- Không cần cloud

### 4.7 PWA / shell
- `manifest`: name **LuckyDraw**, standalone, theme color
- Favicon + apple-touch-icon = **spin.png**
- Safe-area iOS (notch / home indicator)
- Full viewport width trên mọi kích thước (không khóa phone-frame 430px trên desktop)

---

## 5. Màn hình & luồng (IA)

Chỉ **1 màn chính** + **2 overlay**. Không multi-page.

```
┌─────────────────────────────────────┐
│  [Header / Edit]                    │
│                                     │
│           ◉ VÒNG QUAY               │
│          (pointer TL)               │
│                                     │
│            [ SPIN ]                 │
│                                     │
│  Ăn nhậu | Custom1 | Custom2 | C3   │  ← bottom tabs
└─────────────────────────────────────┘
         │                │
         ▼                ▼
   Edit Modal        Result Modal
```

### Flows
1. **Mở app** → tab “Ăn nhậu” + wheel default  
2. **Spin** → animate → result → OK → sẵn sàng quay tiếp  
3. **Đổi tab** → wheel đổi content  
4. **Edit** → chỉnh → Lưu → wheel update  

---

## 6. Layout hiện tại (tham chiếu, **được phá**)

Hiện dùng CSS grid **10 hàng bằng nhau** (`1fr × 10`):

| Hàng | Nội dung |
|------|----------|
| 1 | Edit (align phải) |
| 2 | *trống* |
| 3–7 | Vòng quay (5 hàng) |
| 8–9 | Nút Spin (căn giữa dọc) |
| 10 | Bottom tab bar full-bleed |

**Thiết kế lại được (và nên) bỏ grid 10 hàng.**  
Yêu cầu layout mới:
- Mobile portrait primary (iPhone 14/15 class)
- Wheel chiếm **không gian visual lớn nhất**
- Spin luôn **1 ngón cái với tới** (vùng dưới giữa hoặc gần bottom)
- Tab bar **sát đáy**, full width, không hở lề xám
- Safe-area bottom nằm *trong* nav (nền nav kéo sát mép máy)

---

## 7. Component inventory (cần design)

| Component | Ghi chú |
|-----------|---------|
| App shell / background | Có thể gradient, texture, glow — tránh flat slate nhạt |
| Top bar | Edit (+ optional title / logo spin.png) |
| Wheel | Segments, hub center, shadow tròn (không shadow vuông) |
| Pointer | Droplet trái-dưới; redesign OK |
| Spin button | Primary CTA, disabled state khi quay |
| Bottom tabs | 4 tabs, active indicator rõ |
| Result modal | Backdrop blur, card, hierarchy chữ |
| Edit modal | Count picker, list rows (index + swatch + input), footer 2 nút |
| Empty / error | “Cần ≥ 3 ô” (alert hiện tại — nên toast/inline đẹp hơn) |
| Desktop frame | Optional: phone bezel, ambient bg |

**States cần vẽ:**
- Default / Spinning / Result open / Edit open
- Tab active vs inactive
- Button pressed / disabled
- Long label (2 dòng) trên segment

---

## 8. Copy & ngôn ngữ

- UI language: **Tiếng Việt** (giọng thân mật, party)
- Tên app: **LuckyDraw** (không thêm tagline bắt buộc; design có thể đề xuất 1 dòng phụ)
- Các string chính:
  - Edit, Spin, Kết quả, OK, Hủy, Lưu, Đóng
  - “Chỉnh sửa vòng quay”
  - “Số ô (3 → 9)”
  - “Nội dung từng ô”
  - “Tab: {tên tab}”
  - “1 2 3 dzô”
  - Tab labels như bảng mục 4.1

---

## 9. Màu & style hiện tại (baseline — **được thay**)

```
Background app:     #f1f5f9
Desktop chrome:     #0f172a
Primary blue:       #2563EB / #1d4ed8
Text:               slate-800 / slate-900
Tab inactive:       #64748b
Wheel palette:      #2563EB, #EF4444, #A855F7, #EAB308,
                    #22C55E, #F97316, #EC4899, #A16207, #4F46E5
Hub / pointer:      #0f0f0f + #ef2222
Font:               Barlow 400–900
```

**Hướng style gợi ý cho designer (không bắt buộc):**
- **Neon night market / karaoke / beer party** — tối, glow, accent vàng-đỏ-xanh
- hoặc **Candy casino** — sáng, bo tròn lớn, soft 3D
- hoặc **Retro spin wheel TV game show**

Tránh: dashboard SaaS, fintech green, corporate blue-gray.

---

## 10. Motion (design nên spec)

| Moment | Gợi ý |
|--------|--------|
| Spin start | Nút press + wheel accelerate |
| Spinning | Ease-out cubic ~4.2–5s, 4–6 vòng |
| Land | Micro-bounce hoặc flash segment thắng (optional) |
| Result in | Scale-up pop + confetti nhẹ (optional, không spam) |
| Tab switch | Crossfade wheel / color shift ngắn |
| Modal | Backdrop fade + card spring |

Giữ **performance mobile** (canvas 1 bánh, không particle nặng).

---

## 11. Ràng buộc kỹ thuật (designer cần biết)

| | |
|---|---|
| Stack | Single `index.html` (+ manifest + spin.png) — vanilla JS, Tailwind CDN, Canvas 2D |
| Không framework | Không React/Vue (có thể đổi sau, design đừng phụ thuộc component library) |
| Offline-ish | Static host GitHub Pages ok |
| Touch | `user-scalable=no`, no double-tap zoom |
| Icon asset | `spin.png` 512×512 đã có |
| A11y tối thiểu | Button roles, modal dialog, Esc đóng; contrast chữ trên segment màu |

Design system output nên map được sang **CSS + canvas draw**, không Figma-only magic (blur phức tạp trên canvas có limit).

---

## 12. Deliverables mong muốn từ Grok Design

1. **Visual direction** (1–2 mood): moodboard / style frame  
2. **Màn chính** (mobile 390×844): default + spinning + result  
3. **Edit modal**  
4. **Bottom tab + top Edit** states  
5. **Desktop** optional (phone frame)  
6. **Token**: colors, type scale, radii, shadows, spacing  
7. **Wheel art direction**: segment style, hub, pointer, label rules  
8. **Micro-copy** polish nếu cần  
9. **Hand-off notes**: spacing, safe-area, z-index, dark/light (nếu có)

Format: mockup + short design notes (markdown hoặc annotated frames).  
Ưu tiên **mobile portrait**.

---

## 13. Acceptance criteria (UI mới “đạt”)

- [ ] Mở app trên điện thoại: nhìn **app**, không nhìn “trang web 2019”
- [ ] Vòng quay là focus; Spin bấm 1 tay dễ
- [ ] 4 tab rõ active; full-bleed đáy, không hở lề
- [ ] Result modal “ăn mừng” được, chữ kết quả cực rõ
- [ ] Edit đủ dùng < 30 giây
- [ ] Không mất feature mục 4
- [ ] Dùng được light/dark? (optional — nếu 1 theme thì chọn **1 theme mạnh**, đừng nửa vời)
- [ ] Brand icon `spin.png` xuất hiện có chủ đích (splash visual / header / empty state)

---

## 14. Out of scope (lần redesign này)

- Multiplayer / share result link
- Sound FX / haptic (có thể note “phase 2”)
- i18n English
- Admin CMS
- Thêm/xóa tab động
- Ảnh custom trên segment

---

## 15. Prompt gợi ý dán cho Grok Design

```
Bạn là product designer. Redesign UI cho app mobile PWA tên LuckyDraw
(vòng quay may rủi tiệc nhậu Việt Nam).

Đọc full DESIGN_BRIEF.md đính kèm. Giữ 100% feature & flow.
Phá layout grid 10 hàng hiện tại. Hero = vòng quay. Pointer dưới-trái.
Bottom 4 tabs. Spin CTA. Edit modal. Result modal “1 2 3 dzô”.
Style: party / night / fun — không SaaS. Mobile-first 390×844.
Output: màn chính + states + edit + result + design tokens + notes handoff.
```

---

## 16. Meta

| | |
|---|---|
| Repo | https://github.com/KattyFury/luckydraw |
| App name | LuckyDraw |
| Primary locale | vi-VN |
| Current status | Functional MVP, UI weak |
| Brief version | 1.0 — 2026-07-17 |

---

*Hết brief. Designer tự do visual; PM/dev lock behavior mục 4 & 11.*
