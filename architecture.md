# Arsitektur Teknis: Headless Blog (Blogger API Proxy)

Dokumen ini merinci arsitektur backend berbasis Python yang berfungsi sebagai _Middleware_ dan _Aggregator_ data antara Blogger API v3 dengan Frontend modern.

## 1. Stack Teknologi & Spesifikasi

- **Runtime:** Python 3.12+ (Typed Python).
- **Web Framework:** **FastAPI** (Asynchronous, High Performance).
- **HTTP Client:** `httpx` (Async support for parallel API calls).
- **Data Validation:** `Pydantic v2` (Strict schema validation).
- **JSON Engine:** `orjson` (High-speed serialization).
- **Environment:** `python-dotenv`.
- **Data Persistence:** **None** (Stateless, mengandalkan caching memory/ISR di sisi frontend).

---

## 2. Alur Data (Data Flow)

Sistem ini bertindak sebagai transformator data. Backend mengambil data mentah dari Blogger, memvalidasinya melalui skema Pydantic, melakukan normalisasi (seperti optimasi URL gambar), dan mengirimkan JSON yang bersih ke Frontend.

```mermaid
graph TD
    User((User/Browser)) -->|Request| Consumer[Frontend/Client (External)]
    Consumer -->|API Call| BE[Backend - FastAPI]

    subgraph "Grand Portal Backend (Python)"
        BE -->|Async Dispatch| AGG[Aggregator Service]
        AGG -->|Parallel Fetch| B_API[Blogger API v3]
        B_API -->|JSON Response| AGG
        AGG -->|Pydantic Model| NORM[Normalizer & Validator]
        NORM -->|Transform| IMG[Image Optimizer (Raw URL)]
    end

    IMG -->|Clean JSON| BE
    BE -->|Response| Consumer
```

````

---

## 3. Struktur Folder Modular

Struktur ini dirancang agar mudah dikembangkan (scalable) dan memisahkan logika bisnis dari pemanggilan API eksternal.

```text
/
├── app/
│   ├── main.py              # Entry point FastAPI
│   ├── api/                 # Endpoint / Routes
│   │   ├── v1/
│   │   │   ├── posts.py     # Routing artikel
│   │   │   └── blogs.py     # Routing info blog & label
│   ├── core/                # Konfigurasi Global
│   │   ├── config.py        # Pydantic Settings (env loader)
│   │   └── security.py      # Logic API Key validation
│   ├── schemas/             # Pydantic Models (Data Validation)
│   │   ├── blogger.py       # Skema respons asli Blogger
│   │   └── response.py      # Skema respons output untuk Frontend
│   ├── services/            # Logika Bisnis & Integrasi API
│   │   └── blogger_svc.py   # httpx implementation untuk Blogger API
│   └── utils/               # Helper functions
│       └── image_helper.py  # Blogger/Google Image CDN transformer
├── .env                     # API_KEY & BLOG_IDS
├── requirements.txt
└── README.md

````

---

## 4. Komponen Utama

### 4.1. Service: Multi-Blog Aggregator (`services/blogger_svc.py`)

Menggunakan `httpx.AsyncClient` untuk melakukan _concurrent requests_ ke berbagai `BLOG_IDS`. Data kemudian digabung dan diurutkan berdasarkan `published` date secara universal.

### 4.2. Schema: Pydantic Validation (`schemas/`)

Karena Blogger API sering memberikan data yang tidak konsisten (misal: label bisa ada atau tidak), Pydantic digunakan untuk memastikan frontend selalu menerima struktur data yang aman dengan nilai default.

### 4.3. Utility: Image Transformer (`utils/image_helper.py`)

Fungsi khusus untuk memanipulasi string URL gambar Google.

- Input: `.../s1600/image.jpg`
- Output: `.../w800-h600-c-rw/image.webp` (disesuaikan dengan kebutuhan komponen frontend).

---

## 5. Strategi Tanpa Database (Stateless)

Sesuai dengan PRD, sistem ini **tidak menggunakan database server** (No SQL/No RDBMS).

- **Source of Truth:** Server Google (Blogger).
- **Concurrency:** Memanfaatkan `asyncio` Python untuk menangani ribuan request tanpa memblokir I/O.
- **Caching:** Backend tidak menyimpan cache permanen. Caching dikelola oleh:

1. **Frontend (Next.js ISR):** Menahan data selama periode tertentu.
2. **CDN (Cloudflare/Vercel):** Mengurangi beban ke backend FastAPI.

---

## 6. Keamanan & Optimasi

- **CORS:** Dibatasi hanya untuk domain frontend resmi.
- **Environment Variables:** `BLOG_IDS` disimpan dalam format list di `.env` dan di-parse saat aplikasi _startup_.
- **Performance:** Penggunaan `orjson` sebagai default JSON response di FastAPI untuk kecepatan serialisasi data yang lebih tinggi dibanding pustaka standar.

---
