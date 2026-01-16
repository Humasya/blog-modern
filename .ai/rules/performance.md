# Performance Rules

**Headless Blogger Platform – Performance Contract v1**

---

## 1. Tujuan Dokumen

Dokumen ini mengatur kebijakan dan standar performa platform Headless Blogger untuk mencapai pengalaman pengguna yang optimal sesuai target Core Web Vitals dan praktik terbaik 2026.

---

## 2. Target Utama Performa

| Metrik                          | Target          |
| ------------------------------- | --------------- |
| Largest Contentful Paint (LCP)  | < 2.5 detik     |
| Cumulative Layout Shift (CLS)   | < 0.1 (ideal 0) |
| Interaction to Next Paint (INP) | < 200 ms        |
| JavaScript Payload Size         | < 120 KB        |

---

## 3. Strategi Rendering dan Caching

### 3.1 Static Site Generation (SSG) + Incremental Static Regeneration (ISR)

- Semua halaman utama harus menggunakan SSG dengan ISR sesuai waktu revalidasi yang diatur di `frontend.md`
- Hindari rendering sisi client untuk konten utama agar LCP optimal
- Regenerasi halaman hanya jika ada perubahan konten atau interval waktu tercapai

### 3.2 Cache Policy

- Gunakan cache bawaan Next.js dengan `fetch()` revalidate option
- Tidak menggunakan `cache: 'no-store'` pada fetch data penting
- Cache layer di proxy API backend untuk mengurangi panggilan ke Blogger API

---

## 4. Optimasi Aset Gambar

### 4.1 Gambar Dinamis via Parameter URL

- Manfaatkan fitur transformasi dynamic URL gambar dari CDN Google Photos (via Frontend Image Loader)
- Gunakan `next/image` dengan custom loader untuk menghasilkan format WebP dan ukuran responsif

### 4.2 CLS dan Layout Stability

- Set atribut `width` dan `height` untuk setiap gambar
- Gunakan `aspect-ratio` yang konsisten
- Load gambar hero dengan eager, lainnya lazy loading

---

## 5. Bundling dan JavaScript

- Batasi penggunaan paket eksternal yang besar
- Tree-shaking wajib diterapkan
- Hindari library yang membawa polyfill besar jika tidak perlu
- Prioritaskan rendering server components untuk mengurangi JS yang dikirim ke client

---

## 6. Network & Payload

- Minimalkan request HTTP sebanyak mungkin (gabungkan jika perlu)
- Gunakan HTTP/2 atau HTTP/3 jika memungkinkan
- Kompresi gzip atau Brotli wajib diaktifkan

---

## 7. Interaktivitas dan Hydration

- Hindari hydrating seluruh halaman jika tidak perlu
- Komponen client-only harus minimal dan ringan
- Gunakan teknik lazy hydration bila memungkinkan

---

## 8. Monitoring & Analisis

- Implementasi alat monitoring Core Web Vitals (misal Google PageSpeed Insights, Web Vitals JS)
- Pantau metrik performa di staging dan production secara rutin
- Tindak lanjuti regresi performa dengan cepat

---

## 9. Forbidden Performance Anti-Patterns

❌ Client-side data fetching untuk SEO content
❌ Inline styles berlebihan
❌ Mengirim JS bundle berukuran besar tanpa chunking
❌ Infinite scroll tanpa kontrol pagination
❌ Menggunakan gambar tanpa optimasi ukuran dan format

---

## 10. Validasi Performa (WAJIB)

Sebelum release:

- [ ] LCP < 2.5 detik pada 3G slow throttling
- [ ] CLS = 0 pada semua halaman
- [ ] INP < 200 ms pada interaksi utama
- [ ] Ukuran JS bundle < 120 KB gzipped
- [ ] Semua gambar dengan dimensi eksplisit dan WebP support
- [ ] Semua fetch data cache-aware dengan ISR

---

## 11. Langkah Selanjutnya

👉 Automasi pengujian performa di pipeline CI/CD
👉 Review dan update rutin rules sesuai teknologi dan browser terbaru
👉 Edukasi tim frontend mengenai best practice performa

---

Dokumen ini adalah panduan wajib untuk memastikan Headless Blogger Platform beroperasi dengan performa optimal dan pengalaman pengguna terbaik.

---
