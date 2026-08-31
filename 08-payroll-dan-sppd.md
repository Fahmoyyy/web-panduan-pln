# Buku 08 - Payroll dan SPPD

**Modul:** `altech_payroll_indonesia` (mesin), `pln_payroll_es`, `pln_payroll_approval`, `pln_payroll_analytic`, `pln_payroll_loan`, `pln_hr_skpp`, `pln_hr_divisi`, `pln_sppd`, `pln_sppd_expense`, `pln_sppd_payroll`
**Aplikasi:** Payroll, Employee Self Service
**Peran utama:** Officer Renumerasi, Asisten Manager, Manager, VP HC

Mesin penggajiannya adalah `altech_payroll_indonesia`; `pln_payroll_es`
menambahkan tabel tarif dan laporan khas PLN ES di atasnya.

---

## 1. Peta Menu

```
Payroll
├── Pay Runs / Payslips                        (bawaan Odoo + altech)
├── Employees > Pinjaman · SKPP · Contract To Expired
├── Input Extras
│   ├── Input Income Adjustment · Adjustment Category
│   ├── Input Rutin · Kategori Rutin
│   └── Input Potongan BPRP
├── Tabel Nilai P1 · Tabel PhDP
├── Reporting
│   ├── Payroll Detail > Payroll Detail · Report BPMP to Coretax
│   ├── Report Transfer Gaji per Bank
│   ├── Summary BPJS
│   ├── Export BPA1 Tahunan
│   ├── PPh21-1721 Summary · PPh21-I
│   ├── Audit Dimensi Analitik
│   └── Export Jurnal ke SAP
└── Configuration
    ├── Konfigurasi PLN ES     7 tabel tarif khas PLN ES
    ├── Other Configuration    PTKP, lapis PPh, BPJS, cutoff
    └── Contract Configuration > Component

Employee Self Service > SPPD
├── Daftar SPPD · Master Tipe Input · Pemetaan Biaya SPPD
```

---

## 2. Peran dan Hak Akses

### Rantai persetujuan Pay Run (kategori **PLN ES - Payroll Approval**)

| Grup | Perannya |
|---|---|
| LV1 - Officer Renumerasi | Membuat Pay Run dan menjalankan Compute Sheet |
| LV2 - Asisten Manager | Persetujuan tingkat 2 |
| LV3 - Manager | Persetujuan tingkat 3 |
| LV4 - VP HC | Persetujuan terakhir |

Sifat rantai ini:

- **Tidak ada tombol Submit.** Rantai persetujuan dimulai sendiri begitu Pay
  Run dibuat oleh LV1.
- Tombol **Approve** dan **Tolak** ada di **tampilan kanban Pay Run**, bukan di
  form.
- **Compute Sheet hanya bisa dijalankan LV1.**
- Payslip tidak bisa divalidasi selama Pay Run induknya belum berstatus
  disetujui.

### Grup lain

| Grup | Modul | Bisa apa |
|---|---|---|
| Reporting MHC | pln_payroll_es | Membuka menu laporan payroll MHC |
| SPPD User | pln_sppd | Melihat dan mengelola SPPD |
| Payroll bawaan Odoo (Officer / Administrator) | hr_payroll | Prasyarat untuk membuka app Payroll |

---

## 3. Master dan Tabel Tarif

### Konfigurasi PLN ES (Payroll > Configuration > Konfigurasi PLN ES)

| Tabel | Isinya |
|---|---|
| Tabel Tarif POG (P2A1) | Tarif berdasarkan POG |
| Tabel Nilai P22A (Kota x POG) | Nilai per kombinasi kota dan POG |
| Tabel Tarif Jabatan (P21B) | Tarif menurut jabatan |
| Tabel Tarif Khusus (P23) | Tarif khusus |
| Tabel Nilai Pulsa (Jenis Jabatan x POG) | Tunjangan pulsa |
| Master Kota PLN | Daftar kota beserta klasifikasinya |
| Entitas Asal & Tarif Class | Asal pegawai dan kelas tarifnya |
| Mapping Agama -> Bulan THR | Bulan pembayaran THR menurut agama |

Di menu Payroll juga ada **Tabel Nilai P1** dan **Tabel PhDP**.

### Other Configuration (pengaturan pajak dan iuran)

PTKP, Kelompok PPh21, Lapis PPh Progresif, Limit Biaya Jabatan, Limit Tunjangan
BPJS Kesehatan, Limit Tunjangan Pensiun, Tabel Keselamatan Kerja, dan
**Cutoff Periode**.

> **Cutoff Periode wajib diisi lebih dulu.** Pay Run memakainya untuk menentukan
> rentang tanggal yang dihitung.

### Divisi dan dimensi analitik

**Menu:** Employees > Configuration > Master Data Pegawai > Jabatan &
Organisasi > **Divisi**, dan **Jenis Pegawai**.

Divisi pegawai menentukan pembagian biaya (analytic distribution) pada jurnal
payroll. Pemetaannya diatur di **Accounting > Configuration > Pemetaan Dimensi
Payroll**. Dari form Divisi ada tombol **Buka Analytic Plans**.

---

## 4. Menjalankan Penggajian

**Status Pay Run:** Ready -> Done -> Paid
**Status Payslip:** Draft -> Validated -> Paid

| # | Langkah | Tombol / Menu | Siapa |
|---|---|---|---|
| 1 | Buat Pay Run baru, pilih Cutoff Periode | **New** di Payroll > Pay Runs | LV1 |
| 2 | Rantai persetujuan berjalan otomatis | tampil di kanban | - |
| 3 | Setujui berjenjang | **Approve** di kanban Pay Run | LV2 -> LV3 -> LV4 |
| 4 | Hitung slip | **Compute Sheet** | LV1 |
| 5 | Periksa hasil, validasi payslip | | |
| 6 | Kirim slip ke pegawai | **Kirim Slip Gaji ke Semua** (Pay Run status Done) | |
| 7 | Kirim ulang untuk satu orang | **Kirim Slip Ini** di payslip (status Validated/Paid) | |
| 8 | Buat daftar transfer bank | **Report Transfer Gaji per Bank** di Pay Run | |

> Bila **Compute Sheet** menolak dengan pesan tentang status persetujuan,
> artinya Pay Run belum disetujui sampai LV4. Ini disengaja.

### Input tambahan sebelum menghitung

| Menu | Untuk apa |
|---|---|
| Input Income Adjustment | Tambahan penghasilan tidak rutin (insentif, dll) |
| Input Rutin | Potongan/tambahan rutin |
| Input Potongan BPRP | Cicilan pinjaman BPRP |

**Pinjaman** (Payroll > Employees > Pinjaman), status Draft -> Progress -> Done:

| # | Langkah | Tombol |
|---|---|---|
| 1 | **New** -> isi pokok pinjaman, tenor, dan pegawainya | |
| 2 | Klik **Generate Jadwal** | Jadwal cicilan terbentuk (baris Open/Done/Close) |
| 3 | Klik **Confirm** | Status Progress, cicilan mulai dipotong lewat payroll |
| 4 | Setelah lunas: **Set Selesai** | Status Done |

> **Income Adjustment hanya masuk ke slip bila kategorinya terdaftar di aturan
> gaji.** Kategori di luar daftar itu akan tercatat tetapi bernilai nol rupiah
> tanpa pesan kesalahan. Periksa kategori sebelum memakainya.

---

## 5. Laporan Payroll

| Laporan | Isinya | Cara pakai |
|---|---|---|
| **Payroll Detail** | Rincian komponen gaji seluruh pegawai | Pilih periode -> **Download** |
| **Summary BPJS** | Rekap iuran BPJS | Pilih periode -> **Download** |
| **Report Transfer Gaji per Bank** | Daftar transfer dikelompokkan per bank | **Preview** dulu, lalu **Download Excel** |
| **Export BPA1 Tahunan** | Bukti potong PPh21 tahunan (A1) | Pilih tahun -> **Download** |
| **Report BPMP to Coretax** | Bukti potong PPh21 format Coretax DJP | Pilih periode -> **Download** |
| **PPh21-1721 Summary** dan **PPh21-I** | Laporan SPT PPh21 | Wizard bawaan altech |
| **Audit Dimensi Analitik** | Memeriksa jurnal payroll yang dimensinya kosong/salah | **Jalankan** |
| **Export Jurnal ke SAP** | Berkas jurnal untuk diunggah ke SAP | **Buat Berkas** |

> **Rekening bank pegawai tersimpan di dua tempat** di Odoo. Bila ada pegawai
> yang tidak muncul di Report Transfer Gaji per Bank, periksa apakah
> rekeningnya diisi di tempat yang dibaca laporan ini, bukan hanya di form
> kontak.

---

## 6. SKPP

**Menu:** Payroll > Employees > SKPP

Surat Keterangan Penghentian Pembayaran untuk pegawai yang berhenti.

| # | Langkah | Tombol |
|---|---|---|
| 1 | **New** -> pilih pegawai dan tanggal berhenti | |
| 2 | Klik **Ambil Ulang Data** | Data gaji terakhir ditarik dari payroll |
| 3 | Klik **Cetak SKPP** | PDF surat |

Tombol **Ambil Ulang Data** dipakai lagi bila data payroll berubah setelah SKPP
dibuat.

---

## 7. SPPD (Perjalanan Dinas)

**Menu:** Employee Self Service > SPPD > Daftar SPPD

SPPD **tidak dibuat di Odoo**. Dokumennya datang dari aplikasi **PLN Home**
lewat integrasi (lihat [Buku 07](07-hr-master-dokumen-pegawai.md) bagian
Integrasi PLN Home). Di Odoo, SPPD dipakai untuk dua hal: melihat datanya, dan
mengubah biayanya menjadi komponen penghasilan di payroll.

Isi dokumen SPPD: nomor, perihal, tujuan dan dasar perjalanan, jenis SPPD,
beban anggaran/SKKO, tanggal berangkat dan pulang, kota berangkat dan tujuan
(beserta kota tarifnya), daftar peserta, serta total biaya. Kolom **Status di
HOME** menampilkan status dokumen di aplikasi asalnya.

### Dari SPPD ke slip gaji

| Menu | Fungsinya |
|---|---|
| **Master Tipe Input** | Daftar jenis input SPPD |
| **Pemetaan Biaya SPPD** | Menentukan biaya SPPD jenis apa masuk ke komponen penghasilan mana |

Biaya SPPD yang sudah dipetakan akan muncul sebagai Income Adjustment pada
periode payroll yang bersangkutan.

> Sama seperti Income Adjustment biasa: **kategori yang tidak terdaftar di
> aturan gaji akan bernilai nol rupiah tanpa pesan kesalahan.** Setelah
> membuat pemetaan baru, selalu periksa hasilnya di slip percobaan.

---

## 8. Bila Ada Masalah

| Gejala | Penyebab yang paling sering | Yang harus dilakukan |
|---|---|---|
| App Payroll tidak muncul | Grup Payroll bawaan Odoo belum diberikan | Beri grup Payroll Officer/Administrator |
| Tidak ada tombol Submit di Pay Run | Memang tidak ada - rantai mulai otomatis | Cukup tunggu approver menekan Approve di kanban |
| Tombol Approve tidak terlihat | Sedang membuka form, bukan kanban; atau bukan approver tingkat itu | Buka tampilan kanban Pay Run |
| **Compute Sheet** ditolak | Pay Run belum disetujui sampai LV4, atau user bukan LV1 | Selesaikan persetujuan; jalankan sebagai LV1 |
| Payslip tidak bisa divalidasi | Pay Run induknya belum disetujui | Sama seperti di atas |
| Pay Run tidak bisa dibuat, periode tidak tersedia | Cutoff Periode belum diisi | Payroll > Configuration > Other Configuration > Cutoff Periode |
| Income Adjustment tercatat tapi nilainya nol | Kategorinya tidak terdaftar di aturan gaji | Pakai kategori yang sudah terdaftar, atau minta aturan gaji ditambah |
| Biaya SPPD tidak muncul di slip | Pemetaan Biaya SPPD belum dibuat, atau kategorinya di luar daftar aturan gaji | Periksa Pemetaan Biaya SPPD |
| SPPD tidak ada di Odoo | SPPD dibuat di PLN Home, bukan di Odoo | Cek Log Kiriman di Integrasi PLN Home |
| Pegawai tidak muncul di Report Transfer Gaji per Bank | Rekening bank diisi di tempat yang salah | Isi rekening di data pegawai yang dibaca laporan |
| Tunjangan tertentu nol untuk sebagian pegawai | Baris tabel tarif untuk kombinasi kota/POG/jabatannya belum ada | Lengkapi tabel di Konfigurasi PLN ES |
| Jurnal payroll tanpa dimensi analitik | Divisi pegawai kosong atau pemetaannya belum dibuat | Jalankan **Audit Dimensi Analitik** untuk melihat daftarnya |

---

[<- Buku 07: HR Master & Dokumen Pegawai](07-hr-master-dokumen-pegawai.md) |
[Daftar isi](README.md) |
[Buku 09: GRC ->](09-grc.md)
