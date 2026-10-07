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
