# API Specification

**Headless Blogger Platform – API Contract v1**

---

## 1. Tujuan Dokumen

Dokumen ini mendefinisikan **kontrak API internal** antara:

- **Backend Python (FastAPI / Flask)**
- **Frontend (SSR / SSG / ISR / Edge)**

API ini **BERTINDAK SEBAGAI PROXY TERKONTROL** ke Blogger API v3 dan **MENYEMBUNYIKAN**:

- API Key
- blogId asli
- Struktur mentah Blogger API

---

## 2. Prinsip Desain API

### 2.1 API Philosophy

- **Read-only**
- **SEO-first**
- **Cache-heavy**
- **Stable contract**
- **Frontend-agnostic**

### 2.2 Hard Rules

- ❌ Tidak ada endpoint write
- ❌ Tidak expose data mentah Blogger
- ❌ Tidak ada pagination Blogger langsung
- ❌ Tidak ada auth client-side

---

## 3. Base URL & Versioning

```
/api/v1
```

Versioning **WAJIB eksplisit** untuk menghindari breaking change.

---

## 4. Environment Variables (Required)

```env
BLOGGER_API_KEY=your_api_key
BLOG_IDS=blog_id_1,blog_id_2,blog_id_3
CACHE_TTL_SECONDS=3600
```

- `BLOG_IDS` diparse menjadi list
- Semua fetch Blogger API dilakukan **SECARA PARALEL**

---

## 5. Data Normalization Layer

### 5.1 Normalized Post Schema (Canonical)

```json
{
  "id": "string",
  "slug": "string",
  "title": "string",
  "excerpt": "string",
  "content_html": "string",
  "published_at": "ISO-8601",
  "updated_at": "ISO-8601",
  "author": {
    "name": "string",
    "avatar": "string | null"
  },
  "labels": ["string"],
  "featured_image": {
    "src": "string (original/raw url)",
    "width": "number | null",
    "height": "number | null",
    "placeholder": "string | null"
  },
  "source": {
    "blog_id": "hashed",
    "blog_name": "string"
  },
  "seo": {
    "meta_title": "string",
    "meta_description": "string",
    "canonical_url": "string"
  }
}
```

> **Frontend TIDAK BOLEH mengasumsikan struktur lain.**

---

## 6. Endpoint Specification

---

### 6.1 Get Aggregated Posts (Homepage Feed)

**GET**

```
/api/v1/posts
```

#### Query Params (Optional)

| Param    | Type   | Default | Deskripsi          |
| -------- | ------ | ------- | ------------------ |
| `limit`  | int    | 10      | Jumlah post        |
| `label`  | string | null    | Filter label       |
| `source` | string | null    | Filter blog        |
| `page`   | int    | 1       | Pagination logical |

#### Behavior

- Menggabungkan post dari **SEMUA blog**
- Sort by `published_at DESC`
- Cache TTL: **60 menit**

#### Response

```json
{
  "data": [Post],
  "meta": {
    "total": 120,
    "page": 1,
    "limit": 10
  }
}
```

---

### 6.2 Get Single Post by Slug

**GET**

```
/api/v1/posts/{slug}
```

#### Behavior

- Slug **UNIK secara global**
- Fallback search lintas blog
- Cache TTL: **24 jam**

#### Response

```json
{
  "data": Post
}
```

---

### 6.3 Get Posts by Label (Category)

**GET**

```
/api/v1/labels/{label}
```

#### Behavior

- Label di-normalisasi (lowercase, hyphen)
- Merge lintas blog
- Cache TTL: **1 jam**

---

### 6.4 Get Available Labels (Taxonomy)

**GET**

```
/api/v1/labels
```

#### Response

```json
{
  "data": [
    {
      "label": "seo",
      "count": 42
    }
  ]
}
```

---

### 6.5 Get Sitemap Data

**GET**

```
/api/v1/sitemap
```

#### Behavior

- Digunakan oleh generator sitemap.xml
- Cache TTL: **24 jam**

---

## 7. Image Optimization Rules

### 7.1 Raw URL Strategy

Backend **WAJIB** mengembalikan URL gambar **ORIGINAL/RAW** dari Blogger (`s1600` atau `s0`).

Tujuannya adalah memberikan kebebasan penuh kepada Frontend (`next/image`) untuk melakukan resizing dinamis menggunakan Image Loader.

> Frontend **WAJIB** menggunakan custom loader untuk mengubah parameter `s1600` menjadi ukuran yang dibutuhkan viewport (misal `w1200-h630-p-k-no-nu`).

---

## 8. Caching Strategy

| Endpoint        | TTL      |
| --------------- | -------- |
| `/posts`        | 60 menit |
| `/posts/{slug}` | 24 jam   |
| `/labels`       | 1 jam    |
| `/sitemap`      | 24 jam   |

- Cache key = endpoint + query
- Cache invalidation = time-based only

---

## 9. Error Handling Standard

### Error Response Format

```json
{
  "error": {
    "code": "POST_NOT_FOUND",
    "message": "Post tidak ditemukan"
  }
}
```

### Error Rules

- ❌ Tidak expose stack trace
- ❌ Tidak expose Blogger error mentah
- ✅ Error message human-readable

---

## 10. Rate Limiting (Internal)

- Max 100 req/min per IP
- Priority:

  1. Sitemap
  2. Single post
  3. Feed

---

## 11. Security Constraints

- API Key **tidak pernah dikirim ke client**
- blogId di-hash sebelum response
- Header sensitif di-strip

---

## 12. Forbidden Anti-Patterns

❌ Client fetch langsung ke Blogger
❌ Client-side pagination Blogger
❌ Menyimpan konten ke database
❌ Menambahkan auth tanpa PRD update

---

## 13. Validation Checklist

Sebelum endpoint dianggap selesai:

- [ ] Response sesuai schema
- [ ] Cache aktif
- [ ] SEO field lengkap
- [ ] Tidak ada data mentah Blogger
- [ ] Lolos aturan `agents.md`

---
