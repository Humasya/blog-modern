# Testing Rules

**Headless Blogger Platform – Testing Contract v1**

---

## 1. Tujuan Dokumen

Dokumen ini menetapkan standar pengujian untuk memastikan kualitas, performa, keamanan, dan kepatuhan SEO Headless Blogger Platform sebelum deployment.

---

## 2. Jenis Pengujian

| Jenis Pengujian             | Tujuan                                                             |
| --------------------------- | ------------------------------------------------------------------ |
| Unit Test                   | Memastikan fungsi modul berjalan sesuai spesifikasi                |
| Integration Test            | Memastikan modul bekerja sama, termasuk API, frontend, dan backend |
| End-to-End (E2E) Test       | Simulasi alur pengguna penuh untuk mendeteksi bug dan regresi      |
| Performance Test            | Validasi Core Web Vitals dan respon API                            |
| Security Test               | Validasi akses API, otentikasi, dan proteksi data sensitif         |
| SEO Test                    | Validasi metadata, JSON-LD, sitemap, robots.txt                    |
| Regression Test             | Memastikan update fitur tidak merusak fungsi yang ada              |
| Cross-Browser & Device Test | Validasi tampilan dan performa pada browser dan perangkat berbeda  |

---

## 3. Unit & Integration Testing

### 3.1 Backend

- Gunakan **pytest** untuk unit dan integrasi fungsi Python
- Test wajib mencakup:

  - Parsing API Blogger
  - Normalisasi data multi-blog
  - Otentikasi API (API Key / OAuth 2.0)
  - Rate limiting & error handling

### 3.2 Frontend

- Gunakan **Jest + React Testing Library**
- Test wajib mencakup:

  - Komponen UI (PostCard, Navigation, Image Loader)
  - Properti dinamis (title, date, author)
  - Interaksi user (klik, hover, form input)

---

## 4. End-to-End (E2E) Testing

- Gunakan **Playwright** atau **Cypress**
- Alur pengujian:

  1. Load halaman utama dan sub-page
  2. Navigasi label / kategori
  3. Verifikasi konten multi-blog ter-aggregate dengan benar
  4. Cek komentar ditampilkan sesuai API
  5. Verifikasi regenerasi halaman ISR setelah update konten

---

## 5. Performance Testing

- Gunakan **Lighthouse CI** atau **Web Vitals JS**
- Validasi:

  - LCP < 2.5 detik
  - CLS < 0.1
  - INP < 200 ms

- Test pada kondisi throttling jaringan (3G/Slow 4G)
- Validasi ukuran bundle JS < 120 KB gzipped

---

## 6. Security Testing

- Gunakan **OWASP ZAP** atau **Nikto** untuk audit keamanan API
- Validasi:

  - Tidak ada kebocoran API Key di frontend
  - CORS sesuai domain resmi
  - Token OAuth aman
  - Rate limiting aktif

---

## 7. SEO Testing

- Verifikasi metadata lengkap pada semua halaman
- JSON-LD valid dan sesuai schema.org
- Sitemap.xml dapat diakses dan lengkap
- Robots.txt sesuai spesifikasi
- Gunakan **Google Search Console** atau **SEO audit tools**

---

## 8. Cross-Browser & Device Testing

- Browser: Chrome, Firefox, Edge, Safari (desktop + mobile)
- Mobile: iOS dan Android (PWA kompatibilitas jika ada)
- Test responsivitas UI, tipografi, dan gambar dinamis

---

## 9. Regression Testing

- Setiap perubahan kode wajib dijalankan **full test suite**
- Automasi di CI/CD pipeline
- Hasil regression harus 100% pass sebelum merge ke main branch

---

## 10. Forbidden Testing Anti-Patterns

❌ Menguji hanya di local tanpa environment staging
❌ Tidak mencakup error handling dan edge case
❌ Mengabaikan performance metrics
❌ Tidak memverifikasi keamanan API
❌ Menguji hanya di satu browser atau device

---

## 11. Validasi Testing (WAJIB)

- [ ] Semua unit dan integration test pass > 95% coverage
- [ ] Semua E2E flow utama berhasil
- [ ] Core Web Vitals terpenuhi
- [ ] Semua pengujian keamanan lulus
- [ ] Metadata dan sitemap valid

---

## 12. Langkah Selanjutnya

- Integrasi **CI/CD pipeline** untuk automasi testing
- Update test suite sesuai fitur baru
- Monitoring dan alert untuk regression di production

---

Dokumen ini wajib dipatuhi agar Headless Blogger Platform tetap stabil, aman, cepat, dan SEO-friendly di setiap rilis.

---
