# Security Rules

**Headless Blogger Platform – Security Contract v1**

---

## 1. Tujuan Dokumen

Dokumen ini mengatur standar keamanan untuk seluruh arsitektur Headless Blogger Platform, meliputi:

- Perlindungan data sensitif
- Otentikasi dan otorisasi
- Proteksi API dan frontend
- Kebijakan pengelolaan kunci dan variabel lingkungan

---

## 2. Environment Variables & Secrets

### 2.1 Penanganan Kunci API

- API Key Blogger harus disimpan di `.env` di backend / serverless environment saja
- **Tidak pernah expose API Key di frontend**
- Variabel environment harus diakses hanya di server-side
- Contoh variabel:

  ```
  BLOGGER_API_KEY=xxxxxxx
  BLOG_IDS=12345,67890
  ```

### 2.2 Konfigurasi `.gitignore`

- File `.env` wajib ada di `.gitignore`
- Jangan commit kunci rahasia ke repositori publik

---

## 3. API Security

### 3.1 Proxy API Endpoint

- Semua permintaan API ke Blogger API harus lewat proxy backend
- Proxy harus membatasi origin domain (HTTP Referrer check)
- CORS harus diatur ketat sesuai domain frontend

### 3.2 Rate Limiting

- Terapkan rate limiting untuk mencegah penyalahgunaan API
- Sesuaikan dengan kuota API Blogger (default 10.000 permintaan/hari)

---

## 4. Otentikasi & Otorisasi

### 4.1 API Key untuk Data Publik

- Gunakan API Key untuk akses data publik tanpa login
- Pastikan key hanya dapat dipakai dari domain frontend resmi

### 4.2 OAuth 2.0 untuk Akses Admin

- Jika fitur admin (tulis posting dari frontend) diimplementasikan, gunakan OAuth 2.0
- OAuth harus di-handle secara server-side, token tidak bocor ke frontend

---

## 5. Proteksi Frontend

### 5.1 Hindari Kebocoran Data Sensitif

- Tidak menampilkan atau menanamkan `BLOG_IDS` langsung di frontend
- Semua data sensitif di-fetch dari backend secara terfilter

### 5.2 CSP & Headers Keamanan

- Gunakan Content Security Policy (CSP) yang ketat
- Gunakan header HTTP security seperti `X-Frame-Options`, `X-Content-Type-Options`, `Strict-Transport-Security`

---

## 6. Data Validation & Sanitasi

- Validasi semua input dari pengguna
- Sanitasi output untuk mencegah XSS
- Parsing JSON hasil API harus aman dan handle error

---

## 7. Logging & Monitoring

- Logging kesalahan backend dan API dengan aman (tanpa bocor data sensitif)
- Monitor anomali request API untuk deteksi serangan

---

## 8. Backup & Recovery

- Pastikan backup konfigurasi `.env` tersimpan aman
- Siapkan rencana pemulihan jika kunci bocor atau domain diganti

---

## 9. Forbidden Security Anti-Patterns

❌ Menaruh API Key langsung di frontend
❌ Membuka CORS untuk semua origin
❌ Menyimpan kunci di repositori publik
❌ Memproses OAuth di client-side
❌ Menyimpan token user tanpa enkripsi

---

## 10. Validasi Keamanan (WAJIB)

Sebelum deploy:

- [ ] Semua API Key tersembunyi dari frontend
- [ ] CORS dikonfigurasi ketat
- [ ] `.env` tidak pernah dicommit
- [ ] OAuth token diproses aman
- [ ] Rate limit aktif dan sesuai kuota

---

## 11. Langkah Selanjutnya

👉 **Integrasi security testing otomatis (CI/CD)**
👉 **Audit rutin API dan serverless function**
👉 **Update dokumentasi bila ada perubahan kebijakan**

---

Dokumen ini wajib dipatuhi untuk menjamin keamanan, integritas, dan kepercayaan pengguna platform blog headless ini.

---
