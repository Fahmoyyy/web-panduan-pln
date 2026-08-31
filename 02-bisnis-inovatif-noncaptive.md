# Buku 02 - Bisnis Inovatif dan Non-Captive

**Modul:** `pln_bisnov` (inti), `pln_bisnov_pemasaran` (probing & monitoring), `plnes_noncaptive` (modul lama)
**Aplikasi:** Bisnis Inovatif
**Peran utama:** Bisnov Contract Officer, Bisnov Finance Officer, Pemasaran User/Manager

Bisnis Inovatif adalah pekerjaan yang pelanggannya **di luar PLN**. Berbeda
dengan Captive yang berbasis kontrak payung bulanan, di sini kontraknya satuan
dan penagihannya mengikuti termin atau progres pekerjaan.

Ada dua lini bisnis di dalam satu modul:

- **Electric Services Enterprise (ES)** - jasa ketenagalistrikan
- **Electric Vehicle Ecosystem (EV)** - ekosistem kendaraan listrik

Keduanya memakai model dan form yang sama persis; yang membedakan hanya pintu
masuk menunya dan hak akses per lini.

---

## PENTING: Ada Dua App Bernama "Bisnis Inovatif"

Di layar utama akan terlihat **dua ikon dengan nama sama**. Ini bukan kesalahan
tampilan:

| Ikon | Berasal dari | Isinya | Status |
|---|---|---|---|
| Bisnis Inovatif | `pln_bisnov` | Electric Services Enterprise, Electric Vehicle Ecosystem, Pemasaran Non-Captive, Sales Board, Reporting | **Modul yang dipakai sekarang** |
| Bisnis Inovatif | `plnes_noncaptive` | Pemasaran Non-Captive > Pemasaran & Pelayanan (Lead, Survei, KAK/TOR, Order, BAP, BAST, dst) | Modul lama, sedang ditinggalkan |

Cara membedakan dengan cepat: yang **punya submenu Electric Services
Enterprise** adalah modul baru. Yang di dalamnya langsung ada
**Pemasaran Non-Captive > Pelayanan** dengan BAP/BAST adalah modul lama.

Selama keduanya masih terpasang, pastikan tim memakai app yang sama supaya data
tidak terpecah. Bagian 5 buku ini menjelaskan modul lama sekadar sebagai
rujukan.

---

## 1. Peta Menu (modul `pln_bisnov`)

```
Bisnis Inovatif
├── Electric Services Enterprise
│   ├── Kontrak                  pln.bisnov.contract (lini ES)
│   └── Pengakuan Kinerja        berita acara capaian
├── Electric Vehicle Ecosystem
│   ├── Kontrak                  pln.bisnov.contract (lini EV)
│   └── Pengakuan Kinerja
├── Pemasaran Non-Captive        (dari pln_bisnov_pemasaran)
│   ├── Probing                  prospek/lead
│   ├── Monitoring Project       pemantauan tahapan
│   ├── Rekap Potensi & Close Won
│   ├── Target Pemasaran
│   ├── Dashboard Non-Captive
│   └── Konfigurasi              9 master + wizard import Excel
├── Sales Board
│   ├── Sales Board              termin penagihan
│   └── Internal Order
├── Reporting
│   ├── Monitor Cash In - Electric Service
│   └── Monitor Cash In - EV Ecosystem
└── Konfigurasi                  7 master (khusus Bisnov Admin)
```

---

## 2. Peran dan Hak Akses

Hak akses Bisnov dibagi **dua lapis**. Keduanya ada di
**Settings > Users > tab Access Rights**, dan keduanya perlu diisi.

### Lapis 1 - peran umum (kategori Bisnov Role / Bisnov Approval)

| Grup | Untuk siapa |
|---|---|
| Bisnov Admin | Administrator modul, satu-satunya yang bisa buka Konfigurasi |
| Bisnov Contract Officer | Penyusun kontrak dan RAB |
| Bisnov Finance Officer | Termin, Sales Order, monitor cash in |
| Bisnov Viewer | Hanya melihat |
| Bisnov Approver | Pemberi persetujuan |

### Lapis 2 - hak per lini bisnis (ceklis)

Di tab yang sama ada kotak berjudul **Bisnis Inovatif** dengan dua baris
ceklis:

- **Hak Akses EV Ecosystem**
- **Hak Akses Electric Service**

Tiap baris berisi 12 ceklis: 3 peran pemikul (Manager, Assman, Officer)
dikali 4 hak (Read, Write, Create, Delete), ditambah ceklis Pemikul.

| Ceklis | Artinya |
|---|---|
| ... Read | Boleh membuka dan melihat |
| ... Write | Boleh mengubah dokumen yang sudah ada |
| ... Create | Boleh membuat dokumen baru |
| ... Delete | Boleh menghapus |

> **Aturan praktis:** mencentang Write, Create, atau Delete **tanpa** Read
> membuat dokumennya gagal dibuka. Sistem sudah menyalakan Read otomatis
> ketika salah satu hak tulis dicentang, dan centang Read-nya ikut terlihat
> menyala di layar.

> **Mengapa dibuat kotak sendiri:** ceklis semacam ini biasanya oleh Odoo
> dilempar ke bagian "Extra Rights" yang hanya muncul di mode developer.
> Kotak **Bisnis Inovatif** dibuat khusus supaya admin bisa mengaturnya tanpa
> menyalakan mode developer.

**Contoh pengaturan:** staf yang hanya menangani kendaraan listrik dan boleh
membuat kontrak diberi: `Bisnov Contract Officer` (lapis 1) + ceklis
`Bisnov EV - Officer Read`, `Write`, `Create` (lapis 2). Menu Electric Services
Enterprise tidak akan muncul untuknya.

### Pemasaran

| Grup | Bisa apa |
|---|---|
| Pemasaran Non-Captive / User | Probing, Monitoring, Target, Rekap, Dashboard |
| Pemasaran Non-Captive / Manager | Semua di atas + submenu Konfigurasi |

---

## 3. Master Data yang Harus Siap

**Bisnis Inovatif > Konfigurasi** (Bisnov Admin):

| Master | Isinya |
|---|---|
| Kode Layanan | Kode jasa yang ditawarkan |
| Detail Layanan | Rincian di bawah kode layanan |
| Jenis Kontrak | Metode pengadaan |
| Tipe Kontrak | Tipe kontrak Bisnov |
| Kategori Biaya | Pengelompokan baris RAB |
| Sumber Anggaran | Sumber dana |
| Alasan Amandemen | Pilihan alasan saat amandemen |

**Pemasaran Non-Captive > Konfigurasi** (Pemasaran Manager):
Wilayah/UID PLN, Jenis Pelanggan, Master Layanan, Media Probing, Mekanisme
Penunjukan, Tahapan, Alasan Kegagalan, Approval Threshold, dan wizard
**Import dari Excel**.

---

## 4. Alur Kerja

```
Probing (prospek)  ->  Close Won  ->  Kontrak  ->  Termin / Progres  ->  SO  ->  Invoice
```

### 4.1 Probing - mencatat prospek

**Menu:** Bisnis Inovatif > Pemasaran Non-Captive > Probing
**Status:** Berjalan -> Close Won / Gagal

| # | Langkah | Yang terlihat di layar | Catatan |
|---|---|---|---|
| 1 | **New** -> isi calon pelanggan, layanan, nilai potensi, UID | Prospek berstatus Berjalan | Media probing & mekanisme dari master |
| 2 | Perbarui tahapan sesuai perkembangan | Kolom tahapan berubah | Tahapan diambil dari master Tahapan |
| 3 | Bila gagal: klik **Tandai Gagal** | Wizard meminta alasan kegagalan | Alasan wajib, diambil dari master |
| 4 | Bila berhasil: ubah status ke **Close Won** | Tombol **Buat Kontrak** muncul | |
| 5 | Klik **Buat Kontrak** | Kontrak Bisnov terbentuk, terisi dari data prospek | Kolom Sumber Kontrak pada kontrak menunjuk balik ke prospek |

### 4.2 Monitoring Project

**Menu:** Bisnis Inovatif > Pemasaran Non-Captive > Monitoring Project
**Tahapan:** Probing -> Penawaran -> Menunggu Harga -> In Process -> Close Won / Failed

Dokumen ini hanya pemantauan; tidak punya tombol proses. Statusbar diklik
langsung untuk memindahkan tahapan.

**Import dari Excel** (Konfigurasi > Import dari Excel) dipakai memasukkan data
Probing dan Monitoring sekaligus dari berkas Excel. Setiap baris diproses
sendiri-sendiri: baris yang gagal dilaporkan di log hasil tanpa membatalkan
baris lain yang sudah berhasil.

### 4.3 Kontrak Bisnov

**Menu:** Electric Services Enterprise > Kontrak (atau Electric Vehicle Ecosystem > Kontrak)
**Status:** Draft -> Submitted -> Approved -> **Active** -> Done

| # | Langkah | Yang terlihat di layar | Catatan |
|---|---|---|---|
| 1 | **New** -> isi pelanggan, lini bisnis, tanggal, nilai | Lini bisnis menentukan menu mana yang menampilkannya | |
| 2 | Isi baris RAB satuan | Harga bisa terisi dari master Estimasi Harga per Region | Master harga dipakai bersama dengan Captive |
| 3 | Isi jadwal termin / milestone | Tab termin | Bobot tiap termin dalam persen |
| 4 | Klik **Hitung Bobot Otomatis** | Bobot terisi merata / proporsional | Boleh disesuaikan manual sesudahnya |
| 5 | **Submit** -> **Approve** | Status ke Approved | Persetujuan mengikuti matriks |
| 6 | Klik **Activate** | Status **Active** | Tombol operasional baru muncul di sini |
| 7 | Selesai pekerjaan: klik **Selesai** | Status Done | |

Setelah Active tersedia:

| Tombol | Kegunaan |
|---|---|
| **Buat Termin** | Membuat termin Sales Board dari jadwal termin |
| **Buat Termin dari Progress** | Membuat termin berdasarkan progres pekerjaan yang tercatat |
| **Kurva-S** | Grafik rencana vs realisasi progres |
| **Buat Purchase Request** | PR untuk kebutuhan pengadaan kontrak ini |

**Kembali ke Draft** tersedia dari Submitted atau Cancelled. **Batalkan**
tersedia selama belum Done.

### 4.4 Pengakuan Kinerja

**Menu:** Electric Services Enterprise > Pengakuan Kinerja
**Status:** Draft -> Terbit -> Batal

Dokumen berita acara capaian pekerjaan. **Terbitkan** mengesahkannya;
**Kembali ke Draft** membuka lagi bila ada koreksi.

### 4.5 Sales Board dan penerbitan SO

**Menu:** Bisnis Inovatif > Sales Board > Sales Board
**Status:** Draft -> Terbit -> Dibatalkan

| # | Langkah | Yang terlihat di layar | Catatan |
|---|---|---|---|
| 1 | Termin muncul dari kontrak (tombol Buat Termin) | Daftar termin per kontrak | |
| 2 | Klik **Terbitkan Sales Order** | SO terbentuk, status termin jadi Terbit | Tombol hilang bila SO sudah ada |
| 3 | Bila isinya berubah: klik **Perbarui SO** | SO ikut diperbarui | Hanya selama termin masih Draft |
| 4 | Klik **Batalkan** bila termin dibatalkan | Status Dibatalkan | Hanya dari Draft |

**Internal Order** (Sales Board > Internal Order): Draft -> **Confirm** ->
Confirmed. Fungsinya sama seperti di Captive: menautkan dokumen penjualan dan
pembelian pada satu pekerjaan.

### 4.6 Amandemen, Additional Project, Terminasi, Arsip

| Dokumen | Status | Tombol kunci |
|---|---|---|
| Amandemen | Draft -> Submitted -> Approved -> **Applied** | **Apply ke RAB** yang benar-benar mengubah RAB kontrak |
| Additional Project | Draft -> Submitted -> Cancelled | **Buat Termin** setelah Submitted |
| Terminasi | Draft -> Submitted -> Approved | **Setujui dan Tutup Kontrak** sekaligus menutup kontrak induk |
| Arsip | Draft -> Diarsipkan | **Arsipkan** |

> Sama seperti Captive: amandemen yang sudah **Approved** belum mengubah apa
> pun. **Apply ke RAB** adalah langkah yang mengubah angka kontrak.

---

## 5. Laporan

**Bisnis Inovatif > Reporting:**

| Laporan | Isinya | Siapa yang bisa buka |
|---|---|---|
| Monitor Cash In - Electric Service | Milestone lini ES: Belum Invoice / Sudah Invoice / Terbayar | Bisnov ES Pemikul Read, Viewer, Finance Officer, Admin |
| Monitor Cash In - EV Ecosystem | Sama untuk lini EV | Bisnov EV Pemikul Read, Admin |

**Rekap Potensi & Close Won** (di Pemasaran) menampilkan potensi bisnis dalam
bentuk pivot UP x UID x Status.

---

## 6. Modul Lama `plnes_noncaptive` (rujukan)

Masih terpasang dan menunya masih terbuka. Alurnya:

```
Lead -> Survei -> Kajian RAB -> Proposal -> Order -> Kontrak Rinci
     -> SPK Mitra / Work Assignment -> QC Result -> BAP -> BAST
     -> Invoice Request -> Monitor Garansi
```

Peran: NCAP / Admin, Manajer, Asisten Manajer, Manajer Pengembangan Produk &
SCM, VP, Staff, Tim Pelaksana, Read Only.

Ringkasan status tiap dokumen:

| Dokumen | Status |
|---|---|
| Lead | Baru -> Dihubungi -> Survei Dijadwalkan -> Survei Selesai -> Kajian -> Proposal Terkirim -> Deal |
| Survei | Draft -> Dijadwalkan -> Sedang Berjalan -> Selesai |
| Kajian RAB | Draft -> Diajukan -> Disetujui Asman -> Disetujui Manager -> Final |
| Proposal | Draft -> Diajukan -> Disetujui Manager -> Terkirim -> Diterima / Ditolak / Kedaluwarsa |
| KAK/TOR | Draft -> Diajukan -> Review Asman -> Disetujui Manager -> Dikirim ke PBJ -> PBJ Berjalan |
| Negosiasi | Draft -> Dalam Negosiasi -> BA Negosiasi Signed -> Disetujui Asman -> Disetujui Manager |
| Order | Draft -> Diajukan -> Review Asman -> Disetujui Manajer -> Disetujui VP -> Aktif -> Selesai |
| Kontrak Rinci | Draft -> Diajukan -> Disetujui Manajer -> Disetujui VP -> Aktif -> Selesai |
| SPK Mitra | Draft -> Diajukan -> Review Asman -> Disetujui Manager -> Dikirim ke Mitra -> Dalam Pelaksanaan |
| Work Assignment | Draft -> Ditugaskan -> Sedang Dikerjakan -> Menunggu QC -> QC Passed -> Selesai |
| QC Result | Draft -> Sedang Berjalan -> Passed / Failed |
| BAP | Draft -> Dalam Review -> Signed |
| BAST | Draft -> Signed |
| Invoice Request | Draft -> Diajukan -> Dikirim ke Finance -> Invoice Terbit -> Lunas / Overdue |
| Monitor Garansi | Aktif -> Ada Klaim -> Klaim Selesai -> Expired |

---

## 7. Bila Ada Masalah

| Gejala | Penyebab yang paling sering | Yang harus dilakukan |
|---|---|---|
| Ada dua app bernama Bisnis Inovatif | Modul lama dan baru sama-sama terpasang | Lihat bagian PENTING di atas; sepakati satu app untuk seluruh tim |
| Ikon Bisnis Inovatif tidak muncul | Belum punya grup Bisnov apa pun | Beri salah satu grup Bisnov Role, atau grup Pemasaran |
| Menu Electric Services Enterprise tidak ada padahal EV ada | Ceklis lini ES belum dicentang | Settings > Users > kotak Bisnis Inovatif > Hak Akses Electric Service |
| Dokumen gagal dibuka padahal sudah dicentang Write | Read belum menyala | Centang Read pada peran yang sama |
| Ceklis lini bisnis tidak ketemu | Mencari di bagian Extra Rights | Ada di kotak sendiri bernama **Bisnis Inovatif** di tab Access Rights |
| Tombol Buat Kontrak tidak muncul di Probing | Status belum Close Won | Ubah status prospek ke Close Won |
| Tombol operasional kontrak tidak ada | Kontrak belum Active | Submit -> Approve -> **Activate** |
| RAB tidak berubah setelah amandemen disetujui | Belum ditekan **Apply ke RAB** | Buka amandemen berstatus Approved, klik Apply ke RAB |
| Import Excel Pemasaran sebagian gagal | Data barisnya tidak cocok master | Baca log hasil; baris lain yang berhasil tetap tersimpan |
| Termin tidak bisa diterbitkan jadi SO | SO-nya sudah pernah terbit | Kolom Sale Order pada termin sudah terisi |

---

[<- Buku 01: Captive Management](01-captive-management.md) |
[Daftar isi](README.md) |
[Buku 03: Business Development ->](03-business-development.md)
