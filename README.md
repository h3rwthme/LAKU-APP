# LAKU APP — Voice-Based POS untuk UMKM

Aplikasi kasir berbasis suara untuk membantu UMKM mencatat transaksi hanya dengan berbicara.

---

# Overview

LAKU adalah Progressive Web App (PWA) yang memungkinkan user:

* 🎤 Input transaksi lewat suara
* 📦 Melihat stok barang realtime
* ⚡ Proses cepat tanpa input manual

---

# Arsitektur Sistem

```
Frontend (React PWA)
        │
        ▼
Backend API (FastAPI)
        │
        ├── AI Service (Whisper - HuggingFace)
        │
        ▼
Database (Supabase - PostgreSQL)
```

---

# Alur Sistem

```
User Speak
   ↓
React Record Audio
   ↓
Kirim ke Backend (FastAPI)
   ↓
Whisper AI (Speech to Text)
   ↓
Parser (Text → Intent)
   ↓
Supabase (Database Update)
   ↓
Response ke Frontend
   ↓
UI Update (Realtime)
```

---

# 📁 Struktur Project

```
laku-app/
│
├── frontend/              # React + Vite + Tailwind
├── backend/               # FastAPI + AI Logic
├── docker/                # Docker config
├── docs/                  # ERD & dokumentasi
└── README.md
```

---

# Frontend Structure

```
frontend/
│
├── public/
│   ├── manifest.json
│   └── icons/
│
├── src/
│   ├── components/
│   │   ├── VoiceRecorder.jsx
│   │   ├── StockCard.jsx
│   │   └── TransactionModal.jsx
│   │
│   ├── pages/
│   │   ├── Dashboard.jsx
│   │   └── History.jsx
│   │
│   ├── services/
│   │   └── api.js
│   │
│   ├── hooks/
│   │   └── useRecorder.js
│   │
│   ├── store/
│   │   └── useStore.js
│   │
│   ├── utils/
│   │   └── formatter.js
│   │
│   ├── App.jsx
│   └── main.jsx
```

---

# Backend Structure

```
backend/
│
├── app/
│   ├── main.py
│   │
│   ├── api/
│   │   └── routes.py
│   │
│   ├── services/
│   │   ├── whisper.py
│   │   ├── parser.py
│   │   └── transaction.py
│   │
│   ├── models/
│   │   └── schema.py
│   │
│   ├── db/
│   │   └── supabase.py
│   │
│   ├── core/
│   │   └── config.py
│   │
│   └── utils/
│       └── helper.py
│
├── requirements.txt
└── Dockerfile
```

---

# Database Schema (Supabase)

## profiles

```
id (uuid)
store_name (text)
created_at (timestamp)
```

## products

```
id (uuid)
name (text)
price (integer)
stock (integer)
created_at (timestamp)
```

## transactions

```
id (uuid)
product_id (uuid)
type (IN / OUT)
qty (integer)
total_price (integer)
created_at (timestamp)
```

---

# AI Logic (Parser)

Contoh:

Input:

```
"Laku kopi susu dua"
```

Output:

```
Intent: OUT
Product: kopi susu
Qty: 2
```

Rule dasar:

* "laku", "jual" → transaksi keluar
* "beli", "restok" → transaksi masuk

---

# Voice Processing Flow

```
React (Record Audio)
        ↓
POST /upload-audio
        ↓
FastAPI
        ↓
Whisper API
        ↓
Text Result
        ↓
Parser Logic
        ↓
Supabase
        ↓
Response JSON
```

---

# 🐳 Docker Setup

```
FROM python:3.11

WORKDIR /app

COPY . .

RUN pip install -r requirements.txt

CMD ["uvicorn", "app.main:app", "--host", "0.0.0.0", "--port", "8000"]
```

---

# 🔐 Environment Variables

```
SUPABASE_URL=
SUPABASE_KEY=
HUGGINGFACE_API_KEY=
```

---

# Deployment Plan

### Frontend

* Vercel

### Backend

* Render / Hugging Face Spaces

---

# Catatan Penting

* Mulai dari rule-based parser dulu (jangan langsung AI kompleks)
* Selalu sediakan edit manual (antisipasi salah transkripsi)
* Fokus utama: UX sederhana & cepat

---

# Roadmap

* [x] UI Dashboard
* [x] Mock Data
* [ ] Voice Recording Real
* [ ] Backend Integration
* [ ] AI Parsing Improvement
* [ ] Deployment

---

# Filosofi Produk

> "1 tombol, 1 suara, langsung jadi transaksi."

---

# 👨‍💻 Author

Built with chaos & logic 🚀
