# Prompt gửi Grok Build (copy nguyên xi)

Bạn là senior frontend + product designer. Hãy **build lại hoàn toàn UI** cho app LuckyDraw (vòng quay may rủi đi nhậu) theo style neon party dưới đây.

### 1. Stack & ràng buộc (giữ nguyên)
- Single `index.html` + vanilla JS + Tailwind CDN + Canvas 2D
- Không React/Vue
- Giữ 100% logic hiện tại: tabs, spin animation, localStorage, edit, result modal, pointer vị trí dưới-trái, số ô 3-9
- Mobile-first 390×844, full-screen PWA, safe-area
- **Width:** full viewport mọi màn (không khóa max-width 430px / phone strip trên desktop)
- Project path: `D:\Files\Claude\build_for_me\small_tool\luckydraw`

### 2. Visual Direction (bắt buộc theo đúng)
**Neon Night Karaoke / Beer Party**
- Nền tối hoàn toàn (đen → tím than)
- Mọi thứ đều có **glow neon** mạnh
- Cảm giác đồ chơi party, không SaaS, không xám nhạt

### 3. Design Tokens

```css
/* Background */
--bg: #07070f;
--bg-gradient: radial-gradient(ellipse at 50% 0%, #1a1035 0%, #07070f 70%);

/* Neon colors */
--neon-cyan: #00f0ff;
--neon-pink: #ff2d95;
--neon-purple: #b14cff;
--neon-yellow: #ffe600;
--neon-green: #39ff14;
--neon-orange: #ff6b00;
--neon-red: #ff003c;
--neon-blue: #00a2ff;

/* Text */
--text-primary: #ffffff;
--text-secondary: #a0a0c0;

/* Glass */
--glass: rgba(15, 15, 30, 0.75);
--glass-border: rgba(0, 240, 255, 0.25);

/* Shadows & Glow */
--glow-cyan: 0 0 20px #00f0ff, 0 0 40px #00f0ff66;
--glow-pink: 0 0 20px #ff2d95, 0 0 40px #ff2d9566;
--glow-button: 0 0 30px #00f0ffaa, 0 8px 25px #00f0ff44;
```

Font: **Inter** hoặc **SF Pro** (system-ui), weight 600–800 cho heading, 500 cho body.

### 4. Layout chính (bỏ hết grid 10 hàng cũ)

```
┌─────────────────────────────┐
│  [Logo nhỏ]          [Edit] │  ← top bar (padding safe-area)
│                             │
│                             │
│         ◉ VÒNG QUAY         │  ← chiếm ~55-60% chiều cao
│        (pointer dưới-trái)  │
│                             │
│                             │
│          [  SPIN  ]         │  ← nút to, dễ bấm 1 ngón cái
│                             │
│  Ăn nhậu | C1 | C2 | C3     │  ← bottom tab full-bleed + safe-area
└─────────────────────────────┘
```

### 5. Chi tiết component

**Top bar**
- Trái: icon vòng quay nhỏ (dùng spin.png) + chữ “LuckyDraw” (optional, có thể bỏ)
- Phải: nút **Edit** dạng pill, nền `rgba(0,240,255,0.15)`, border neon-cyan mỏng, chữ cyan, có glow nhẹ

**Vòng quay (Canvas)**
- Diameter lớn nhất có thể (gần full width trừ padding 24px)
- 9 màu neon mạnh (hỗ trợ max 9 ô; 8 màu gốc + 1 màu thứ 9):
  1. #00a2ff
  2. #ff003c
  3. #b14cff
  4. #ffe600
  5. #39ff14
  6. #ff6b00
  7. #ff2d95
  8. #00f0ff
  9. #7c3aed  (tím đậm — chỉ khi 9 ô)
- Mỗi segment có:
  - Fill màu neon
  - Viền mỏng trắng 1.5px opacity 0.3
  - Soft outer glow nhẹ
- Center hub: tròn đen bóng + chấm đỏ neon ở giữa
- **Pointer**: giữ vị trí dưới-trái, style droplet đỏ neon có glow (có thể redesign đẹp hơn nhưng vị trí không đổi)
- Chữ trên segment: trắng đậm, size vừa, đọc theo hướng mép → tâm

**Nút SPIN (quan trọng nhất)**
- Rất to (height 64–72px, width ~70% màn hình)
- Nền gradient: từ #00f0ff → #0080ff
- Chữ “SPIN” trắng đậm size 22–24px
- Box-shadow + glow cyan cực mạnh
- Khi disabled (đang quay): opacity 0.5 + chữ “Đang quay...”

**Bottom tabs**
- Full width, nền gần như đen + blur nhẹ
- Active tab: chữ neon-pink + gạch chân glow hồng
- Inactive: chữ xám nhạt
- Labels đầy đủ: Ăn nhậu | Custom 1 | Custom 2 | Custom 3
- Safe-area bottom nằm trong tab bar

### 6. Result Modal
- Backdrop: đen 70% + blur
- Card: glass tối, border neon-cyan mỏng + glow
- Nội dung:
  - “Kết quả” nhỏ (cyan)
  - Kết quả lớn (trắng 32–36px, bold)
  - “1 2 3 dzô” màu neon-yellow
  - Nút OK: nền neon-pink, chữ trắng, glow hồng
- Xuất hiện có scale + fade nhẹ

### 7. Edit Modal
- Full-screen glass tối
- Header: “Chỉnh sửa vòng quay” + “Tab: Ăn nhậu”
- Picker số ô: hàng pill ngang 3→9, active có nền neon-cyan
- List ô: mỗi dòng = swatch tròn neon + input text
- Footer: Hủy (outline) + Lưu (solid neon-cyan)

### 8. Motion
- Spin: ease-out mạnh, 4.5–5s, 5–6 vòng
- Nút SPIN có press effect (scale 0.96)
- Result modal: spring scale-up nhẹ
- Tab switch: crossfade ngắn

### 9. Yêu cầu cuối
- Trông như **app native** chứ không phải web 2019
- Glow neon phải rõ, không bị “dirty”
- Nút SPIN phải “đập vào mắt”
- Tất cả chữ tiếng Việt đúng chính tả
- Performance tốt trên mobile thật

Hãy viết lại toàn bộ HTML/CSS/JS theo đúng style trên, giữ nguyên toàn bộ logic cũ.
