# Buku 11 - Strategy Planning dan Investment/PMO

**Modul:** `plnes_strategy_planning`, `plnes_investment_pmo`
**Aplikasi:** Strategy Planning (REN), Investment Committee & PMO
**Peran utama:** Staff/Asman/Manager Perencanaan Korporat, VP Perencanaan, Direksi, Ketua PMO, Anggota TVV

Dua modul perencanaan tingkat korporat: **REN** menyusun rencana dan mengukur
kinerja; **KSK** memutuskan investasi dan menjalankan program lintas fungsi.

---

## 1. Peta Menu

```
Strategy Planning (REN)
├── Rencana Jangka Panjang > Daftar RJP
├── RKAP
│   ├── Daftar RKAP · Usulan Unit
│   ├── BAT (Berita Acara Trilateral) · SKKO/SKKI
├── Kontrak Manajemen
│   ├── KM Korporat · Semua KM · Revisi KM
├── Manajemen Kinerja
│   ├── Laporan KPI · NKO · NKO Korporat
├── Forum PBR/PD
│   ├── PBR (PLN Pusat) · PD (Unit Pelaksana) · Semua Forum
├── Dashboard
│   ├── Dashboard KM · Dashboard NKO · KPI Saya
│   ├── Laporan KPI Pending · Action Items PBR
├── Master Data
│   ├── Strategic Intent · KPI Library · Tahun Anggaran
│   ├── Asumsi Makro · Pedoman RKAP · Kalender Kinerja
└── Konfigurasi > Pengguna                      (REN / Admin)

Investment Committee & PMO
├── Investasi
│   ├── Usulan Investasi                        <- pintu masuk
│   ├── Kajian Kelayakan Finansial (KKF)
│   ├── Kajian Kelayakan Operasional (KKO)
│   ├── Kajian Risiko
│   ├── Rapat Tim Verifikasi & Validasi (TVV)
│   ├── Review Holding · Keputusan Direksi
├── Program Management Office
│   ├── Program · Kebutuhan Awal · Kerangka Acuan Kerja
│   ├── Monitoring Mingguan · Notulen Rapat
│   ├── Laporan Steering Committee · Tindak Lanjut
│   └── Anggota Steering Committee
├── Dashboard > Dashboard Investasi · Dashboard PMO
└── Konfigurasi > Jenis Investasi · Anggota TVV · Pengaturan
```

---

## 2. Peran dan Hak Akses

### Strategy Planning (kategori **PLN ES - REN**)

| Grup | Untuk siapa |
|---|---|
| REN / Staff Unit | Pengisi usulan dan laporan di unit |
| REN / Manager Unit | Persetujuan tingkat unit |
| REN / Staff Kinerja & Portofolio | Pengelola KPI dan NKO |
| REN / Asman Perencanaan Korporat | Reviewer korporat |
| REN / Manager Perencanaan Korporat | Manajer perencanaan |
| REN / VP Perencanaan | Persetujuan VP |
| REN / Direksi | Persetujuan Direksi |
| REN / Divisi Keuangan | Sisi keuangan RKAP |
| REN / Divisi Pembina | Divisi pembina |
| REN / PLN Pusat (Observer) | Pengamat dari PLN Pusat |
| REN / Read Only | Hanya melihat |
| REN / Admin | Administrator modul |

### Investment & PMO (kategori **PLN ES - KSK**)

Dibagi dua rumpun. **KSK INV** untuk komite investasi:

| Grup | Perannya |
|---|---|
| KSK INV / Pengusul | Mengajukan usulan investasi |
| KSK INV / VP Sponsor | Mengevaluasi dan meneruskan ke TVV |
| KSK INV / Sekretaris TVV | Memverifikasi kelengkapan, menjadwalkan rapat TVV |
| KSK INV / Anggota TVV | Anggota tim verifikasi dan validasi |
| KSK INV / Divisi Keuangan | Validasi kajian kelayakan finansial |
| KSK INV / Direksi | Pengambil keputusan |
| KSK INV / Sekretaris Perusahaan | Mencatat keputusan Direksi |
| KSK INV / Board of Commissioners | Dewan Komisaris |
| KSK INV / Holding Observer | Pengamat dari holding |

**KSK PMO** untuk program management office:

| Grup | Perannya |
|---|---|
| KSK PMO / Ketua PMO | Mereview KAK, kebutuhan awal, notulen |
| KSK PMO / Analis PMO | Pelaksana harian PMO |
| KSK PMO / Steering Committee | Menyetujui KAK dan laporan |
| KSK PMO / Liaison Officer | Penghubung antar fungsi |
| KSK PMO / Sekretaris Direksi | Administrasi |
| KSK PMO / Business Process Owner | Pemilik proses |

Ditambah **KSK / Admin** (administrator, pemegang Konfigurasi) dan
**KSK / Read Only**.

---

## 3. Strategy Planning (REN)

### 3.1 Master data lebih dulu (REN / Admin)

| Master | Isinya | Alur |
|---|---|---|
| Strategic Intent | Arah strategis korporat | Draf -> **Aktivasi** -> **Arsipkan** |
| KPI Library | Kumpulan indikator kinerja baku | Draf -> **Validasi** -> **Aktivasi** -> **Arsipkan** |
| Tahun Anggaran | Periode anggaran | - |
| Asumsi Makro | Asumsi ekonomi (kurs, inflasi, dll) | Draf -> **Aktivasi** -> **Arsipkan** |
| Pedoman RKAP | Pedoman penyusunan RKAP | Draf -> **Aktivasi** -> **Arsipkan** |
| Kalender Kinerja | Jadwal siklus kinerja | Draf -> **Aktivasi** -> **Arsipkan** |

> Master yang masih Draf tidak akan terpakai. KPI Library harus **Validasi**
> dulu baru bisa **Aktivasi**.

### 3.2 RJP - Rencana Jangka Panjang

Draf -> **Ajukan** -> **Kirim ke Review** -> **Aktifkan** -> Kedaluwarsa
Tombol **Tolak** membuka wizard alasan penolakan.

### 3.3 RKAP - rencana kerja dan anggaran tahunan

Ini alur terpanjang di REN, melibatkan unit sampai RUPS.

| # | Langkah | Tombol | Siapa |
|---|---|---|---|
| 1 | Unit menyusun usulannya | **Usulan Unit** -> **Ajukan** | REN / Staff Unit |
| 2 | Unit menyetujui usulannya | **Setujui** | REN / Manager Unit |
| 3 | Buat dokumen RKAP korporat, klik **Ajukan** | Status Diajukan | Perencanaan Korporat |
| 4 | Klik **Konsolidasi** | Wizard menggabungkan seluruh usulan unit | |
| 5 | Klik **Teruskan ke Direksi** | Status Dalam Review | |
| 6 | Klik **Submit RUPS** | Diajukan ke RUPS | |
| 7 | Klik **Aktivasi** | RKAP berlaku | |
| 8 | Perlu koreksi setelah aktif: **Rollback (4-eyes)** | Wizard dua tahap | |

Tombol pintas **Usulan Unit** dan **BAT** membuka dokumen terkait.

**BAT (Berita Acara Trilateral):** Draf -> **Tanda Tangani**.

**SKKO/SKKI:** Draf -> **Ajukan** -> **Setujui** -> **Terbitkan**.

> **Rollback (4-eyes)** berjalan dua tahap: satu orang menekan **Kirim
> Permintaan**, orang lain menekan **Eksekusi Rollback**. Satu orang tidak bisa
> melakukan keduanya. Ini kontrol yang disengaja.

### 3.4 Kontrak Manajemen (KM)

**Menu:** Kontrak Manajemen > KM Korporat
**Status:** Draf -> Diajukan -> Tervalidasi SMART -> Dalam Review -> Ditandatangani -> Aktif

| # | Langkah | Tombol | Catatan |
|---|---|---|---|
| 1 | **New** -> susun KPI beserta target | KPI dari KPI Library | |
| 2 | Klik **Ajukan** | Status Diajukan | |
| 3 | Klik **Validasi SMART** | Sistem memeriksa KPI memenuhi kaidah SMART | |
| 4 | Klik **Kirim ke Review** | Status Dalam Review | |
| 5 | Klik **Tanda Tangan** | Status Ditandatangani | |
| 6 | Klik **Aktivasi** | KM berlaku, KPI mulai diukur | |
| 7 | Perlu koreksi setelah aktif | **Rollback (4-eyes)** | dua orang berbeda |

**Revisi KM** memakai alur yang sama, dipakai bila target perlu diubah di
tengah periode.

### 3.5 Manajemen Kinerja

**Laporan KPI:** Draf -> **Ajukan** -> **Setujui** -> **Kirim**
Tombol **Ambil Snapshot KPI** (saat masih Draf) menyalin capaian KPI dari
Kontrak Manajemen ke laporan, sehingga angka laporan tidak ikut berubah bila
data sumbernya berubah kemudian.

**NKO (Nilai Kinerja Organisasi):** Draf -> **Ajukan Review** -> **Setujui** ->
**Publikasikan**. Juga punya tombol **Ambil Snapshot KPI**.

**Dashboard:** Dashboard KM, Dashboard NKO, **KPI Saya** (KPI yang menjadi
tanggung jawab user yang sedang login), dan **Laporan KPI Pending**.

### 3.6 Forum PBR/PD

**Menu:** Forum PBR/PD
Draf -> **Jadwalkan** -> **Mulai** -> **Selesaikan** -> **Tutup**

PBR adalah forum dengan PLN Pusat, PD dengan Unit Pelaksana - keduanya memakai
dokumen yang sama, hanya disaring bedanya di menu. Hasil forum berupa
**Action Items PBR** (Terbuka -> Sedang Dikerjakan -> Selesai) yang bisa
dipantau lewat Dashboard.

---

## 4. Investment Committee

### 4.1 Alur usulan investasi

**Menu:** Investasi > Usulan Investasi

| # | Langkah | Tombol | Siapa |
|---|---|---|---|
| 1 | **New** -> susun usulan investasi | | KSK INV / Pengusul |
| 2 | Klik **Ajukan ke VP** | Status Diajukan ke VP | Pengusul |
| 3 | Klik **VP: Evaluasi & Teruskan ke TVV** | Status Diajukan ke TVV | VP Sponsor |
| 4 | Klik **Sek TVV: Verifikasi Kelengkapan** | Kelengkapan berkas diperiksa | Sekretaris TVV |
| 5 | Lengkapi tiga kajian (lihat di bawah) | | |
| 6 | Jadwalkan dan jalankan **Rapat TVV** | **Finalize Meeting** | Sekretaris TVV |
| 7 | Bila perlu: **Review Holding** | | |
| 8 | **Keputusan Direksi** -> **Putuskan** | Status Sudah Diputuskan | Sekretaris Perusahaan |
| 9 | Klik **Finance: Selesaikan** | Pelaksanaan keuangan selesai | Divisi Keuangan |

### 4.2 Tiga kajian pendukung

| Kajian | Alur | Siapa |
|---|---|---|
| **KKF** - Kajian Kelayakan Finansial | Draf -> **Validasi (Divisi Finance)** | Divisi Keuangan |
| **KKO** - Kajian Kelayakan Operasional | Draf -> Diajukan -> Disetujui | Fungsi operasi |
| **Kajian Risiko** | Draf -> Diajukan | Fungsi risiko |

Usulan tidak akan lolos rapat TVV bila ketiganya belum lengkap.

### 4.3 Review Holding

Draf -> **Kirim ke Holding** -> Dalam Review -> **Catat Respon** ->
Disetujui / Ditolak. Dipakai untuk investasi yang perlu persetujuan induk.

---

## 5. Program Management Office

### 5.1 Alur program

**Menu:** Program Management Office > Program
**Status:** Draf -> KAK Dalam Penyusunan -> KAK Disetujui SC -> Dalam Pelaksanaan

| # | Langkah | Dokumen | Tombol |
|---|---|---|---|
| 1 | Susun kebutuhan awal | Kebutuhan Awal | **Ajukan ke Ketua PMO** -> **Setujui** |
| 2 | Susun Kerangka Acuan Kerja | KAK | **Review Ketua PMO** -> **Submit ke SC** -> **Setujui (SC)** -> **Aktifkan KAK** |
| 3 | Teruskan ke pengadaan | KAK | **Route Procurement** |
| 4 | Pantau mingguan | Monitoring Mingguan | **Submit** -> **Review** |
| 5 | Catat rapat | Notulen Rapat (MoM) | **Review Ketua PMO** -> **Finalize MoM** |
| 6 | Laporkan ke Steering Committee | Laporan SC | **Submit ke SC** -> **SC Review Selesai** |
| 7 | Kelola tindak lanjut | Tindak Lanjut | **Tandai Selesai** |

Anggota Steering Committee diatur di **Anggota Steering Committee**
(KSK / Admin).

---

## 6. Bila Ada Masalah

| Gejala | Penyebab yang paling sering | Yang harus dilakukan |
|---|---|---|
| App Strategy Planning tidak muncul | Belum punya grup REN | Beri minimal REN / Read Only |
| KPI tidak muncul saat menyusun KM | KPI Library masih Draf atau Tervalidasi | Jalankan **Validasi** lalu **Aktivasi** |
| Tombol **Validasi SMART** menolak | Ada KPI yang belum memenuhi kaidah SMART | Perbaiki rumusan KPI-nya |
| Angka laporan KPI berubah sendiri | Snapshot belum diambil | Klik **Ambil Snapshot KPI** saat masih Draf |
| Tombol **Konsolidasi** tidak muncul di RKAP | RKAP belum berstatus Diajukan | Klik **Ajukan** dulu |
| Usulan unit tidak ikut terkonsolidasi | Usulan belum disetujui unitnya | Manager Unit harus klik **Setujui** |
| **Rollback (4-eyes)** tidak bisa diselesaikan sendiri | Memang harus dua orang berbeda | Minta rekan menekan **Eksekusi Rollback** |
| Usulan investasi tidak bisa masuk rapat TVV | KKF / KKO / Kajian Risiko belum lengkap | Lengkapi ketiganya |
| Tombol **Validasi (Divisi Finance)** hilang | Sudah pernah divalidasi | Cek penanda validasi di dokumen KKF |
| KAK tidak bisa diteruskan ke pengadaan | KAK belum Aktif | **Setujui (SC)** lalu **Aktifkan KAK** |
| Menu Konfigurasi tidak ada | Bukan REN / Admin atau KSK / Admin | Minta administrator modul |

---

[<- Buku 10: ITSM & Helpdesk](10-itsm-helpdesk.md) |
[Daftar isi](README.md)
