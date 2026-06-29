# MataAer Arsip

Sistem informasi pengarsipan surat untuk **PT Mata Aer Makmurindo** — perusahaan jasa parkir profesional. Aplikasi ini digunakan untuk mengelola surat masuk, surat keluar, dan data pengguna secara digital.

## Teknologi

| Layer | Teknologi |
|---|---|
| Frontend | React 19, Vite, Tailwind CSS 4, Shadcn/UI |
| Backend | Node.js, Express.js |
| Database | MySQL 8.0 |
| Chart | Recharts |
| Form | react-hook-form |
| Upload | Multer |
| Export PDF | jsPDF + jspdf-autotable |
| Notifikasi | react-toastify |

## Fitur

- **Login/Logout** — autentikasi dengan email dan password
- **Dashboard** — statistik surat masuk/keluar dalam bentuk chart dan kartu
- **Surat Masuk** — tambah, edit, hapus, cari, upload lampiran PDF
- **Surat Keluar** — tambah, edit, hapus, cari, upload lampiran PDF
- **Buku Agenda** — rekap agenda surat masuk dan keluar
- **Referensi Klasifikasi** — kelola kode klasifikasi surat
- **Kelola Pengguna** — CRUD akun user (khusus admin)

## Instalasi

### Prerequisites

- Node.js >= 18
- MySQL 8.0
- npm atau pnpm

### 1. Clone Repository

```bash
git clone https://github.com/yjm26/MataAer-Arsip.git
cd MataAer-Arsip
```

### 2. Setup Database

Buat database MySQL:

```sql
CREATE DATABASE pengarsipan;
USE pengarsipan;

-- Tabel akun
CREATE TABLE account (
  id INT AUTO_INCREMENT PRIMARY KEY,
  email VARCHAR(255) NOT NULL UNIQUE,
  password VARCHAR(255) NOT NULL,
  role ENUM('admin', 'user') DEFAULT 'user'
);

-- Tabel surat masuk
CREATE TABLE surat_masuk (
  nomor_surat VARCHAR(50) PRIMARY KEY,
  pengirim VARCHAR(255),
  nomor_agenda VARCHAR(50),
  tanggal_surat DATE,
  tanggal_diterima DATE,
  ringkasan TEXT,
  kode_klasifikasi VARCHAR(100),
  keterangan TEXT,
  lampiran VARCHAR(255)
);

-- Tabel surat keluar
CREATE TABLE surat_keluar (
  nomor_surat VARCHAR(50) PRIMARY KEY,
  tujuan VARCHAR(255),
  nomor_agenda VARCHAR(50),
  tanggal_surat DATE,
  tanggal_keluar DATE,
  ringkasan TEXT,
  kode_klasifikasi VARCHAR(100),
  keterangan TEXT,
  lampiran VARCHAR(255)
);

-- Tabel referensi klasifikasi
CREATE TABLE refrensi (
  id INT AUTO_INCREMENT PRIMARY KEY,
  klasifikasi VARCHAR(255) NOT NULL
);

-- Insert admin default
INSERT INTO account (email, password, role) VALUES ('admin@mataaer.com', 'admin123', 'admin');
```

### 3. Jalankan Backend

```bash
cd backend
npm install
node index.js
```

Backend berjalan di `http://localhost:5000`

### 4. Jalankan Frontend

```bash
# Di root folder
npm install
npm run dev
```

Frontend berjalan di `http://localhost:5173`

## Struktur Folder

```
├── backend/
│   ├── index.js          # Entry point Express server
│   ├── db.js             # Koneksi MySQL pool
│   ├── routes/
│   │   ├── login.js      # Endpoint autentikasi
│   │   ├── accounts.js   # CRUD akun pengguna
│   │   ├── suratMasuk.js # CRUD surat masuk + upload
│   │   ├── suratKeluar.js# CRUD surat keluar + upload
│   │   ├── chart.js      # Data statistik dashboard
│   │   └── refrensi.js   # CRUD klasifikasi
│   └── uploads/          # Folder file lampiran
├── src/
│   ├── pages/            # Halaman utama (login, dashboard, surat, dll)
│   ├── layouts/          # Komponen layout (sidebar, table, form, chart)
│   ├── services/         # Service layer untuk API calls
│   ├── components/ui/    # Komponen UI reusable (button, card, input, dll)
│   ├── data/             # Data statis (menu items)
│   ├── hooks/            # Custom hooks
│   ├── lib/              # Utility functions
│   └── routes.jsx        # Konfigurasi routing React
├── public/img/           # Asset gambar (logo, greeting)
├── package.json
└── vite.config.js
```

## Endpoint API

| Method | Endpoint | Keterangan |
|---|---|---|
| POST | `/api/login` | Login (email + password) |
| GET | `/api/accounts` | Ambil semua akun |
| POST | `/api/accounts` | Tambah akun baru |
| DELETE | `/api/accounts/:id` | Hapus akun |
| GET | `/api/surat-masuk` | Ambil semua surat masuk |
| POST | `/api/surat-masuk` | Tambah surat masuk (+ lampiran) |
| PUT | `/api/surat-masuk/:id` | Edit surat masuk |
| DELETE | `/api/surat-masuk/:id` | Hapus surat masuk |
| GET | `/api/surat-keluar` | Ambil semua surat keluar |
| POST | `/api/surat-keluar` | Tambah surat keluar (+ lampiran) |
| PUT | `/api/surat-keluar/:id` | Edit surat keluar |
| DELETE | `/api/surat-keluar/:id` | Hapus surat keluar |
| GET | `/api/chart` | Data statistik dashboard |
| GET | `/api/refrensi` | Ambil semua klasifikasi |
| POST | `/api/refrensi` | Tambah klasifikasi |
| DELETE | `/api/refrensi/:id` | Hapus klasifikasi |

## Konfigurasi Database

Edit file `backend/db.js` jika konfigurasi MySQL berbeda:

```js
const db = mysql.createPool({
  host: 'localhost',
  user: 'root',
  password: '',           // ganti sesuai password MySQL kamu
  database: 'pengarsipan'
});
```

## Lisensi

Proyek ini dibuat untuk keperluan Kerja Praktik (KP) — Universitas Pamulang.
