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

### 3.1 Pos Anggaran dan Payung Anggaran (konfigurasi)

**Menu:** Anggaran > Konfigurasi > Pos Anggaran / Payung Anggaran

Pos Anggaran adalah unit kontrol pengeluaran, terkait ke satu **UP / Divisi**
pemilik dan satu atau lebih akun GL. Setiap Pos punya tiga flag yang
diwariskan ke baris PRK yang memakainya:

| Flag | Nilai | Efek |
|---|---|---|
| Budget Restrict | Allow / Restrict | Restrict memblokir transaksi yang melebihi pagu |
| Allow Transfer | Allow / Restrict | Mengizinkan/melarang Pos ini ikut Rekomposisi |
| Budget Carry Next Month | Allow / Restrict | Menentukan boleh tidaknya sisa Pos diluncurkan |

**Level Kontrol Anggaran** menentukan cara pengecekan pagu: **Per Pos
Anggaran** (dicek di level Pos itu sendiri) atau **Payung Anggaran** (dicek
agregat di level payung). Payung Anggaran **tidak membagi pagu secara
otomatis** ke Pos di bawahnya - payung hanya mengagregasi pengecekan pagu
per pasangan (Payung, SKK); bila satu Pos dalam payung habis, transaksi
tetap diperbolehkan selama total payung pada SKK yang sama masih cukup.

**Basis Konsumsi** pada Payung umumnya **PO Confirm (outstanding)** - pagu
berkurang saat PO dikonfirmasi (dicatat sebagai komitmen outstanding),
bukan saat Vendor Bill dibuat.

### 3.2 SKK - dokumen anggaran

**Menu:** Anggaran > Transaksi > SKK (Anggaran)
**Status:** Draft -> Dikonfirmasi -> Divalidasi -> Selesai

Header SKK memuat Nama Anggaran, **Nomor Kategori SKK** (SKKO Captive, SKKO
Non-Captive, SKKI Tahun Berjalan, SKKI Luncuran, Hutang Usaha Captive,
Hutang Usaha Non-Captive), Kategori (Investasi/Operasi), Penanggung Jawab,
UP/Divisi, Tanggal Mulai-Selesai, dan Tahun Anggaran (otomatis dari
Tanggal Mulai).

| # | Langkah | Tombol | Catatan |
|---|---|---|---|
| 1 | **New** -> isi header lalu tambah baris **PRK** (Program Rencana Kerja) per Pos Anggaran | | Pos dari Konfigurasi > Pos Anggaran |
| 2 | Klik **Konfirmasi** | Status Dikonfirmasi | Anggaran Awal tiap PRK terisi sesuai Penetapan; rekomposisi dan permohonan tambah bisa dibuat |
| 3 | Klik **Validasi** lalu **Selesaikan** | Anggaran berlaku | |
| 4 | Perlu koreksi: **Set ke Draft** | Dari Dikonfirmasi atau Dibatalkan | |

**Validasi anti-duplikasi:** satu UP / Divisi hanya boleh punya satu SKK
aktif per tahun anggaran per kategori. SKK dengan kombinasi UP, tahun, dan
kategori yang sama dengan SKK aktif yang sudah ada akan ditolak sistem.

Dari form SKK ada tombol pintas ke **Rekomposisi**, **Permohonan Tambah**, dan
**Alokasi Payung** yang terkait.

### 3.3 Baris PRK (Program Rencana Kerja)

Setiap baris PRK pada SKK punya kolom berikut:

| Kolom | Sumber | Keterangan |
|---|---|---|
| Kode PRK | Input | Kode identifikasi PRK |
| Pos Anggaran | Input | Menentukan Payung Anggaran (otomatis) dan flag default |
| Anggaran Awal | Otomatis | Terisi sesuai Penetapan setelah SKK dikonfirmasi |
| Tambah Anggaran | Otomatis | Dari Permohonan Tambah yang disetujui |
| Transfer | Otomatis | Selisih bersih Rekomposisi: negatif di baris asal, positif di baris tujuan |
| Alokasi Payung & Luncuran | Otomatis | Dari mekanisme payung dan carry-over |
| **Penetapan** | Computed | Pagu akhir = Anggaran Awal + Tambah Anggaran + Transfer + Alokasi Payung + Luncuran |

### 3.4 Rekomposisi Anggaran - menggeser pagu antar PRK

**Menu:** Anggaran > Transaksi > Rekomposisi Anggaran, atau tombol
**Rekomposisi Anggaran** di form SKK berstatus Dikonfirmasi.

Isi PRK asal, PRK tujuan, dan nominal yang digeser (SKK asal/tujuan terisi
otomatis karena rekomposisi selalu dalam satu SKK yang sama). Setiap
pergeseran tercatat di **Pelaporan > Log Rekomposisi Anggaran**.

**Aturan wajib:** transfer hanya diizinkan antar Pos dalam konteks yang
sama - sesama Pos bernaung Payung Anggaran, atau sesama Pos non-payung.
Transfer lintas konteks (payung ke non-payung atau sebaliknya) ditolak
sistem.

### 3.5 Permohonan Tambah

**Menu:** Anggaran > Transaksi > Permohonan Tambah
**Status:** Draft -> To Approve -> Approved / Rejected

| # | Langkah | Tombol | Siapa |
|---|---|---|---|
| 1 | **New** -> isi PRK dan nilai tambahan (Requested Amount), sertakan alasan | | Request Budget User |
| 2 | Klik **Kirim untuk Approval** | Status To Approve | User |
| 3 | Klik **Setujui** atau **Tolak** | Wizard konfirmasi, mengisi Approved Amount | Request Budget Manager |

Setelah Approved, kolom **Tambah Anggaran** pada PRK terkait ter-update
otomatis sesuai Approved Amount.

> Jenjang/matriks persetujuan Permohonan Tambah masih berstatus **perlu
> konfirmasi** pada blueprint pengembangan modul ini - pastikan pengaturan
> approval group saat ini (Request Budget Manager) sudah sesuai keputusan
> final BPO Anggaran sebelum dijadikan acuan.

### 3.6 Payung Anggaran dan Luncuran

Dua mekanisme yang sering tertukar - lihat 3.1 untuk cara kerja Payung
Anggaran (kontrol pagu agregat, bukan pembagian otomatis).

**Mapping Luncuran** (Konfigurasi > Mapping Luncuran) mengatur pemindahan
sisa anggaran tahun berjalan ke kategori SKK tahun berikutnya:

| Field | Keterangan |
|---|---|
| Kategori SKK Asal / Tujuan | Pasangan kategori sumber dan tujuan |
| Tanggal & Bulan Cut-Off | Batas proses luncuran, mis. 31 Desember |
| Jumlah Luncuran | Smart button jumlah jembatan luncuran yang sudah diproses |

Contoh mapping: SKKO Captive -> Hutang Usaha Captive; SKKO Non-Captive ->
Hutang Usaha Non-Captive; SKKI Tahun Berjalan -> SKKI Luncuran.

Nilai yang diluncurkan = sisa komitmen PO outstanding (komitmen dikurangi
realisasi terposting). PO yang diluncurkan **tidak dibatalkan dan tidak
diedit** - anggarannya dibaca melalui **Jembatan Luncuran PO** (read-only,
referensi berformat LNC/TAHUN/NOMOR), sehingga PO tetap berjalan normal di
tahun berikutnya.

**Luncuran** punya dua jalur:

- **Otomatis** - batch harian memproses mapping aktif setiap kali tanggal
  cut-off tercapai, tanpa intervensi pengguna.
- **Manual** - tombol **Jalankan Luncuran Sekarang** pada form Mapping
  Luncuran. Hasilnya di Pelaporan > Jembatan Luncuran PO (kolom Purchase
  Order, Nomor PRK, SKK Asal, SKK Luncuran, PRK Luncuran, Nilai Luncuran,
  Status).

> "Alokasi Otomatis Payung" (status Berlaku/Di-reverse/Pembalik) dan
> "Luncuran Sisa Anggaran (Manual)" dengan alur Draft -> Setujui -> Approved
> yang ada di menu saat ini tidak dirinci pada blueprint pengembangan
> modul Anggaran (PRJ-0006) - kemungkinan diatur di dokumen terpisah
> "Blueprint AGR-03 Monitoring & Persetujuan Anggaran". Cek dokumen itu
> bila perlu memverifikasi perilakunya.

### 3.7 Konsumsi Anggaran pada Purchase Order

Setelah SKK berstatus Selesai, setiap PO wajib mencantumkan field **SKK**
dan **PRK** agar konsumsi anggaran terpotong otomatis saat PO
**dikonfirmasi** (bukan saat Vendor Bill dibuat). Jika Pos memakai
mekanisme payung, transaksi tetap diperbolehkan selama total pagu payung
masih mencukupi meski satu Pos di dalamnya sudah habis.

Smart button pada form SKK:
- **Rekomposisi** - jumlah transaksi rekomposisi, diklik untuk membuka detail transfer.
- **Permohonan Tambahan** - jumlah permohonan tambah beserta status.

### 3.8 Laporan Anggaran

| Laporan | Menjawab pertanyaan |
|---|---|
| Analisa Serapan Anggaran / dashboard sisa anggaran per Pos & PRK | Penetapan, Realisasi, Sisa, dan Komitmen |
| Rekap Payung Anggaran lintas SKK | Pagu, konsumsi, dan sisa agregat per payung |
| Akun Biaya Belum Dipetakan | Akun biaya yang belum punya pos anggaran - biaya di akun ini tidak akan terhitung sebagai serapan |
| Log Rekomposisi Anggaran | Riwayat semua pergeseran pagu |
| Jembatan Luncuran PO | Riwayat PO yang diluncurkan ke SKK tahun berikutnya |

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
| SKK tidak bisa disimpan/dikonfirmasi | Kombinasi UP, tahun anggaran, dan kategori sudah dipakai SKK aktif lain | Validasi anti-duplikasi aktif; edit SKK yang sudah ada atau ubah kombinasinya |
| Rekomposisi ditolak sistem | Transfer melintasi Pos berpayung dan Pos non-payung | Rekomposisi hanya diizinkan sesama Pos berpayung atau sesama Pos non-payung |
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
