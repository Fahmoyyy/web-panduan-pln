# Buku 03 - Business Development

**Modul:** `plnes_bdev`
**Aplikasi:** Business Development
**Peran utama:** Staff Fungsi Partnership, Staff Fungsi Pengembangan Bisnis, Asman, Manager, VP, Direksi, Fungsi Hukum

Modul ini menangani pengembangan usaha PLN ES di luar pekerjaan harian:
kemitraan dengan pihak ketiga, inkubasi bisnis baru, investasi, pembinaan anak
perusahaan, dan hubungan dengan pemangku kepentingan eksternal.

---

## 1. Peta Menu

```
Business Development
├── Strategic Priority
│   └── Roadmap Prioritas
├── Kemitraan                    <- inti modul
│   ├── Kemitraan                hub yang menaungi 6 dokumen di bawahnya
│   ├── NDA                      perjanjian kerahasiaan
│   ├── MoU                      nota kesepahaman
│   ├── PKS                      perjanjian kerja sama
│   ├── Amendment PKS
│   ├── Due Diligence            penilaian calon mitra
│   ├── Joint Planning Session
│   └── Grading Kemitraan
├── Inkubasi Bisnis
│   ├── Project Inkubasi
│   └── Evaluasi Inkubasi
├── Investasi
│   ├── Schema Investasi
│   ├── Monitoring Investasi
│   └── Evaluasi Investasi
├── Anak Perusahaan
│   ├── AP Roadmap
│   ├── Monitoring AP
│   └── Evaluasi AP
├── Relasi Eksternal
│   ├── Tracking Direktif Eksternal
│   └── Evaluasi Relasi Eksternal
└── Konfigurasi                  7 master (Admin & Direksi)
```

---

## 2. Peran dan Hak Akses

Semua grup ada di **Settings > Users > Access Rights**, kategori
**PLN ES Business Development**.

Peran BDEV disusun **berjenjang**: peran yang lebih tinggi otomatis mewarisi
hak peran di bawahnya. Karena semua peran pada akhirnya mewarisi
**Read Only (Auditor)**, seluruh menu terlihat oleh semua peran BDEV. Yang
membedakan adalah **apa yang boleh diubah**, bukan apa yang terlihat.

| Grup | Mewarisi | Untuk siapa |
|---|---|---|
| BDEV - Read Only (Auditor) | Role / User | Auditor, pemantau |
| BDEV - Staff Fungsi Partnership | Read Only + Approval User | Pelaksana kemitraan |
| BDEV - Staff Fungsi Pengembangan Bisnis | Read Only + Approval User | Pelaksana pengembangan bisnis |
| BDEV - Asman Pengembangan Bisnis | kedua Staff di atas | Reviewer L1 |
| BDEV - Manager Pengembangan Bisnis dan Mutu | Asman | Reviewer L2 |
| BDEV - VP Perencanaan | Manager | Persetujuan tingkat VP |
| BDEV - Direksi | VP Perencanaan | Persetujuan Direksi |
| BDEV - Direktur Utama | Direksi | Persetujuan tertinggi |
| BDEV - Fungsi Hukum | Read Only + Approval User | Review hukum |
| BDEV - Sekretaris Perusahaan | Read Only | Administrasi dokumen |
| BDEV - Direksi Anak Perusahaan | Read Only | Direksi AP |
| BDEV - Manager Non-Captive | Read Only | |
| BDEV - VP Niaga & Pemasaran | Read Only | |
| BDEV - Admin | Approval Manager + Manager + Sekretaris Perusahaan | Administrator modul |

**Konsekuensi pewarisan:** memberi user grup **Manager** berarti dia otomatis
juga Asman, kedua Staff, dan Read Only. Tidak perlu (dan sebaiknya jangan)
mencentang grup di bawahnya satu per satu.

**Menu Konfigurasi** hanya terbuka untuk **BDEV - Admin** dan **BDEV - Direksi**.

---

## 3. Master Data yang Harus Siap

**Business Development > Konfigurasi:**

| Master | Isinya | Dipakai di |
|---|---|---|
| Master Mitra BDEV | Daftar mitra (res.partner bertanda BDEV) | NDA, MoU, PKS |
| Master Anak Perusahaan | Daftar AP + statusnya | AP Roadmap, Monitoring AP |
| Master Klaster Bisnis | Pengelompokan bidang usaha | Kemitraan, Inkubasi |
| Master Stakeholder Eksternal | Pihak eksternal | Tracking Direktif |
| Kriteria Due Diligence | Butir penilaian + bobot | Due Diligence |
| Grading Threshold | Batas nilai tiap grade | Grading Kemitraan |
| Template Dokumen | Kerangka dokumen | Seluruh dokumen |

Mitra hanya muncul di pilihan dokumen BDEV bila kontaknya sudah ditandai
sebagai mitra BDEV. Dari form kontak yang sudah ditandai, tersedia tombol
pintas ke **NDA**, **MoU**, dan **PKS** miliknya.

---

## 4. Alur Persetujuan yang Berlaku Umum

Hampir semua dokumen BDEV memakai rangkaian status yang sama:

```
Draf -> Diajukan -> Direview L1 (Asman) -> Direview L2 (Manager)
     -> Direview Hukum -> Disetujui -> Ditandatangani -> Aktif
     -> Selesai / Tertutup
```

Status lain yang bisa muncul: **Ditolak**, **Dibatalkan**, **Diputus**
(terminasi), **Habis Berlaku**.

Tombol yang berlaku umum:

| Tombol | Muncul saat | Akibat |
|---|---|---|
| **Ajukan** | Draf | Dokumen masuk antrean review |
| **Tanda Tangani** | Diajukan (untuk NDA/MoU/PKS/Amendment) | Status Ditandatangani lalu Aktif |
| **Tolak** | Sedang direview | Status Ditolak |
| **Batalkan** | Draf atau Diajukan | Status Dibatalkan |
| **Terminasi** | Aktif | Wizard pemutusan, status Diputus |
| **Rollback** | tahap review | Mundur satu tahap |

> Tombol **Ajukan** pada Due Diligence baru aktif setelah **seluruh checklist
> terisi dan semua penilai sudah memberi nilai**. Kalau tombolnya tidak muncul,
> periksa dua hal itu lebih dulu.

---

## 5. Alur Kerja per Bagian

### 5.1 Kemitraan - hub dokumen

**Menu:** Business Development > Kemitraan > Kemitraan
**Tahapan (kanban):** Initial Meeting -> Introduction Stage -> Legal Compliance -> Agreement Stage -> Commercial Stage

Dokumen **Kemitraan** adalah payung yang menghimpun seluruh dokumen satu mitra
dalam satu tempat: NDA, MoU, PKS, Amendment PKS, Due Diligence, Joint Planning
Session, dan Grading. Tahapannya bergerak mengikuti kemajuan hubungan dengan
mitra tersebut.

| # | Langkah | Yang terlihat di layar | Catatan |
|---|---|---|---|
| 1 | **New** -> pilih mitra dan klaster bisnis | Kartu kemitraan di tahap Initial Meeting | Mitra harus sudah ada di Master Mitra BDEV |
| 2 | Buat NDA, jalankan Due Diligence | Dokumen terhubung ke kemitraan ini | Geser kartu ke Introduction Stage |
| 3 | Susun MoU lalu PKS | | Legal Compliance / Agreement Stage |
| 4 | Jalankan Grading Kemitraan | Nilai dan grade mitra keluar | Batas grade dari master Grading Threshold |
| 5 | Kemitraan berjalan komersial | Commercial Stage | |

### 5.2 NDA, MoU, PKS, Amendment PKS

Keempatnya berperilaku sama:

| # | Langkah | Yang terlihat di layar |
|---|---|---|
| 1 | **New** -> pilih mitra, isi masa berlaku dan isi pokok | Nomor dokumen otomatis |
| 2 | **Ajukan** | Masuk antrean review berjenjang |
| 3 | Reviewer L1, L2, dan Hukum memproses | Status bergerak |
| 4 | **Tanda Tangani** | Status Ditandatangani lalu Aktif |
| 5 | Bila perlu diputus: **Terminasi** | Wizard alasan pemutusan |

Dari form MoU tersedia tombol pintas ke **PKS** turunannya; dari PKS ke
**Amendment PKS**; dari PKS ke **Due Diligence** mitranya.

### 5.3 Due Diligence

**Menu:** Kemitraan > Due Diligence

| # | Langkah | Yang terlihat di layar | Catatan |
|---|---|---|---|
| 1 | **New** -> pilih mitra | Butir penilaian terisi dari Kriteria Due Diligence | Bobot ikut master |
| 2 | Lengkapi checklist dokumen | Kolom checklist | Wajib lengkap |
| 3 | Tiap penilai mengisi nilainya | Kolom nilai per penilai | Wajib terisi semua |
| 4 | Klik **Ajukan** | Skor akhir terhitung | Tombol tidak muncul bila langkah 2 atau 3 belum tuntas |

### 5.4 Grading Kemitraan

Menilai mitra yang sedang berjalan, lalu memberinya grade sesuai
**Grading Threshold**. Alur status sama dengan dokumen BDEV lainnya.

### 5.5 Inkubasi Bisnis

| Menu | Isinya |
|---|---|
| Project Inkubasi | Gagasan bisnis baru + milestone-nya |
| Evaluasi Inkubasi | Penilaian berkala atas project inkubasi |

Milestone inkubasi berstatus: Belum Mulai -> Berjalan -> Selesai, dan akan
ditandai **Terlambat** bila melewati tanggal rencana.

Dari form Project Inkubasi ada tombol pintas ke **Evaluasi Inkubasi** miliknya.

### 5.6 Investasi

**Status:** Inisiasi -> Planning -> Closure

| Menu | Isinya |
|---|---|
| Schema Investasi | Rancangan skema investasi |
| Monitoring Investasi | Pemantauan pelaksanaan |
| Evaluasi Investasi | Penilaian hasil |

Tombol **Teruskan ke Investment Committee** pada Schema Investasi mengirimkan
usulan ke Komite Investasi (lihat [Buku 11](11-strategy-planning-investment-pmo.md)).
Tombol ini menghilang setelah sekali diteruskan.

### 5.7 Anak Perusahaan

| Menu | Isinya |
|---|---|
| AP Roadmap | Peta jalan anak perusahaan |
| Monitoring AP | Pemantauan kinerja berkala |
| Evaluasi AP | Penilaian |

AP Roadmap punya tombol tambahan: **Aktifkan Roadmap** (dari Diajukan) dan
**Selesaikan** (dari Aktif). Status anak perusahaan sendiri di masternya:
Aktif, Dormant, Dalam Likuidasi, Tutup.

### 5.8 Relasi Eksternal

| Menu | Isinya |
|---|---|
| Tracking Direktif Eksternal | Arahan dari pihak luar dan tindak lanjutnya |
| Evaluasi Relasi Eksternal | Penilaian hubungan dengan stakeholder |

Tiap baris direktif berstatus: Belum Selesai -> Selesai / Dibatalkan.
Dari master Stakeholder Eksternal ada tombol pintas ke direktif miliknya.

### 5.9 Roadmap Prioritas

**Menu:** Strategic Priority > Roadmap Prioritas (Inisiasi -> Planning -> Closure)
Tiap baris prioritas berstatus Direncanakan -> Berjalan -> Selesai / Dibatalkan.

---

## 6. Bila Ada Masalah

| Gejala | Penyebab yang paling sering | Yang harus dilakukan |
|---|---|---|
| App Business Development tidak muncul | Belum punya grup BDEV apa pun | Beri minimal **BDEV - Read Only (Auditor)** |
| Semua menu terlihat tapi tidak bisa mengubah apa-apa | Memang hanya punya Read Only | Naikkan ke grup Staff sesuai fungsinya |
| Menu Konfigurasi tidak ada | Hanya untuk Admin dan Direksi | Minta Admin yang mengubah master |
| Tombol **Ajukan** di Due Diligence tidak muncul | Checklist belum lengkap atau ada penilai yang belum menilai | Lengkapi keduanya |
| Mitra tidak muncul di pilihan NDA/MoU/PKS | Kontaknya belum ditandai mitra BDEV | Tandai lewat Konfigurasi > Master Mitra BDEV |
| Sudah diberi grup Manager tapi masih diminta grup Staff | Pewarisan belum tersimpan | Simpan form user, lalu muat ulang halaman |
| Tombol **Teruskan ke Investment Committee** hilang | Sudah pernah diteruskan | Cek dokumen di Investment Committee & PMO |
| Dokumen mentok di Direview Hukum | Menunggu **BDEV - Fungsi Hukum** | Pastikan ada user dengan grup itu |

---

[<- Buku 02: Bisnis Inovatif](02-bisnis-inovatif-noncaptive.md) |
[Daftar isi](README.md) |
[Buku 04: Pengadaan, SCM & Vendor ->](04-pengadaan-scm-vendor.md)
