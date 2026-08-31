# Buku 04 - Pengadaan, SCM dan Vendor

**Modul:** `pln_scm`, `pln_pengadaan`, `purchase_request`, `pln_vendor_document`, `pln_vendor_evaluation`, `pln_bank_garansi`, `pln_final_kontrak`, `advance_approval_matrix`
**Aplikasi:** SCM, Pengadaan, Purchase, Purchase Requests, Approval Matrix
**Peran utama:** PIC Logistik UP, Asman SCM, Tim Rikmatek, Manager Unit Pelaksana, VP Operasi/Niaga

Buku ini mencakup seluruh rantai pasok dan pengadaan: dari permintaan material
di unit, kontrak harga satuan dengan vendor, pemeriksaan barang, sampai
penilaian kinerja vendor.

---

## 1. Peta Menu

```
SCM
├── Master Data
│   ├── Material SCM · Vendor SCM · UP / Warehouse · Clustering
├── Transaksi
│   ├── RFM (Request Material)   permintaan material dari unit
│   ├── KHS (Kontrak Vendor)     kontrak harga satuan
│   ├── Kontrak Rinci            turunan KHS -> menghasilkan PO
│   ├── Rikmatek                 pemeriksaan teknis barang datang
│   ├── BAPP / BAST              berita acara
│   ├── Pemakaian · Pengembalian · Penggantian Barang
│   ├── Klaim Garansi
│   ├── Evaluasi Kebutuhan
│   └── Katalog Bursa · Laporan Bursa
├── Dashboard
│   ├── SCM Master Overview · Rikmatek Performance · SLA Bursa · Idle Material
└── Konfigurasi                  (SCM Admin)

Pengadaan
├── Purchase Agreement
│   ├── Purchase Agreement · Vendor Pricelists
│   ├── Penilaian Vendor · Form Penilaian Kinerja Penyedia
├── Bank Garansi                 daftar PO yang wajib jaminan
└── Final Kontrak                penetapan pemenang tender

Purchase (aplikasi bawaan Odoo)
├── Orders > Purchase Requests / Purchase Request Lines / PR tanpa KHS
├── Document Compliance
│   ├── Document Submissions · Dokumen Perusahaan Vendor
│   ├── Master Document Types
│   └── Document Dashboard > Pending Verification / Rejected Documents
└── Configuration > Term Garansi · Jenis Pekerjaan Penyedia · Kriteria Penilaian Vendor
```

---

## 2. Peran dan Hak Akses

### SCM (kategori **PLN ES - SCM**)

| Grup | Untuk siapa |
|---|---|
| PIC Logistik Unit Layanan | Pembuat RFM di unit layanan |
| PIC Logistik UP | Logistik Unit Pelaksana |
| Asman SCM | Review permintaan dan klaim |
| Manager Unit Pelaksana | Persetujuan tingkat UP |
| Asman Operasi / Niaga (Approve Anggaran) | Memutuskan ketersediaan anggaran |
| VP Operasi / Niaga | Persetujuan final penggantian barang |
| Fungsi SCM Kantor Pusat | Sourcing lintas UP dan bursa |
| Tim Rikmatek - Ketua I / II | Ketua tim pemeriksa |
| Tim Rikmatek - Sekretaris | Sekretaris tim pemeriksa |
| Tim Rikmatek - Anggota | Anggota tim pemeriksa |
| SCM Admin (4-eyes & Konfigurasi) | Konfigurasi + rollback dokumen terkunci |
| Auditor SCM (Read-Only) | Hanya melihat |

### Modul pendamping

| Grup | Modul | Untuk siapa |
|---|---|---|
| Procurement User (BG) | Bank Garansi | Melihat dan mengisi data jaminan |
| Procurement Manager (BG) | Bank Garansi | Menyetujui, mengesampingkan tenggat |
| Procurement Pusat (BG) | Bank Garansi | Monitoring lintas UP |
| Final Kontrak Viewer / User / Manager | Final Kontrak | Berjenjang: lihat, kerjakan, setujui |
| Evaluator Vendor | Penilaian Vendor | Mengisi penilaian |
| Manager Penilaian Vendor | Penilaian Vendor | Menyetujui + mengelola master kriteria |
| Auditor Penilaian Vendor | Penilaian Vendor | Hanya melihat |
| Purchase Request User / Manager | Purchase Request | Membuat / menyetujui PR |
| Approval User / Manager / Administrator | Approval Matrix | Lihat [Buku 00](00-panduan-umum-dan-hak-akses.md) |

> **4-eyes principle:** beberapa aksi SCM (rollback KHS yang sudah aktif)
> sengaja hanya bisa dilakukan **SCM Admin**, bukan oleh orang yang menyusun
> dokumennya. Ini disengaja sebagai kontrol, bukan kekurangan hak akses.

---

## 3. Alur Kerja SCM

### 3.1 RFM - permintaan material

**Menu:** SCM > Transaksi > RFM (Request Material)
**Status:** Draf -> Diajukan -> Dalam Review -> Sourced (Stok Lokal / UP Lain / Kantor Pusat / Pengadaan) -> In Picking -> Delivered -> Received -> Selesai

| # | Langkah | Yang terlihat di layar | Siapa |
|---|---|---|---|
| 1 | **New** -> isi material dan jumlah yang dibutuhkan | Nomor RFM otomatis | PIC Logistik Unit Layanan |
| 2 | Klik **Ajukan** | Status Diajukan | PIC Logistik |
| 3 | Klik **Mulai Review** | Status Dalam Review | Asman SCM |
| 4 | Klik **Cek Availability** | Sistem memeriksa stok lokal, UP lain, dan Kantor Pusat | |
| 5 | Klik **Pilih Source** | Wizard memilih sumber pemenuhan | Menentukan status Sourced mana |
| 6 | **Mark Delivered** -> **Mark Received** | Barang jalan lalu diterima | |
| 7 | Klik **Finalize Done** | Status Selesai | |

Tombol **Tolak** dan **Batalkan** tersedia selama belum selesai. Tombol pintas
**Picking** dan **Pemakaian** menghubungkan ke dokumen turunannya.

### 3.2 KHS - kontrak harga satuan vendor

**Menu:** SCM > Transaksi > KHS (Kontrak Vendor)
**Status:** Draf -> Diajukan -> Dalam Review -> Disetujui -> **Aktif** -> Akan Berakhir -> Berakhir

| # | Langkah | Yang terlihat di layar | Catatan |
|---|---|---|---|
| 1 | **New** -> pilih vendor, masa berlaku, daftar harga satuan | | |
| 2 | **Ajukan** -> **Mulai Review** -> disetujui | Status berjalan | |
| 3 | Klik **Aktivasi** | Status Aktif, harga bisa dipakai | |
| 4 | Bila perlu dihentikan sementara: **Tahan** | Status On Hold | **Lanjutkan** untuk mengaktifkan lagi |
| 5 | Koreksi kontrak yang sudah aktif: **Rollback (4-eyes)** | Wizard alasan | **Hanya SCM Admin** |

Status **Akan Berakhir** dan **Berakhir** berubah sendiri mengikuti tanggal.
Tombol pintas **Kontrak Rinci** membuka turunannya.

### 3.3 Kontrak Rinci -> Purchase Order

**Menu:** SCM > Transaksi > Kontrak Rinci
**Status:** Draf -> Diajukan -> Dikonfirmasi -> Sedang Diproses -> Selesai

Tombol **Konfirmasi -> Generate PO** membuat Purchase Order secara otomatis.
Tombol pintas **Purchase Order** membuka PO hasilnya.

### 3.4 Rikmatek - pemeriksaan teknis

**Menu:** SCM > Transaksi > Rikmatek
**Status:** Draf -> Tim Ditugaskan -> Sedang Inspeksi -> Diajukan untuk Review -> Review Teknis OK -> Review Admin OK -> (Review Logistik) -> Approved -> BAST Terbit

| # | Langkah | Tombol | Siapa |
|---|---|---|---|
| 1 | Tugaskan tim pemeriksa | **Assign Tim** | Ketua Tim Rikmatek |
| 2 | Siapkan checklist | **Generate dari Template** | muncul bila checklist masih kosong |
| 3 | Mulai memeriksa barang | **Mulai Inspeksi** | Anggota tim |
| 4 | Kirim hasil | **Submit Review** | Sekretaris |
| 5 | Review teknis | **Tech Review OK** | |
| 6 | Review administrasi | **Admin Review OK** | |
| 7 | Review logistik (bila diperlukan) | **Logistik Review (Captive KP)** | hanya untuk kasus tertentu |
| 8 | Setujui | **Approve** | |
| 9 | Terbitkan berita acara | **Terbitkan BAST** | BAST dan BAPP bisa dibuka lewat tombol pintas |

### 3.5 Pemakaian, Pengembalian, Penggantian

| Dokumen | Status | Tombol kunci |
|---|---|---|
| **Pemakaian** | Draf -> Diajukan -> Selesai | **Cetak Nota PDF** setelah Selesai |
| **Pengembalian** | Draf -> Diajukan -> Diinspeksi -> Kembali ke Stok / Klaim Garansi / Disposal | **Inspeksi** memilah barang layak dan tidak layak |
| **Penggantian Barang** | Draf -> Menunggu PIC UP -> Asman SCM -> Asman Terkait -> VP -> Diganti | persetujuan berjenjang, ada cabang anggaran |

**Pengembalian** setelah **Inspeksi** memunculkan tiga tombol sesuai hasil
pemilahan:

- **Kembali ke Stok (Layak)** - untuk barang yang masih baik
- **Trigger Klaim Garansi** - untuk barang rusak yang masih bergaransi
- **Trigger Disposal** - untuk barang rusak tanpa garansi

**Penggantian Barang** punya cabang khusus anggaran: di tahap Asman Terkait,
tombol **Anggaran Tersedia** meneruskan ke VP, sedangkan **Anggaran Tidak
Tersedia** menahan dokumen dan memunculkan **Cetak Surat Rekomposisi**. Dari
situ **Kembali ke Asman Terkait** dipakai bila anggaran sudah tersedia.

### 3.6 Klaim Garansi

**Menu:** SCM > Transaksi > Klaim Garansi
Draf -> Diajukan -> Direview Asman SCM -> Diajukan ke Vendor -> Menunggu Respon Vendor -> (Diterima / Ditolak Vendor)

| Tombol | Kapan dipakai |
|---|---|
| **Vendor TERIMA** / **Vendor TOLAK** | Mencatat jawaban vendor |
| **Register Pengganti** | Setelah vendor menerima klaim |
| **Tutup (Diganti)** | Barang pengganti sudah diterima |
| **Escalate** | Vendor menolak, perkara dinaikkan |
| **Tutup (Tidak Resolved)** | Eskalasi tidak membuahkan hasil |
| **Cetak Form** | Formulir klaim untuk vendor |

### 3.7 Bursa Material

Bursa adalah mekanisme berbagi material menganggur antar unit.

| # | Langkah | Tombol | Dokumen |
|---|---|---|---|
| 1 | Kumpulkan lot menganggur | **Load Lot Bursa** | Laporan Bursa (Draf) |
| 2 | Ajukan ke Manager UP | **Ajukan ke Manager UP** | |
| 3 | Manager UP menyetujui | **Approve Manager UP** | |
| 4 | Kirim ke Kantor Pusat | **Kirim ke KP** | |
| 5 | Terbitkan ke katalog | **Publish ke Katalog** | material tampil di Katalog Bursa |
| 6 | Unit lain meminta | **Request via RFM** | dari Katalog Bursa |
| 7 | Menarik kembali penawaran | **Withdraw** | |

**Cetak PDF** tersedia untuk laporan bursa.

### 3.8 Evaluasi Kebutuhan

Draf -> Diajukan -> Dianalisis, lalu diarahkan lewat salah satu dari tiga
tombol: **Route -> Pemesanan**, **Route -> Amendemen**, atau
**Route -> Pengadaan Baru**.

---

## 4. Pengadaan

### 4.1 Final Kontrak - penetapan pemenang tender

**Menu:** Pengadaan > Final Kontrak
**Status:** Draft -> Proses Evaluasi -> Finalisasi -> Approved -> Selesai

| # | Langkah | Yang terlihat di layar | Catatan |
|---|---|---|---|
| 1 | **New** -> isi paket tender dan daftar peserta | Tiap peserta punya status sendiri | |
| 2 | Klik **Proses Evaluasi** | Status Proses Evaluasi | |
| 3 | Tandai status tiap peserta | Menang / Kalah / Gugur Administrasi / Mengundurkan Diri | |
| 4 | Klik **Finalisasi** | Status Finalisasi | |
| 5 | Klik **Buat Purchase Agreement** | Purchase Agreement terbentuk untuk pemenang | Tombol hanya muncul di status Finalisasi **dan** ada peserta berstatus Menang |
| 6 | **Approve** lalu **Selesaikan** | Status Selesai | |

Dari Purchase Agreement tersedia tombol **Tender Asal** (kembali ke Final
Kontrak) dan **Histori Harga**.

### 4.2 Bank Garansi - kontrol jaminan pelaksanaan

**Menu:** Pengadaan > Bank Garansi (isinya daftar Purchase Order)

Kontrol ini menempel pada Purchase Order, bukan dokumen tersendiri. PO yang
nilainya melewati ambang tertentu wajib punya dokumen jaminan pelaksanaan.

| Field di PO | Isinya |
|---|---|
| Status Jaminan Pelaksanaan | Tidak Diperlukan / Belum Lengkap / Lengkap |
| No. Bank Garansi, Bank Penerbit, Nilai Jaminan | Data jaminan |
| Masa Berlaku (Mulai / Akhir) | Periode jaminan |
| Tenggat BG | Dihitung otomatis dari tanggal konfirmasi PO + jumlah hari |
| Terlambat, Umur Keterlambatan | Terisi sendiri bila lewat tenggat |

**Pengaturan** ada di Settings: **Ambang Bank Garansi (Rp)** dan
**Tenggat Upload BG (hari)**.

> Sifatnya *soft enforcement*: PO tidak diblokir, tetapi statusnya terlihat
> Belum Lengkap dan keterlambatannya terhitung untuk dipantau.

### 4.3 Document Compliance - dokumen vendor

**Menu:** Purchase > Document Compliance

Dua lapis dokumen:

| Lapis | Menu | Isinya |
|---|---|---|
| Permanen (per vendor) | Dokumen Perusahaan Vendor | Akta, TDP, SIUP, NPWP - berlaku untuk semua transaksi |
| Per transaksi | Document Submissions | Dokumen yang diminta untuk satu PO tertentu |

Dokumen perusahaan berstatus: **Valid** -> **Expiring Soon** -> **Expired**.

**Cara meminta dokumen ke vendor** (dari form Purchase Order):

| # | Langkah | Tombol |
|---|---|---|
| 1 | Buat tautan khusus untuk vendor | **Generate Vendor Link** |
| 2 | Kirim lewat email | **Send Email to Vendor** |
| 3 | Atau salin tautannya untuk dikirim manual | **Copy Vendor Link** |
| 4 | Bila tautan bocor / perlu diganti | **Regenerate Link (Reset)** |
| 5 | Pakai dokumen permanen yang sudah ada | **Gunakan Dokumen Vendor** |

Vendor mengunggah lewat tautan itu tanpa perlu akun Odoo. Hasil unggahannya
masuk sebagai Document Submission berstatus **Pending**.

**Memverifikasi** (Purchase > Document Compliance > Document Submissions):
buka dokumen -> **Download File** untuk memeriksa isinya -> **Approve** atau
**Reject** (wizard meminta alasan). Dokumen yang ditolak bisa dikembalikan
dengan **Reset to Pending**.

Pintasan pemantauan: **Document Dashboard > Pending Verification** dan
**Rejected Documents**.

### 4.4 Penilaian Vendor

**Menu:** Pengadaan > Purchase Agreement > Penilaian Vendor
**Status:** Draf -> Diajukan -> Disetujui / Ditolak

| # | Langkah | Tombol | Siapa |
|---|---|---|---|
| 1 | **New** -> pilih vendor dan periode penilaian | | Evaluator Vendor |
| 2 | Klik **Buat Baris Penilaian** | Baris terisi dari master Kriteria Penilaian Vendor | |
| 3 | Isi nilai tiap kriteria | Skor akhir terhitung | |
| 4 | Klik **Ajukan** | Status Diajukan | |
| 5 | **Setujui** atau **Tolak** | | Manager Penilaian Vendor |
| 6 | Bila ditolak: **Kembalikan ke Draf** | Bisa diperbaiki lalu diajukan lagi | |

Master pendukung ada di **Purchase > Configuration**: **Kriteria Penilaian
Vendor** dan **Jenis Pekerjaan Penyedia** (keduanya milik Manager Penilaian
Vendor). Dari form kontak vendor ada tombol pintas ke riwayat penilaiannya.

### 4.5 Purchase Request

**Menu:** Purchase Requests, atau Purchase > Orders > Purchase Requests
**Status dokumen:** Draft -> In progress -> Done
**Status per baris:** Draft -> To be approved -> Approved -> In progress -> Done / Rejected

Menu **PR tanpa KHS** (Purchase > Orders) menyaring PR yang barangnya belum
punya kontrak harga satuan - dipakai untuk memutuskan mana yang perlu
pengadaan baru.

---

## 5. Bila Ada Masalah

| Gejala | Penyebab yang paling sering | Yang harus dilakukan |
|---|---|---|
| App SCM tidak muncul | Belum punya grup SCM | Beri grup sesuai fungsi, minimal Auditor SCM |
| Menu SCM > Konfigurasi tidak ada | Hanya untuk SCM Admin | Minta SCM Admin |
| Tombol **Rollback (4-eyes)** tidak muncul | Bukan SCM Admin | Memang disengaja sebagai kontrol |
| Tombol **Pilih Source** tidak ada di RFM | Belum klik **Cek Availability** | Jalankan pengecekan dulu |
| Tombol **Buat Purchase Agreement** tidak muncul di Final Kontrak | Status belum Finalisasi, atau belum ada peserta berstatus Menang | Tandai pemenangnya dulu |
| Checklist Rikmatek kosong | Belum di-generate | Klik **Generate dari Template** |
| Status Jaminan Pelaksanaan tetap Belum Lengkap | Dokumen BG belum diunggah atau data BG belum diisi | Lengkapi field BG di PO |
| PO tetap bisa diproses padahal BG belum lengkap | Memang tidak diblokir | Kontrolnya berupa pemantauan, bukan penguncian |
| Vendor tidak bisa membuka tautan unggah dokumen | Tautan belum dibuat atau sudah di-reset | **Generate Vendor Link** lalu kirim ulang |
| Baris penilaian vendor kosong | Belum klik **Buat Baris Penilaian** | Klik tombol itu saat status masih Draf |
| Tombol persetujuan tidak muncul padahal statusnya menunggu | Bukan approver di matriks | Lihat [Buku 00](00-panduan-umum-dan-hak-akses.md) bagian Approval Matrix |

---

[<- Buku 03: Business Development](03-business-development.md) |
[Daftar isi](README.md) |
[Buku 05: Finance & Pajak ->](05-finance-dan-pajak.md)
