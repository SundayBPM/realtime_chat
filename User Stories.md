## 1. Login & Registrasi
### User Story
ebagai pengguna baru, saya ingin bisa mendaftar akun dan login menggunakan email dan password agar saya dapat mengakses fitur-fitur aplikasi.

### Acceptance Criteria
- Pengguna dapat mengisi form pendaftaran dengan email dan password.
- Sistem melakukan validasi email.
- Sistem melakukan validasi password.
- Sistem memverifikasi apakah email sudah terdaftar atau belum.
- Sistem menyimpan data pengguna baru.
- Pengguna berhasil login setelah mendaftar.
- Setelah berhasil login, pengguna akan diarahkan ke halaman utama 

### Catatan Tambahan
~

## 2. Pendaftaran dan Pembayaran Hackathon
### User Story
Sebagai peserta, saya ingin dapat mendaftar untuk acara hackathon, melakukan pembayaran, dan menerima konfirmasi pendaftaran.


### Acceptance Criteria
- Pengguna dapat mengisi form pendaftaran hackathon.
- Pengguna dapat memilih untuk mendaftar sebagai individu atau tim.
- Setelah mengisi formulir, pengguna mendapatkan detail tagihan.
- Pengguna dapat melakukan pembayaran sesuai dengan tagihan.
- Sistem memvalidasi bukti pembayaran.
- Jika pembayaran valid, sistem akan mengirimkan konfirmasi pendaftaran.
- Pengguna dapat melihat status pendaftaran mereka (misalnya "Menunggu Pembayaran", "Sudah Bayar", "Verifikasi", dll.).

### Catatan Tambahan
-

## 3. Proses Penilaian dan Pengumuman Pemenang
### User Story
Sebagai juri, saya ingin dapat menilai proyek-proyek peserta, mengumumkan pemenang, dan menerbitkan sertifikat.

### Acceptance Criteria
- Juri dapat mengakses daftar peserta dan proyek yang diajukan.
- Juri dapat memberikan penilaian berdasarkan kriteria yang telah ditentukan.
- Juri dapat memberikan catatan atau komentar pada setiap penilaian.
- Juri dapat mengirimkan penilaian.
- Sistem dapat memproses penilaian dari semua juri untuk menentukan pemenang.
- Sistem dapat mengumumkan pemenang.
- Sistem dapat menerbitkan sertifikat untuk pemenang.

### Catatan Tambahan
- Alur ini melibatkan peran "Back Office" atau juri dan sistem.

## 4. Pengajuan Proposal Sponsor
### User Story
Sebagai perwakilan perusahaan, saya ingin dapat mengajukan proposal sponsor untuk acara hackathon dan mendapatkan konfirmasi dari panitia.

### Acceptance Criteria
- Perusahaan dapat mengajukan form pengajuan sponsorship/kemitraan.
- Sistem memvalidasi form pengajuan.
- Back Office dapat mereview pengajuan proposal.
- Back Office dapat menerima atau menolak proposal.
- Jika proposal diterima, sistem mengirimkan konfirmasi kepada perusahaan
- Jika proposal ditolak, sistem mengirimkan pemberitahuan penolakan

### Catatan Tambahan
- Alur ini melibatkan peran "Company" dan "Back Office".
