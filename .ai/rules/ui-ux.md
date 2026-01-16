# UI / UX Rules

**Headless Blogger Platform – Interface Contract v1**

---

## 1. Tujuan Dokumen

Dokumen ini mendefinisikan **aturan UI & UX yang WAJIB diikuti** oleh:

- Frontend developer
- Backend/API developer (data shaping)
- AI Agent (layout, HTML, Tailwind, React, dsb)

UI ini **bersifat dashboard-grade**, **read-heavy**, **SEO-aware**, dan **performance-first**.

---

## 2. Design Philosophy (Non-Negotiable)

### 2.1 Core Principles

- **Information density > decoration**
- **Hierarchy sebelum warna**
- **Read-first, write-second**
- **Micro-interaction = feedback, bukan gimmick**
- **Dark mode adalah PRIMARY mode**

### 2.2 UX Tone

- Calm
- Confident
- Technical
- Enterprise-grade
- Zero playful animation

---

## 3. Layout Architecture

### 3.1 Global Layout Structure

```
┌──────────────── Sidebar (Fixed) ────────────────┐
│ Brand / User                                     │
│ Mini Stats                                       │
│ Navigation                                       │
│ System Section                                   │
│ Primary Action (New Post)                        │
└─────────────────────────────────────────────────┘

┌──────────────── Main Content ───────────────────┐
│ Sticky Top Command Bar                           │
│ Horizontal Filters                               │
│ Content Feed / Article / Analytics               │
│ Floating Action Button (Secondary)               │
└─────────────────────────────────────────────────┘
```

### 3.2 Sidebar Rules

- **Width:** 260–280px fixed
- **Position:** fixed, full height
- **Scroll:** internal only
- **Active state:** color + background + icon emphasis
- **Primary CTA:** selalu di footer sidebar

❌ Tidak boleh collapse otomatis
❌ Tidak boleh hamburger di desktop

---

## 4. Navigation & Information Hierarchy

### 4.1 Sidebar Navigation

- Level 1 only (tidak nested)
- Grouping jelas (Content vs System)
- Icon + text wajib

### 4.2 Breadcrumb (Detail View)

- Digunakan di **post detail / analytics**
- Format: `Posts / Category / Title`
- Title terakhir = aktif (warna primary)

---

## 5. Command Bar (Top Header)

### 5.1 Behavior

- Sticky
- Blur backdrop
- Border subtle
- Tidak boleh tinggi (>64px)

### 5.2 Mandatory Elements

- Global search (⌘K hint)
- Notification icon
- Contextual action (Edit, Filter, dsb)

---

## 6. Feed & Card System (Global Feed)

### 6.1 Card Anatomy (WAJIB)

```
│ Status Strip (left)
│ Category + Date
│ Title
│ Excerpt (max 2 lines)
│ Metadata (status, views, read time)
│ Quick Actions (hover)
│ Authors (avatar stack)
```

### 6.2 Status Indicator Strip

- **Published:** Emerald
- **Draft:** Amber (pulse allowed)
- **In Review:** Purple
- Strip selalu di **kiri**
- Lebar tetap ±4px

---

## 7. Typography System

### 7.1 Font Stack

- **Display:** Space Grotesk
- **Body:** Inter

### 7.2 Hierarchy Rules

| Element    | Style             |
| ---------- | ----------------- |
| Page Title | Display, Bold     |
| Card Title | Display, Semibold |
| Body       | Inter, Regular    |
| Meta       | Small, Uppercase  |
| Code       | Mono              |

❌ Tidak boleh lebih dari 2 font
❌ Tidak boleh decorative font

---

## 8. Color System (Derived from HTML)

### 8.1 Primary Tokens

- `primary`: `#0f756d`
- `primary-dark`
- `background-dark`: `#121416`
- `surface-dark`: `#1F222B`
- `border-dark`: subtle, low contrast

### 8.2 Color Usage Rules

- Primary **hanya untuk emphasis & action**
- Status = semantic color
- Background ≠ Surface ≠ Border (harus jelas)

---

## 9. Dark Mode Rules (Critical)

- Dark mode = default
- Light mode = optional
- Tidak boleh pure black
- Kontras minimal WCAG AA
- Shadow lebih penting dari border

---

## 10. Article / Post Detail View

### 10.1 Layout

- **Main content:** 8–9 kolom
- **Right sidebar:** 3–4 kolom
- Grid 12 kolom wajib

### 10.2 Hero Section

- Aspect ratio cinematic (21:9)
- Gradient overlay WAJIB
- Title overlay di image
- No text directly on image tanpa overlay

---

## 11. Content Rules

### 11.1 Prose

- `prose-invert`
- Max width disabled
- Line-height long-form friendly

### 11.2 Code Block

- Dark container
- Line numbers
- Copy button (hover)
- Horizontal scroll allowed

---

## 12. Sidebar Context (Detail Page)

### Widgets Allowed:

- Author Card
- Table of Contents (sticky)
- Related Posts

❌ Tidak boleh ads
❌ Tidak boleh infinite widget

---

## 13. Interaction Rules

### 13.1 Hover

- Border highlight
- Shadow increase
- Icon fade-in

### 13.2 Animation

- Duration ≤ 200ms
- Ease-out
- Tidak looping kecuali status pulse

---

## 14. Accessibility Rules

- Semua icon ada label (aria)
- Focus ring visible
- Keyboard navigable
- No hover-only critical action

---

## 15. Performance & SEO UX

- Image aspect ratio FIXED (CLS safe)
- Lazy load kecuali hero
- Read-only comments
- No client-side fetch for SEO content

---

## 16. Forbidden UI Anti-Patterns

❌ Modal heavy workflow
❌ Infinite scroll tanpa tombol
❌ Masonry feed
❌ Inline editing di feed
❌ Rainbow badge

---

## 17. Validation Checklist (WAJIB)

Sebelum UI dianggap valid:

- [ ] Sidebar fixed & usable
- [ ] Dark mode default
- [ ] Card anatomy konsisten
- [ ] CLS = 0
- [ ] Status color konsisten
- [ ] Sesuai `api-spec.md`
- [ ] Lolos `agents.md`

---
