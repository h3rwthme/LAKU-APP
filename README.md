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
│   │   |── History.jsx
│   |   ├── Login.jsx       
│   |   └── Register.jsx 
│   │
│   ├── services/
│   ├── api.js
│   └── auth.js   
│   │
│   ├── hooks/
│   │   └── useRecorder.js
│   │
│   ├── store/
│   │   |── useStore.js
│   |   └── useAuthStore.js
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
│   │   |── routes.py
│   │   ├── auth.py          
│   │
│   ├── services/
│   │   ├── whisper.py
│   │   ├── parser.py
│   │   └── transaction.py
|   |
│   ├── middleware/
│   │   └── auth_middleware.py 
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
user_id   ← WAJIB
name (text)
price (integer)
stock (integer)
created_at (timestamp)
```

## transactions

```
id (uuid)
user_id   ← WAJIB
product_id (uuid)
type (IN / OUT)
qty (integer)
total_price (integer)
created_at (timestamp)
```

---
# Security (RLS)
```
auth.uid() = user_id
```
---
# API Tambah Stok
```
POST /add-stock
{
  "product_id": "uuid",
  "qty": 10
}
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

Fitur LAKU APP (Final)

## Core Features (Fitur Utama)

* **Voice-Based Transaction**

  * Input transaksi hanya dengan suara
  * Contoh: "Laku kopi susu dua"

* **Smart Parsing (Text → Action)**

  * Deteksi intent (jual / restok)
  * Ambil nama produk & jumlah otomatis

* 📦 **Manajemen Stok Realtime**

  * Stok langsung update setelah transaksi
  * Sinkronisasi dengan database

---

## 👤 User System (Multi Akun)

* Setiap user memiliki akun sendiri (login/register)
* Data terisolasi (tidak tercampur antar user)
* Setiap user hanya bisa melihat & mengelola datanya sendiri

---

## Inventory Management

* ➕ Tambah stok manual (tanpa suara)
* ➖ Pengurangan stok otomatis saat penjualan
* 🛠️ Tambah & edit produk

---

## 📊 Dashboard & Monitoring

* Tampilan stok semua produk
* Informasi:

  * Nama barang
  * Harga
  * Sisa stok
* UI sederhana & mobile-friendly

---

## Transaction History

* Riwayat semua transaksi
* Tercatat otomatis dengan timestamp
* Bisa digunakan untuk analisis ke depan

---

## Voice Recording System

* Rekam suara langsung dari browser
* Support perangkat mobile
* Terintegrasi dengan AI (speech-to-text)

---

## Error Handling (Human Control)

* User bisa edit hasil transkripsi sebelum disimpan
* Mengurangi kesalahan dari AI

---

## Security & Data Isolation

* Sistem login (authentication)
* Data dilindungi dengan user_id
* Row Level Security (RLS) di database

---

## System Architecture

* Frontend: React + Vite + Tailwind
* Backend: FastAPI (Python)
* AI: Whisper (Speech-to-Text)
* Database: Supabase (PostgreSQL)

---

## Progressive Web App (PWA)

* Bisa di-install di HP
* Experience seperti aplikasi native
* Akses cepat tanpa buka browser manual

---

## 🐳 DevOps Ready

* Backend menggunakan Docker
* Mudah deploy ke cloud (Render / HuggingFace)
* Environment konsisten

---

## Future Features (Pengembangan)

* Analytics (penjualan harian/bulanan)
* Role system (owner / kasir)
* Export laporan (Excel / PDF)
* Multi bahasa & dialek
* Offline mode (edge processing)
* Integrasi pembayaran digital

---

# Inti Produk

> "Setiap user punya sistem stok sendiri, dan cukup bicara untuk mencatat transaksi."

---


# Author h3rwthme

Built with chaos & logic 
