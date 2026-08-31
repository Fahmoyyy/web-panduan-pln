# Buku 05 - Finance dan Pajak

**Modul:** `plnes_finance`, `plnes_finance_pembiayaan`, `plnes_finance_tax`, `plnes_fin_journal_workflow`, `plnes_fin_accrual`, `plnes_fin_accrual_revenue`, `plnes_fin_accrual_multiso`, `plnes_internal_transfer_payment`, `plnes_invoice_category`, `lm_id_ppn`, `lm_id_pph_sales`, `lm_id_withholding`
**Aplikasi:** Finance PLN ES, Tax Management, Accounting, Invoicing
**Peran utama:** Staff Akuntansi, Staff Pajak, Asman Akuntansi, Asman Pajak, Manager Akuntansi & Pajak, Manager Pembendaharaan, Direktur Keuangan

---

## 1. Peta Menu

```
Finance PLN ES
├── Dashboard
│   ├── AR/AP Outstanding · Akrual Pending · Cashflow Bulan Ini
│   ├── Period Lock Status · VIP Submission
│   └── Coretax Status · E-billing Status · BPE Archive   (Staff Pajak)
├── Akuntansi
│   ├── Akrual Bulan-End · Internal Order · Internal Transfer Payments
├── Pajak
│   ├── Coretax Export · e-Billing PPh · Arsip BPE · Ekualisasi PSIAP
├── Pembiayaan
│   ├── Batch Pembayaran Vendor · Import Bank Statement · VIP Submission Captive
└── Konfigurasi > Period Lock / Unlock

Accounting > Entries > Journal Workflow
├── Reversal Dokumen · Reclass Dokumen
└── Akrualisasi Biaya · Akrualisasi Pendapatan

Tax Management
├── PPN Keluaran > Faktur Keluaran · Replacement / Cancelled Invoices
├── PPN Masukan  > Faktur Masukan · Input Tax Crosscheck
├── PPh > Bukti Potong Diterima · PPh Sales Control · Monitoring PPh Unifikasi
├── Pengajuan Replacement & Cancel
└── Configuration > Invoice Categories · Document Requirements · Taxes
```

---

## 2. Peran dan Hak Akses

Kategori **PLN ES - Finance** di Settings > Users:

| Grup | Untuk siapa |
|---|---|
| Finance / Staff Akuntansi | Membuat jurnal, akrual, reclass |
| Finance / Asman Akuntansi | Review pekerjaan staff akuntansi |
| Finance / Staff Pajak | Coretax, e-Billing, BPE, faktur pajak |
| Finance / Asman Pajak | Review pekerjaan staff pajak |
| Finance / Manager Akuntansi & Pajak | Otorisasi, buka/tutup periode |
| Finance / Manager Pembendaharaan | Batch pembayaran vendor |
| Finance / Direktur Keuangan | Persetujuan tertinggi, cashflow |
| Finance / Auditor | Hanya melihat |
| Finance / Administrator | Administrator modul |

Selain itu masih berlaku grup akuntansi bawaan Odoo yang mengatur menu
**Tax Management** dan **Accounting**:

| Grup bawaan | Akibatnya |
|---|---|
| Accounting / Billing (`group_account_invoice`) | Bisa membuka Tax Management dan membuat faktur |
| Accounting / Read Only | Bisa membuka Tax Management, tidak bisa mengubah |
| Accounting / Administrator (`group_account_manager`) | Bisa membuka Tax Management > Configuration |

> Kedua kelompok grup ini harus diberikan bersama. User dengan
> "Finance / Staff Pajak" saja tetap tidak akan melihat menu Tax Management
> bila grup akuntansi bawaannya kosong.

---

## 3. Alur Kerja Jurnal

### 3.1 Reversal Dokumen - membatalkan jurnal yang sudah posted

**Menu:** Accounting > Entries > Journal Workflow > Reversal Dokumen
**Status:** Draft -> Dalam Review -> Disetujui Staff -> Ter-Reversal -> Ter-Otorisasi

| # | Langkah | Tombol | Siapa |
|---|---|---|---|
| 1 | **New** -> pilih jurnal yang akan dibalik, tulis alasan | | Staff Akuntansi |
| 2 | Klik **Submit** | Status Dalam Review | Staff |
| 3 | Klik **Execute Reversal** | Jurnal pembalik terbentuk, status Ter-Reversal | Reviewer |
| 4 | Klik **Authorize** | Status Ter-Otorisasi (final) | Manager |
| 5 | Bila salah: **Rollback** atau **Reject** (wizard alasan) | Kembali / ditolak | |

Tombol pintas **Dokumen Asal** dan **Jurnal Reversal** membuka keduanya.
Alasan ditulis bebas, tidak ada panjang minimum.

### 3.2 Reclass Dokumen - memindahkan akun

**Menu:** Journal Workflow > Reclass Dokumen
**Status:** Draft -> Dalam Review -> Menunggu Otorisasi -> Diotorisasi -> Ter-Reclass

| # | Langkah | Tombol |
|---|---|---|
| 1 | **New** -> pilih jurnal asal, tentukan akun tujuan, tulis alasan | |
| 2 | **Submit** | Status Dalam Review |
| 3 | Reviewer: **Authorize** | Status Diotorisasi |
| 4 | **Execute Reclass** | Jurnal reclass terbentuk |
| 5 | Bila perlu diperbaiki: **Return ke User** (wizard) | Status Dikembalikan, bisa Submit lagi |

### 3.3 Akrualisasi Biaya

**Menu:** Journal Workflow > Akrualisasi Biaya
**Status:** Draft -> Diajukan -> Menunggu Konfirmasi User -> Terverifikasi -> Ter-post -> Diotorisasi

| # | Langkah | Tombol | Catatan |
|---|---|---|---|
| 1 | **New** -> isi biaya yang akan diakru | | |
| 2 | **Submit** | Status Diajukan | |
| 3 | Bila perlu konfirmasi pemilik biaya: **Request Konfirmasi User** | Wizard mengirim permintaan | |
| 4 | Pemilik biaya klik **Konfirmasi Data** | Status Terverifikasi | |
| 5 | Klik **Post Jurnal Akrual** | Jurnal akrual terbentuk | Bisa dari Diajukan atau Terverifikasi |
| 6 | Klik **Authorize** | Status Diotorisasi | |
| 7 | Bulan berikutnya: **Request Reversal** | Akrual dibalik | |
| 8 | Ada koreksi: **Return for Fix** (wizard) | Kembali untuk diperbaiki | |

**Akrualisasi Pendapatan** berjalan dengan pola yang sama. Dari Sales Order ada
tombol pintas ke akrual pendapatan yang terkait dengannya.

**Akrual Bulan-End** (Finance PLN ES > Akuntansi) adalah wizard massal: tombol
**Generate Akrual** membuat akrual sekaligus untuk banyak dokumen.

### 3.4 Internal Order

**Menu:** Finance PLN ES > Akuntansi > Internal Order
Draft -> **Aktifkan** -> Aktif -> **Tutup** -> Tutup

IO dipakai menandai dokumen milik satu pekerjaan yang sama. **Reset ke Draft**
tersedia dari status Tutup atau Dibatalkan.

### 3.5 Internal Transfer Payment

**Menu:** Finance PLN ES > Akuntansi > Internal Transfer Payments
Pembayaran antar rekening internal. Alurnya mengikuti pembayaran Odoo:
**Confirm** -> **Validate**, dengan **Mark as Sent** / **Unmark as Sent**
untuk menandai sudah dikirim.

> Bila pembayaran tidak menghasilkan journal entry, penyebabnya hampir selalu
> **Outstanding Account belum diatur** pada jurnal bank/kas yang dipakai.

---

## 4. Pembiayaan

### 4.1 Batch Pembayaran Vendor

**Menu:** Finance PLN ES > Pembiayaan > Batch Pembayaran Vendor (Manager Pembendaharaan)
**Status:** Draft -> Disetujui -> Dieksekusi

| # | Langkah | Tombol |
|---|---|---|
| 1 | **New** -> pilih tagihan vendor yang akan dibayar sekaligus | |
| 2 | Klik **Buat Pembayaran** | Pembayaran per vendor terbentuk |
| 3 | Klik **Setujui** | Status Disetujui |
| 4 | Klik **Eksekusi** | Pembayaran diproses |

### 4.2 Import Bank Statement

**Menu:** Pembiayaan > Import Bank Statement
**Status:** Draft -> Ter-parse -> Ter-import -> Ter-rekonsiliasi (atau Error)

Unggah berkas rekening koran -> **Parse File** -> periksa hasil bacanya ->
**Import** -> **Rekonsiliasi**. Bila status menjadi **Error**, perbaiki
berkasnya lalu **Parse File** lagi.

### 4.3 VIP Submission Captive

**Menu:** Pembiayaan > VIP Submission Captive
Draft -> Dikirim ke VIP -> Terverifikasi UP3 -> Terverifikasi UID ->
Terverifikasi Treasury -> Lunas

Tombol **Submit ke VIP** mengirim berkas tagihan; verifikasi berikutnya
mengikuti proses di sisi PLN.

---

## 5. Pajak

### 5.1 Coretax Export

**Menu:** Finance PLN ES > Pajak > Coretax Export (Staff Pajak)
**Status:** Draft -> Tervalidasi -> XML Digenerate -> Dikirim -> BPE Diterima

| # | Langkah | Tombol |
|---|---|---|
| 1 | **New** -> pilih periode dan faktur yang akan dilaporkan | |
| 2 | Klik **Validasi** | Sistem memeriksa kelengkapan data |
| 3 | Klik **Generate XML** | Berkas XML terbentuk |
| 4 | Klik **Submit ke Coretax** | Status Dikirim |
| 5 | Setelah BPE turun, arsipkan | Menu **Arsip BPE** |

Bila statusnya **Error**, perbaiki datanya lalu ulangi dari **Submit ke
Coretax** atau **Batalkan**.

### 5.2 e-Billing PPh

**Menu:** Pajak > e-Billing PPh
Draft -> Diterbitkan -> **Setujui** -> Menunggu Bayar -> **Tandai Lunas** ->
Lunas -> SPT Dilaporkan. Status **Expired** muncul bila kode billing kedaluwarsa.

### 5.3 Ekualisasi PSIAP

**Menu:** Pajak > Ekualisasi PSIAP
Draft -> Di-review -> Disetujui -> Tutup. Dipakai mencocokkan angka pajak
dengan sistem PSIAP.

### 5.4 Faktur Pajak Keluaran - Replacement dan Cancel

**Menu:** Tax Management > PPN Keluaran > Faktur Keluaran

Faktur pajak yang sudah terbit tidak boleh diubah begitu saja. Dari form
faktur (**harus berstatus Posted**) tersedia dua tombol:

| Tombol | Untuk apa |
|---|---|
| **Mark as Replacement** | Faktur pengganti atas faktur yang salah |
| **Cancel Invoice** | Pembatalan faktur pajak |

Keduanya membuka **form Pengajuan** (bukan wizard singkat) berisi alasan dan
data pendukung. Setelah pengajuan disetujui - bila konfigurasi persetujuan
mensyaratkannya - tombol **Terapkan** menjalankan perubahannya.

Daftar seluruh pengajuan ada di **Tax Management > Pengajuan Replacement &
Cancel**; hasilnya terlihat di **Replacement / Cancelled Invoices**.

> Bila tombol **Terapkan** tidak bisa ditekan, artinya pengajuan itu masih
> menunggu persetujuan. Periksa konfigurasi Approval Matrix untuk model
> `lm_id.tax.invoice.request`. **Tanpa konfigurasi matriks, tombol persetujuan
> tidak muncul sama sekali** dan pengajuan akan mandek.

### 5.5 PPN Masukan

**Menu:** Tax Management > PPN Masukan
**Faktur Masukan** berisi faktur dari vendor; **Input Tax Crosscheck** dipakai
mencocokkannya dengan data yang tercatat di sistem pajak.

### 5.6 PPh - Bukti Potong

| Menu | Isinya |
|---|---|
| PPh > Bukti Potong Diterima | Bukti potong dari pelanggan atas penjualan PLN ES |
| PPh > PPh Sales Control | Kontrol PPh atas penjualan |
| PPh > Monitoring PPh Unifikasi > Issued (Purchase) | Bukti potong yang PLN ES terbitkan ke vendor |
| PPh > Monitoring PPh Unifikasi > Received (Sales) | Bukti potong yang PLN ES terima |
| Accounting > Reporting > Indonesia (PPh) > Bukti Potong PPh | Daftar seluruh bukti potong (Draft / Issued / Cancelled) |
| Accounting > Reporting > Indonesia (PPh) > Export SPT PPh | Wizard ekspor SPT |

### 5.7 Kategori Faktur dan Checklist Dokumen

**Menu:** Tax Management > Configuration > Invoice Categories & Document Requirements

Tiap kategori faktur bisa punya daftar dokumen wajib. Pada form faktur, setelah
kategori dipilih, tombol **Load Document Checklist** memunculkan daftar
dokumen yang harus dilampirkan. Pada Sales Order captive tombol serupa bernama
**Muat Ulang Checklist**.

### 5.8 Period Lock

**Menu:** Finance PLN ES > Konfigurasi > Period Lock / Unlock
Draft -> **Ajukan** -> **Setujui** -> **Eksekusi**. Setelah dieksekusi,
periode akuntansi terkunci dan jurnal di dalamnya tidak bisa diubah lagi.
Persetujuan ada di Manager Akuntansi & Pajak.

---

## 6. Bila Ada Masalah

| Gejala | Penyebab yang paling sering | Yang harus dilakukan |
|---|---|---|
| Menu Tax Management tidak muncul | Grup akuntansi bawaan Odoo belum diberikan | Tambah `Accounting / Billing` atau `Read Only` |
| Menu Tax Management > Configuration tidak ada | Bukan Accounting / Administrator | Minta admin akuntansi |
| Tombol **Mark as Replacement** tidak muncul | Faktur belum Posted, atau bukan faktur keluaran | Post dulu fakturnya |
| Tombol **Terapkan** pada pengajuan pajak mati | Menunggu persetujuan, atau matriks belum dikonfigurasi | Cek Approval Matrix untuk `lm_id.tax.invoice.request` |
| Pembayaran tidak membuat journal entry | Outstanding Account belum diatur di jurnal | Atur di Accounting > Configuration > Journals |
| Jurnal tidak bisa diubah | Periodenya sudah dikunci | Ajukan Period Unlock |
| Import bank statement berstatus Error | Format berkas tidak dikenali | Perbaiki berkas lalu **Parse File** ulang |
| Coretax Export berstatus Error | Ada data faktur yang belum lengkap | Jalankan **Validasi** untuk melihat penyebabnya |
| **Vendor bill dari PO gagal di-post** | Pemeriksaan 3-way match memakai field lama yang tidak ada lagi di Odoo 19 (`quantity_done` di `plnes_finance_pembiayaan`) | **Cacat program yang belum diperbaiki.** Laporkan ke tim pengembang; sementara ini bill yang barisnya terhubung ke PO tidak bisa di-post |

---

[<- Buku 04: Pengadaan, SCM & Vendor](04-pengadaan-scm-vendor.md) |
[Daftar isi](README.md) |
[Buku 06: Anggaran, Aset & Sewa ->](06-anggaran-aset-sewa.md)
