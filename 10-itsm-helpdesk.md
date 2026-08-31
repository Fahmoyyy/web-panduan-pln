# Buku 10 - IT Service Management dan Helpdesk

**Modul:** `plnes_itsm`, `plnes_itsm_governance`, `plnes_itsm_operations`, `plnes_itsm_recovery`, `pln_helpdesk`
**Aplikasi:** IT Service Management, Helpdesk
**Peran utama:** IT Service Desk, Staff TI, Asman Aplikasi/Infrastruktur TI, SOC Analyst, End User

Modul ini menjalankan layanan TI PLN ES mengikuti kerangka ITIL: dari tiket
masuk, penanganan insiden dan problem, tata kelola, operasi harian, sampai
pemulihan bencana.

---

## 1. Peta Menu

```
IT Service Management
├── Tiket Saya                     tiket milik sendiri
├── Antrian Triage                 tiket yang belum dikategorikan (IT Service Desk)
├── Dashboard
├── Layanan
│   ├── Perangkat IT · Akses Data Center
├── Governance
│   ├── Proyek IT · SSDLC · Security Operation Center
│   ├── Manajemen Lisensi · PDP Compliance · User Access Matrix
├── Operasi
│   ├── Backup Jobs · Log Backup · Alert Monitoring
│   ├── Kapasitas Storage · Kebijakan Password
├── Recovery
│   ├── Restore Request · DRC Exercise
├── Master Data
│   ├── Service Catalog · Aplikasi · Role · BPO Assignment
│   ├── DC Location · Known Error Database
└── Konfigurasi > Pengaturan       (ITSM / Admin)

Helpdesk                            (aplikasi bawaan, diperluas)
└── Configuration > Helpdesk Problem
```

---

## 2. Peran dan Hak Akses

Kategori **PLN ES - ITSM**:

| Grup | Untuk siapa |
|---|---|
| ITSM / End User | Seluruh pegawai - membuat tiket, melihat tiketnya sendiri |
| ITSM / IT Service Desk | Penerima dan pemilah tiket (triage) |
| ITSM / Staff TI | Pelaksana penanganan |
| ITSM / Development Specialist | Pengembangan aplikasi, SSDLC |
| ITSM / Infrastructure Specialist | Infrastruktur, storage, backup |
| ITSM / SOC Analyst | Security Operation Center |
| ITSM / Asman Aplikasi TI | Atasan bidang aplikasi |
| ITSM / Asman Infrastruktur TI | Atasan bidang infrastruktur |
| ITSM / Manager STI | Manajer TI |
| ITSM / Business Process Owner | Pemilik proses bisnis pengguna aplikasi |
| ITSM / Data Protection Officer | Kepatuhan pelindungan data pribadi |
| ITSM / Auditor | Hanya melihat |
| ITSM / Admin | Administrator modul, pemegang menu Konfigurasi |

Menu yang dibatasi peran tertentu:

| Menu | Hanya untuk |
|---|---|
| Antrian Triage | ITSM / IT Service Desk |
| Security Operation Center | ITSM / SOC Analyst |
| Kapasitas Storage | ITSM / Infrastructure Specialist |
| Konfigurasi | ITSM / Admin |

---

## 3. Tiket - Alur Utama

Semua permintaan layanan TI masuk lewat satu pintu: **tiket**. Tiket punya
dua penanda yang menentukan perlakuannya.

### 3.1 Tipe tiket

| Tipe | Untuk apa |
|---|---|
| Insiden | Layanan terganggu |
| Problem | Penyebab akar dari insiden berulang |
| Service Request | Permintaan layanan biasa |
| Change Request | Permintaan perubahan |
| Access Request | Permintaan hak akses |
| Akses Data Center | Permintaan masuk ruang DC |
| Pembuatan User | User baru |
| Terminasi User | Penonaktifan user |
| Mutasi User | Perpindahan user |
| Reset Password | Reset kata sandi |
| Provisi Perangkat | Penyediaan perangkat |

### 3.2 Status tiket

Baru -> Terkategorisasi -> Sedang Diproses -> Terselesaikan -> Ditutup
Status lain: Dieskalasi, Menunggu User, Menunggu Vendor, Dibuka Kembali, Dibatalkan

### 3.3 Langkah penanganan

| # | Langkah | Tombol | Siapa |
|---|---|---|---|
| 1 | Pengguna membuat tiket | **New** di Helpdesk atau ITSM | End User |
| 2 | Tentukan tipe ITSM-nya, klik **Kategorisasi** | Status Terkategorisasi | IT Service Desk |
| 3 | Klik **Mulai Proses** | Status Sedang Diproses | Petugas |
| 4 | Bila tidak tertangani di tingkat ini: **Eskalasi** | Wizard eskalasi | |
| 5 | Klik **Selesaikan** | Status Terselesaikan | |
| 6 | Klik **Tutup** | Status Ditutup | |
| 7 | Bila masalah muncul lagi: **Buka Kembali** | | |

**Tombol yang muncul menurut tipe tiket:**

| Tombol | Muncul untuk tipe |
|---|---|
| **Provisi Akses** | Pembuatan User |
| **Wizard Penonaktifan** | Terminasi User |
| **Reset Password** | Reset Password |
| **Konversi ke Problem** | Insiden yang terdeteksi berulang |
| **Buat RFC** | Problem - membuat Request for Change |
| **Publish KEDB** | Problem yang sudah selesai - simpan solusinya ke Known Error Database |

> Tombol **Konversi ke Problem** hanya muncul bila sistem menandai tiket itu
> sebagai calon problem (insiden serupa berulang). Ini bukan tombol yang selalu
> ada.

### 3.4 Known Error Database (KEDB)

**Menu:** Master Data > Known Error Database (Draft -> Dipublikasikan -> Diarsipkan)

Kumpulan masalah yang sudah pernah terjadi beserta cara mengatasinya. Diisi
dari tiket problem lewat tombol **Publish KEDB**, atau dibuat manual lalu
**Publikasikan**. Petugas sebaiknya memeriksa KEDB dulu sebelum menangani tiket
baru.

---

## 4. Layanan

### 4.1 Perangkat IT

**Menu:** Layanan > Perangkat IT
**Status:** Stok -> Ditugaskan -> Dalam Perbaikan / Pensiun / Hilang-Rusak

| Tombol | Kegunaan |
|---|---|
| **Tugaskan** | Menyerahkan perangkat ke pegawai (dari Stok) |
| **Kembalikan ke Stok** | Perangkat dikembalikan |
| **Pensiun** | Perangkat tidak dipakai lagi |

### 4.2 Akses Data Center

**Menu:** Layanan > Akses Data Center
Draf -> **Ajukan** -> **Setujui** -> **Check In** -> **Check Out**

Check In dan Check Out mencatat waktu masuk dan keluar ruang data center.
Lokasi DC diambil dari Master Data > DC Location.

---

## 5. Governance

### 5.1 Proyek IT

Draf -> **Mulai Perencanaan** -> **Mulai Proyek** -> Selesai
Tombol **Tunda** menghentikan sementara; **Lanjutkan** menjalankannya lagi.

### 5.2 SSDLC

**Menu:** Governance > SSDLC (Draf -> **Mulai Review** -> **Selesai**)
Pemeriksaan keamanan pada siklus pengembangan perangkat lunak.

### 5.3 Security Operation Center

**Menu:** Governance > Security Operation Center (SOC Analyst)
**Status:** Baru -> Investigasi -> Terkendali -> Teratasi -> Ditutup

| Tombol | Kegunaan |
|---|---|
| **Investigasi** | Mulai menelaah kejadian keamanan |
| **Kendalikan** | Ancaman sudah dibatasi |
| **Selesaikan** | Kejadian tertangani |
| **False Positive** | Ternyata bukan ancaman |
| **Eskalasi ke CSO** | Naikkan ke Chief Security Officer |
| **Tutup** | Menutup kejadian |

### 5.4 Manajemen Lisensi

Status berubah sendiri mengikuti tanggal: Aktif -> Akan Kadaluwarsa ->
Kadaluwarsa. Dipakai memantau lisensi perangkat lunak yang harus diperpanjang.

### 5.5 PDP Compliance

**Menu:** Governance > PDP Compliance (Data Protection Officer)
Draf -> **Submit Review** -> **Setujui** -> Aktif -> **Arsip**
Berisi dokumen kepatuhan pelindungan data pribadi.

### 5.6 User Access Matrix

**Menu:** Governance > User Access Matrix
Draf -> **Setujui** -> Aktif -> **Cabut** / Kedaluwarsa

Matriks siapa berhak mengakses aplikasi apa dengan peran apa. Tiap baris akses
punya statusnya sendiri (Draf, Diminta, Aktif, Dicabut, Kedaluwarsa). Aplikasi
dan peran diambil dari Master Data > Aplikasi dan Role.

---

## 6. Operasi

### 6.1 Backup

| Menu | Isinya |
|---|---|
| **Backup Jobs** | Definisi pekerjaan backup (Aktif / Dijeda / Error) |
| **Log Backup** | Catatan tiap kali backup dijalankan |

| Tombol | Ada di | Kegunaan |
|---|---|---|
| **Jalankan Sekarang** | Backup Job | Menjalankan backup di luar jadwal |
| **Jeda** / **Aktifkan** | Backup Job | Menghentikan/melanjutkan penjadwalan |
| **Verifikasi** | Log Backup | Menandai hasil backup sudah diperiksa |
| **Buat Insiden** | Log Backup | Membuat tiket insiden bila backup gagal |

### 6.2 Alert Monitoring

**Menu:** Operasi > Alert Monitoring
Baru -> **Terima** -> Sedang Ditangani -> **Selesai**, atau
**False Positive**.

Tombol **Buat Insiden** mengubah alert menjadi tiket insiden. Tombol ini
hilang bila tiket sudah dibuat otomatis oleh sistem.

### 6.3 Kapasitas Storage dan Kebijakan Password

Keduanya bersifat catatan/master: kapasitas penyimpanan dipantau di
**Kapasitas Storage** (Infrastructure Specialist), sedangkan aturan kata sandi
didokumentasikan di **Kebijakan Password**.

---

## 7. Recovery

### 7.1 Restore Request

Draf -> **Ajukan** -> **Setujui** -> **Mulai Restore** -> **Selesai** ->
**Verifikasi**

Langkah **Verifikasi** di akhir memastikan data hasil restore benar-benar
terbaca, bukan sekadar prosesnya selesai.

### 7.2 DRC Exercise

**Menu:** Recovery > DRC Exercise
Draf -> **Setujui** -> **Mulai** -> **Selesai**
Latihan pemulihan bencana di Disaster Recovery Center.

---

## 8. Helpdesk (aplikasi bawaan)

App **Helpdesk** memakai tahapan (stage) dan tombol yang lebih sederhana:

| Tombol | Muncul saat tahap |
|---|---|
| **Confirm** | New, Open |
| **On Hold** | In Progress |
| **Solved** | In Progress, On Hold |
| **Cancel** | New, In Progress, On Hold |

Tiket yang sama bisa terlihat di kedua aplikasi. Bedanya: **Helpdesk**
menampilkan tahapan umum, sedangkan **IT Service Management** menampilkan
Status ITSM dan tombol-tombol khusus TI. Untuk pekerjaan TI, gunakan app IT
Service Management.

---

## 9. Bila Ada Masalah

| Gejala | Penyebab yang paling sering | Yang harus dilakukan |
|---|---|---|
| App IT Service Management tidak muncul | Belum punya grup ITSM | Beri minimal ITSM / End User |
| Menu Antrian Triage tidak ada | Bukan IT Service Desk | Memang dibatasi |
| Menu Security Operation Center tidak ada | Bukan SOC Analyst | Memang dibatasi |
| Tombol **Kategorisasi** tidak muncul | Tipe ITSM belum dipilih | Isi field Tipe ITSM dulu |
| Tombol **Provisi Akses** / **Reset Password** tidak ada | Tipe tiketnya bukan yang bersangkutan | Ubah Tipe ITSM sesuai kebutuhan |
| Tombol **Konversi ke Problem** tidak muncul | Tiket belum ditandai sebagai calon problem | Hanya muncul untuk insiden berulang |
| Tiket sudah selesai tapi masih di daftar aktif | Baru **Selesaikan**, belum **Tutup** | Klik **Tutup** |
| Alert tidak bisa dibuatkan insiden | Tiket sudah dibuat otomatis | Cek tiket yang sudah ada |
| Lisensi mendadak berstatus Kadaluwarsa | Status mengikuti tanggal berakhir | Perbarui tanggal setelah lisensi diperpanjang |
| Akses user masih aktif padahal pegawai sudah keluar | Baris User Access Matrix belum dicabut | Klik **Cabut** pada barisnya |

---

[<- Buku 09: GRC](09-grc.md) |
[Daftar isi](README.md) |
[Buku 11: Strategy Planning & Investment/PMO ->](11-strategy-planning-investment-pmo.md)
