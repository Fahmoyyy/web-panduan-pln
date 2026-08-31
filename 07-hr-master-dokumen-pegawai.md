# Buku 07 - HR Master dan Dokumen Pegawai

**Modul:** `pln_hr_master`, `pln_hr_docs`, `pln_hr_development`, `pln_hr_biodata`, `pln_hr_uji_jabatan`, `pln_hr_konsolidasi`, `pln_hr_broadcast`, `pln_hr_ess`, `pln_home_api`
**Aplikasi:** Employees, Employee Self Service, Integrasi PLN Home
**Peran utama:** HR Officer, HR Administrator, atasan langsung, seluruh pegawai (ESS)

Buku ini mencakup data induk pegawai, struktur organisasi, dokumen
kepegawaian, dan layanan mandiri pegawai. Untuk penggajian lihat
[Buku 08](08-payroll-dan-sppd.md).

---

## 1. Peta Menu

```
Employees                                      (aplikasi bawaan Odoo, diperluas)
├── Employees                                  data induk pegawai (tab diperluas)
├── Struktur Organisasi                        hr.jabatan - formasi jabatan
├── Mutasi Jabatan                             riwayat jabatan + SK
├── Uji Jabatan
├── Pengembangan Pegawai
│   ├── Data Pengembangan Pegawai
│   └── Rekap per Jenis
├── Dokumen
│   ├── Surat Peringatan · Surat Teguran
│   ├── Surat Keterangan · Surat Paklaring · Surat Penugasan
│   ├── BAP Klarifikasi/Keterangan · Penanggungjawaban
├── Hubungan Industrial > Monitoring LKS Bipartit
├── Evaluasi > Evaluasi Kinerja Karyawan
├── PHK > Pengajuan PHK · Daftar PHK · Jenis PHK
├── Broadcast Email > Daftar Broadcast
├── Konsolidasi PLN                            wizard ekspor data pegawai
└── Configuration > Master Data Pegawai        ~60 master data

Employee Self Service                          (untuk semua pegawai)
├── Biodata
├── My Requests > Attendance Request · Permission Request
│                 Business Trip · Leave Extension
├── Ijin > Daftar Ijin · Master Tipe Izin
├── Payslip Employee
├── Attendance > Presensi · Presensi Approval
└── Configuration > Attendance Request Types · Permission Types

Integrasi PLN Home                             (Home API Manager)
├── Event Presensi · Peta Record · Log Kiriman · Buat API Key
```

---

## 2. Peran dan Hak Akses

### Grup bawaan Odoo yang wajib ada lebih dulu

| Grup | Akibatnya |
|---|---|
| Employees / Employee (`group_hr_user`) | Bisa membuka app Employees dan melihat data pegawai |
| Employees / Officer: Manage all employees | Bisa mengubah data seluruh pegawai |
| Employees / Administrator | Bisa membuka Configuration > Master Data Pegawai |
| Time Off / Officer atau Administrator | Untuk master tipe izin |

### Grup khusus PLN ES

| Grup | Kategori | Untuk siapa |
|---|---|---|
| Employee Self Service / Employee | Employee Self Service | Seluruh pegawai - pengajuan mandiri |
| Employee Self Service / Supervisor | Employee Self Service | Atasan langsung - persetujuan tahap 1 |
| Employee Self Service / HR Officer | Employee Self Service | HR - persetujuan tahap 3 + konfigurasi ESS |
| Employee Self Service / Administrator | Employee Self Service | Administrator ESS |
| Hubungan Industrial / User | pln_hr_docs | Membuat dokumen hubungan industrial |
| Hubungan Industrial / Manager | pln_hr_docs | Menyetujui, mengelola |
| Broadcast Email / Officer | pln_hr_broadcast | Menyusun broadcast |
| Broadcast Email / Administrator | pln_hr_broadcast | Mengirim broadcast |
| Penguji Jabatan | pln_hr_uji_jabatan | Menjadwalkan dan menilai uji jabatan |
| Approval Mutasi Jabatan / Approver | pln_hr_master | Menyetujui SK mutasi |
| Home API Manager | pln_home_api | Mengelola integrasi PLN Home |

> **Peringatan hak akses:** sejumlah field pegawai (NIPEG, data gaji, data
> pribadi tertentu) dikunci di tingkat field. Kunci semacam ini **tidak bisa
> dibuka lewat pengaturan hak akses model biasa** - satu-satunya cara adalah
> memberi user grup yang memang memegang field itu.

---

## 3. Data Induk Pegawai

**Menu:** Employees > Employees

Form pegawai diperluas dengan beberapa tab berisi data induk kepegawaian:
identitas, keluarga, pendidikan, riwayat jabatan, diklat, sertifikasi,
penghargaan, organisasi, dan lain-lain. Isian tiap tab mengambil pilihan dari
**Configuration > Master Data Pegawai** yang dikelompokkan menjadi:

| Kelompok master | Contoh isi |
|---|---|
| Data Pribadi | Agama, Gelar Depan/Belakang, Golongan Darah, Status Nikah, Kewarganegaraan, Hubungan Keluarga |
| Jabatan & Organisasi | Jabatan, Jenjang Jabatan dan Benefit, Jenis Jenjang, Unit Organisasi, SK Organisasi, Profesi |
| Kategori & Jenis | Jenis Diklat, Sertifikat, Award, DPLK, Identitas, Karya Tulis, Pendidikan, Grievances, dan lainnya |
| Organisasi (SAP) | Company Code dan padanan struktur SAP |
| Master lain | Sekolah, Fakultas, Jurusan, Pekerjaan, Gelar, Status Wajib Pajak |

> Master harus diisi lebih dulu. Bila pilihan pada form pegawai kosong,
> penyebabnya hampir selalu master yang bersangkutan belum berisi.

### Print CV

Dari form pegawai, tombol **Print CV** mencetak daftar riwayat hidup berisi 31
butir data yang diambil dari tab-tab di atas. Data yang belum diisi akan
tercetak kosong, jadi lengkapi dulu tabnya sebelum mencetak.

---

## 4. Struktur Organisasi dan Mutasi Jabatan

### 4.1 Struktur Organisasi (formasi)

**Menu:** Employees > Struktur Organisasi

Berisi daftar formasi jabatan: satu baris satu kursi jabatan, lengkap dengan
unit organisasi dan jenjangnya. Tombol **Lekatkan Pegawai** mengisi formasi
dengan pegawai tertentu (muncul bila formasi punya ID formasi). Tombol pintas
**Lihat Pegawai** menampilkan siapa yang menempatinya.

**Import Struktur Organisasi** (Configuration > Master Data Pegawai >
Jabatan & Organisasi > Import Struktur Organisasi):

| # | Langkah | Tombol | Yang terjadi |
|---|---|---|---|
| 1 | Unggah berkas struktur | **Import** | Baris terbaca dan dicocokkan ke pegawai lewat NIPEG |
| 2 | Periksa hasil pencocokan | | Tiap baris bertanda: POG Sama, POG Terisi, POG Berubah, NIPEG Tidak Ditemukan, Pegawai Diarsipkan, Tertinggal |
| 3 | Bila hanya ingin mencocokkan ulang tanpa berkas baru | **Petakan Ulang (tanpa file)** | |
| 4 | Klik **Terapkan ke Pegawai** | Formasi tertulis ke data pegawai | |

### 4.2 Mutasi Jabatan dan SK

**Menu:** Employees > Mutasi Jabatan

Dokumen ini punya **dua status yang berjalan sendiri-sendiri** dan sering
tertukar:

| Status | Nilainya | Mengatur apa |
|---|---|---|
| **Status** | Draft / Active / Inactive | Jabatan mana yang sedang berlaku bagi pegawai |
| **Status SK** | Draft / Review / Approved/Signed / Effective | Alur persetujuan surat keputusannya |

Tiga jenis keputusan yang bisa dicetak:

| Jenis | Untuk kasus |
|---|---|
| Mutasi Jabatan | Mutasi internal PLN ES (promosi, rotasi, demosi) |
| Penetapan Jabatan | Tugas karya dari PLN ke PLN ES |
| Tugas Karya ke Entitas Lain | Penugasan pegawai PLN/PLN ES ke entitas lain |

Alur SK:

| # | Langkah | Tombol | Catatan |
|---|---|---|---|
| 1 | **New** -> pilih pegawai, jabatan baru, jenis keputusan, alasan/pertimbangan | | Isi Menimbang dan Mengingat bila dicetak |
| 2 | Klik **Ajukan Review** | Status SK: Review | |
| 3 | Klik **Approve/Sign** | Status SK: Approved/Signed | Approver mutasi jabatan |
| 4 | Klik **Cetak SK** atau **Preview SK** | PDF surat keputusan | **Baru bisa setelah Approved/Signed** |
| 5 | Klik **Jadikan Efektif** | Status SK: Effective | |
| 6 | Klik **Active** | Jabatan ini menjadi jabatan berlaku pegawai | Status (bukan Status SK) |

**Kembalikan ke Draft** tersedia selagi Status SK masih Review.

### 4.3 Uji Jabatan

**Menu:** Employees > Uji Jabatan
**Status:** Draft -> Terjadwal -> Lulus / Tidak Lulus

| # | Langkah | Tombol | Siapa |
|---|---|---|---|
| 1 | **New** -> pilih pegawai dan jabatan yang diuji | | HR / Penguji Jabatan |
| 2 | Klik **Jadwalkan** | Status Terjadwal | |
| 3 | Setelah pelaksanaan: **Lulus** atau **Tidak Lulus** | Hasil tercatat | Penguji Jabatan |
| 4 | Ada koreksi hasil: **Buka Kembali** | Kembali ke Terjadwal | |
| 5 | Batal: **Batalkan**, lalu **Kembalikan ke Draft** bila perlu | | |

> Uji jabatan bukan penilaian kinerja (Appraisal) dan tidak otomatis membuat
> mutasi. Hasilnya menjadi bahan pertimbangan, bukan pemicu.

---

## 5. Dokumen Kepegawaian

**Menu:** Employees > Dokumen

| Dokumen | Status | Tombol |
|---|---|---|
| Surat Peringatan (SP) | Draf -> Konfirmasi -> Selesai | Konfirmasi, Batalkan, **Preview**, **Print** |
| Surat Teguran (ST) | Draf -> Konfirmasi -> Selesai | sama |
| BAP Klarifikasi/Keterangan | Draf -> Konfirmasi | Konfirmasi, Batalkan, Preview, Print |
| Surat Keterangan | Draf -> Konfirmasi | Konfirmasi, Batalkan |
| Surat Paklaring | Draf -> Konfirmasi | Konfirmasi, Batalkan |
| Surat Penugasan | Draf -> Konfirmasi | Konfirmasi, Batalkan |
| Penanggungjawaban | Draf -> Konfirmasi | Konfirmasi, Batalkan |

Cara pakai umumnya sama:

| # | Langkah | Yang terlihat di layar |
|---|---|---|
| 1 | **New** -> pilih pegawai, isi perihal dan uraian | Nomor surat otomatis |
| 2 | Klik **Konfirmasi** | Surat sah, tombol Preview dan Print muncul |
| 3 | Klik **Preview** untuk memeriksa tampilan, **Print** untuk PDF | |

> **Surat Peringatan dan Surat Teguran** berpindah sendiri ke status
> **Selesai** ketika masa berlakunya habis - dijalankan penjadwal harian, tidak
> perlu ditutup manual.

### Monitoring LKS Bipartit

**Menu:** Employees > Hubungan Industrial > Monitoring LKS Bipartit

Notulen rapat Lembaga Kerja Sama Bipartit. Peserta dari pihak perusahaan
dipilih dari daftar pegawai, sedangkan peserta dari serikat pekerja diketik
bebas. Dokumen ini tidak punya statusbar - tersedia tombol **Preview** dan
**Print**.

### Evaluasi Kinerja Karyawan

**Menu:** Employees > Evaluasi > Evaluasi Kinerja Karyawan
Draft -> **Confirm** -> Confirm. Tersedia **Print Pdf** dan **Print Excel**.
Butir penilaian diambil dari master Evaluasi Kinerja Karyawan.

### PHK

**Menu:** Employees > PHK
Draft -> **Process** -> **Done**. Jenis PHK diambil dari master Jenis PHK.

---

## 6. Broadcast Email

**Menu:** Employees > Broadcast Email > Daftar Broadcast
**Status:** Draf -> Diantrikan -> Selesai / Gagal Sebagian

| # | Langkah | Tombol | Catatan |
|---|---|---|---|
| 1 | **New** -> tulis subjek dan isi, pilih penerima | Penerima dipilih dari daftar pegawai | |
| 2 | Klik **Kirim Uji ke Saya** | Email contoh masuk ke kotak surat sendiri | Selalu lakukan ini dulu |
| 3 | Klik **Kirim** | Status Diantrikan lalu Selesai | |
| 4 | Bila status **Gagal Sebagian** | Ada penerima yang emailnya gagal | Periksa alamat email pegawainya |

> Dua syarat yang harus dipenuhi lebih dulu: **email pegawai harus terisi**
> (kolom Work Email), dan **server email keluar harus sudah dikonfigurasi**.
> Tanpa keduanya, broadcast akan berstatus Gagal Sebagian atau tidak terkirim
> sama sekali.

---

## 7. Employee Self Service (ESS)

**Aplikasi:** Employee Self Service - untuk seluruh pegawai.

### 7.1 Pengajuan mandiri

Tiga jenis pengajuan memakai alur persetujuan yang sama persis:

| Pengajuan | Untuk apa |
|---|---|
| Attendance Request | Koreksi/pengajuan presensi |
| Permission Request | Izin (jenisnya dari master Permission Types) |
| Business Trip | Perjalanan dinas |

**Alur:** Draft -> Submitted -> Supervisor Review -> Manager Review ->
HR Review -> Approved -> Completed

| # | Langkah | Tombol | Siapa |
|---|---|---|---|
| 1 | **New** -> isi tanggal dan alasan | | Pegawai |
| 2 | Klik **Ajukan** | Status Submitted | Pegawai |
| 3 | Klik **Setujui (Atasan)** | Status Supervisor Review | Atasan langsung |
| 4 | Klik **Setujui (Manajer)** | Status Manager Review | Manajer |
| 5 | Klik **Setujui (HR)** lalu **Setujui** | Status Approved | HR Officer |
| 6 | Klik **Selesaikan** | Status Completed | |
| 7 | Menolak di tahap mana pun: **Tolak** | Wizard meminta alasan | |

**Batal** dan **Set ke Draft** tersedia untuk pengajuan yang belum selesai.

### 7.2 Biodata mandiri

**Menu:** Employee Self Service > Biodata

Pegawai bisa memperbarui datanya sendiri, tetapi hanya field tertentu yang
boleh diubah. Data pokok seperti NIPEG dan jabatan tetap milik HR.

> **Catatan penting:** menu **My Profile** bawaan Odoo masih membolehkan
> pegawai mengubah sebagian data yang di layar Biodata sudah dikunci.
> Gunakan menu **Biodata**, bukan My Profile.

### 7.3 Slip gaji dan presensi

- **Payslip Employee** - pegawai melihat slip gajinya sendiri, tombol
  **Cetak PDF** untuk mengunduh.
- **Attendance > Presensi** - data presensi; **Presensi Approval** untuk
  atasan.
- **Ijin > Daftar Ijin** - daftar izin yang diajukan.

---

## 8. Integrasi PLN Home

**Menu:** Integrasi PLN Home (Home API Manager)

Menerima data presensi, SPPD, dan cuti dari aplikasi PLN Home.

| Menu | Isinya |
|---|---|
| Event Presensi | Data presensi masuk dari PLN Home, termasuk titik GPS |
| Peta Record | Padanan record PLN Home dengan record Odoo |
| Log Kiriman | Riwayat semua kiriman, dipakai menelusuri data yang tidak masuk |
| Buat API Key | Wizard membuat kunci akses untuk PLN Home |

> **Untuk data cuti (time off) bisa masuk, HR harus mengisi kode PLN Home pada
> tiap Leave Type lebih dulu.** Tipe cuti yang kodenya kosong akan ditolak
> tanpa pesan yang jelas di sisi pengirim.

---

## 9. Konsolidasi PLN

**Menu:** Employees > Konsolidasi PLN

Wizard ekspor data pegawai untuk dilaporkan ke PLN. Pilih parameter yang
diminta lalu jalankan; hasilnya berkas yang bisa diunduh.

---

## 10. Bila Ada Masalah

| Gejala | Penyebab yang paling sering | Yang harus dilakukan |
|---|---|---|
| App Employees tidak muncul | Belum punya grup Employees bawaan | Beri `Employees / Employee` |
| Menu Configuration > Master Data Pegawai tidak ada | Bukan Employees / Administrator | Minta admin HR |
| Pilihan pada tab pegawai kosong | Master terkait belum diisi | Isi master di Configuration > Master Data Pegawai |
| Field NIPEG / data gaji tidak terlihat | Field dikunci per grup | Tidak bisa dibuka lewat ACL; user harus diberi grup pemegang field itu |
| Tombol **Cetak SK** tidak muncul | Status SK belum Approved/Signed | Jalankan Ajukan Review lalu Approve/Sign |
| Jabatan baru sudah di-SK tapi data pegawai belum berubah | **Status** (bukan Status SK) belum Active | Klik tombol **Active** |
| Import struktur organisasi tidak mengubah pegawai | Belum klik **Terapkan ke Pegawai** | Jalankan setelah memeriksa hasil pemetaan |
| Baris import bertanda "NIPEG Tidak Ditemukan" | NIPEG di berkas tidak ada di data pegawai | Perbaiki berkas atau lengkapi NIPEG pegawai |
| Broadcast berstatus Gagal Sebagian | Sebagian pegawai tidak punya Work Email | Lengkapi email pegawai |
| Broadcast tidak terkirim sama sekali | Server email keluar belum dikonfigurasi | Minta admin sistem mengatur SMTP |
| Pengajuan ESS mandek di Supervisor Review | Atasan langsung pegawai belum diisi | Isi field Manager di data pegawai |
| Presensi dari PLN Home tidak masuk | Kunci API belum dibuat, atau kode Home pada tipe cuti kosong | Cek Log Kiriman untuk penyebabnya |
| Pegawai bisa mengubah data yang seharusnya terkunci | Mereka memakai My Profile bawaan Odoo | Arahkan memakai menu **Biodata** |

---

[<- Buku 06: Anggaran, Aset & Sewa](06-anggaran-aset-sewa.md) |
[Daftar isi](README.md) |
[Buku 08: Payroll & SPPD ->](08-payroll-dan-sppd.md)
