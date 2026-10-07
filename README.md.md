# Sistem Jadwal & Rekap Instalasi Farmasi

Aplikasi web berbasis HTML + JavaScript untuk manajemen jadwal shift, rekap kehadiran, jam kerja, dan analitik staf Instalasi Farmasi RSUD.

## 🎯 Fitur Utama

### Staff (Tanpa Login)
- **Form Pengajuan** — izin, sakit, cuti, telat/pulang cepat, absensi eror, tukar shift
- **Rekap Jadwal** — lihat siapa bertugas hari ini per ruangan
- **Jadwal Lengkap** — grid jadwal bulanan semua staf

### Admin (Perlu Login)
- **Dashboard Analytics** — KPI, grafik tren, peringkat kehadiran, multiskill, heatmap
- **Rekap Jam Kerja** — total jam & hari kerja per staf per periode
- **Rekap Kehadiran** — breakdown cuti/izin/sakit/DL + % kehadiran
- **Master Data Staff** — CRUD data kepegawaian
- **Riwayat Penempatan** — rotasi & penempatan staff + import massal dari Excel
- **Import Jadwal** — upload jadwal bulanan dari Excel
- **Daftar Pengajuan** — kelola pengajuan dari staff
- **Mode Edit** — edit jadwal langsung dari tabel

## 🛠️ Teknologi

| Komponen | Teknologi |
|----------|-----------|
| Frontend | HTML5, CSS3 (Custom Properties), Vanilla JavaScript |
| Database | Supabase (PostgreSQL) |
| Chart | Chart.js 4.4.0 |
| PDF Export | jsPDF 2.5.1 + html2canvas 1.4.1 |
| Font | Inter, JetBrains Mono (Google Fonts) |

## 📁 Struktur File
├── index.html # Aplikasi utama (single file)
├── penggunaan.html # Panduan penggunaan
├── README.md # Dokumentasi ini
└── (database di Supabase)

## 🗄️ Skema Database (Supabase)

### Tabel Utama

| Tabel | Kegunaan |
|-------|----------|
| `staff` | Master data kepegawaian (NIP, nama, jabatan, dll) |
| `jadwal` | Jadwal harian per staff (tanggal, kode shift, ruangan) |
| `staff_penempatan` | Riwayat rotasi & penempatan staff per ruangan |
| `pengajuan` | Pengajuan izin/sakit/cuti/telat/dll dari staff |
| `admin_users` | Daftar user admin (relasi ke `auth.users`) |
| `pengaturan_jam` | Jam kerja per kode shift (P, TS, S, M, dll) |
| `pengaturan_jam_ruangan` | Jam kerja per kombinasi Ruangan × Kode |
| `log_edit_jadwal` | Audit log perubahan jadwal |

### RPC (Stored Procedures)

| RPC | Fungsi |
|-----|--------|
| `rpc_get_jadwal_bulan` | Ambil jadwal 1 bulan penuh |
| `rpc_dashboard_stats` | Statistik dashboard (KPI, tren, dll) |
| `rpc_rekap_kehadiran` | Rekap kehadiran per staff per periode |
| `rpc_rekap_jam` | Rekap jam kerja per staff per tahun |
| `rpc_import_jadwal` | Bulk upsert jadwal dari Excel |
| `rpc_submit_pengajuan` | Submit pengajuan + auto-update jadwal |
| `rpc_cek_jadwal_staff` | Cek jadwal untuk tukar shift |
| `rpc_update_pengaturan_jam` | Update jam kerja global |
| `rpc_update_jam_ruangan` | Update jam kerja per ruangan |

## 🔑 Kode Shift

| Kode | Keterangan | Kategori |
|------|------------|----------|
| P | Pagi | Shift |
| TS | Middle | Shift |
| S | Siang | Shift |
| M | Malam | Shift |
| L | Libur | Off |
| SKT | Sakit | Off |
| IZIN | Izin | Off |
| DL | Dinas Luar | Off |
| CT | Cuti Tahunan | Off |
| CM | Cuti Bersalin | Off |

## 📊 Formula % Kehadiran
Piket = hariKerja (P + TS + S + M)
Non-Hadir = IZIN + SKT + DL + CT + CM
Hari Kerja Wajib = Piket + Non-Hadir
% Kehadiran = (Piket / Hari Kerja Wajib) × 100%


**Kategori:**
- ≥ 95% → Sangat baik (hijau)
- 80–94% → Cukup (kuning)
- < 80% → Perlu perhatian (merah)

## 🚀 Cara Menjalankan

1. **Persiapan Database:**
   - Buat project di [Supabase](https://supabase.com)
   - Jalankan SQL schema sesuai struktur tabel di atas
   - Aktifkan RLS (Row Level Security) sesuai kebutuhan

2. **Konfigurasi Aplikasi:**
   - Buka `index.html`
   - Cari bagian `SUPABASE_URL` dan `SUPABASE_ANON_KEY`
   - Ganti dengan kredensial project Anda

3. **Deploy:**
   - Upload `index.html` ke hosting statis (Netlify, Vercel, GitHub Pages)
   - Atau jalankan lokal via `file://`

4. **Setup Admin:**
   - Daftarkan user baru di Supabase Auth
   - Tambahkan `user_id` user tersebut ke tabel `admin_users`

## 📥 Import Data

### Import Jadwal dari Excel
1. Blok semua data di Excel → Copy (Ctrl+C)
2. Buka menu **Import Jadwal** → Paste (Ctrl+V)
3. Klik **Preview** → **Import ke Database**

**Format header tanggal yang didukung:**
- `1 Oktober 2026`
- `1-Okt-2026`
- `01/10/2026`
- `2026-10-01`

### Import Penempatan Staff
1. Siapkan data di Excel: `NIP | Ruangan | Peran | Tanggal Mulai | Tanggal Selesai | Keterangan`
2. Buka menu **Riwayat Penempatan** → klik **Import Penempatan**
3. Paste → Preview → Import

**Peran valid:** `Kepala Instalasi`, `Koordinator`, `Staff`, `PIC`, `Magang`, `Lainnya`

## 🎨 Fitur Interaktif

- **Klik grafik** → Popup fullscreen (perbesar tampilan)
- **Klik nama staff** → Detail kehadiran/jam kerja per bulan
- **Klik kartu ruangan** → Expand/collapse daftar staff bertugas
- **Tombol ESC** → Tutup semua modal

## 🔐 Keamanan

- Autentikasi via Supabase Auth (email + password)
- Admin dikontrol lewat tabel `admin_users`
- RLS di setiap tabel mencegah akses tidak sah
- Anon key dibatasi hanya untuk operasi publik

## 🐛 Troubleshooting

| Masalah | Solusi |
|---------|--------|
| Data tidak muncul | Cek Console (F12), pastikan RLS sudah aktif |
| Modal tidak muncul | Pastikan modal HTML ada **sebelum** tag `<script>` |
| Tampilan rusak | Cek keseimbangan tag `<div>` pembuka/penutup |
| Jam kerja desimal panjang | Gunakan `Number(x).toFixed(1)` di tampilan |
| Chart tidak rata | Pastikan `max-height` canvas sudah diset |

## 📝 Changelog

### v1.0 — Rilis Awal
- Fitur dasar jadwal, rekap, dan dashboard
- Form pengajuan staff

### v1.1 — Tambahan Fitur
- Import jadwal dari Excel
- Master data staff
- Riwayat penempatan + Import Penempatan
- Dashboard analytics dengan multiskill chart

### v1.2 — Perbaikan
- Fix bug event listener null
- Layout KPI horizontal
- Grafik seimbang + fullscreen popup
- Pembulatan jam kerja (1 desimal)

## 👨‍💻 Kontributor

Dikembangkan untuk **Instalasi Farmasi RSUD**.

## 📄 Lisensi

Internal use only — tidak untuk distribusi publik.

---

**Versi:** 1.2  
**Terakhir Diperbarui:** 2026
