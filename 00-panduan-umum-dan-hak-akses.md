# Buku 00 - Panduan Umum dan Hak Akses

Buku ini berlaku untuk **semua modul** PLN ES. Isinya hal-hal yang berulang di
mana-mana: cara masuk, cara membaca layar, cara kerja persetujuan berjenjang,
dan cara memberi hak akses kepada user. Buku 01 sampai 11 hanya membahas hal
yang khas modulnya masing-masing dan menunjuk balik ke sini.

---

## 1. Masuk ke Sistem

| # | Langkah | Catatan |
|---|---|---|
| 1 | Buka alamat Odoo PLN ES di peramban | Gunakan Google Chrome atau Microsoft Edge versi terbaru |
| 2 | Isi Email dan Password, klik **Log in** | Kredensial diberikan administrator |
| 3 | Ganti kata sandi pada login pertama | Klik nama Anda di kanan atas -> **My Profile** -> tab **Preferences** -> **Change Password** |

Lupa kata sandi: klik **Reset Password** di halaman login, atau hubungi
administrator.

Keluar: klik nama Anda di kanan atas -> **Log out**.

---

## 2. Mengenal Layar Odoo

### 2.1 Berpindah aplikasi

Klik ikon kotak-kotak di kiri atas untuk membuka daftar aplikasi. Aplikasi yang
terlihat **hanya yang hak aksesnya Anda miliki** - kalau sebuah app tidak
muncul, itu soal hak akses, bukan kerusakan.

Jalur menu di buku ini ditulis dengan tanda `>`, misalnya
**Captive Management > Contracts > Master Contracts**.

### 2.2 Tiga bentuk tampilan

| Bentuk | Untuk apa | Ikon |
|---|---|---|
| **List** | Melihat banyak baris sekaligus | garis-garis |
| **Kanban** | Melihat dokumen sebagai kartu per tahap | kotak-kotak |
| **Form** | Melihat dan mengubah satu dokumen | terbuka saat baris diklik |

Ikon pergantian tampilan ada di kanan atas.

### 2.3 Mencari dan menyaring

Kotak pencarian di atas daftar punya tiga bagian:

| Bagian | Kegunaan |
|---|---|
| **Filters** | Menyaring baris (mis. hanya yang berstatus Draft) |
| **Group By** | Mengelompokkan baris (mis. per bulan, per unit) |
| **Favorites** | Menyimpan kombinasi filter agar bisa dipakai lagi |

Menyimpan penyaringan: atur filter dan pengelompokan yang diinginkan ->
**Favorites** -> **Save current search** -> beri nama.

### 2.4 Mengunduh data

Dari tampilan list: centang baris yang diinginkan (atau centang kotak di
kepala kolom untuk semua) -> tombol **Actions** (roda gigi) -> **Export**.
Pilih field yang ingin diunduh, lalu **Export** ke XLSX atau CSV.

### 2.5 Chatter - catatan dan riwayat

Di bagian bawah form ada panel chatter:

| Bagian | Kegunaan |
|---|---|
| **Send message** | Pesan yang dikirimkan ke pengikut dokumen |
| **Log note** | Catatan internal, tidak dikirim sebagai email |
| **Activities** | Menjadwalkan tugas untuk diri sendiri atau orang lain |
| **Followers** | Siapa yang mendapat pemberitahuan |
| Riwayat | Semua perubahan status dan field penting tercatat di sini |

Bila timbul pertanyaan "siapa yang mengubah ini dan kapan", jawabannya hampir
selalu ada di chatter.

### 2.6 Mode developer

Beberapa hal di Odoo hanya terlihat bila mode developer menyala. **Untuk
pekerjaan sehari-hari mode ini sebaiknya dimatikan** supaya layar tidak
membingungkan. Satu hal penting yang terpengaruh mode ini dibahas di bagian
5.2.

---

## 3. Konvensi Dokumen PLN ES

Hampir semua dokumen di modul PLN ES mengikuti pola yang sama. Memahami pola
ini sekali membuat modul mana pun jadi mudah diikuti.

### 3.1 Statusbar

Di kanan atas form ada deretan status. Status yang sedang berlaku disorot.
Statusbar bukan sekadar hiasan: **tombol yang muncul di layar ditentukan oleh
status yang sedang berlaku.**

```
Draft  ->  Submitted  ->  Approved  ->  Active
              ^
        status saat ini
```

### 3.2 Tombol muncul dan hilang

Bila sebuah tombol yang Anda cari tidak ada, penyebabnya hanya tiga:

1. **Statusnya belum tepat** - misalnya tombol operasional kontrak baru muncul
   setelah kontrak berstatus Active.
2. **Hak akses Anda tidak mencakupnya** - misalnya tombol persetujuan hanya
   muncul untuk approver yang terdaftar.
3. **Ada syarat lain yang belum terpenuhi** - misalnya tombol **Ajukan** pada
   Due Diligence baru muncul setelah seluruh checklist terisi.

**Kebalikannya juga perlu diketahui: tombol yang terlihat belum tentu boleh
ditekan.** Sebagian besar tombol di modul PLN ES (sekitar 6 dari 7) hanya
disembunyikan menurut status dokumen, bukan menurut grup pengguna. Pemegang
peran "hanya melihat" tetap melihat tombol prosesnya, dan baru mendapat pesan
**AccessError** ketika tombolnya ditekan. Itu bukan kerusakan sistem,
melainkan hak akses yang memang hanya membaca.

### 3.3 Nomor dokumen

Nomor dokumen dibuat sistem dan tidak bisa diketik. Nomor baru muncul saat
dokumen pertama kali disimpan.

### 3.4 Wizard

Sebagian tombol tidak langsung menjalankan aksinya, melainkan membuka jendela
kecil (wizard) yang meminta keterangan tambahan - alasan penolakan, rentang
tanggal, berkas yang diunggah. Tekan tombol di dalam wizard untuk menyelesaikan
aksinya; menutup wizard berarti membatalkan.

### 3.5 Rollback dan prinsip empat mata

Beberapa modul menyediakan **Rollback** untuk mengembalikan dokumen ke tahap
sebelumnya. Pada dokumen bernilai penting, rollback dibuat **4-eyes**: satu
orang mengirim permintaan, orang lain yang mengeksekusi. Satu orang tidak bisa
melakukan keduanya. Ini kontrol yang disengaja, bukan kekurangan hak akses.

---

## 4. Persetujuan Berjenjang (Approval Matrix)

**Aplikasi:** Approval Matrix (modul `advance_approval_matrix`)

Sebagian besar dokumen PLN ES yang butuh persetujuan memakai satu mesin yang
sama. Memahaminya sekali cukup untuk semua modul.

### 4.1 Cara kerjanya

1. Dokumen mencapai status yang mensyaratkan persetujuan.
2. Sistem mencari **konfigurasi matriks** yang cocok untuk model dokumen itu.
3. Bila cocok, terbentuk **Approval Request** berisi baris-baris persetujuan
   sesuai tingkatannya.
4. Approver pada tingkat yang sedang aktif melihat tombol **Approve** dan
   **Reject** di dokumennya.
5. Setelah semua tingkat menyetujui, dokumen berpindah status.

**Status Approval Request:** Draft -> Pending Approval -> Approved / Rejected /
Cancelled.

### 4.2 Menu

| Menu | Isinya |
|---|---|
| Approval Matrix > Dashboard | Ringkasan persetujuan |
| Approval Matrix > Approval Requests > **My Requests** | Pengajuan yang Anda buat |
| Approval Matrix > Approval Requests > **Pending Approvals** | Yang menunggu persetujuan Anda |
| Approval Matrix > Approval Requests > All Requests | Semua (Approval Manager) |
| Approval Matrix > **My Delegations** | Pelimpahan wewenang Anda |
| Approval Matrix > Configuration | Konfigurasi (Approval Manager) |

> Kebiasaan yang disarankan bagi para approver: buka **Pending Approvals**
> setiap pagi. Dokumen yang menunggu persetujuan tidak selalu mengirim
> pemberitahuan.

### 4.3 Menyusun konfigurasi matriks

**Menu:** Approval Matrix > Configuration > Approval Configurations
(Approval Manager)

| # | Langkah | Isian |
|---|---|---|
| 1 | **New** -> beri nama, pilih **model dokumen** yang diatur | mis. `pln.captive.contract` |
| 2 | Tentukan batas nilai bila perlu | Minimum Amount, Maximum Amount, field nilai yang dipakai |
| 3 | Tambahkan **baris persetujuan** per tingkat | Sequence, nama tingkat |
| 4 | Pada tiap baris, isi **user** yang berwenang | Field Users |
| 5 | Isi jumlah persetujuan minimum per baris | Minimum Approval |
| 6 | Bila perlu syarat tambahan, isi **Criteria** | mis. hanya untuk unit tertentu |

**Tiga hal yang paling sering menjadi masalah:**

1. **Tanpa konfigurasi, persetujuan tidak berjalan sama sekali.** Dokumen yang
   modelnya belum punya konfigurasi matriks tidak akan memunculkan tombol
   persetujuan dan bisa mandek tanpa pesan kesalahan.
2. **Field Groups pada baris matriks hanya alat bantu pengisian.** Yang
   menentukan siapa boleh menyetujui adalah daftar **user** pada baris itu -
   bukan grupnya. Mengisi grup saja tidak cukup.
3. **Pembuat Approval Request memerlukan grup Approval Manager.** Bila sebuah
   dokumen gagal mengajukan persetujuan padahal konfigurasinya sudah ada,
   periksa hak akses pengajunya.

**Template** memudahkan pengulangan: tombol **Make Template** menyimpan
konfigurasi sebagai contoh, **Apply Template** memakainya untuk model lain, dan
**Duplicate** menggandakannya.

### 4.4 Pelimpahan wewenang (delegasi)

**Menu:** Approval Matrix > Configuration > User Delegations, atau
My Delegations

Dipakai bila approver sedang cuti atau dinas:

| # | Langkah | Isian |
|---|---|---|
| 1 | **New** -> pilih pemberi dan penerima wewenang | User, Delegate User |
| 2 | Tentukan masa berlakunya | From Date, To Date |
| 3 | Isi alasannya | Reason for Delegation |
| 4 | Klik **Activate** | Delegasi berlaku |
| 5 | Setelah kembali, klik **Deactivate** | Delegasi berhenti |

---

## 5. Mengatur Hak Akses User

**Menu:** Settings > Users & Companies > Users (khusus administrator)

### 5.1 Membuat user baru

| # | Langkah | Catatan |
|---|---|---|
| 1 | **New** -> isi Name dan Email Address | Email menjadi nama login |
| 2 | Buka tab **Access Rights** | Di sinilah semua hak akses diberikan |
| 3 | Pilih grup sesuai perannya | Lihat bagian 5.2 |
| 4 | Simpan | |
| 5 | Klik **Send Password Reset Instructions** | User menerima email untuk membuat kata sandinya |

### 5.2 Dua bentuk pemberian hak akses

Di tab **Access Rights**, hak akses muncul dalam dua bentuk berbeda dan
keduanya harus diperiksa:

**Bentuk pertama - daftar pilihan (dropdown).** Satu kategori, satu pilihan.
Contoh: kategori **Captive Role** berisi pilihan Captive Admin, Captive
Contract Officer, Captive Viewer, dan seterusnya. Kebanyakan grup PLN ES
berbentuk ini.

**Bentuk kedua - ceklis.** Beberapa modul memakai ceklis karena satu user bisa
memegang beberapa hak sekaligus.

> **Jebakan yang penting diketahui:** grup yang tidak punya kategori akan
> dirender Odoo sebagai ceklis di bagian bernama **"Extra Rights"**, dan
> **bagian itu hanya terlihat bila mode developer menyala**. Administrator yang
> tidak menyalakan mode developer tidak akan menemukannya.
>
> Modul yang terkena hal ini di PLN ES adalah **Bisnis Inovatif**. Karena itu
> `pln_bisnov` membuat kotak ceklisnya sendiri bernama **"Bisnis Inovatif"**
> di tab Access Rights, sehingga bisa diatur tanpa mode developer. Lihat
> [Buku 02](02-bisnis-inovatif-noncaptive.md).

### 5.3 Pewarisan grup

Banyak grup PLN ES **mewarisi** grup lain: memberi grup Manager berarti user
otomatis juga memegang hak Asman dan Staff di bawahnya. Kolom **Mewarisi** pada
Lampiran A menunjukkan pewarisan tiap grup.

Akibat praktisnya: **jangan mencentang grup bawahannya satu per satu.** Cukup
beri grup tertinggi yang sesuai perannya.

### 5.4 Grup modul PLN ES vs grup bawaan Odoo

Grup PLN ES seringkali **menumpang di atas** grup bawaan Odoo dan tidak bisa
berdiri sendiri. Contoh yang paling sering menjadi keluhan:

| Grup PLN ES | Juga membutuhkan |
|---|---|
| Finance / Staff Pajak | `Accounting / Billing` atau `Accounting / Read Only` |
| Peran HR mana pun | `Employees / Employee` |
| Peran payroll mana pun | grup Payroll bawaan Odoo |
| Peran SCM/pengadaan | grup Purchase bawaan Odoo |

Bila menu tidak muncul padahal grup PLN ES-nya sudah diberikan, periksa grup
bawaan Odoo-nya.

### 5.5 Batasan yang tidak bisa dibuka lewat pengaturan

Dua hal yang tidak bisa diatasi dengan menambah grup di layar Users:

1. **Field yang dikunci per grup.** Beberapa field (mis. NIPEG dan sebagian
   data pegawai) hanya terlihat bagi pemegang grup tertentu. Kunci ini melekat
   pada definisi field, bukan pada hak akses model, sehingga tidak bisa dibuka
   lewat pengaturan biasa.
2. **Record rule.** Sebagian modul membatasi *baris mana* yang terlihat -
   misalnya hanya dokumen unit sendiri. User bisa saja punya hak akses model
   penuh tetapi tetap melihat daftar kosong karena record rule menyaringnya.

Keduanya perlu penyesuaian di sisi program bila memang harus diubah.

---

## 6. Peta Aplikasi PLN ES

| Aplikasi | Isinya | Buku |
|---|---|---|
| Captive Management | Kontrak payung PLN, RAB, penagihan bulanan | [01](01-captive-management.md) |
| Bisnis Inovatif | Kontrak satuan non-PLN, ES & EV, pemasaran | [02](02-bisnis-inovatif-noncaptive.md) |
| Business Development | Kemitraan, inkubasi, investasi, anak perusahaan | [03](03-business-development.md) |
| SCM | Rantai pasok material | [04](04-pengadaan-scm-vendor.md) |
| Pengadaan | Purchase Agreement, Bank Garansi, Final Kontrak | [04](04-pengadaan-scm-vendor.md) |
| Purchase, Purchase Requests | Pengadaan dan dokumen vendor | [04](04-pengadaan-scm-vendor.md) |
| Finance PLN ES | Akuntansi, pajak, pembiayaan PLN ES | [05](05-finance-dan-pajak.md) |
| Tax Management | PPN, PPh, faktur pajak | [05](05-finance-dan-pajak.md) |
| Accounting / Invoicing | Akuntansi bawaan + Journal Workflow | [05](05-finance-dan-pajak.md) |
| Anggaran | SKK, rekomposisi, luncuran | [06](06-anggaran-aset-sewa.md) |
| Assets | Aset tetap dan penyusutan | [06](06-anggaran-aset-sewa.md) |
| Lease Management | Sewa IFRS 16 | [06](06-anggaran-aset-sewa.md) |
| Employees | Data induk pegawai, dokumen, organisasi | [07](07-hr-master-dokumen-pegawai.md) |
| Employee Self Service | Layanan mandiri pegawai | [07](07-hr-master-dokumen-pegawai.md) |
| Integrasi PLN Home | Presensi, SPPD, cuti dari PLN Home | [07](07-hr-master-dokumen-pegawai.md) |
| Payroll | Penggajian dan laporannya | [08](08-payroll-dan-sppd.md) |
| GRC | Risiko, hukum, SPI, tata kelola, mutu | [09](09-grc.md) |
| IT Service Management | Layanan TI | [10](10-itsm-helpdesk.md) |
| Helpdesk | Tiket umum | [10](10-itsm-helpdesk.md) |
| Strategy Planning (REN) | RJP, RKAP, Kontrak Manajemen, KPI | [11](11-strategy-planning-investment-pmo.md) |
| Investment Committee & PMO | Usulan investasi dan program | [11](11-strategy-planning-investment-pmo.md) |
| Approval Matrix | Mesin persetujuan berjenjang | Buku ini, bagian 4 |

---

## 7. Bila Ada Masalah - Umum

| Gejala | Penyebab yang paling sering | Yang harus dilakukan |
|---|---|---|
| Aplikasi tidak muncul di daftar app | Grup akses belum diberikan | Settings > Users > tab Access Rights |
| Menu Configuration sebuah modul tidak ada | Menu itu memang khusus admin modulnya | Minta administrator modul |
| Daftar kosong padahal datanya ada | Record rule membatasi ke unit sendiri, atau filter masih menyala | Hapus filter dulu; bila tetap kosong, ini soal record rule |
| Tombol yang dicari tidak ada | Status belum tepat / bukan approver / ada syarat yang belum terpenuhi | Lihat bagian 3.2 |
| Tombol Approve tidak muncul padahal status menunggu persetujuan | User tidak terdaftar di baris matriks | Approval Matrix > Configuration, periksa daftar user pada barisnya |
| Dokumen mandek, tidak ada tombol apa pun | Model dokumen belum punya konfigurasi matriks | Buat konfigurasi untuk model itu |
| Approver sedang cuti, dokumen menumpuk | Belum ada delegasi | Buat User Delegation |
| Field tertentu tidak terlihat | Field dikunci per grup | Perlu penyesuaian program, tidak bisa lewat pengaturan |
| Grup PLN ES sudah diberikan tapi menu tetap tidak muncul | Grup bawaan Odoo-nya belum ada | Lihat bagian 5.4 |
| Ada dua app dengan nama sama | Modul lama dan penggantinya masih terpasang bersama | Lihat [Buku 02](02-bisnis-inovatif-noncaptive.md) |
| Ingin tahu siapa mengubah dokumen | | Buka chatter di bagian bawah form |

---

## Lampiran A - Daftar Lengkap Grup Akses PLN ES

206 grup dari modul kustom PLN ES, dikelompokkan menurut kategori yang tampil
di **Settings > Users > tab Access Rights**. Kolom **Mewarisi** menunjukkan
grup lain yang ikut terbawa.

Daftar ini ditarik langsung dari basis data (`pln_es6`, `pln_mhc2`,
`pln_trial1`). Grup yang kategorinya kosong tampil sebagai ceklis - lihat
bagian 5.2.

### (tanpa kategori - tampil sebagai ceklis)

Modul: `account_parent`, `advance_approval_matrix`, `pln_bisnov`, `simplify_access_management`. Rincian di Buku -, 00, 02, 06.

| Grup | Mewarisi | XML ID |
|---|---|---|
| Show Chart of Account Structure | Administrator + Bookkeeper | `account_parent.group_coas_user` |
| Approval Administrator | Approval Manager | `advance_approval_matrix.group_approval_admin` |
| Approval Manager | Approval User | `advance_approval_matrix.group_approval_manager` |
| Approval User | - | `advance_approval_matrix.group_approval_user` |
| Directors | - | `advance_approval_matrix.group_directors` |
| Managers | - | `advance_approval_matrix.group_managers` |
| Supervisors | - | `advance_approval_matrix.group_supervisors` |
| Bisnov ES - Assman Create | Bisnov ES Pemikul - Create | `pln_bisnov.group_bisnov_es_assman_create` |
| Bisnov ES - Assman Delete | Bisnov ES Pemikul - Delete | `pln_bisnov.group_bisnov_es_assman_delete` |
| Bisnov ES - Assman Read | Bisnov ES Pemikul - Read | `pln_bisnov.group_bisnov_es_assman_read` |
| Bisnov ES - Assman Write | Bisnov ES Pemikul - Write | `pln_bisnov.group_bisnov_es_assman_write` |
| Bisnov ES - Manager Create | Bisnov ES Pemikul - Create | `pln_bisnov.group_bisnov_es_manager_create` |
| Bisnov ES - Manager Delete | Bisnov ES Pemikul - Delete | `pln_bisnov.group_bisnov_es_manager_delete` |
| Bisnov ES - Manager Read | Bisnov ES Pemikul - Read | `pln_bisnov.group_bisnov_es_manager_read` |
| Bisnov ES - Manager Write | Bisnov ES Pemikul - Write | `pln_bisnov.group_bisnov_es_manager_write` |
| Bisnov ES - Officer Create | Bisnov ES Pemikul - Create | `pln_bisnov.group_bisnov_es_officer_create` |
| Bisnov ES - Officer Delete | Bisnov ES Pemikul - Delete | `pln_bisnov.group_bisnov_es_officer_delete` |
| Bisnov ES - Officer Read | Bisnov ES Pemikul - Read | `pln_bisnov.group_bisnov_es_officer_read` |
| Bisnov ES - Officer Write | Bisnov ES Pemikul - Write | `pln_bisnov.group_bisnov_es_officer_write` |
| Bisnov ES Pemikul - Create | Role / User | `pln_bisnov.group_bisnov_es_create` |
| Bisnov ES Pemikul - Delete | Role / User | `pln_bisnov.group_bisnov_es_delete` |
| Bisnov ES Pemikul - Read | Role / User | `pln_bisnov.group_bisnov_es_read` |
| Bisnov ES Pemikul - Write | Role / User | `pln_bisnov.group_bisnov_es_write` |
| Bisnov EV - Assman Create | Bisnov EV Pemikul - Create | `pln_bisnov.group_bisnov_ev_assman_create` |
| Bisnov EV - Assman Delete | Bisnov EV Pemikul - Delete | `pln_bisnov.group_bisnov_ev_assman_delete` |
| Bisnov EV - Assman Read | Bisnov EV Pemikul - Read | `pln_bisnov.group_bisnov_ev_assman_read` |
| Bisnov EV - Assman Write | Bisnov EV Pemikul - Write | `pln_bisnov.group_bisnov_ev_assman_write` |
| Bisnov EV - Manager Create | Bisnov EV Pemikul - Create | `pln_bisnov.group_bisnov_ev_manager_create` |
| Bisnov EV - Manager Delete | Bisnov EV Pemikul - Delete | `pln_bisnov.group_bisnov_ev_manager_delete` |
| Bisnov EV - Manager Read | Bisnov EV Pemikul - Read | `pln_bisnov.group_bisnov_ev_manager_read` |
| Bisnov EV - Manager Write | Bisnov EV Pemikul - Write | `pln_bisnov.group_bisnov_ev_manager_write` |
| Bisnov EV - Officer Create | Bisnov EV Pemikul - Create | `pln_bisnov.group_bisnov_ev_officer_create` |
| Bisnov EV - Officer Delete | Bisnov EV Pemikul - Delete | `pln_bisnov.group_bisnov_ev_officer_delete` |
| Bisnov EV - Officer Read | Bisnov EV Pemikul - Read | `pln_bisnov.group_bisnov_ev_officer_read` |
| Bisnov EV - Officer Write | Bisnov EV Pemikul - Write | `pln_bisnov.group_bisnov_ev_officer_write` |
| Bisnov EV Pemikul - Create | Role / User | `pln_bisnov.group_bisnov_ev_create` |
| Bisnov EV Pemikul - Delete | Role / User | `pln_bisnov.group_bisnov_ev_delete` |
| Bisnov EV Pemikul - Read | Role / User | `pln_bisnov.group_bisnov_ev_read` |
| Bisnov EV Pemikul - Write | Role / User | `pln_bisnov.group_bisnov_ev_write` |
| Access Management | - | `simplify_access_management.group_access_management_bits` |

### Approval Mutasi Jabatan

Modul: `pln_hr_master`. Rincian di Buku 07.

| Grup | Mewarisi | XML ID |
|---|---|---|
| Approver | Role / User | `pln_hr_master.group_mutasi_jabatan_approver` |

### Bank Garansi

Modul: `pln_bank_garansi`. Rincian di Buku 04.

| Grup | Mewarisi | XML ID |
|---|---|---|
| Procurement Manager (BG) | Procurement User (BG) | `pln_bank_garansi.group_bg_manager` |
| Procurement Pusat (BG - Monitoring Lintas UP) | Procurement Manager (BG) | `pln_bank_garansi.group_bg_pusat` |
| Procurement User (BG) | User | `pln_bank_garansi.group_bg_user` |

### Bisnov Approval

Modul: `pln_bisnov`. Rincian di Buku 02.

| Grup | Mewarisi | XML ID |
|---|---|---|
| Bisnov Approver | Role / User + Approval User | `pln_bisnov.group_bisnov_approver` |

### Bisnov Role

Modul: `pln_bisnov`. Rincian di Buku 02.

| Grup | Mewarisi | XML ID |
|---|---|---|
| Bisnov Admin | Role / User | `pln_bisnov.group_bisnov_admin` |
| Bisnov Contract Officer | Role / User + Approval User | `pln_bisnov.group_bisnov_contract_officer` |
| Bisnov Finance Officer | Role / User + Approval User | `pln_bisnov.group_bisnov_finance_officer` |
| Bisnov Viewer | Role / User | `pln_bisnov.group_bisnov_viewer` |

### Broadcast Email

Modul: `pln_hr_broadcast`. Rincian di Buku 07.

| Grup | Mewarisi | XML ID |
|---|---|---|
| Administrator | Officer | `pln_hr_broadcast.group_broadcast_admin` |
| Officer | Role / User | `pln_hr_broadcast.group_broadcast_officer` |

### Captive Approval

Modul: `pln_captive_umbrella`. Rincian di Buku 01.

| Grup | Mewarisi | XML ID |
|---|---|---|
| Captive Approver | Role / User + Approval User | `pln_captive_umbrella.group_captive_approver` |

### Captive Role

Modul: `pln_captive_umbrella`. Rincian di Buku 01.

| Grup | Mewarisi | XML ID |
|---|---|---|
| Captive Admin | Role / User | `pln_captive_umbrella.group_captive_admin` |
| Captive Contract Officer | Role / User + Approval User | `pln_captive_umbrella.group_captive_contract_officer` |
| Captive Finance Officer | Role / User + Approval User | `pln_captive_umbrella.group_captive_finance_officer` |
| Captive Operations Officer | Role / User + Approval User | `pln_captive_umbrella.group_captive_operations_officer` |
| Captive Tax Officer | Role / User + Approval User | `pln_captive_umbrella.group_captive_tax_officer` |
| Captive Viewer | Role / User | `pln_captive_umbrella.group_captive_viewer` |

### Employee Self Service

Modul: `pln_hr_ess`. Rincian di Buku 07.

| Grup | Mewarisi | XML ID |
|---|---|---|
| Administrator | HR Officer | `pln_hr_ess.group_ess_admin` |
| Employee | Role / User | `pln_hr_ess.group_ess_employee` |
| HR Officer | Supervisor | `pln_hr_ess.group_ess_hr` |
| Supervisor | Employee | `pln_hr_ess.group_ess_supervisor` |

### Final Kontrak

Modul: `pln_final_kontrak`. Rincian di Buku 04.

| Grup | Mewarisi | XML ID |
|---|---|---|
| Final Kontrak Manager | Final Kontrak User | `pln_final_kontrak.group_fk_manager` |
| Final Kontrak User | User + Final Kontrak Viewer | `pln_final_kontrak.group_fk_user` |
| Final Kontrak Viewer | - | `pln_final_kontrak.group_fk_viewer` |

### Hubungan Industrial

Modul: `pln_hr_docs`. Rincian di Buku 07.

| Grup | Mewarisi | XML ID |
|---|---|---|
| Manager | User | `pln_hr_docs.group_hubungan_industrial_manager` |
| User | Role / User | `pln_hr_docs.group_hubungan_industrial_user` |

### Integrasi PLN Home

Modul: `pln_home_api`. Rincian di Buku 07.

| Grup | Mewarisi | XML ID |
|---|---|---|
| Home API Manager | Role / User | `pln_home_api.group_home_api_manager` |

### Lease Management

Modul: `ss_lease_accounting`. Rincian di Buku 06.

| Grup | Mewarisi | XML ID |
|---|---|---|
| Lease Manager | Administrator + Lease Officer | `ss_lease_accounting.group_ss_lease_manager` |
| Lease Officer | Lease User + Bookkeeper | `ss_lease_accounting.group_ss_lease_officer` |
| Lease User | Read-only | `ss_lease_accounting.group_ss_lease_user` |

### Loans

Modul: `dev_hr_loan`. Rincian di Buku 08.

| Grup | Mewarisi | XML ID |
|---|---|---|
| Administrator | Officer: Manage all employees | `dev_hr_loan.group_department_manager` |

### Material Request

Modul: `cr_material_purchase_requisitions`. Rincian di Buku 04.

| Grup | Mewarisi | XML ID |
|---|---|---|
| Admin Kapal | - | `cr_material_purchase_requisitions.admin_kapal` |
| Inventory Kapal | - | `cr_material_purchase_requisitions.inventory_kapal` |
| Material Request Manager | Material Request User | `cr_material_purchase_requisitions.manager_access` |
| Material Request User | - | `cr_material_purchase_requisitions.user_access` |
| Purchase Requisition Department Manager | - | `cr_material_purchase_requisitions.department_access` |
| Reallocation Reviewer | - | `cr_material_purchase_requisitions.ho_reviewer` |

### NCAP

Modul: `plnes_noncaptive`. Rincian di Buku 02.

| Grup | Mewarisi | XML ID |
|---|---|---|
| NCAP / Admin | NCAP / VP + NCAP / Manajer Pengembangan Produk & SCM + NCAP / Tim Pelaksana | `plnes_noncaptive.group_ncap_admin` |
| NCAP / Asisten Manajer | NCAP / Staff | `plnes_noncaptive.group_ncap_asman` |
| NCAP / Manajer | NCAP / Asisten Manajer | `plnes_noncaptive.group_ncap_manager` |
| NCAP / Manajer Pengembangan Produk & SCM | NCAP / Staff | `plnes_noncaptive.group_ncap_mgr_pscm` |
| NCAP / Read Only | - | `plnes_noncaptive.group_ncap_readonly` |
| NCAP / Staff | Role / User | `plnes_noncaptive.group_ncap_staff` |
| NCAP / Tim Pelaksana | Role / User | `plnes_noncaptive.group_ncap_tim_pelaksana` |
| NCAP / VP | NCAP / Manajer | `plnes_noncaptive.group_ncap_vp` |

### PLN Account Budget

Modul: `pln_account_budget`. Rincian di Buku 06.

| Grup | Mewarisi | XML ID |
|---|---|---|
| Budget Admin | Request Budget User + Request Budget Manager + Transfer Budget | `pln_account_budget.group_budget_admin` |
| Request Budget Manager | - | `pln_account_budget.group_request_budget_manager` |
| Request Budget User | - | `pln_account_budget.group_request_budget_user` |
| Transfer Budget | - | `pln_account_budget.group_transfer_budget` |

### PLN ES - MHC

Modul: `pln_payroll_es`. Rincian di Buku 08.

| Grup | Mewarisi | XML ID |
|---|---|---|
| Reporting MHC | - | `pln_payroll_es.group_mhc_reporting` |

### PLN ES - Payroll Approval

Modul: `pln_payroll_approval`. Rincian di Buku 08.

| Grup | Mewarisi | XML ID |
|---|---|---|
| LV1 - Officer Renumerasi | Approval User | `pln_payroll_approval.group_payroll_lv1_officer` |
| LV2 - Asisten Manager | LV1 - Officer Renumerasi | `pln_payroll_approval.group_payroll_lv2_asman` |
| LV3 - Manager | LV2 - Asisten Manager | `pln_payroll_approval.group_payroll_lv3_manager` |
| LV4 - VP HC | LV3 - Manager | `pln_payroll_approval.group_payroll_lv4_vphc` |

### PLN ES Business Development

Modul: `plnes_bdev`. Rincian di Buku 03.

| Grup | Mewarisi | XML ID |
|---|---|---|
| BDEV — Admin | Approval Manager + BDEV — Manager Pengembangan Bisnis dan Mutu + BDEV — Sekretaris Perusahaan | `plnes_bdev.group_bdev_admin` |
| BDEV — Asman Pengembangan Bisnis | BDEV — Staff Fungsi Partnership + BDEV — Staff Fungsi Pengembangan Bisnis | `plnes_bdev.group_bdev_asman` |
| BDEV — Direksi | BDEV — VP Perencanaan | `plnes_bdev.group_bdev_direksi` |
| BDEV — Direksi Anak Perusahaan | BDEV — Read Only (Auditor) | `plnes_bdev.group_bdev_direksi_ap` |
| BDEV — Direktur Utama | BDEV — Direksi | `plnes_bdev.group_bdev_dirut` |
| BDEV — Fungsi Hukum | Approval User + BDEV — Read Only (Auditor) | `plnes_bdev.group_bdev_fungsi_hukum` |
| BDEV — Manager Non-Captive | BDEV — Read Only (Auditor) | `plnes_bdev.group_bdev_manager_noncaptive` |
| BDEV — Manager Pengembangan Bisnis dan Mutu | BDEV — Asman Pengembangan Bisnis | `plnes_bdev.group_bdev_manager` |
| BDEV — Read Only (Auditor) | Role / User | `plnes_bdev.group_bdev_readonly` |
| BDEV — Sekretaris Perusahaan | BDEV — Read Only (Auditor) | `plnes_bdev.group_bdev_sekper` |
| BDEV — Staff Fungsi Partnership | Approval User + BDEV — Read Only (Auditor) | `plnes_bdev.group_bdev_staff_partnership` |
| BDEV — Staff Fungsi Pengembangan Bisnis | Approval User + BDEV — Read Only (Auditor) | `plnes_bdev.group_bdev_staff_dev` |
| BDEV — VP Niaga & Pemasaran | BDEV — Read Only (Auditor) | `plnes_bdev.group_bdev_vp_niaga` |
| BDEV — VP Perencanaan | BDEV — Manager Pengembangan Bisnis dan Mutu | `plnes_bdev.group_bdev_vp_perencanaan` |

### PLN ES — Finance

Modul: `plnes_finance`. Rincian di Buku 05.

| Grup | Mewarisi | XML ID |
|---|---|---|
| Finance / Administrator | Finance / Auditor + Finance / Direktur Keuangan + Finance / Manager Pembendaharaan | `plnes_finance.group_fin_admin` |
| Finance / Asman Akuntansi | Finance / Staff Akuntansi | `plnes_finance.group_fin_asman_akt` |
| Finance / Asman Pajak | Finance / Staff Pajak | `plnes_finance.group_fin_asman_pjk` |
| Finance / Auditor | - | `plnes_finance.group_fin_auditor` |
| Finance / Direktur Keuangan | Finance / Manager Akuntansi & Pajak | `plnes_finance.group_fin_direktur` |
| Finance / Manager Akuntansi & Pajak | Administrator + Finance / Asman Akuntansi + Finance / Asman Pajak | `plnes_finance.group_fin_manager_akt_pjk` |
| Finance / Manager Pembendaharaan | Administrator + Finance / Staff Akuntansi | `plnes_finance.group_fin_manager_treasury` |
| Finance / Staff Akuntansi | Bookkeeper | `plnes_finance.group_fin_staff_akt` |
| Finance / Staff Pajak | Bookkeeper | `plnes_finance.group_fin_staff_pjk` |

### PLN ES — GRC

Modul: `plnes_grc_hukum`, `plnes_grc_mutu`, `plnes_grc_risk`, `plnes_grc_spi`, `plnes_grc_tatakelola`. Rincian di Buku 09.

| Grup | Mewarisi | XML ID |
|---|---|---|
| GRC / Hukum — Asisten Manajer | GRC / Hukum — Staf | `plnes_grc_hukum.group_asman_hukum` |
| GRC / Hukum — BPO Eksternal (Requestor) | - | `plnes_grc_hukum.group_bpo_external` |
| GRC / Hukum — Manager | GRC / Hukum — Asisten Manajer | `plnes_grc_hukum.group_manager_hukum` |
| GRC / Hukum — Staf | - | `plnes_grc_hukum.group_staff_hukum` |
| GRC / Mutu — Sekretaris Perusahaan SKP | - | `plnes_grc_mutu.group_sek_per_skp` |
| GRC / Mutu — Unit Pelaksana Tindak Lanjut | - | `plnes_grc_mutu.group_unit_pelaksana_tindak_lanjut` |
| GRC / Administrator | GRC / Lini 2 — Risk & Compliance Oversight + GRC / WBS — Fungsi Kepatuhan | `plnes_grc_risk.group_grc_admin` |
| GRC / Lini 1 — Risk Owner | - | `plnes_grc_risk.group_risk_owner_lini1` |
| GRC / Lini 2 — Risk & Compliance Oversight | GRC / Lini 1 — Risk Owner | `plnes_grc_risk.group_risk_lini2` |
| GRC / Lini 3 — Independent Assurance | - | `plnes_grc_risk.group_risk_lini3` |
| GRC / Read-Only (Top Mgmt / External Audit) | - | `plnes_grc_risk.group_grc_readonly` |
| GRC / WBS — Bantuan Hukum | - | `plnes_grc_risk.group_wbs_bantuan_hukum` |
| GRC / WBS — Fungsi Kepatuhan | - | `plnes_grc_risk.group_wbs_kepatuhan` |
| GRC / WBS — HC Investigator | - | `plnes_grc_risk.group_wbs_hc_investigator` |
| GRC / WBS — Pengawasan Intern | - | `plnes_grc_risk.group_wbs_pengawasan_intern` |
| GRC / SPI — Auditor | - | `plnes_grc_spi.group_spi_auditor` |
| GRC / SPI — Direksi | - | `plnes_grc_spi.group_spi_direksi` |
| GRC / SPI — Kepala SPI | - | `plnes_grc_spi.group_spi_kepala` |
| GRC / TK — Asman Internal Direksi | - | `plnes_grc_tatakelola.group_asman_internal_direksi` |
| GRC / TK — Divisi Keuangan | - | `plnes_grc_tatakelola.group_div_keuangan_tk` |
| GRC / TK — Manager Tata Kelola | GRC / TK — Asman Internal Direksi | `plnes_grc_tatakelola.group_manager_tk` |
| GRC / TK — Sekretariat Direksi | - | `plnes_grc_tatakelola.group_sek_direksi` |
| GRC / TK — Sekretaris Perusahaan | GRC / TK — Manager Tata Kelola | `plnes_grc_tatakelola.group_sek_perusahaan` |

### PLN ES — ITSM

Modul: `plnes_itsm`. Rincian di Buku 10.

| Grup | Mewarisi | XML ID |
|---|---|---|
| ITSM / Admin | ITSM / Infrastructure Specialist + ITSM / Development Specialist + ITSM / Staff TI + ITSM / Manager STI + ITSM / Business Process Owner + ITSM / SOC Analyst + ITSM / Data Protection Officer + ITSM / Auditor | `plnes_itsm.group_itsm_admin` |
| ITSM / Asman Aplikasi TI | ITSM / IT Service Desk | `plnes_itsm.group_itsm_asman_aplikasi` |
| ITSM / Asman Infrastruktur TI | ITSM / IT Service Desk | `plnes_itsm.group_itsm_asman_infra` |
| ITSM / Auditor | ITSM / End User | `plnes_itsm.group_itsm_auditor` |
| ITSM / Business Process Owner | ITSM / End User | `plnes_itsm.group_itsm_bpo` |
| ITSM / Data Protection Officer | ITSM / End User | `plnes_itsm.group_itsm_dpo` |
| ITSM / Development Specialist | ITSM / IT Service Desk | `plnes_itsm.group_itsm_dev_spec` |
| ITSM / End User | Role / User | `plnes_itsm.group_itsm_user` |
| ITSM / IT Service Desk | ITSM / End User | `plnes_itsm.group_itsm_service_desk` |
| ITSM / Infrastructure Specialist | ITSM / IT Service Desk | `plnes_itsm.group_itsm_infra_spec` |
| ITSM / Manager STI | ITSM / Asman Aplikasi TI + ITSM / Asman Infrastruktur TI | `plnes_itsm.group_itsm_manager_sti` |
| ITSM / SOC Analyst | ITSM / IT Service Desk | `plnes_itsm.group_itsm_soc_analyst` |
| ITSM / Staff TI | ITSM / IT Service Desk | `plnes_itsm.group_itsm_staff_ti` |

### PLN ES — KSK

Modul: `plnes_investment_pmo`. Rincian di Buku 11.

| Grup | Mewarisi | XML ID |
|---|---|---|
| KSK / Admin | KSK PMO / Ketua PMO + KSK PMO / Business Process Owner + KSK INV / Anggota TVV + KSK INV / Board of Commissioners + KSK INV / Direksi + KSK INV / Divisi Keuangan + KSK INV / Holding Observer + KSK INV / Pengusul + KSK INV / Sekretaris Perusahaan + KSK INV / Sekretaris TVV + KSK INV / VP Sponsor + KSK PMO / Analis PMO + KSK PMO / Liaison Officer + KSK PMO / Sekretaris Direksi + KSK PMO / Steering Committee | `plnes_investment_pmo.group_ksk_admin` |
| KSK / Read Only | Role / User | `plnes_investment_pmo.group_ksk_readonly` |
| KSK INV / Anggota TVV | Role / User | `plnes_investment_pmo.group_inv_tvv_member` |
| KSK INV / Board of Commissioners | Role / User | `plnes_investment_pmo.group_inv_boc` |
| KSK INV / Direksi | Role / User | `plnes_investment_pmo.group_inv_direksi` |
| KSK INV / Divisi Keuangan | Role / User | `plnes_investment_pmo.group_inv_div_keu` |
| KSK INV / Holding Observer | Role / User | `plnes_investment_pmo.group_inv_holding_observer` |
| KSK INV / Pengusul | Role / User | `plnes_investment_pmo.group_inv_pengusul` |
| KSK INV / Sekretaris Perusahaan | Role / User | `plnes_investment_pmo.group_inv_sec_per` |
| KSK INV / Sekretaris TVV | Role / User | `plnes_investment_pmo.group_inv_sec_tvv` |
| KSK INV / VP Sponsor | Role / User | `plnes_investment_pmo.group_inv_vp_sponsor` |
| KSK PMO / Analis PMO | Role / User | `plnes_investment_pmo.group_pmo_analyst` |
| KSK PMO / Business Process Owner | Role / User | `plnes_investment_pmo.group_pmo_bpo` |
| KSK PMO / Ketua PMO | Role / User | `plnes_investment_pmo.group_pmo_ketua` |
| KSK PMO / Liaison Officer | Role / User | `plnes_investment_pmo.group_pmo_lo` |
| KSK PMO / Sekretaris Direksi | Role / User | `plnes_investment_pmo.group_pmo_sec_dir` |
| KSK PMO / Steering Committee | Role / User | `plnes_investment_pmo.group_pmo_sc` |

### PLN ES — REN

Modul: `plnes_strategy_planning`. Rincian di Buku 11.

| Grup | Mewarisi | XML ID |
|---|---|---|
| REN / Admin | REN / Direksi | `plnes_strategy_planning.group_ren_admin` |
| REN / Asman Perencanaan Korporat | REN / Staff Kinerja & Portofolio | `plnes_strategy_planning.group_ren_asman_pkp` |
| REN / Direksi | REN / VP Perencanaan | `plnes_strategy_planning.group_ren_direksi` |
| REN / Divisi Keuangan | Role / User | `plnes_strategy_planning.group_ren_div_keuangan` |
| REN / Divisi Pembina | Role / User | `plnes_strategy_planning.group_ren_div_pembina` |
| REN / Manager Perencanaan Korporat | REN / Asman Perencanaan Korporat | `plnes_strategy_planning.group_ren_manager_pkp` |
| REN / Manager Unit | Role / User | `plnes_strategy_planning.group_ren_manager_unit` |
| REN / PLN Pusat (Observer) | Role / User | `plnes_strategy_planning.group_ren_pln_pusat_observer` |
| REN / Read Only | Role / User | `plnes_strategy_planning.group_ren_readonly` |
| REN / Staff Kinerja & Portofolio | Role / User | `plnes_strategy_planning.group_ren_staff_kin` |
| REN / Staff Unit | Role / User | `plnes_strategy_planning.group_ren_staff_unit` |
| REN / VP Perencanaan | REN / Manager Perencanaan Korporat | `plnes_strategy_planning.group_ren_vp_perencanaan` |

### PLN ES — SCM

Modul: `pln_scm`. Rincian di Buku 04.

| Grup | Mewarisi | XML ID |
|---|---|---|
| Asman Operasi / Niaga (Approve Anggaran) | Role / User | `pln_scm.group_asman_terkait` |
| Asman SCM | Manager Unit Pelaksana | `pln_scm.group_asman_scm` |
| Auditor SCM (Read-Only) | Role / User | `pln_scm.group_auditor_scm` |
| Fungsi SCM Kantor Pusat | Asman SCM | `pln_scm.group_scm_kantor_pusat` |
| Manager Unit Pelaksana | PIC Logistik UP | `pln_scm.group_manager_up` |
| PIC Logistik UP | Role / User + User | `pln_scm.group_pic_logistik_up` |
| PIC Logistik Unit Layanan | Role / User | `pln_scm.group_pic_logistik_unit_layanan` |
| SCM Admin (4-eyes & Konfigurasi) | Fungsi SCM Kantor Pusat | `pln_scm.group_scm_admin` |
| Tim Rikmatek — Anggota | Role / User | `pln_scm.group_rikmatek_anggota` |
| Tim Rikmatek — Ketua I / II | Tim Rikmatek — Sekretaris | `pln_scm.group_rikmatek_ketua` |
| Tim Rikmatek — Sekretaris | Tim Rikmatek — Anggota | `pln_scm.group_rikmatek_sekretaris` |
| VP Operasi / Niaga | Role / User | `pln_scm.group_vp_operasi_niaga` |

### Pemasaran Non-Captive Role

Modul: `pln_bisnov_pemasaran`. Rincian di Buku 02.

| Grup | Mewarisi | XML ID |
|---|---|---|
| Pemasaran Non-Captive / Manager | Pemasaran Non-Captive / User | `pln_bisnov_pemasaran.group_pemasaran_manager` |
| Pemasaran Non-Captive / User | Role / User | `pln_bisnov_pemasaran.group_pemasaran_user` |

### Penilaian Vendor

Modul: `pln_vendor_evaluation`. Rincian di Buku 04.

| Grup | Mewarisi | XML ID |
|---|---|---|
| Auditor Penilaian Vendor | Role / User | `pln_vendor_evaluation.group_vendor_evaluation_auditor` |
| Evaluator Vendor | User | `pln_vendor_evaluation.group_vendor_evaluation_evaluator` |
| Manager Penilaian Vendor | Administrator + Evaluator Vendor | `pln_vendor_evaluation.group_vendor_evaluation_manager` |

### Purchase Request

Modul: `purchase_request`. Rincian di Buku 04.

| Grup | Mewarisi | XML ID |
|---|---|---|
| Purchase Request Manager | Purchase Request User | `purchase_request.group_purchase_request_manager` |
| Purchase Request User | Role / User | `purchase_request.group_purchase_request_user` |

### Realokasi Stok

Modul: `pln_material_request`. Rincian di Buku 01.

| Grup | Mewarisi | XML ID |
|---|---|---|
| Approver Realokasi Stok | - | `pln_material_request.group_scm_reservation_approver` |

### SPPD

Modul: `pln_sppd`. Rincian di Buku 08.

| Grup | Mewarisi | XML ID |
|---|---|---|
| SPPD User | Role / User | `pln_sppd.group_sppd_user` |

### Show Full Dashboard Features

Modul: `ks_dashboard_ninja`. Rincian di Buku -.

| Grup | Mewarisi | XML ID |
|---|---|---|
| Show Full Dashboard Features | - | `ks_dashboard_ninja.ks_dashboard_ninja_group_manager` |

### Uji Jabatan

Modul: `pln_hr_uji_jabatan`. Rincian di Buku 07.

| Grup | Mewarisi | XML ID |
|---|---|---|
| Penguji Jabatan | Role / User | `pln_hr_uji_jabatan.group_penguji_jabatan` |

---

[Daftar isi](README.md) | [Buku 01: Captive Management ->](01-captive-management.md)
