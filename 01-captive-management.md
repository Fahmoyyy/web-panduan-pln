# Buku 01 - Captive Management

**Modul:** `pln_captive_umbrella` (inti), `pln_material_request` (material proyek)
**Aplikasi:** Captive Management
**Peran utama:** Contract Officer, Operations Officer, Finance Officer, Tax Officer, Approver

Captive adalah pekerjaan yang pelanggannya PLN sendiri (kontrak payung ke Unit
Induk / Unit Pelaksana). Modul ini menampung seluruh siklusnya: permintaan
proyek, kontrak payung beserta RAB-nya, penagihan bulanan, sampai kontrak
berakhir atau diputus.

---

## 1. Peta Menu

```
Captive Management
├── Contracts
│   ├── Project Requests       permintaan proyek dari unit
│   ├── Master Contracts       kontrak payung + RAB          <- pusat modul
│   ├── Additional Project     pekerjaan tambahan di luar RAB
│   ├── Amendments             amandemen kontrak
│   ├── Terminations           pemutusan kontrak
│   └── Archives               arsip dokumen kontrak
├── Operations
│   └── SLA Reports            laporan capaian SLA per periode
├── Billing
│   └── Sales Orders           SO hasil penagihan
├── Finance
│   ├── Receivable Collection  pemantauan piutang
│   └── Revenue Accrual        akrual pendapatan
├── Tax
│   └── Output VAT             PPN keluaran
├── Internal Order             kode IO yang dipakai lintas dokumen
├── Reporting                  5 daftar siap pakai
└── Configuration              13 master data (khusus Captive Admin)
```

---

## 2. Peran dan Hak Akses

Semua grup di bawah ini ada di **Settings > Users & Companies > Users**, tab
**Access Rights**, kategori **Captive Role** dan **Captive Approval**.

| Grup | Untuk siapa | Otomatis dapat |
|---|---|---|
| Captive Admin | Administrator modul | Role / User |
| Captive Contract Officer | Penyusun kontrak & RAB | Role / User + Approval User |
| Captive Operations Officer | Pelaksana lapangan, pengisi SLA | Role / User + Approval User |
| Captive Finance Officer | Penagihan, piutang, akrual | Role / User + Approval User |
| Captive Tax Officer | PPN keluaran & faktur pajak | Role / User + Approval User |
| Captive Approver | Pemberi persetujuan | Role / User + Approval User |
| Captive Viewer | Hanya melihat | Role / User |

XML ID lengkap ada di [Buku 00](00-panduan-umum-dan-hak-akses.md).

**Yang membedakan tiap peran di layar:**

| Menu | Siapa yang bisa membuka |
|---|---|
| Contracts (semua submenu) | Semua peran Captive |
| Additional Project | Semua **kecuali** Tax Officer dan Approver |
| Internal Order | Admin, Contract Officer, Finance Officer, Viewer |
| Operations, Billing, Finance, Tax, Reporting | Semua peran Captive |
| **Configuration** | **Hanya Captive Admin** |

> **Melihat menu belum tentu boleh mengubah isinya.** Tombol persetujuan hanya
> muncul untuk user yang terdaftar di baris matriks persetujuan (lihat
> [Buku 00](00-panduan-umum-dan-hak-akses.md) bagian Approval Matrix).
>
> **Captive Viewer perlu perhatian khusus:** tombol-tombol proses (Submit,
> Approve, Activate, dan seterusnya) **tetap terlihat** di layarnya, karena
> tombol Captive disembunyikan berdasarkan status dokumen, bukan berdasarkan
> grup. Viewer hanya punya hak baca, sehingga menekan tombol itu akan
> memunculkan pesan AccessError. Ini bukan kerusakan - hak aksesnya memang
> hanya membaca.

---

## 3. Master Data yang Harus Siap Lebih Dulu

Dikerjakan **Captive Admin** di **Captive Management > Configuration**. Tanpa
ini, form kontrak akan menolak disimpan karena field wajibnya kosong.

| Master | Isinya | Dipakai di |
|---|---|---|
| Master Budget Source | Sumber dana (AI, AO, dll) | Kontrak |
| Master Jenis Kontrak | Metode pengadaan | Kontrak |
| Master Tipe Pekerjaan | Yantek, OPKIT, dll | Kontrak, RAB |
| Master Business Category | Kategori bisnis | Kontrak |
| Master Service Category | Kategori layanan | Kontrak |
| Master SLA Type | Jenis SLA + bobot | SLA Report |
| Master Amandement Type | Jenis amandemen | Amendment |
| Master Reason Category | Alasan (amandemen/terminasi) | Amendment, Termination |
| Master Unit Layanan | Unit Layanan pelanggan | Kontrak |
| Master Termin | Termin penagihan | Sales Board |
| Master Group Region | Pengelompokan UP/UID | Kontrak, laporan |
| Master Cost Category | Kategori biaya RAB | Baris RAB |
| Kamus Pemetaan RAB | Padanan nama item Excel ke produk Odoo | Import RAB |

**Kamus Pemetaan RAB** paling sering jadi penyebab impor gagal. Isinya pasangan
"tulisan di Excel" dengan "produk di Odoo". Semakin lengkap kamusnya, semakin
sedikit baris yang harus dipetakan manual saat impor.

---

## 4. Alur Kerja

Urutan besarnya:

```
Project Request  ->  Master Contract (+RAB)  ->  Sales Board  ->  SO  ->  Invoice
                                  |
                                  +-> Amendment / Additional Project / Termination
```

### 4.1 Project Request - permintaan proyek dari unit

**Menu:** Captive Management > Contracts > Project Requests
**Status:** Draft -> Under Review -> Waiting Approval -> Approved -> Generated Contract

| # | Langkah | Yang terlihat di layar | Catatan |
|---|---|---|---|
| 1 | Klik **New**, isi unit peminta, jenis pekerjaan, nilai perkiraan | Nomor dokumen terisi otomatis | Nomor dari sequence, tidak bisa diketik |
| 2 | Klik **Submit** | Status pindah ke Under Review | Tombol Submit hilang setelah ditekan |
| 3a | Bila nilainya melewati batas matriks: klik **Request Approval** | Status ke Waiting Approval, muncul daftar approver | Tombol ini hanya muncul kalau matriks memang mensyaratkan persetujuan |
| 3b | Bila di bawah batas: klik **Approve** (Direct Approve) | Langsung ke Approved | Tanpa lewat approver |
| 4 | Approver membuka dokumen, klik **Approve** atau **Reject** | Status ke Approved / Rejected | Tombol hanya tampil bagi approver yang berhak |
| 5 | Klik **Generate Contract** | Master Contract baru terbentuk, status ke Generated Contract | Tombol **Contract** di kanan atas membuka kontraknya |

Bila ditolak: tombol **Reset to Draft** mengembalikan ke Draft untuk diperbaiki.
Selagi Under Review, **Rollback** mengembalikan dokumen ke tahap sebelumnya.

### 4.2 Master Contract - kontrak payung

**Menu:** Captive Management > Contracts > Master Contracts
**Status:** Draft -> Under Review -> Legal Review -> Waiting Approval -> Approved -> **Active** -> Expired / Terminated

| # | Langkah | Yang terlihat di layar | Catatan |
|---|---|---|---|
| 1 | **New** -> isi pelanggan (UP/UID), tanggal mulai & selesai, nilai kontrak | Header terisi | Tanggal menentukan periode penagihan |
| 2 | Isi tab **RAB** | Baris RAB tersusun per section | Cara lengkap: [panduan RAB](../panduan-rab-master-contract.md) |
| 3 | Klik **Submit** -> **Legal Review** | Status berjalan bertahap | Legal Review dikerjakan fungsi hukum |
| 4 | **Request Approval** atau **Approve** | Sesuai matriks persetujuan | Sama polanya dengan Project Request |
| 5 | Klik **Activate** | Status **Active** | **Baru setelah Active** tombol operasional muncul |

Setelah Active barulah tersedia:

| Tombol | Kegunaan |
|---|---|
| **Generate Fixed Cost** | Membuat baris Sales Board untuk biaya tetap seluruh periode kontrak |
| **Create Variable Cost** | Wizard biaya variabel: unduh template, isi qty aktual, impor, buat baris board |
| **Tarik Additional Project** | Menarik pekerjaan tambahan yang sudah disetujui ke Sales Board |
| **Create Billing (SO)** | Membuat Sales Order dari baris board yang dipilih |
| **Buat PR Tahunan** | Membuat Purchase Request satu dokumen per tahun kontrak (hanya untuk RAB bulanan) |
| **Buat / Perbarui Purchase Request** | PR menyusul untuk baris RAB yang belum punya PR |
| **Import RAB Lengkap** | Membaca berkas Excel RAB satu file penuh |
| **Lihat per Section** | Membuka baris RAB dikelompokkan per section, bisa dilipat |

### 4.3 Mengisi RAB

RAB adalah jantung kontrak: dari sinilah angka penagihan dan PR berasal.

| # | Langkah | Yang terlihat di layar | Catatan |
|---|---|---|---|
| 1 | Buka tab **RAB** di kontrak Active | Daftar baris dikelompokkan per section | Section bertingkat sampai 3 (A, A.I, A.V.a) |
| 2 | Klik **Import RAB Lengkap** | Wizard impor terbuka | Status wizard: Belum Dibaca -> Menunggu Pemetaan -> Sudah Dipromosikan |
| 3 | Unggah berkas Excel, klik **Baca Berkas** | Jumlah baris terbaca + jumlah yang belum terpetakan | Baris tanpa padanan produk tidak akan hilang, hanya menunggu |
| 4 | Untuk yang belum terpetakan: pilih produk manual, atau klik **Buat Produk untuk yang Belum Terpetakan** | Produk baru terbentuk massal | Cek dulu, jangan asal buat produk kembar |
| 5 | Klik **Promosikan** | Baris masuk ke RAB kontrak | Sesudah ini periksa kolom Selisih vs Excel |
| 6 | Periksa **Selisih vs Excel** | Kolom pembanding di layar RAB | Selisih besar berarti tipe perhitungan barisnya salah |

> **Yang sering keliru:** tipe perhitungan baris. Ada lima tipe dan masing-masing
> punya rumus sendiri. Salah tipe membuat angka tahunan meleset padahal harga
> satuannya benar. Perbandingan terhadap kolom Excel adalah pengaman utamanya.

Alternatif mengisi tanpa Excel: tombol **Katalog** pada baris section untuk
mengambil item dari katalog produk, atau ketik manual.

### 4.4 Sales Board dan penagihan bulanan

Satu bulan bisa menghasilkan lebih dari satu Sales Order: **Termin Awal**
(tagihan pokok), **Termin Penyesuaian** (koreksi), dan susulan.

| # | Langkah | Yang terlihat di layar | Catatan |
|---|---|---|---|
| 1 | Di kontrak Active klik **Generate Fixed Cost** | Baris board per bulan terbentuk | Sekali saja per periode |
| 2 | Untuk biaya variabel klik **Create Variable Cost** | Wizard 3 tombol | Unduh Template -> isi qty aktual -> Impor Qty Actual -> Buat Baris Board |
| 3 | Untuk pekerjaan tambahan klik **Tarik Additional Project** | Baris board tambahan muncul | Hanya Additional Project berstatus Submitted |
| 4 | Klik **Termin Awal** atau **Termin Penyesuaian** | Baris board dikelompokkan jadi termin | Termin menentukan SO mana yang terbit |
| 5 | Klik **Create Billing (SO)** | Wizard memilih baris, lalu SO terbentuk | Bisa juga dari baris board: tombol **Create SO** |
| 6 | Buka SO di **Billing > Sales Orders** | SO berisi baris dari board | SO captive punya tombol persetujuan sendiri |
| 7 | Setujui SO lalu Confirm | SO terkonfirmasi, invoice bisa dibuat | Alur invoice mengikuti Odoo biasa |

Tombol **Rincian** pada baris board memperlihatkan asal angkanya.

### 4.5 SLA Report

**Menu:** Captive Management > Operations > SLA Reports
**Status:** Draft -> Submitted -> Validated -> Done

| # | Langkah | Yang terlihat di layar | Catatan |
|---|---|---|---|
| 1 | **New** -> pilih kontrak dan periode | Baris SLA terisi dari Master SLA Type | Bobot mengikuti master |
| 2 | Isi capaian tiap baris | Kolom capaian | Diisi Operations Officer |
| 3 | Klik **Compute SLA** | Nilai akhir & potongan terhitung | Hanya bisa saat Draft |
| 4 | Klik **Submit** lalu **Validate** | Status ke Validated | Validasi oleh atasan |
| 5 | Klik **Generate SO Billing** | SO penagihan terbentuk memakai hasil SLA | Tombol hanya di status Validated |

Tombol **Reset to Draft** tersedia dari Submitted, Validated, dan Cancelled.

### 4.6 Additional Project - pekerjaan tambahan

**Menu:** Captive Management > Contracts > Additional Project
**Status:** Draft -> Submitted -> Cancelled

| # | Langkah | Yang terlihat di layar | Catatan |
|---|---|---|---|
| 1 | **New** -> pilih kontrak induk, isi uraian pekerjaan | Terhubung ke kontrak | |
| 2 | Klik **Catalog** untuk mengambil item | Baris terisi dari katalog | Sama seperti katalog RAB |
| 3 | Klik **Submit** | Status Submitted | Baru status ini yang bisa ditarik ke Sales Board |
| 4 | Bila batal: **Cancel**, lalu **Reset to Draft** bila perlu dipakai lagi | | |

### 4.7 Amendment - amandemen kontrak

**Menu:** Captive Management > Contracts > Amendments
**Status:** Draft -> Under Review -> Legal Review -> Waiting Approval -> Approved -> **Effective** -> **Applied**

| # | Langkah | Yang terlihat di layar | Catatan |
|---|---|---|---|
| 1 | **New** -> pilih kontrak, jenis amandemen, alasan | | Jenis & alasan dari master |
| 2 | Isi perubahan (nilai, tanggal, baris RAB) | | Tombol **Catalog** tersedia |
| 3 | **Submit** -> **Legal Review** -> persetujuan | Berjalan seperti kontrak | **Rollback** tersedia saat Legal Review |
| 4 | Klik **Mark Effective** | Status Effective | Amandemen sudah sah, belum mengubah kontrak |
| 5 | Klik **Apply to Contract** | Perubahan masuk ke kontrak induk, status Applied | **Langkah ini yang benar-benar mengubah kontrak** |

> Effective dan Applied itu dua hal berbeda. Selama masih Effective, kontrak
> induk belum berubah. Bila angka kontrak dirasa belum berubah setelah
> amandemen disetujui, periksa apakah **Apply to Contract** sudah ditekan.

### 4.8 Termination - pemutusan kontrak

**Menu:** Captive Management > Contracts > Terminations
Alur reviewnya paling panjang, berurutan dan tidak bisa dilompati:

```
Draft -> Under Review -> Operational Review -> Procurement Review
      -> Financial Review -> Legal Review -> Waiting Approval
      -> Approved -> Effective -> Settlement Pending -> Closed
```

| Tombol | Muncul saat status | Akibat |
|---|---|---|
| Submit | Draft | Ke Under Review |
| Operational Review | Under Review | Ke Operational Review |
| Procurement Review | Operational Review | Ke Procurement Review |
| Financial Review | Procurement Review | Ke Financial Review |
| Legal Review | Financial Review | Ke Legal Review |
| Request Approval / Approve | Legal Review | Sesuai matriks |
| Mark Effective | Approved | Pemutusan berlaku |
| Set Settlement Pending | Effective | Menunggu penyelesaian kewajiban |
| Close Contract | Effective / Settlement Pending | Kontrak induk jadi Terminated |
| Reopen Termination | Closed | Membuka kembali bila ada koreksi |

**Rollback** tersedia di tahap review untuk mundur satu langkah.

### 4.9 Archive - arsip dokumen kontrak

**Menu:** Captive Management > Contracts > Archives
Draft -> Document Review -> Archive Validation -> (persetujuan) -> Archived -> Locked

Tombol **Create New Version** dipakai bila dokumen arsip perlu diperbarui;
versi lama menjadi Obsolete, bukan dihapus. **Lock Archive** mengunci arsip
supaya tidak bisa diubah lagi.

### 4.10 Finance dan Tax

| Dokumen | Menu | Status | Cara kerja |
|---|---|---|---|
| Billing | dari kontrak / SO | Draft -> Submitted -> Approved -> Invoiced -> Paid | **Create Invoice** setelah Approved |
| Receivable Collection | Finance > Receivable Collection | Draft -> Submitted -> Done | Terbentuk dari invoice, dipakai memantau pelunasan |
| Revenue Accrual | Finance > Revenue Accrual | Draft -> Submitted -> Approved -> Posted | **Post Journal** membuat jurnal akrual |
| Output VAT | Tax > Output VAT | Draft -> Waiting Approval -> Approved -> Done | Untuk PPN keluaran atas tagihan captive |

### 4.11 Internal Order

**Menu:** Captive Management > Internal Order (Draft -> Confirmed)

Internal Order (IO) adalah kode yang menautkan dokumen penjualan dan
pembelian pada satu pekerjaan yang sama. Satu field IO dipakai di invoice
maupun bill, dan ikut terbawa sampai pembayaran, sehingga biaya dan
pendapatan satu pekerjaan bisa dipertemukan.

Buat IO -> klik **Confirm** -> IO siap dipilih di dokumen lain.

---

## 5. Laporan

**Menu:** Captive Management > Reporting - lima daftar siap pakai, tinggal buka:

| Laporan | Menjawab pertanyaan |
|---|---|
| Sisa Tagih Bulan Berjalan | Baris board bulan ini yang belum jadi SO |
| Nilai Tidak Tertagih | Nilai yang sudah lewat periodenya tapi belum tertagih |
| Termin Belum Terbit SO | Termin yang sudah dibentuk tapi SO-nya belum ada |
| Progress Penagihan per Kontrak | Berapa persen kontrak sudah tertagih |
| Kontrak Akan Berakhir | Kontrak yang mendekati tanggal selesai |

Semua daftar bisa difilter dan dikelompokkan seperti daftar Odoo biasa, lalu
diekspor lewat **Actions > Export**.

---

## 6. Material Proyek (`pln_material_request`)

Modul pendamping untuk material yang dipakai pekerjaan captive. Menunya berdiri
sendiri di luar app Captive:

| Menu | Model | Status |
|---|---|---|
| Reservasi Stok per Project | stok yang dikunci per proyek | - |
| Realokasi Stok | Draf -> Menunggu Persetujuan -> Disetujui / Ditolak | butuh grup **Approver Realokasi Stok** |
| Pengembalian Material | Draf -> Selesai / Dibatalkan | |
| Klaim Garansi | Draf -> Diajukan ke Penyedia -> Sudah Diganti / Ditolak Penyedia | |
| Sewa Vendor | Berjalan -> Selesai / Dibatalkan | |

Master **Term Garansi** ada di Purchase > Configuration > Term Garansi.

---

## 7. Bila Ada Masalah

| Gejala | Penyebab yang paling sering | Yang harus dilakukan |
|---|---|---|
| Menu Captive Management tidak muncul | User belum punya grup Captive apa pun | Settings > Users > tambah salah satu grup Captive Role |
| Menu Configuration tidak ada | Memang hanya untuk Captive Admin | Minta Admin yang mengubah master data |
| Tombol Approve tidak muncul padahal statusnya Waiting Approval | User bukan approver pada baris matriks itu | Cek Approval Matrix untuk model tersebut |
| Tombol Generate Fixed Cost / Create Billing tidak ada | Kontrak belum **Active** | Selesaikan persetujuan lalu klik **Activate** |
| Angka RAB tidak cocok dengan Excel | Tipe perhitungan baris salah | Perbaiki tipe baris, lihat kolom Selisih vs Excel |
| Impor RAB menyisakan banyak baris tak terpetakan | Kamus Pemetaan RAB belum lengkap | Lengkapi kamus, atau petakan manual di wizard |
| Amandemen sudah Approved tapi kontrak tidak berubah | Belum ditekan **Apply to Contract** | Mark Effective, lalu Apply to Contract |
| Baris board tidak bisa dibuatkan SO | Barisnya sudah punya SO | Kolom Sale Order pada baris sudah terisi |
| Additional Project tidak muncul saat Tarik Additional Project | Statusnya masih Draft | Klik **Submit** dulu di dokumen Additional Project |

---

[<- Buku 00: Panduan Umum & Hak Akses](00-panduan-umum-dan-hak-akses.md) |
[Daftar isi](README.md) |
[Buku 02: Bisnis Inovatif ->](02-bisnis-inovatif-noncaptive.md)
