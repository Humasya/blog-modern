# Frontend Rules

**Headless Blogger Platform – Frontend Contract v1**

---

## 1. Tujuan Dokumen

Dokumen ini mendefinisikan **aturan implementasi frontend** untuk platform Headless Blogger, meliputi:

- Framework & rendering strategy
- Data fetching rules
- Struktur folder frontend
- SEO & performance constraints
- Interaksi dengan API internal

Frontend **TIDAK BOLEH** membuat keputusan arsitektural di luar dokumen ini.

---

## 2. Technology Stack (Locked)

### 2.1 Core Stack

| Layer      | Teknologi                    |
| ---------- | ---------------------------- |
| Framework  | **Next.js (App Router)**     |
| Language   | TypeScript                   |
| Styling    | Tailwind CSS                 |
| UI Pattern | Server Components-first      |
| Data Fetch | `fetch()` native             |
| Image      | `next/image` (custom loader) |

❌ Tidak boleh React CSR-only
❌ Tidak boleh Pages Router

---

## 3. Rendering Strategy (Non-Negotiable)

### 3.1 Rendering Mode by Page Type

| Page              | Strategy           |
| ----------------- | ------------------ |
| Homepage          | SSG + ISR          |
| Post Detail       | SSG + ISR          |
| Category / Label  | SSG + ISR          |
| Sitemap           | SSG                |
| Search (optional) | Server-side render |

### 3.2 Revalidation Rules

```ts
export const revalidate = 3600;
```

- Homepage & label: 1 jam
- Post detail: 24 jam
- Sitemap: 24 jam

❌ Tidak boleh client-side revalidation

---

## 4. Data Fetching Rules

### 4.1 Single Source of Truth

Frontend **HANYA BOLEH** fetch dari:

```
/api/v1/*
```

❌ Tidak boleh fetch Blogger API
❌ Tidak boleh bypass proxy

---

### 4.2 Fetch Pattern (WAJIB)

```ts
const res = await fetch(`${API_BASE}/posts?limit=10`, {
  next: { revalidate: 3600 },
});
```

Rules:

- `cache: 'no-store'` ❌ DILARANG
- Semua fetch harus **cache-aware**
- Error handling wajib graceful

---

## 5. Folder Structure (Frontend)

```
src/
├─ app/
│  ├─ layout.tsx
│  ├─ page.tsx
│  ├─ blog/
│  │  ├─ [slug]/
│  │  │  └─ page.tsx
│  │  └─ label/
│  │     └─ [label]/
│  │        └─ page.tsx
│  ├─ sitemap.xml/
│  │  └─ route.ts
│
├─ components/
│  ├─ layout/
│  ├─ navigation/
│  ├─ cards/
│  ├─ article/
│  ├─ sidebar/
│  └─ ui/
│
├─ lib/
│  ├─ api.ts
│  ├─ seo.ts
│  ├─ image.ts
│  └─ constants.ts
│
├─ styles/
│  └─ globals.css
```

---

## 6. Component Rules

### 6.1 Server vs Client Components

| Component    | Type   |
| ------------ | ------ |
| Pages        | Server |
| Feed         | Server |
| Post Content | Server |
| Sidebar      | Server |
| Search Input | Client |
| Theme Toggle | Client |

❌ Jangan menambahkan `"use client"` tanpa alasan jelas

---

## 7. Image Handling Rules (Critical)

### 7.1 Image Loader

- Semua image Blogger **WAJIB lewat util**
- Tidak boleh raw `<img>`

```ts
getOptimizedImage(src, {
  width: 1200,
  height: 630,
  webp: true,
});
```

### 7.2 CLS Rules

- Width & height **WAJIB**
- Aspect ratio fixed
  - **Hero Image:** 21:9 (Cinematic)
  - **Card:** 16:9 or 3:2
- Hero image eager load
- Others lazy

---

## 8. SEO Rules

### 8.1 Metadata API

Semua page **WAJIB** pakai:

```ts
export async function generateMetadata();
```

Fields mandatory:

- title
- description
- canonical
- openGraph
- twitter

---

### 8.2 Structured Data

- JSON-LD injected server-side
- Type: `Article` / `BlogPosting`
- Tidak boleh client-side injection

---

## 9. Styling Rules (Tailwind)

### 9.1 Tailwind Principles

- Utility-first
- No inline style
- No CSS-in-JS
- Dark mode default

### 9.2 Forbidden

❌ Arbitrary hex color
❌ Inline `style={{}}`
❌ Custom CSS kecuali global

---

## 10. Interaction & State

### 10.1 State Management

- Local state only
- No Redux
- No Zustand

### 10.2 Interactions

- Hover = visual feedback only
- No content mutation client-side

---

## 11. Error & Empty State UX

- Empty feed → informative message
- 404 post → SEO-friendly page
- Error fetch → fallback static UI

---

## 12. Performance Budget

| Metric     | Target  |
| ---------- | ------- |
| LCP        | < 2.5s  |
| CLS        | 0       |
| INP        | < 200ms |
| JS Payload | < 120kb |

---

## 13. Security Rules (Frontend)

- No env var leak
- No API key in bundle
- No blogId exposure

---

## 14. Forbidden Frontend Anti-Patterns

❌ Client-side fetch for **initial** SEO content (Title, Body, Meta)

> _Exception: Infinite scroll / Load More untuk pagination diperbolehkan secara client-side._
> ❌ Infinite scroll without button
> ❌ Hydrating full page unnecessarily
> ❌ Inline SVG icon abuse
> ❌ DOM manipulation

---

## 15. Validation Checklist

Sebelum merge:

- [ ] Semua page SSG/ISR
- [ ] Fetch via `/api/v1`
- [ ] CLS = 0
- [ ] Dark mode default
- [ ] Metadata lengkap
- [ ] Sesuai `ui-ux.md`
- [ ] Lolos `agents.md`

---
