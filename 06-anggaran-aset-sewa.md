# Buku 06 - Anggaran, Aset dan Sewa

**Modul:** `pln_account_budget`, `kaitech_asset_management_v19`, `ss_lease_accounting`, `account_parent`
**Aplikasi:** Anggaran, Assets, Lease Management
**Peran utama:** Budget Admin, Request Budget User/Manager, Lease Officer/Manager, akuntansi aset

---

## 1. Peta Menu

```
Anggaran
├── Transaksi
│   ├── SKK (Anggaran)                dokumen anggaran induk
│   ├── Rekomposisi Anggaran          pergeseran antar pos
│   ├── Permohonan Tambah             permintaan tambahan anggaran
│   └── Luncuran Sisa Anggaran (Manual)
├── Pelaporan
│   ├── Analisa Serapan Anggaran      realisasi vs pagu
│   ├── Alokasi Otomatis Payung
│   ├── Jembatan Luncuran PO
│   ├── Log Rekomposisi Anggaran
│   └── Akun Biaya Belum Dipetakan
└── Konfigurasi
    ├── Pos Anggaran · Kategori Anggaran · Payung Anggaran
    └── No Kategori SKK · Mapping Luncuran

Assets
├── (daftar Assets & Asset Models bawaan Odoo)
├── Assets Location · Assets Movement · Batch Asset Movement
└── Generate Assets Entries
Accounting > Configuration > Assets and Revenues > Asset Types
Accounting > Reporting > Assets

Lease Management
├── Lease Agreements · Lease Accounting · Lease Schedule Summary
├── Dashboard
├── Reporting > Liability Forecast · Posting & Reconciliation · Consolidated Analysis
└── Configuration > Lease Types
```

---

## 2. Peran dan Hak Akses

| Grup | Kategori | Bisa apa |
|---|---|---|
| Budget Admin | PLN Account Budget | Seluruh menu Anggaran termasuk Konfigurasi |
| Request Budget User | PLN Account Budget | Membuat Permohonan Tambah |
| Request Budget Manager | PLN Account Budget | Menyetujui/menolak Permohonan Tambah |
| Transfer Budget | PLN Account Budget | Melakukan Rekomposisi Anggaran |
| Lease User / Officer / Manager | Lease Management | Lihat / kerjakan / setujui sewa |
| Show Chart of Account Structure | account_parent | Memunculkan menu struktur COA berjenjang |

Modul **Assets** memakai grup akuntansi bawaan Odoo (Accounting / Billing dan
Administrator), bukan grup tersendiri.

---

## 3. Anggaran

### 3.1 SKK - dokumen anggaran

**Menu:** Anggaran > Transaksi > SKK (Anggaran)
**Status:** Draft -> Dikonfirmasi -> Divalidasi -> Selesai

| # | Langkah | Tombol | Catatan |
|---|---|---|---|
| 1 | **New** -> isi periode, pos anggaran, dan pagu tiap baris | | Pos dari Konfigurasi > Pos Anggaran |
| 2 | Klik **Konfirmasi** | Status Dikonfirmasi | **Baru di status ini** rekomposisi dan permohonan tambah bisa dibuat |
| 3 | Klik **Validasi** lalu **Selesaikan** | Anggaran berlaku | |
| 4 | Perlu koreksi: **Set ke Draft** | Dari Dikonfirmasi atau Dibatalkan | |

Dari form SKK ada tombol pintas ke **Rekomposisi**, **Permohonan Tambah**, dan
**Alokasi Payung** yang terkait.

### 3.2 Rekomposisi Anggaran - menggeser pagu antar pos

**Menu:** Anggaran > Transaksi > Rekomposisi Anggaran, atau tombol
**Rekomposisi Anggaran** di form SKK berstatus Dikonfirmasi.

Isi pos asal, pos tujuan, dan nilai yang digeser. Setiap pergeseran tercatat di
**Pelaporan > Log Rekomposisi Anggaran** sehingga bisa ditelusuri.

### 3.3 Permohonan Tambah

**Menu:** Anggaran > Transaksi > Permohonan Tambah
**Status:** Draft -> To Approve -> Approved / Rejected

| # | Langkah | Tombol | Siapa |
|---|---|---|---|
| 1 | **New** -> isi pos dan nilai tambahan, sertakan alasan | | Request Budget User |
| 2 | Klik **Kirim untuk Approval** | Status To Approve | User |
| 3 | Klik **Setujui** atau **Tolak** | Wizard konfirmasi | Request Budget Manager |

### 3.4 Payung Anggaran dan Luncuran

Dua mekanisme yang sering tertukar:

| Istilah | Artinya |
|---|---|
| **Payung Anggaran** | Satu pagu besar yang dibagi otomatis ke beberapa pos di bawahnya |
| **Luncuran** | Sisa anggaran tahun berjalan yang diteruskan ke tahun berikutnya |

**Payung Anggaran** disiapkan di Konfigurasi > Payung Anggaran. Hasil
pembagiannya terlihat di **Pelaporan > Alokasi Otomatis Payung** dengan status
**Berlaku**, **Di-reverse**, atau **Pembalik**.

**Luncuran** punya dua jalur:

- **Otomatis** - diatur di Konfigurasi > Mapping Luncuran, dijalankan dengan
  tombol **Jalankan Luncuran Sekarang**. Hasilnya di Pelaporan > Jembatan
  Luncuran PO (status Aktif / Di-reverse).
- **Manual** - Transaksi > Luncuran Sisa Anggaran (Manual), Draft ->
  **Setujui** -> Approved.

### 3.5 Laporan Anggaran

| Laporan | Menjawab pertanyaan |
|---|---|
| Analisa Serapan Anggaran | Berapa pagu terpakai dibanding rencana |
| Akun Biaya Belum Dipetakan | Akun biaya yang belum punya pos anggaran - biaya di akun ini tidak akan terhitung sebagai serapan |
| Log Rekomposisi Anggaran | Riwayat semua pergeseran pagu |

---

## 4. Aset

### 4.1 Aset dan penyusutan

**Menu:** Assets
**Status:** Draft -> Running -> Close (aset bawaan Odoo juga punya Model, On Hold, Cancelled)

| # | Langkah | Tombol | Catatan |
|---|---|---|---|
| 1 | **New** -> isi nilai perolehan, tipe aset, metode penyusutan | Tipe dari Accounting > Configuration > Asset Types | |
| 2 | Klik **Compute Depreciation** | Tabel penyusutan terbentuk | Masih di status Draft |
| 3 | Klik **Confirm** | Status Running | |
| 4 | Tiap periode: Assets > **Generate Assets Entries** | Wizard, tombol **Generate Entries** | Membuat jurnal penyusutan massal |
| 5 | Perlu mengubah masa manfaat: **Modify Depreciation** | Wizard | Hanya saat Running |

**Melepas aset:**

| Tombol | Untuk apa |
|---|---|
| **Sell or Dispose** | Menutup aset (dijual atau dilepas) |
| **Create Write-Off Entry** | Jurnal penghapusan, bila jenis pelepasannya write-off |
| **Create Disposal For Gain/Loss** | Jurnal laba/rugi pelepasan, bila jenisnya penjualan |
| **Set to Close** | Menutup aset yang penyusutannya sudah selesai |

### 4.2 Lokasi dan mutasi aset

| Menu | Isinya |
|---|---|
| Assets Location | Master lokasi penempatan aset |
| Assets Movement | Perpindahan satu aset (Draft -> Pending Approval -> Confirmed) |
| Batch Asset Movement | Perpindahan banyak aset sekaligus |

**Mutasi massal lewat Excel** (Batch Asset Movement):

| # | Langkah | Tombol |
|---|---|---|
| 1 | **New** -> isi keterangan batch | |
| 2 | Klik **Download Template** | Berkas Excel contoh |
| 3 | Isi berkas, lalu klik **Import from Excel** | Wizard impor |
| 4 | Klik **Validasi** | Baris yang bermasalah ditandai |
| 5 | Klik **Process Valid Lines** | Hanya baris yang lolos yang diproses |
| 6 | Klik **Confirm** | Mutasi berlaku |

**Cancel** dan **Set to Draft** tersedia bila batch perlu dibatalkan.

Dari form pegawai (hr.employee) dan lokasi tersedia tombol pintas ke daftar
aset yang dipegang.

---

## 5. Sewa (Lease, IFRS 16)

### 5.1 Lease Agreement

**Menu:** Lease Management > Lease Agreements
**Status:** Draft -> In Progress -> Closed / Cancelled

| # | Langkah | Tombol | Catatan |
|---|---|---|---|
| 1 | **New** -> isi objek sewa, tipe sewa, jangka waktu, nilai, suku bunga | Tipe dari Configuration > Lease Types | |
| 2 | Klik **Generate Schedule** | Jadwal pembayaran & bunga terbentuk | Bisa diulang selama belum Closed |
| 3 | Klik **Start Lease** | Status In Progress | |
| 4 | Bila suku bunga berubah: **Apply New Interest Rate** | Jadwal dihitung ulang dengan tarif baru | |
| 5 | Bila ada koreksi lain: **Recalculate Schedule** | Jadwal disusun ulang | |
| 6 | Selesai masa sewa: **Close** | Status Closed | **Reset to Draft** tersedia dari Closed/Cancelled |

Tombol **Download XLSX** mencetak jadwal sewa; **Import Data** dan **Download
Template** dipakai memasukkan penyesuaian dari Excel.

### 5.2 Lease Accounting - jurnal IFRS 16

**Menu:** Lease Management > Lease Accounting

Di sinilah jurnal-jurnal sewa dibuat. Tiap tombol membuat satu jenis jurnal:

| Tombol | Jurnal yang dibuat |
|---|---|
| **Create Initial IFRS Entry** | Pengakuan awal aset hak-guna dan liabilitas sewa |
| **Post Initial Payment** | Pembayaran di muka |
| **Create Rent Invoice** | Tagihan sewa periodik |
| **Create Rent Reversal Entry** | Pembalik tagihan sewa |
| **Create Depreciation Entry** | Penyusutan aset hak-guna |
| **Create Tax Payment Entry** | Pembayaran PPh sewa (muncul bila tarif potongan diisi) |

Tombol **Lock** mengunci data supaya tidak berubah; **Unlock** membukanya lagi.
Tombol pintas **Journal Entries** membuka jurnal yang sudah terbentuk.

> Tombol-tombol jurnal baru muncul setelah **tanggal mulai akuntansi** diisi
> atau jurnal pengakuan awal sudah ada. Kalau semuanya tidak terlihat, itu
> penyebab pertama yang harus diperiksa.

### 5.3 Laporan Sewa

| Laporan | Isinya |
|---|---|
| Lease Schedule Summary | Seluruh baris jadwal sewa |
| Liability Forecast | Proyeksi liabilitas sewa ke depan |
| Posting & Reconciliation | Baris jadwal yang sudah/belum dijurnal |
| Consolidated Analysis | Gabungan lintas perusahaan (perlu grup multi-company) |

---

## 6. Bila Ada Masalah

| Gejala | Penyebab yang paling sering | Yang harus dilakukan |
|---|---|---|
| Tombol Rekomposisi / Permohonan Tambah tidak muncul di SKK | SKK belum berstatus Dikonfirmasi | Klik **Konfirmasi** dulu |
| Serapan anggaran terlihat lebih kecil dari kenyataan | Ada akun biaya yang belum dipetakan ke pos anggaran | Buka Pelaporan > **Akun Biaya Belum Dipetakan** |
| Luncuran tidak berjalan | Mapping Luncuran belum aktif | Aktifkan mapping, lalu **Jalankan Luncuran Sekarang** |
| Tabel penyusutan aset kosong | Belum klik **Compute Depreciation** | Jalankan saat status masih Draft |
| Tombol Modify Depreciation tidak ada | Aset belum Running | Confirm dulu asetnya |
| Import mutasi aset menolak sebagian baris | Data baris tidak lolos validasi | **Validasi** menandai barisnya; **Process Valid Lines** tetap memproses yang lolos |
| Tombol jurnal di Lease Accounting tidak muncul | Tanggal mulai akuntansi belum diisi | Isi dulu tanggalnya di Lease Accounting |
| Jadwal sewa tidak bisa diubah | Data sedang terkunci | Klik **Unlock** |
| Menu struktur COA berjenjang tidak ada | Grup `Show Chart of Account Structure` belum diberikan | Tambah grup itu di Settings > Users |

---

[<- Buku 05: Finance & Pajak](05-finance-dan-pajak.md) |
[Daftar isi](README.md) |
[Buku 07: HR Master & Dokumen Pegawai ->](07-hr-master-dokumen-pegawai.md)
