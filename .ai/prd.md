# PRD: Arsitektur Headless Blog Berbasis Blogger API v3

| Status          | Versi | Tanggal         | Pemilik Produk |
| --------------- | ----- | --------------- | -------------- |
| **Draf/Review** | v1.0  | 16 Januari 2026 | Grand Mastery  |

---

## 1. Pendahuluan & Visi Strategis

Laporan ini merinci kerangka kerja pengembangan platform blog modern yang memisahkan antara manajemen konten (**Blogger API v3**) dan presentasi visual (**Next.js & Tailwind CSS**).

### Visi Utama

Membangun aset digital yang tangguh, cepat, dan skalabel dengan memanfaatkan stabilitas infrastruktur Google namun tetap memiliki fleksibilitas desain tanpa batas.

### Tiga Pilar Filosofis

- **Ketahanan (Resilience):** Mengandalkan server Google untuk basis data dan otentikasi guna meminimalkan _downtime_.
- **Kecepatan (Velocity):** Pengiriman konten instan melalui _Static Site Generation_ (SSG) dan _Incremental Static Regeneration_ (ISR).
- **Skalabilitas (Scalability):** Manajemen ribuan entri konten secara efisien melalui metadata dan optimasi gambar dinamis.

---

## 2. Analisis Komparatif

Perbandingan antara pendekatan tradisional dengan arsitektur _Headless_:

| Dimensi Visi             | Pendekatan Tradisional      | Headless Blogger (Blogger API)         |
| ------------------------ | --------------------------- | -------------------------------------- |
| **Manajemen Konten**     | Terikat pada desain templat | Terpisah sebagai data API (JSON)       |
| **Fleksibilitas Desain** | Terbatas pada widget XML    | Kebebasan penuh (React/Tailwind)       |
| **Performa Laman**       | Bergantung skrip eksternal  | Optimasi total (SSG/ISR)               |
| **Keamanan**             | Keamanan terpusat Google    | Keamanan berlapis (Frontend & Backend) |
| **Biaya Operasional**    | Gratis, fitur terbatas      | Efisiensi tinggi, infrastruktur gratis |

---

## 3. Profil Pengguna Target

| Atribut             | Kreator Konten         | Pengembang Web      | Pemilik Bisnis        |
| ------------------- | ---------------------- | ------------------- | --------------------- |
| **Motivasi Utama**  | Estetika & Branding    | Efisiensi & Kontrol | SEO & Konversi        |
| **Prioritas Fitur** | Tipografi & Multimedia | Kualitas API & DX   | Skor LCP & INP        |
| **Hambatan**        | Desain Kaku            | Maintenance Server  | Kecepatan Lambat      |
| **Solusi**          | Desain Custom          | API Managed Google  | Skor Performa 100/100 |

---

## 4. Fitur Utama & Spesifikasi Fungsional

### 4.1. Agregasi Konten Multi-Blog

- **Deskripsi:** Kemampuan menggabungkan konten dari beberapa ID blog ke dalam satu antarmuka.
- **Logika:** Melakukan pemanggilan API paralel ke setiap ID dan normalisasi data sebelum ditampilkan.
- **Konfigurasi:** Menggunakan variabel lingkungan `.env` untuk menyimpan `BLOG_IDS`.
- **Pagination Strategy**

### 4.2. Sistem Navigasi & Taksonomi Dinamis

- Pemetaan label Blogger menjadi kategori navigasi cerdas.
- Fitur pencarian lintas blog berbasis parameter API secara paralel.

### 4.3. Optimasi Aset Gambar Real-Time

Memanfaatkan parameter URL Google Photos untuk transformasi on-the-fly:

| Parameter   | Fungsi                   | Dampak Performa                |
| ----------- | ------------------------ | ------------------------------ |
| `w[lebar]`  | Menentukan lebar gambar  | Mengurangi beban data peramban |
| `h[tinggi]` | Menentukan tinggi gambar | Stabilitas tata letak (CLS)    |
| `-rw`       | Memaksa output WebP      | Penghematan bandwidth ~30%     |

### 4.4. Rich Content Rendering

Mendukung sintaks highlight kode dan pemetaan otomatis Table of Contents.

### 4.5. Admin/Super Admin View

Menambahkan dashboard internal untuk memantau performa artikel (LCP, INP, Views).

---

## 5. Arsitektur Teknis

### 5.1. Model Data

Data diambil melalui Blogger API v3 dalam format JSON, mencakup sumber daya: `Blog`, `Post`, `Comment`, `Page`, dan `User`.

### 5.2. Manajemen Kuota & Caching

Untuk mengoptimalkan kuota 10.000 permintaan/hari, digunakan strategi ISR:

### 5.3. Konfigurasi Lingkungan (`.env`)

```bash
BLOGGER_API_KEY=your_api_key_here
BLOG_IDS=blog_id_1,blog_id_2

```

---

## 6. Strategi SEO & Performa 2026

Target performa berdasarkan standar _Core Web Vitals_ terbaru:

- **LCP (Largest Contentful Paint):** < 2,5 detik (Optimasi gambar & CDN).
- **CLS (Cumulative Layout Shift):** < 0,1 (Eksplisit dimensi gambar).
- **INP (Interaction to Next Paint):** < 200 ms (Minimalisasi main thread JS).
- **Schema.org:** Implementasi JSON-LD (`Article`, `BlogPosting`) untuk keterbacaan oleh mesin AI (SGE).

---

## 7. Alur Kerja Manajemen Konten

1. **Drafting:** Penulis tetap bekerja di Dashboard Blogger yang familiar.
2. **Asset Management:** Gambar dikelola secara otomatis oleh infrastruktur Google.
3. **Publishing:** Setelah _publish_, sistem ISR pada frontend melakukan regenerasi halaman secara otomatis/interval.

---

## 8. Langkah Strategis Implementasi

- **Frontend:** Next.js (App Router) + Tailwind CSS.
- **Data Strategy:** Normalisasi skema data untuk konsistensi UI.
- **Keamanan:** Pembatasan API Key melalui HTTP Referrer,API Key Protection (Backend Side),CORS Policy.

---

## 9. Pertanyaan Klarifikasi (Finalisasi)

Untuk menyelesaikan PRD ini, mohon konfirmasi beberapa poin berikut:

1. **Metode Agregasi:** Apakah konten digabung secara kronologis atau dipisah per-seksi blog? (kronologis)
2. **Postingan Unggulan:** Apakah perlu label khusus (misal: `featured`) untuk filter lintas blog? (tidak)
3. **Interaktivitas:** Apakah diperlukan fitur _tulis komentar_ (OAuth 2.0) atau cukup _baca komentar_? (baca komentar)
4. **Multibahasa:** Apakah ada rencana mendukung multi-bahasa melalui ID blog terpisah? (tidak)

---
