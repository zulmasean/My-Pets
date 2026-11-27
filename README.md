# Tutorial Lengkap Pengguna Edarin

Dokumen ini menjelaskan cara memakai Edarin dari nol: fungsi setiap peran, alur kerja dari administrator sampai sales, langkah operasional harian, serta aturan bisnis yang mudah salah diterapkan.

Edarin terdiri dari dua aplikasi:

| Aplikasi | Dipakai di | Untuk siapa | Fungsi utama |
| --- | --- | --- | --- |
| **Edarin Console (backoffice)** | Browser desktop/laptop | Administrator platform, administrator distributor, finance pusat, admin keuangan area, gudang, supervisor, manajemen | Mengelola master data, pengguna, pembelian, stok, keuangan, laporan, dan pemantauan |
| **Edarin Mobile (PWA)** | Browser HP (dapat dipasang ke layar utama) | Sales, team leader, supervisor, manajemen, admin keuangan area, gudang | Absensi, kunjungan, kasir/penjualan lapangan, piutang, stok gudang, kas area, pemantauan tim |

Keduanya memakai akun yang sama. Backoffice dan PWA adalah dua aplikasi terpisah; PWA berkomunikasi ke server backoffice melalui API.

---

## Daftar Isi

1. [Sebelum mulai](#1-sebelum-mulai)
2. [Peran dan fungsinya](#2-peran-dan-fungsinya)
3. [Alur besar dari nol](#3-alur-besar-dari-nol)
4. [Tutorial per peran](#4-tutorial-per-peran)
5. [Skenario satu hari operasional](#5-skenario-satu-hari-operasional)
6. [Aturan bisnis yang wajib dipahami](#6-aturan-bisnis-yang-wajib-dipahami)
7. [Matriks akses menu](#7-matriks-akses-menu)
8. [Checklist onboarding](#8-checklist-onboarding)
9. [Troubleshooting dan FAQ](#9-troubleshooting-dan-faq)

---

## 1. Sebelum mulai

### 1.1 Login dan akun

- Login hanya di satu pintu: halaman `/login`.
- Identitas login: **email atau nomor HP** (format Indonesia dinormalisasi, mis. `0812...` menjadi `+62812...`) ditambah kata sandi yang sama.
- **Tidak ada registrasi mandiri dan tidak ada “lupa kata sandi”.** Semua akun dibuat dan dikelola administrator. Jika pengguna lupa kata sandi, administrator mengganti kata sandi dari menu pengguna.
- Kata sandi baru minimal **12 karakter**, harus memuat huruf besar, huruf kecil, angka, dan simbol.
- 2FA (autentikasi dua faktor) dapat diaktifkan pengguna sendiri dari **Pengaturan → Keamanan**.
- Akun yang dinonaktifkan langsung kehilangan sesi aktif.

### 1.2 Area aktif

Data Edarin dipisahkan per **area** (gudang atau pusat).

- Peran yang melihat semua area: **administrator platform, administrator distributor, finance pusat, manajemen**. Mereka tidak memilih area aktif.
- Peran operasional (gudang, admin keuangan area, supervisor, team leader, sales) hanya melihat **area yang ditugaskan** dan bekerja pada **satu area aktif**.
- Jika pengguna punya lebih dari satu area, pemilih **Area aktif** muncul di header backoffice. Di PWA, area dipilih dari menu akun/beranda.
- Data yang tampil, transaksi yang bisa dibuat, dan stok yang diakses selalu mengikuti area aktif.

### 1.3 Istilah penting

| Istilah | Arti |
| --- | --- |
| **Tenant / distributor** | Satu perusahaan distributor yang memakai Edarin. Data antar-distributor terpisah. |
| **Pusat** | Area bertipe `central`. Hanya ada satu per distributor. Pusat tidak menyimpan kas fisik dan hanya menjual ke subdistributor. |
| **Gudang / area** | Area bertipe `warehouse`. Bisa beberapa, masing-masing mencakup satu atau beberapa kabupaten/kota. |
| **Kanvas** | Stok yang sudah diambil sales dari gudang dan berada di tangan sales. |
| **Pickup / pengambilan sales** | Serah terima barang dari gudang ke sales. Dibuat gudang, dikonfirmasi sales di PWA. |
| **Nota / faktur** | Dokumen penjualan. Nota tunai langsung lunas; nota tempo menimbulkan piutang. |
| **Sampel** | Barang uji coba; punya stok, tidak dijual normal. |
| **Bonus** | Promosi “beli X gratis Y” pada penjualan. |
| **Media promosi** | Barang nonjual (stiker, poster, spanduk) yang dipasang di outlet. |
| **Wajib disetor** | Bagian kas area yang belum disisihkan sebagai kas kecil dan harus disetor ke pusat. |

---

## 2. Peran dan fungsinya

| Peran | Label di aplikasi | Lingkup data | Fungsi utama |
| --- | --- | --- | --- |
| `platform_admin` | Administrator platform | Semua distributor (bukan data operasional) | Membuat/menonaktifkan distributor dan administrator distributor |
| `tenant_admin` | Administrator distributor | Semua area dalam satu distributor | Mengelola master data, pengguna & tim, pembelian, seluruh keuangan, opname, laporan |
| `central_finance` | Finance pusat | Semua area | Purchase order, pembayaran, kas, konfirmasi setoran area, pengeluaran, laporan; ikut mengelola master data |
| `area_finance` | Admin keuangan area | Area yang ditugaskan | Kas area, setoran sales, pengajuan setoran ke pusat, pengeluaran, kas kecil, piutang, outlet |
| `warehouse` | Gudang | Area yang ditugaskan | Seluruh mutasi stok: penerimaan, pickup, retur sales, sampel, waste, transfer, opname baca |
| `supervisor` | Supervisor area | Area yang ditugaskan | Memantau tim & aktivitas lapangan; dapat mencatat penjualan dan transaksi keuangan area |
| `team_leader` | Team leader | Area yang ditugaskan | Menjual untuk dirinya dan sales binaannya, mengatur cakupan kecamatan tim, memantau tim |
| `sales` | Sales | Area yang ditugaskan | Absensi, kunjungan, kasir/penjualan kanvas, piutang sendiri |
| `management` | Manajemen | Semua area | Analisis penjualan, stok, keuangan, aktivitas, laba rugi; menetapkan target penjualan |

### 2.1 Siapa mengerjakan apa (ringkas)

- **Administrator platform** menyiapkan distributor dan akun administrator distributor.
- **Administrator distributor** menyiapkan seluruh fondasi: master data, area, outlet, pengguna, struktur atasan-bawahan, dan cakupan kecamatan sales.
- **Finance pusat / tenant admin** membuat dan menyetujui purchase order; gudang menerima barangnya.
- **Gudang** menyerahkan stok ke sales (pickup), menerima retur, mencatat waste/sampel/transfer, dan mengoreksi selisih kasir.
- **Sales** menjual dari stok kanvas, mencatat kunjungan dan pemasangan media, menutup kasir, lalu menyerahkan uang/barang.
- **Admin keuangan area** menerima setoran sales, mencatat pengeluaran dan pembayaran piutang, mengajukan setoran area ke pusat.
- **Supervisor dan team leader** memastikan aktivitas lapangan berjalan: kehadiran, kunjungan, cakupan wilayah, dan kinerja penjualan.
- **Manajemen** membaca hasil dan menetapkan target penjualan.

---

## 3. Alur besar dari nol

```text
[1] ADMINISTRATOR PLATFORM
    buat distributor + area Pusat + akun administrator distributor
            |
            v
[2] ADMINISTRATOR DISTRIBUTOR
    master data -> area & gudang -> outlet -> pengguna & tim -> cakupan kecamatan
            |
            v
[3] FINANCE PUSAT / TENANT ADMIN          [4] GUDANG
    buat PO -> setujui PO  --------------->  terima PO / penerimaan barang
                                                    |
                                                    v
                                             stok gudang bertambah
                                                    |
                                                    v
[5] GUDANG  ---- pickup (foto serah terima) ---->  [6] SALES (PWA)
                                                    konfirmasi pickup -> buka kasir
                                                    absen -> kunjungan -> jualan
                                                    tutup kasir -> retur otomatis
                                                    |
                                                    v
[7] GUDANG  ---- hitung & konfirmasi retur ---->  stok kembali, absensi sales ditutup
                                                    |
                                                    v
[8] ADMIN KEUANGAN AREA
    catat pembayaran piutang, terima setoran sales, catat pengeluaran
    ajukan setoran area -> pusat  --->  [9] TENANT ADMIN / FINANCE PUSAT konfirmasi
                                                    |
                                                    v
[10] SUPERVISOR / TEAM LEADER / MANAJEMEN
     pantau kehadiran, kunjungan, peta, penjualan, piutang, laba rugi
```

Alur uang dan stok selalu bersambung:

- **Stok:** pemasok → gudang → sales (pickup) → outlet (penjualan) atau kembali ke gudang (retur).
- **Uang:** outlet → sales (tunai) atau piutang → pembayaran → setoran sales ke kas area → setoran area ke pusat (transfer bank).

---

## 4. Tutorial per peran

## 4.1 Administrator platform

Akun administrator platform dibuat di server (bukan lewat aplikasi), misalnya:

```bash
docker compose exec app php artisan edarin:platform-admin \
  --name="Administrator Utama" \
  --email=admin@perusahaan.id \
  --phone=08123456789 \
  --password="KataSandiKuat123!"
```

### Langkah kerja

1. Login di `/login` memakai email/HP dan kata sandi.
2. Buka **Distributor** (menu samping).
3. Klik tombol tambah, lalu isi:
   - Nama distributor.
   - Slug (opsional; dibuat otomatis dari nama jika kosong).
   - Nama area pusat pertama (default `Pusat`).
   - Nama, email, nomor HP administrator distributor.
   - Kata sandi administrator (minimal 12 karakter, campuran huruf besar/kecil, angka, simbol) dan konfirmasinya.
4. Simpan. Sistem otomatis membuat distributor, area Pusat, dan akun administrator distributor dalam satu langkah.
5. Sampaikan kredensial administrator distributor melalui kanal yang aman.

### Yang perlu diketahui

- **Ringkasan platform** hanya menampilkan agregat: jumlah distributor aktif/tertunda/provisioning, jumlah pengguna, jumlah administrator distributor, dan distributor baru bulan ini.
- Administrator platform **tidak membuka data operasional** (penjualan, stok, keuangan distributor).
- Status distributor bisa **ditangguhkan** dan **diaktifkan kembali**. Saat ditangguhkan, pengguna distributor tidak dapat mengakses aplikasi.
- Distributor yang masih berstatus *provisioning* tidak bisa diaktifkan/ditangguhkan sampai proses provisioning selesai.

---

## 4.2 Administrator distributor

Ini peran paling penting saat onboarding. Kerjakan berurutan.

### A. Lengkapi profil distributor

1. Klik menu pengguna di kanan bawah → **Pengaturan**.
2. Buka **Distributor**: nama, telepon, email, alamat, dan logo.
3. Buka **Profil** untuk data akun sendiri, dan **Keamanan** untuk mengganti kata sandi atau mengaktifkan 2FA.

### B. Siapkan master data (urutan yang disarankan)

Menu: **Master data**.

1. **Kategori barang** dan **Jenis barang**
   - Kategori dan beberapa jenis bawaan sudah tersedia saat distributor dibuat.
   - Tambah jika perlu. Jenis selalu berada di dalam satu kategori.

2. **Merek**
   - Kode, nama, gambar (opsional), status aktif.

3. **Barang / Produk**
   - SKU, nama, merek, kategori, jenis.
   - Satuan dasar (mis. pcs).
   - Satuan kemasan dan faktornya (mis. slop = 10 pcs, karton = 800 pcs).
   - Empat harga jual: **warung**, **subdistributor**, **grosir**, **konsumen**.
   - Harga pokok (HPP).
   - Tipe produk: reguler, **sampel**, atau **media promosi**. Sampel dan media promosi tidak dapat dijual normal.
   - Jenis media promosi bila tipe produk media promosi.
   - Status aktif dan gambar.

4. **Area & gudang**
   - Kode, nama, jenis area (**Gudang** atau **Pusat**), dan kabupaten/kota yang dicakup.
   - Satu gudang boleh mencakup beberapa kabupaten/kota.
   - Pusat hanya satu dan tidak menyimpan kas fisik.

5. **Outlet / Pelanggan**
   - Kode, nama, jenis pelanggan: **Warung** atau **Grosir**.
   - Alamat, telepon, area, kabupaten/kota, kecamatan.
   - Koordinat (untuk peta dan pencarian rute).
   - **Batas kredit** dan **tempo (hari)**. Keduanya dipakai saat penjualan tempo.
   - Subdistributor dikelola di menu terpisah **Subdistributor**, bukan di menu Outlet.

6. **Subdistributor**
   - Kode, nama, alamat, telepon, batas kredit, tempo, status.
   - Penjualan subdistributor hanya dibukukan dari area **Pusat**.

7. **Pemasok**
   - Kode, nama, telepon, alamat; dipakai saat membuat purchase order dan penerimaan.

8. **Bonus**
   - Nama program, area (kosong = semua area), kategori pelanggan sasaran.
   - Produk yang harus dibeli dan minimal pembelian.
   - Produk bonus dan jumlah maksimalnya.
   - Berlaku kelipatan, sekali per outlet, periode mulai/selesai.

9. **Program diskon**
   - Area, kategori pelanggan, produk, per satuan, diskon maksimal per satuan, periode.

10. **Kategori biaya**
    - Kategori pengeluaran operasional (mis. BBM, sewa, konsumsi).

> Tips: matikan status “Aktif” untuk data yang tidak dipakai, jangan menghapus. Data nonaktif tidak muncul di transaksi baru tetapi riwayat lama tetap utuh.

### C. Buat pengguna dan struktur tim

Menu: **Master data → Pengguna & tim**.

1. Klik tambah pengguna.
2. Isi:
   - Nama, email, nomor HP (opsional).
   - **Peran**.
   - Kata sandi dan konfirmasi (minimal 12 karakter, campuran huruf besar/kecil, angka, simbol).
   - Foto profil (opsional).
3. Tentukan **area penugasan**:
   - Peran operasional wajib punya minimal satu area.
   - Peran pusat (administrator distributor, finance pusat, manajemen) tidak perlu area karena melihat semua area.
4. Tentukan **atasan** bila relevan:
   - **Team leader** berada di bawah **supervisor**.
   - **Sales** boleh berada di bawah **team leader** atau **supervisor**.
   - Atasan harus memiliki minimal satu area yang sama dengan bawahannya.
   - Pengguna tidak boleh menjadi atasan dirinya sendiri.
5. Tentukan **cakupan kecamatan** untuk sales dan team leader:
   - Hanya kecamatan yang berada di area yang ditugaskan.
   - Kabupaten/kota harus termasuk cakupan area.
   - Kecamatan yang sama tidak boleh dipilih dua kali dalam satu area.
6. Simpan. Akun langsung aktif dan email dianggap terverifikasi. Sampaikan kredensial lewat kanal aman dan minta pengguna mengganti kata sandi.

### D. Kelola pengguna

- Cari berdasarkan nama/email/HP; filter peran, area, dan atasan.
- **Ubah peran/area/atasan** lewat form edit. Perubahan pada bawahan harus menjaga relasi atasan tetap valid.
- **Aktif/nonaktif cepat** dan **ubah peran cepat** tersedia dari daftar.
- Mengganti kata sandi pengguna akan memutus semua sesi pengguna tersebut.
- Pengaman penting:
  - Administrator tidak dapat menurunkan peran dirinya sendiri.
  - Administrator terakhir tidak dapat diturunkan atau dinonaktifkan.
  - Administrator tidak dapat menonaktifkan akunnya sendiri.
  - Perubahan area tidak boleh menghilangkan area bersama antara atasan dan bawahan.

### E. Tugas administrator distributor lainnya

- **Stok opname**: menu **Persediaan → Stok opname**. Alasan wajib diisi.
- **Purchase order**: membuat dan menyetujui PO (lihat bagian finance pusat).
- **Seluruh keuangan**: semua area, termasuk konfirmasi setoran area ke pusat.
- **Laporan**: semua laporan, termasuk **Laba Rugi**.
- **Ringkasan**: menetapkan **target penjualan** (bersama manajemen dan finance pusat).
- **Pengaturan distributor**: profil, logo, dan data perusahaan.

---

## 4.3 Finance pusat

Finance pusat melihat semua area dan bertanggung jawab pada pembelian serta pengawasan keuangan pusat.

### A. Membuat purchase order (PO)

1. Buka **Keuangan → Purchase order**.
2. Buat PO baru:
   - Pemasok.
   - Tanggal pesan dan perkiraan tanggal datang.
   - Catatan (opsional).
   - Baris produk: produk, jumlah, satuan, harga satuan.
3. Simpan sebagai **draft**.
4. Setujui PO. Setelah disetujui, gudang dapat menerima barang.
5. PO yang belum diterima dapat dibatalkan. PO yang sudah diterima tidak dapat dibatalkan.

### B. Menerima PO

Finance pusat (dan tenant admin) juga dapat menerima PO. Langkahnya sama dengan gudang:

1. Buka **Keuangan → Purchase order**, pilih PO berstatus disetujui.
2. Klik terima, isi tanggal terima dan jumlah yang benar-benar diterima per baris.
3. Jika ada barang rusak/kurang, isi jumlah **waste** atau **retur** dan tulis alasannya.
4. Simpan. Stok gudang bertambah sesuai jumlah bersih.

### C. Keuangan dan kas

- **Piutang**: melihat seluruh tagihan, umur piutang, dan detail per outlet.
- **Pembayaran**: mencatat pembayaran dari outlet. Alokasi otomatis ke nota tertua (FIFO). Pembayaran tidak boleh melebihi sisa piutang.
- **Kas**:
  - Menerima/mencatat **setoran sales** (langsung masuk kas area).
  - Mencatat **setoran area ke pusat** yang langsung diterima (karena dilakukan finance pusat/tenant admin).
  - **Mengonfirmasi** pengajuan setoran area dari admin keuangan area/supervisor.
  - Menambah/mengurangi kas kecil area.
- **Pengeluaran**: finance pusat dan tenant admin tidak memegang kas area; pengeluaran yang mereka catat otomatis bersumber **Lainnya** dan hanya menjadi catatan biaya, tidak mengurangi kas area/kas kecil.
- **PO** dan **penerimaan PO** seperti di atas.

### D. Laporan dan master data

- Laporan penjualan, stok, umur piutang, kas, pengeluaran, laba rugi, kehadiran, coverage kunjungan, media promosi. Tersedia ekspor.
- Finance pusat termasuk penulis master data (barang, merek, outlet, bonus, diskon, pemasok, kategori biaya) dan pengelola outlet.

---

## 4.4 Admin keuangan area

Admin keuangan area bekerja pada area aktif yang ditugaskan.

### A. Menerima setoran sales

1. Buka **Keuangan → Kas**.
2. Klik **Setoran sales**.
3. Pilih sales/team leader, isi jumlah uang yang diserahkan dan catatan.
4. Simpan. Setoran **langsung diterima**: kas area bertambah, uang yang dipegang sales berkurang.

Catatan: sales tidak menyetor sendiri lewat aplikasi. Setoran dicatat admin keuangan area setelah uang fisik diterima.

### B. Mengajukan setoran area ke pusat

1. Di **Kas**, klik **Setoran area ke pusat**.
2. Pilih area sumber (harus area gudang), isi jumlah, tanggal, referensi bank, catatan.
3. Simpan. Pengajuan berstatus **menunggu konfirmasi** finance pusat/tenant admin.
4. Anda hanya dapat membatalkan pengajuan Anda sendiri yang belum dikonfirmasi.
5. Saat dikonfirmasi, kas fisik area berkurang. Pusat tidak mencatat kas fisik masuk karena setoran dilakukan via transfer bank.

Batas setoran: tidak boleh melebihi kas area yang wajib disetor (kas area dikurangi saldo kas kecil).

### C. Menerima pembayaran piutang

1. Buka **Keuangan → Piutang** atau **Pembayaran**.
2. Pilih outlet, isi jumlah, metode (**tunai** atau **transfer**), tanggal bayar, dan referensi.
3. Simpan. Pembayaran dialokasikan FIFO ke nota paling lama.

### D. Mencatat pengeluaran

1. Buka **Keuangan → Pengeluaran**.
2. Isi kategori biaya, deskripsi, jumlah, tanggal, dan **sumber dana**:
   - **Kas area**: kas area berkurang; wajib disetor ikut turun.
   - **Kas kecil**: saldo kas kecil dan kas fisik turun; wajib disetor **tidak berubah**.
   - **Lainnya**: hanya dicatat sebagai biaya; tidak mengubah kas.
3. Simpan.

### E. Mengelola kas kecil

Di **Kas** tersedia **Tambah kas kecil** dan **Kurangi kas kecil**:

- **Tambah**: menyisihkan kas area yang belum disisihkan menjadi kas kecil. Maksimal sebesar kas area dikurangi saldo kas kecil saat ini. Wajib disetor turun.
- **Kurangi**: mengembalikan kas kecil ke kas area. Maksimal sebesar saldo kas kecil. Wajib disetor naik.
- Semua perubahan tercatat sebagai riwayat di halaman Kas dan PWA.

### F. Outlet dan master data

- Admin keuangan area dapat **membuat dan memperbarui outlet** (termasuk dari PWA) untuk area yang ditugaskan.
- Master data lain dapat dilihat, tetapi penulisan produk/pemasok/bonus/diskon berada pada tenant admin dan finance pusat.

---

## 4.5 Gudang

Gudang mengelola seluruh pergerakan stok pada area aktif. Tidak ada akses penjualan dan keuangan.

### A. Menerima barang

Dua jalur:

1. **Penerimaan PO**
   - Menu **Gudang → Penerimaan PO**.
   - Pilih PO yang sudah disetujui, isi tanggal terima dan jumlah aktual.
   - Isi waste/retur bila ada, sertakan alasan.
   - Simpan: stok gudang bertambah, PO selesai.

2. **Penerimaan barang tanpa PO**
   - Menu **Pergerakan stok → Penerimaan barang**.
   - Pilih pemasok (opsional), tambah barang dan jumlah, lalu simpan.

### B. Menyerahkan stok ke sales (pickup)

1. Menu **Pergerakan stok → Pengambilan sales**.
2. Pilih sales penerima.
3. Tambah barang dan jumlah.
4. Unggah **foto serah terima** (wajib).
5. Simpan. Stok gudang direservasi dan status menunggu konfirmasi sales.

Sales mengonfirmasi pickup di PWA sebelum kasir dibuka. Sales tidak dapat membuat pickup sendiri, dan pickup tidak dapat dibuat jika sales masih memiliki sesi kasir aktif.

### C. Menerima retur stok sales

Retur terbentuk otomatis saat sales menutup kasir.

1. Buka **Pergerakan stok → Retur stok sales** (atau **Tugas gudang** di PWA gudang).
2. Periksa jumlah yang dilaporkan sales, lalu lakukan hitungan fisik.
3. Bila ada selisih, koreksi jumlah. Sistem menampilkan dampak selisih (kompensasi/tanggungan atau waste) sebelum konfirmasi.
4. Konfirmasi penerimaan. Stok gudang diperbarui dan absensi sales otomatis ditutup.

Setelah retur dikonfirmasi, sales tidak dapat lagi mengubah hitungan.

### D. Operasi stok lainnya

- **Pengeluaran sampel**: pilih outlet dan alasan; mengurangi stok sampel (gudang atau stok sales, sesuai pemegang).
- **Waste**: barang rusak/kedaluwarsa; alasan wajib.
- **Konversi ke sampel**: mengubah barang reguler menjadi sampel; alasan wajib.
- **Kirim transfer**: kirim stok ke gudang/pusat lain. Stok keluar dari area asal dan berstatus transit.
- **Terima transfer**: penerima membuka **Persediaan → Transit & transfer** dan mengonfirmasi penerimaan.
- **Stok opname**: hanya administrator distributor. Alasan wajib; saldo disesuaikan.
- **Batalkan dokumen**: membuat dokumen pembalik. Dokumen asli tetap tersimpan untuk jejak audit. Dapat dilakukan tenant admin, finance pusat, gudang, dan supervisor.

### E. Memantau stok

- **Persediaan → Saldo stok**: saldo per barang, per area, dan per pemegang (gudang atau sales).
- **Dokumen stok**: riwayat semua dokumen mutasi.
- **Jurnal stok**: jejak pergerakan per dokumen.
- **Laporan → Stok & sampel**: rekap stok dan sampel.

### F. PWA gudang

- **Beranda gudang**: ringkasan stok dan tugas.
- **Stok gudang**: daftar saldo dan jurnal per barang.
- **Tugas gudang**: penerimaan, pickup (dengan foto), daftar retur, hitung dan konfirmasi retur, serta bukti foto dokumen.
- Absensi mandiri tersedia untuk pengguna gudang dari PWA.

---

## 4.6 Sales

Sales bekerja terutama di **Edarin Mobile (PWA)**.

### A. Persiapan

1. Buka tautan PWA yang diberikan admin di browser HP, lalu **Tambahkan ke Layar Utama**.
2. Login dengan email/HP dan kata sandi.
3. Pilih area aktif bila lebih dari satu.
4. Aktifkan izin lokasi dan kamera; keduanya dipakai untuk absensi, kunjungan, dan bukti pengambilan.

### B. Absen masuk (wajib sebelum kasir)

1. Di Beranda, lakukan **absen masuk**.
2. Ambil foto dan izinkan lokasi.
3. Absen hanya sekali per hari. Absen masuk wajib dan harus masih aktif (belum clock out) sebelum kasir dapat dibuka.

### C. Konfirmasi pickup dan buka kasir

1. Gudang membuat pickup; pickup muncul di layar **Kasir**.
2. Periksa barang dan jumlah yang diserahkan.
3. Tekan konfirmasi pickup. Stok pindah menjadi stok kanvas (di tangan sales) dan sesi kasir terbuka.
4. Satu sales hanya memiliki satu sesi kasir aktif. Sesi terikat pada perangkat yang membukanya; perangkat lain dapat mengambil alih bila diperlukan.

### D. Mencatat kunjungan dan aktivitas

Menu **Aktivitas**:

- **Kunjungan**: pilih outlet, pilih hasil kunjungan (order, tanpa order, tutup, follow up), isi catatan, ambil foto + lokasi.
- **Pemasangan media promosi**: pilih outlet dan jenis media, ambil foto + lokasi. Stok media sales berkurang.
- **Pengeluaran sampel**: berikan sampel ke outlet; stok sampel sales berkurang.
- **Riwayat visit** dan **rute aktivitas** untuk melihat kembali aktivitas hari itu.

Catatan: “order” pada kunjungan menandai bahwa kunjungan menghasilkan penjualan. Nota penjualan tetap dibuat dari Kasir/Penjualan. Kunjungan ber-order dihitung sebagai kunjungan, bukan event terpisah.

### E. Menjual di POS

1. Buka **Kasir**.
2. Pilih pelanggan: outlet, grosir, atau konsumen. Subdistributor tidak dijual oleh sales.
3. Pilih barang. Hanya barang dari pickup sesi aktif yang dapat dijual.
4. Harga mengikuti jenis pelanggan (warung/grosir/konsumen). Sales tidak dapat mengganti kategori harga.
5. Pilih **tunai** atau **tempo**:
   - Tunai: nota langsung lunas; uang tunai dipegang sales sampai disetor.
   - Tempo: dibatasi **batas kredit** dan **tempo (hari)** outlet. Nota melebihi limit ditolak.
6. Bonus: pilih bonus yang tersedia sesuai program dan jumlah minimal pembelian.
7. Diskon/harga khusus: boleh diubah, tetapi **alasan negosiasi wajib diisi**.
8. Simpan. Nota terbentuk dan stok sales berkurang.

### F. Bekerja tanpa sinyal (offline)

- Setelah PWA pernah dibuka online dan data kasir tersinkron, penjualan dapat dilakukan offline.
- Transaksi tersimpan di HP meskipun aplikasi ditutup, lalu disinkronkan saat internet kembali.
- Jangan menghapus data aplikasi/browser atau meng-uninstall PWA sebelum semua transaksi tersinkron.
- Jika harga/diskon berubah di server, transaksi berkonflik tidak diposting otomatis; buka antrean, tinjau penawaran terbaru, lalu kirim ulang secara manual.

### G. Menutup kasir

1. Di layar Kasir, tekan **Hitung sisa & tutup kasir**.
2. Hitung sisa barang per item.
3. Jika ada selisih, sistem menampilkan pratinjau kompensasi; konfirmasi terkunci sampai hitungan tersimpan.
4. Kirim. Status menjadi **menunggu gudang** dan pengajuan retur stok otomatis dibuat.
5. Gudang menghitung dan mengonfirmasi. Setelah dikonfirmasi, stok gudang diperbarui dan **absensi sales otomatis clock out**.

Sales tidak dapat check-out manual selama sesi kasir masih aktif.

### H. Piutang dan setoran

- **Piutang** di PWA menampilkan tagihan berjalan per outlet (read-only).
- Pembayaran tunai/transfer dicatat admin keuangan area (atau sales melalui backoffice jika diizinkan).
- Uang tunai hasil penjualan/tagihan dipegang sales sampai diserahkan dan dicatat sebagai **Setoran sales** oleh admin keuangan area.

### I. Yang bisa dibuka sales di backoffice

Sales dapat login ke backoffice untuk melihat **Penjualan**, **Piutang saya**, **Kas saya**, stok kanvas, status pickup/retur, aktivitas lapangan, dan laporan. Penulisan transaksi lapangan tetap diutamakan melalui PWA.

---

## 4.7 Team leader

Team leader adalah peran tersendiri di bawah supervisor. Dua tanggung jawab utamanya: menjual dan membina sales langsung.

### A. Menjual

- Memakai alur sales yang sama di PWA: absen, konfirmasi pickup, buka kasir, jualan, tutup kasir.
- Penjualan selalu tercatat **atas nama team leader sendiri**.
- Outlet yang boleh dilayani mencakup cakupan kecamatan **dirinya sendiri dan sales langsungnya**. Jadi team leader dapat melayani wilayah binaannya, tetapi notanya tetap atas namanya.

### B. Mengatur cakupan tim (PWA)

1. Buka **Tim & wilayah**.
2. Pilih sales binaan.
3. Atur kecamatan yang menjadi cakupannya pada area aktif.

Aturan:

- Hanya kecamatan resmi di dalam kabupaten/kota area aktif.
- Sales harus sudah ditugaskan ke area tersebut; jika belum, minta administrator menambahkan area.
- Team leader hanya dapat mengatur cakupan dirinya dan sales langsungnya.
- Supervisor dapat melihat tim tetapi tidak mengubah cakupan.

### C. Memantau

- **Tim & wilayah**: daftar sales langsung, aktivitas terakhir, dan kinerja.
- **Statistik**: ringkasan penjualan dan aktivitas tim.
- **Absensi**: kehadiran sales langsung.
- **Rute aktivitas**: titik kunjungan/order tim pada peta.

### D. Keuangan

Team leader hanya melihat **Piutang saya** dan **Kas saya**. Tidak ada menu pembayaran, pengeluaran, atau PO.

---

## 4.8 Supervisor

Supervisor memantau seluruh pegawai operasional pada area aktif.

### A. PWA supervisor

1. **Beranda**: ringkasan area, termasuk **Absensi hari ini** yang dikelompokkan per peran: team leader, sales, gudang, admin keuangan area. Yang belum absen tampil paling atas; tersedia total sudah/belum dan tautan ke halaman absensi penuh.
2. **Tim leader**:
   - Layar awal menampilkan kartu statistik per team leader, diurutkan berdasarkan penjualan bersih.
   - Setiap kartu mencakup leader itu sendiri ditambah sales langsungnya.
   - Sales tanpa team leader dikumpulkan pada kartu **Tanpa tim leader**.
   - Klik kartu untuk fokus pada tim tersebut; tombol **Semua tim** kembali ke daftar.
3. **Statistik**: top sales, top subdistributor, kartu per area, dan grafik harian.
4. **Penjualan** dan **Piutang**: daftar nota dan tagihan pada area.
5. **Keuangan**: buku kas/keuangan area.
6. **Pelanggan**: daftar outlet (lihat). Pembuatan/perubahan outlet oleh supervisor dilakukan melalui backoffice, bukan PWA.

### B. Backoffice supervisor

- **Peta monitoring**: layer kunjungan, penjualan/order, dan media promosi; detail lokasi menampilkan aktivitas lain di lokasi yang sama pada hari itu.
- **Aktivitas lapangan**: absensi, kunjungan, media promosi (dengan foto bukti).
- **Penjualan**: melihat dan mencatat penjualan area.
- **Keuangan**: piutang, pembayaran, kas, pengeluaran, setoran, PO, dan penerimaan PO.
- **Persediaan dan pergerakan stok**: melihat dan mencatat mutasi stok area.
- **Laporan**: termasuk laba rugi.

### C. Batasan supervisor

- Melihat beberapa area yang ditugaskan, tetapi bekerja pada satu area aktif.
- Tidak mengelola pengguna, area, subdistributor, atau pemasok.
- Tidak dapat mengonfirmasi setoran area ke pusat; pengajuan supervisor berstatus menunggu konfirmasi finance pusat/tenant admin.

---

## 4.9 Manajemen

Manajemen adalah peran analisis lintas area.

### A. Yang dapat dilakukan

- Melihat **semua area** tanpa memilih area aktif.
- **Ringkasan**: KPI penjualan, piutang, kas, stok, dan grafik.
- Menetapkan **target penjualan**.
- **Penjualan**: melihat semua nota, filter area, kanal, sales, status.
- **Piutang & aging**, **Kas**, **Pengeluaran**, **PO** (lihat).
- **Persediaan**: saldo, dokumen, jurnal (lihat).
- **Aktivitas lapangan**: peta monitoring, absensi, kunjungan, media promosi.
- **Laporan**: penjualan, stok, umur piutang, kas, pengeluaran, laba rugi, kehadiran, coverage kunjungan, media promosi; ekspor.
- **PWA**: Beranda, Penjualan, Pelanggan, Piutang, Tim, Statistik, Keuangan.

### B. Batasan manajemen

- Tidak membuat transaksi penjualan, pembelian, atau mutasi stok.
- Tidak mengelola pengguna dan master data.
- Tidak masuk ke menu pergerakan stok operasional.

---

## 5. Skenario satu hari operasional

Contoh alur lengkap dari pagi sampai tutup buku.

### Pagi — gudang dan sales

1. **Gudang** menerima barang dari pemasok (PO atau penerimaan langsung).
2. **Gudang** membuat pickup untuk sales A: 5 item, foto serah terima diunggah.
3. **Sales A** absen masuk di PWA (foto + lokasi).
4. **Sales A** membuka Kasir, memeriksa dan mengonfirmasi pickup.
5. Stok kanvas sales A aktif; POS siap dipakai.

### Siang — kunjungan dan penjualan

6. **Sales A** mengunjungi outlet B, mencatat kunjungan dengan hasil “order”, foto, dan lokasi.
7. Di outlet B, Sales A membuat nota: 2 item tunai + 1 item tempo, ditambah 1 bonus sesuai program. Harga salah satu item dinegosiasi, alasan diisi.
8. Nota tersimpan. Stok kanvas berkurang. Nota tempo menambah piutang outlet B sesuai tempo outlet.
9. Sinyal hilang di tengah rute: Sales A tetap dapat mencatat transaksi; antrean tersinkron saat sinyal kembali.
10. **Sales A** memasang poster di outlet C; stok media dan bukti foto tercatat.

### Sore — tutup kasir dan retur

11. **Sales A** menghitung sisa barang dan menutup kasir.
12. Sistem membuat pengajuan retur stok.
13. **Gudang** menghitung fisik, mengoreksi bila ada selisih, lalu mengonfirmasi.
14. Stok gudang diperbarui; absensi Sales A otomatis clock out.

### Sore — keuangan area

15. **Admin keuangan area** menerima uang tunai dari Sales A dan mencatat **Setoran sales** → kas area bertambah.
16. **Admin keuangan area** mencatat pembayaran transfer dari outlet B → piutang outlet B berkurang (FIFO).
17. **Admin keuangan area** mencatat pengeluaran BBM dari kas area, dan konsumsi rapat dari kas kecil.
18. **Admin keuangan area** menyisihkan kas kecil untuk operasional besok.
19. **Admin keuangan area** mengajukan setoran area ke pusat via transfer bank.

### Sore — pusat dan pemantauan

20. **Finance pusat** mengonfirmasi setoran area; kas fisik area sumber berkurang.
21. **Finance pusat** membuat PO baru untuk stok minggu depan dan menyetujuinya.
22. **Supervisor** memeriksa absensi tim dan peta kunjungan; memastikan semua sales punya aktivitas.
23. **Manajemen** melihat laporan penjualan, piutang, dan laba rugi harian.

---

## 6. Aturan bisnis yang wajib dipahami

### 6.1 Empat kategori harga

Semua produk memiliki empat harga: **warung**, **subdistributor**, **grosir**, dan **konsumen**, ditambah HPP.

- Jenis pelanggan menentukan kategori harga.
- Sales dan team leader wajib memakai kategori harga yang sesuai jenis pelanggan; tidak dapat memilih harga lain.
- Finance/admin dapat mencatat penjualan langsung dengan kategori harga yang dipilih, tetapi tetap harus konsisten dengan jenis pelanggan.
- Perubahan harga atau diskon manual **wajib disertai alasan negosiasi**.

### 6.2 Bonus, sampel, dan media promosi itu berbeda

| Jenis | Bisa dijual? | Tujuan | Efek stok/biaya |
| --- | --- | --- | --- |
| **Bonus** | Ya, sebagai barang gratis pada nota | Promosi beli X gratis Y | Keluar dari stok sesuai nota; nilai jual 0 |
| **Sampel** | Tidak | Uji coba ke outlet | Keluar dari stok sampel; tanpa piutang |
| **Media promosi** | Tidak | Dipasang di outlet | Keluar saat pemasangan; menjadi beban saat dipakai |

Jangan memperlakukan barang yang dibawa sales sebagai sampel. Barang reguler tetap barang jual; sampel adalah barang tersendiri.

### 6.3 Pickup, kasir, dan retur saling terikat

- Pickup dibuat gudang, bukan sales.
- Sales wajib **absen masuk** sebelum membuka kasir.
- POS hanya menjual barang dari pickup sesi aktif.
- Menutup kasir otomatis membuat pengajuan retur.
- Gudang mengoreksi dan mengonfirmasi retur; setelah itu absensi sales ditutup otomatis.
- Satu sales hanya satu sesi kasir aktif; tidak bisa membuat pickup baru saat sesi masih berjalan.

### 6.4 Selisih stok kasir

Saat tutup kasir, selisih antara hitungan fisik dan catatan ditangani sebagai:

- **Kompensasi/tanggungan sales** (mis. dipotong melalui nota kompensasi), atau
- **Waste** (barang rusak/hilang dengan alasan).

PWA menampilkan pratinjau kompensasi sebelum konfirmasi, dan konfirmasi terkunci sampai hitungan tersimpan.

### 6.5 Alur kas dan setoran

- **Sales tidak menyetor lewat aplikasi.** Uang tunai diserahkan ke admin keuangan area, lalu dicatat sebagai **Setoran sales** dan langsung diterima.
- **Konfirmasi hanya untuk setoran area ke pusat.** Pengajuan admin keuangan area/supervisor menunggu konfirmasi tenant admin/finance pusat. Jika dicatat tenant admin/finance pusat, langsung diterima.
- Setoran area ke pusat adalah **transfer bank**: kas fisik hanya berkurang di area sumber. **Pusat tidak menyimpan kas fisik.**
- Kas fisik hanya ada di area gudang.

### 6.6 Sumber dana pengeluaran

| Sumber | Kas fisik area | Saldo kas kecil | Wajib disetor | Catatan |
| --- | --- | --- | --- | --- |
| **Kas area** | Turun | Tetap | Turun | Default area |
| **Kas kecil** | Turun (kas kecil bagian dari kas fisik) | Turun | Tetap | Pemakaian tercatat di riwayat kas kecil |
| **Lainnya** | Tetap | Tetap | Tetap | Hanya catatan biaya |

Finance pusat dan tenant admin selalu memakai sumber **Lainnya** karena tidak memegang kas area.

### 6.7 Kas kecil

- Tidak diatur sebagai nilai absolut.
- **Tambah** menyisihkan kas area yang belum disisihkan (maks. kas area − kas kecil).
- **Kurangi** mengembalikan kas kecil ke kas area (maks. saldo kas kecil).
- Semua perubahan memiliki riwayat.

### 6.8 Piutang

- Nota tempo memakai **batas kredit** dan **tempo** outlet.
- Pembayaran dialokasikan FIFO ke nota tertua.
- Pembayaran tidak boleh melebihi total sisa piutang.
- Sales hanya melihat piutang atas namanya.
- Retur faktur mengurangi nilai nota dan mengembalikan stok ke gudang area penjualan (untuk kanvas, kembali ke area penjualan).

### 6.9 Cakupan wilayah

- Satu gudang dapat mencakup beberapa kabupaten/kota.
- Sales ditugaskan ke area dan kecamatan tertentu.
- Penjualan kanvas hanya untuk outlet di kecamatan yang menjadi cakupan sales tersebut.
- Team leader menjual atas namanya sendiri, tetapi cakupan outlet yang boleh dilayani meliputi cakupan dirinya dan sales langsungnya.
- Pembeli konsumen tanpa outlet cukup terdaftar pada area yang ditugaskan, dan penjualan konsumen **wajib tunai**.

### 6.10 Pusat dan subdistributor

- Pusat hanya menjual ke subdistributor.
- Nota subdistributor dibukukan di area Pusat dan hanya dapat dibuat tenant admin/finance pusat.
- Stok dapat diambil dari gudang sumber; barang keluar dari gudang tersebut, penjualan tetap tercatat di Pusat.
- Kartu area Pusat pada statistik berfokus pada subdistributor: jumlah pembeli, transaksi, rata-rata per transaksi, dan piutang berjalan.

### 6.11 Penjualan langsung dari backoffice

Selain sales di PWA, penjualan dapat dicatat langsung di backoffice oleh tenant admin, finance pusat, admin keuangan area, supervisor, team leader, dan sales (sesuai batas peran).

- Kanvas = stok sales; wajib memilih sales pemegang stok.
- Langsung = stok gudang; tidak memerlukan kasir.
- Subdistributor = hanya dari Pusat.
- Penjualan langsung tidak memakai sesi kasir dan tidak memerlukan absensi.

---

## 7. Matriks akses menu

Keterangan: ✓ = tersedia; L = hanya lihat; – = tidak tersedia.

| Menu | Platform | Tenant admin | Finance pusat | Keuangan area | Gudang | Supervisor | Team leader | Sales | Manajemen |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| Distributor | ✓ | – | – | – | – | – | – | – | – |
| Ringkasan | ✓ | ✓ | ✓ | ✓ | ✓ | ✓ | ✓ | ✓ | ✓ |
| Penjualan | – | ✓ | ✓ | ✓ | – | ✓ | ✓ | ✓ | L |
| Piutang | – | ✓ | ✓ | ✓ | – | ✓ | ✓ (saya) | ✓ (saya) | L |
| Pembayaran | – | ✓ | ✓ | ✓ | – | ✓ | – | – | L |
| Kas | – | ✓ | ✓ | ✓ | – | ✓ | ✓ (saya) | ✓ (saya) | L |
| Pengeluaran | – | ✓ | ✓ | ✓ | – | ✓ | – | – | L |
| Purchase order | – | ✓ | ✓ | L | L | L | – | – | L |
| Penerimaan PO | – | ✓ | ✓ | – | ✓ | ✓ | – | – | – |
| Saldo stok | – | ✓ | ✓ | ✓ | ✓ | ✓ | ✓ | ✓ (kanvas) | L |
| Stok opname | – | ✓ | – | – | – | – | – | – | – |
| Dokumen & jurnal stok | – | ✓ | ✓ | ✓ | ✓ | ✓ | ✓ | ✓ | L |
| Transit & transfer | – | ✓ | ✓ | ✓ | ✓ | ✓ | – | – | L |
| Penerimaan barang | – | ✓ | ✓ | ✓ | ✓ | ✓ | – | – | – |
| Pengambilan sales | – | ✓ | ✓ | ✓ | ✓ | ✓ | L | L | – |
| Retur stok sales | – | ✓ | ✓ | ✓ | ✓ | ✓ | L | L | – |
| Pengeluaran sampel | – | ✓ | ✓ | ✓ | ✓ | ✓ | ✓ | ✓ | – |
| Waste & konversi sampel | – | ✓ | ✓ | ✓ | ✓ | ✓ | – | – | – |
| Kirim transfer | – | ✓ | ✓ | ✓ | ✓ | ✓ | – | – | – |
| Peta monitoring | – | ✓ | ✓ | – | – | ✓ | ✓ | ✓ | ✓ |
| Absensi | – | ✓ | ✓ | – | – | ✓ | ✓ | ✓ | ✓ |
| Kunjungan | – | ✓ | ✓ | – | – | ✓ | ✓ | ✓ | ✓ |
| Media promosi | – | ✓ | ✓ | – | – | ✓ | ✓ | ✓ | ✓ |
| Laporan (umum) | – | ✓ | ✓ | ✓ | – | ✓ | ✓ | ✓ | ✓ |
| Laporan laba rugi | – | ✓ | ✓ | ✓ | – | ✓ | – | – | ✓ |
| Master data (tulis) | – | ✓ | ✓ | Sebagian (outlet) | – | Sebagian (outlet) | – | – | – |
| Pengguna & tim | – | ✓ | – | – | – | – | – | – | – |
| Pengaturan distributor | – | ✓ | – | – | – | – | – | – | – |

Catatan:

- Gudang hanya melihat menu master data tertentu (barang dan merek); menu area, outlet, subdistributor, pemasok, bonus, diskon, kategori/jenis, dan kategori biaya disembunyikan.
- Gudang tidak memiliki grup menu Laporan umum, tetapi tetap dapat membuka **Stok & sampel** dari menu Gudang.
- Admin keuangan area tidak melihat menu aktivitas lapangan dan tiga laporan aktivitas (kehadiran, coverage kunjungan, media promosi).
- Team leader dan sales tidak melihat menu pembayaran, pengeluaran, PO, dan transfer.
- Manajemen tidak memiliki menu pergerakan stok operasional.

---

## 8. Checklist onboarding

### Administrator platform

- [ ] Akun administrator platform tersedia.
- [ ] Distributor dibuat beserta area Pusat dan akun administrator distributor.
- [ ] Kredensial administrator distributor diserahkan lewat kanal aman.

### Administrator distributor

- [ ] Profil distributor dan logo diisi.
- [ ] Kategori dan jenis barang sesuai kebutuhan.
- [ ] Merek dan produk lengkap dengan empat harga, HPP, satuan, dan tipe produk.
- [ ] Area gudang dibuat dengan cakupan kabupaten/kota.
- [ ] Outlet dibuat lengkap dengan kecamatan, koordinat, batas kredit, dan tempo.
- [ ] Subdistributor dan pemasok didaftarkan.
- [ ] Bonus dan program diskon aktif sesuai periode.
- [ ] Kategori biaya tersedia.
- [ ] Akun gudang, admin keuangan area, supervisor, team leader, dan sales dibuat.
- [ ] Struktur atasan-bawahan benar (TL → supervisor; sales → TL/supervisor).
- [ ] Cakupan kecamatan sales dan TL diisi.
- [ ] Target penjualan ditetapkan.

### Sebelum operasional pertama

- [ ] Stok awal masuk melalui penerimaan barang atau PO.
- [ ] Kas kecil area ditetapkan melalui **Tambah kas kecil**.
- [ ] Sales sudah menginstal PWA dan berhasil login.
- [ ] Sales berhasil absen dan membuka kasir dengan pickup percobaan.
- [ ] Gudang berhasil membuat pickup dengan foto.
- [ ] Finance area berhasil mencatat setoran sales percobaan.
- [ ] Supervisor dapat melihat peta dan absensi.

---

## 9. Troubleshooting dan FAQ

**Sales tidak bisa membuka kasir.**
Pastikan sudah absen masuk hari ini dan absensi belum clock out. Pastikan pickup sudah dibuat gudang dan dikonfirmasi. Periksa juga apakah sesi kasir aktif masih terikat di perangkat lain.

**Pickup tidak muncul di PWA sales.**
Pastikan pickup dibuat untuk sales yang benar pada area yang sama, dan sales memilih area aktif yang sesuai.

**Penjualan ditolak karena cakupan.**
Outlet berada di kecamatan yang tidak termasuk cakupan sales/team leader, atau sales belum ditugaskan ke area tersebut. Administrator atau team leader perlu memperbaiki cakupan.

**Penjualan tempo ditolak.**
Batas kredit outlet tidak mencukupi atau outlet belum memiliki tempo. Periksa menu Outlet.

**Tidak bisa mencatat pembayaran.**
Jumlah melebihi sisa piutang, outlet tidak punya piutang terbuka, atau sales mencoba mencatat piutang milik sales lain.

**Setoran area masih menunggu.**
Pengajuan admin keuangan area/supervisor harus dikonfirmasi tenant admin/finance pusat. Hanya pengaju yang dapat membatalkan pengajuannya sendiri.

**Pengeluaran tidak mengurangi kas.**
Periksa sumber dana. Sumber **Lainnya** hanya mencatat biaya. Finance pusat dan tenant admin selalu memakai sumber Lainnya.

**Saldo kas kecil tidak berubah saat pengeluaran.**
Pastikan sumber dana pengeluaran adalah **Kas kecil**, bukan Kas area.

**Stok tidak bertambah setelah penerimaan.**
Pastikan jumlah bersih (setelah waste/retur) lebih dari nol dan dokumen tersimpan. Periksa **Dokumen stok** dan **Jurnal stok**.

**Tutup kasir tidak selesai.**
Sesi menunggu gudang. Gudang harus menghitung dan mengonfirmasi retur. Absensi sales baru tertutup setelah konfirmasi.

**Transaksi offline belum masuk.**
Buka PWA dengan internet dan biarkan antrean tersinkron. Jika ada konflik harga, tinjau penawaran terbaru lalu kirim ulang. Jangan menghapus data aplikasi sebelum antrean kosong.

**Menu tidak muncul.**
Menu mengikuti peran dan area. Menu yang tidak menjadi hak peran tidak ditampilkan. Jika menu yang seharusnya ada tidak muncul, periksa peran dan penugasan area pada **Pengguna & tim**.

**Lupa kata sandi.**
Hubungi administrator distributor. Pengguna tidak dapat mereset sendiri karena tidak ada alur lupa kata sandi.

**Akun nonaktif.**
Administrator menonaktifkan akun; sesi langsung berakhir. Minta administrator mengaktifkan kembali.

**Distributor tidak bisa diakses.**
Status distributor mungkin ditangguhkan administrator platform. Hubungi administrator platform.

---

Dokumen ini mengikuti perilaku aplikasi per November 2026. Jika ada perubahan alur bisnis, perbarui dokumen ini bersamaan dengan perubahan kode.
