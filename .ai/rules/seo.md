# SEO Rules

**Headless Blogger Platform – SEO Contract v1**

---

## 1. Tujuan Dokumen

Dokumen ini mengatur pedoman SEO untuk memastikan platform Headless Blogger dapat meraih peringkat optimal di mesin pencari dan pengalaman pengguna terbaik.

---

## 2. Metadata dan Struktur Data

### 2.1 Metadata Wajib

Setiap halaman wajib menyertakan metadata berikut:

- Title (unik dan deskriptif, max 60 karakter)
- Meta description (maksimal 160 karakter)
- Canonical URL yang mengarah ke versi resmi halaman
- Open Graph metadata untuk media sosial (og:title, og:description, og:image)
- Twitter Card metadata (twitter:card, twitter:title, twitter:description, twitter:image)

### 2.2 Implementasi Metadata

- Metadata harus di-generate server-side menggunakan Next.js `generateMetadata()`
- Metadata dinamis mengikuti konten halaman (judul post, tanggal, deskripsi)

---

## 3. Data Terstruktur (Schema.org)

- Setiap posting harus menyisipkan JSON-LD tipe `Article` atau `BlogPosting`
- Metadata harus mencakup:

  - `headline`
  - `author`
  - `datePublished`
  - `mainEntityOfPage`
  - `image` (gunakan URL gambar yang dioptimasi)

- JSON-LD disisipkan di server-side, bukan client-side injection

---

## 4. URL dan Navigasi SEO-Friendly

- Gunakan struktur URL bersih:

  - Blog post: `/blog/[slug]`
  - Label/category: `/blog/label/[label]`

- Hindari query parameter untuk konten utama
- Sitemap XML harus otomatis di-generate dan diakses via `/sitemap.xml`
- Sitemap mencakup seluruh blog yang digabung dalam sistem

---

## 5. Konten & Internal Linking

- Pastikan konten blog konsisten, dengan heading yang terstruktur (H1, H2, dst)
- Gunakan internal linking ke artikel terkait dan label relevan
- Hindari duplikasi konten lintas blog

---

## 6. Kecepatan dan Core Web Vitals

- Target skor 100/100 di Google PageSpeed Insights
- Optimasi gambar otomatis lewat transformasi parameter URL
- Implementasi ISR agar laman selalu cepat dan fresh
- Minimal CLS, LCP < 2.5 detik, INP < 200 ms

---

## 7. Robots.txt dan Crawling

- Robots.txt harus membatasi akses hanya ke resource yang diperlukan
- Blokir halaman admin dan API dari indexing
- Sitemap.xml harus direferensikan di robots.txt

---

## 8. Multibahasa (Jika Diperlukan)

- Gunakan struktur URL terpisah per bahasa (subfolder atau subdomain)
- Sitemap dan metadata mendukung hreflang untuk bahasa berbeda
- Kelola konten terjemahan menggunakan blog ID terpisah jika diperlukan

---

## 9. Forbidden SEO Anti-Patterns

❌ Metadata kosong atau duplikat
❌ Duplikasi URL tanpa canonical
❌ Mengandalkan client-side rendering untuk konten utama
❌ Sitemap tidak lengkap atau statis
❌ Konten tanpa heading terstruktur
❌ Gambar tanpa alt text

---

## 10. Validasi SEO (WAJIB)

- [ ] Semua halaman ada metadata lengkap
- [ ] JSON-LD terpasang dengan benar
- [ ] Sitemap ter-generate dan dapat diakses
- [ ] Struktur URL konsisten dan SEO-friendly
- [ ] PageSpeed Insights skor 90+ untuk desktop dan mobile
- [ ] Robots.txt dan sitemap sesuai standar

---

## 11. Langkah Selanjutnya

👉 Audit SEO rutin (automasi dan manual)
👉 Pelatihan tim konten tentang SEO best practice
👉 Update dokumen sesuai algoritma mesin pencari terbaru

---

Dokumen ini wajib dipatuhi agar Headless Blogger Platform dapat bersaing secara maksimal di ranah pencarian dan memberikan pengalaman terbaik bagi pengguna.

---
