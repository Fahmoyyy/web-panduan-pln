# Buku 09 - GRC (Governance, Risk, Compliance)

**Modul:** `plnes_grc_risk`, `plnes_grc_hukum`, `plnes_grc_spi`, `plnes_grc_tatakelola`, `plnes_grc_mutu`
**Aplikasi:** GRC
**Peran utama:** Risk Owner, Risk & Compliance Oversight, Kepala SPI, Manager Hukum, Sekretaris Perusahaan

Satu aplikasi menaungi enam bidang: manajemen risiko, pengendalian internal,
whistleblowing, hukum, satuan pengawasan intern, tata kelola & kesekretariatan,
serta mutu & kepuasan pelanggan.

---

## 1. Peta Menu

```
GRC
├── Manajemen Risiko
│   ├── Risk Register · Assessment · Treatment Plan · Monitoring
├── Pengendalian Internal
│   ├── Process Library · Control Library · Testing · Deficiency & Remediation
├── Whistleblowing System
│   ├── Pengaduan · Investigasi · Perlindungan · Cheating House (KPK)
├── Hukum
│   ├── Reviu Dokumen · Legal Opinion · Nota Dinas Reviu · Sengketa
├── Satuan Pengawasan Intern
│   ├── PKPT · LHA · Temuan Audit · QAIP
├── Tata Kelola & Sekretariat
│   ├── RUPS · Rapat Direksi
│   ├── Petty Cash Direksi > Permohonan · Dropping · Realisasi
│   └── Konfigurasi > Petty Cash Config
├── Mutu & Kepuasan Pelanggan
│   ├── Rencana SKP · Pelaksanaan SKP · Hasil SKP · Tindak Lanjut
└── Master Data                                (GRC / Administrator)
    ├── Risk Taxonomy · Impact Scale · Likelihood Scale
    ├── Risk Appetite · WBS Channels
```

---

## 2. Peran dan Hak Akses

Kategori **PLN ES - GRC**. Perannya dibagi menurut model tiga lini pertahanan:

| Grup | Untuk siapa |
|---|---|
| GRC / Lini 1 - Risk Owner | Pemilik risiko di unit kerja |
| GRC / Lini 2 - Risk & Compliance Oversight | Pengawas risiko dan kepatuhan |
| GRC / Lini 3 - Independent Assurance | Penjaminan independen |
| GRC / Administrator | Administrator modul, satu-satunya pemegang Master Data |
| GRC / Read-Only (Top Mgmt / External Audit) | Manajemen puncak dan auditor eksternal |

**Whistleblowing** punya peran tersendiri karena kerahasiaannya:

| Grup | Untuk siapa |
|---|---|
| GRC / WBS - Fungsi Kepatuhan | Penerima dan penelaah pengaduan |
| GRC / WBS - HC Investigator | Investigator dari fungsi SDM |
| GRC / WBS - Pengawasan Intern | Investigator dari SPI |
| GRC / WBS - Bantuan Hukum | Pendampingan hukum |

**Bidang lain:**

| Grup | Bidang |
|---|---|
| GRC / Hukum - Staf, Asisten Manajer, Manager | Hukum |
| GRC / Hukum - BPO Eksternal (Requestor) | Pengaju reviu dokumen dari luar fungsi hukum |
| GRC / SPI - Auditor, Kepala SPI, Direksi | Satuan Pengawasan Intern |
| GRC / TK - Sekretariat Direksi, Asman Internal Direksi, Manager Tata Kelola, Sekretaris Perusahaan, Divisi Keuangan | Tata Kelola |
| GRC / Mutu - Sekretaris Perusahaan SKP, Unit Pelaksana Tindak Lanjut | Mutu & SKP |

> Data whistleblowing dibatasi per peran. Pengguna GRC biasa **tidak** melihat
> isi pengaduan; hanya peran WBS yang bisa.

---

## 3. Manajemen Risiko

### 3.1 Master data (GRC / Administrator)

Harus diisi lebih dulu, karena penilaian risiko mengambil skalanya dari sini:

| Master | Isinya |
|---|---|
| Risk Taxonomy | Pengelompokan jenis risiko |
| Impact Scale | Skala dampak (mis. 1-5 beserta artinya) |
| Likelihood Scale | Skala kemungkinan |
| Risk Appetite | Selera risiko organisasi (Draft -> Published -> Archived) |
| WBS Channels | Kanal penerimaan pengaduan |

### 3.2 Risk Register - daftar risiko

**Menu:** GRC > Manajemen Risiko > Risk Register
**Status:** Draft -> Teridentifikasi -> Dinilai -> Ditangani -> Dimonitor -> Ditutup / Dieskalasi

| # | Langkah | Tombol | Siapa |
|---|---|---|---|
| 1 | **New** -> tulis risikonya, pilih taksonomi dan unit pemilik | | Risk Owner |
| 2 | Klik **Identifikasi** | Status Teridentifikasi | Risk Owner |
| 3 | Buat **Assessment**, lalu klik **Nilai Risiko** | Status Dinilai | Nilai memakai Impact & Likelihood Scale |
| 4 | Buat **Treatment Plan**, klik **Tetapkan Treatment** | Status Ditangani | |
| 5 | Klik **Mulai Monitoring** | Status Dimonitor | Lini 2 |
| 6 | Klik **Tutup Risiko** atau **Eskalasi** | Selesai atau naik tingkat | |

**Treatment Plan** (Draft -> Diajukan -> Disetujui -> Dalam Proses -> Selesai)
punya tombol **Setujui**, **Mulai**, **Selesai**, **Batalkan**.

**Monitoring** (Draft -> Diajukan -> Direview) punya **Submit** dan **Review**.

### 3.3 Pengendalian Internal

| Menu | Isinya |
|---|---|
| Process Library | Daftar proses bisnis |
| Control Library | Daftar kontrol atas proses |
| Testing | Pengujian efektivitas kontrol |
| Deficiency & Remediation | Kelemahan yang ditemukan dan perbaikannya |

**Testing:** Draft -> **Mulai Testing** -> **Selesaikan** -> **Laporkan**.

**Deficiency:** Identified -> **Komunikasikan** -> **Rencanakan Remediasi** ->
**Mulai Remediasi** -> **Retest** -> **Tutup**.

### 3.4 Whistleblowing System

**Pengaduan** (GRC > Whistleblowing System > Pengaduan):

| # | Langkah | Tombol |
|---|---|---|
| 1 | Pengaduan masuk lewat kanal yang terdaftar | status Diterima |
| 2 | Klik **Tentukan Terlapor** | Terlapor Ditentukan |
| 3 | Klik **Selesai Telaah Awal** | Telaah Awal lalu Analisa & Evaluasi |
| 4 | Klik **Distribusikan** | Diteruskan ke fungsi yang berwenang |
| 5 | Klik **Mulai Investigasi** | Dokumen Investigasi dibuat |
| 6 | Klik **Tindak Lanjut** lalu **Tutup** | Perkara selesai |
| 7 | Bila perlu dilaporkan ke KPK: **Sync KPK** | Dokumen Cheating House |

**Perlindungan pelapor:** Draft -> **Minta Perlindungan** -> **Berikan
Perlindungan** -> **Tutup**.

**Cheating House (KPK):** Draft -> **Submit ke KPK** -> **Sync KPK** ->
**Tutup**.

---

## 4. Hukum

### 4.1 Reviu Dokumen

**Menu:** GRC > Hukum > Reviu Dokumen
Alur terpanjang di bidang hukum, dipakai fungsi lain yang butuh reviu hukum:

| # | Langkah | Tombol | Siapa |
|---|---|---|---|
| 1 | **New** -> lampirkan dokumen yang perlu direviu | | BPO Eksternal (Requestor) |
| 2 | Klik **Submit ke Legal** | Review Legal | Requestor |
| 3 | Klik **Mulai Pembahasan** | Pembahasan | Staf Hukum |
| 4 | Klik **Kirim ke Manager** | Review Manager Hukum | Staf/Asman |
| 5 | Klik **Setuju** atau **Tidak Setuju** | | Manager Hukum |
| 6 | Nota dinas disusun, klik **Approve Nota Dinas** | | |
| 7 | Klik **Distribusikan** lalu **Konfirmasi Diterima** | Selesai | |

### 4.2 Legal Opinion

Draft -> **Mulai Menyusun** -> **Kirim ke Manager** -> **Setujui** ->
**Distribusikan**.

### 4.3 Nota Dinas Reviu

Draft -> **Tandatangani** -> **Kirim** -> **Konfirmasi Diterima**.

### 4.4 Sengketa

Draft -> **Ajukan** -> **Setujui** -> **Mulai Proses** -> Penyelesaian ->
Putusan -> Ditutup. Dipakai memantau perkara hukum yang sedang berjalan.

---

## 5. Satuan Pengawasan Intern

| Dokumen | Alur |
|---|---|
| **PKPT** (program kerja audit tahunan) | Draft -> **Ajukan** -> **Setujui** -> **Mulai Pelaksanaan** -> **Selesaikan** |
| **LHA** (laporan hasil audit) | Draft -> **Mulai Fieldwork** -> **Susun Laporan** -> **Kirim ke Kepala SPI** -> **Distribusikan** -> **Tutup LHA** |
| **Temuan Audit** | Terbuka -> **Mulai Proses** -> **Tutup Temuan** |
| **QAIP** (penjaminan mutu audit) | Draft -> **Mulai Assessment** -> **Susun Laporan** -> **Setujui** -> **Tutup QAIP** |

Temuan Audit adalah rincian di bawah LHA; tiap temuan ditutup sendiri-sendiri
setelah unit terkait menindaklanjuti.

---

## 6. Tata Kelola dan Sekretariat

### 6.1 RUPS dan Rapat Direksi

| Dokumen | Alur |
|---|---|
| **RUPS** | Draft -> **Jadwalkan** -> **Materi Siap** -> **Terlaksana** -> **Tutup** |
| **Rapat Direksi** | Draft -> **Jadwalkan** -> **Terlaksana** -> **Susun MoM** -> **Finalkan MoM** -> **Distribusikan** |

### 6.2 Petty Cash Direksi

Alur paling panjang di modul ini karena menyangkut uang tunai. Tiga dokumen
saling terkait: **Permohonan**, **Dropping**, dan **Realisasi**.

**Permohonan** (satu dokumen, banyak tahap):

| # | Tahap | Tombol | Siapa |
|---|---|---|---|
| 1 | Pengajuan | **Ajukan** | Sekretariat Direksi |
| 2 | Verifikasi | **Verifikasi** | Asman Internal Direksi |
| 3 | Persetujuan | **Setujui (Sek Per)** | Sekretaris Perusahaan |
| 4 | Persetujuan keuangan | **Setujui (Div Keu)** | Divisi Keuangan |
| 5 | Minta pencairan | **Minta Dropping** | |
| 6 | Verifikasi dropping | **Verifikasi Dropping** | |
| 7 | Setujui dropping | **Setujui Dropping (Sek Per)** | |
| 8 | Selesaikan dropping | **Selesaikan Dropping** | |
| 9 | Cairkan dana | **Cairkan** | |
| 10 | Dana dipakai | **Gunakan** | |
| 11 | Laporkan pemakaian | **Laporkan** | |
| 12 | Tutup | **Tutup** | |

**Dropping** dan **Realisasi** adalah dokumen pendamping dengan alur lebih
pendek (Minta / Laporkan -> Verifikasi -> Setujui / Tutup).

Konfigurasi batas dan parameter ada di **Konfigurasi > Petty Cash Config**.

---

## 7. Mutu dan Kepuasan Pelanggan (SKP)

Siklus survei kepuasan pelanggan:

| Dokumen | Alur |
|---|---|
| **Rencana SKP** | Draft -> Disetujui -> Dilaksanakan -> Ditutup |
| **Pelaksanaan SKP** | Draft -> **Pilih Vendor** -> **Mulai Pelaksanaan** -> **Terima Hasil** |
| **Hasil SKP** | Draft -> Program Kerja Disusun -> Dikirim ke Unit Pelaksana -> Dalam Implementasi |
| **Tindak Lanjut** | Draft -> Diajukan -> Dievaluasi Sek Per |

Alurnya: rencana disusun -> survei dilaksanakan (biasanya oleh vendor) ->
hasilnya diterima dan diterjemahkan jadi program kerja -> unit pelaksana
menindaklanjuti -> Sekretaris Perusahaan mengevaluasi.

---

## 8. Bila Ada Masalah

| Gejala | Penyebab yang paling sering | Yang harus dilakukan |
|---|---|---|
| App GRC tidak muncul | Belum punya grup GRC apa pun | Beri minimal GRC / Read-Only |
| Menu Master Data tidak ada | Hanya untuk GRC / Administrator | Minta administrator GRC |
| Menu Whistleblowing terlihat tapi isinya kosong | Bukan pemegang peran WBS | Memang disengaja demi kerahasiaan |
| Tombol **Nilai Risiko** tidak muncul | Risk Register belum berstatus Teridentifikasi | Klik **Identifikasi** dulu |
| Nilai risiko tidak bisa diisi | Impact Scale / Likelihood Scale belum diisi | Lengkapi Master Data |
| Risk Appetite tidak terpakai | Masih berstatus Draft | Ubah ke Published |
| Reviu Dokumen mandek di Review Legal | Belum ada staf hukum yang menekan **Mulai Pembahasan** | Pastikan ada user bergrup GRC / Hukum - Staf |
| Petty Cash mandek | Salah satu dari 12 tahap belum ditekan | Lihat statusbar untuk mengetahui tahap dan siapa yang harus bertindak |
| Temuan audit tidak bisa ditutup padahal LHA sudah selesai | Temuan ditutup sendiri-sendiri | Buka tiap temuan lalu **Tutup Temuan** |
| Pengaduan tidak bisa didistribusikan | Telaah awal belum selesai | Jalankan **Selesai Telaah Awal** dulu |

---

[<- Buku 08: Payroll & SPPD](08-payroll-dan-sppd.md) |
[Daftar isi](README.md) |
[Buku 10: ITSM & Helpdesk ->](10-itsm-helpdesk.md)
