# JUSTSTERLING — Firebase Starter

Starter website layanan digital dengan halaman publik, member area, admin panel multi-role, katalog layanan, pemesanan, pengaturan kontak, lima tema, dan maintenance flag.

## Stack
- HTML/CSS/JavaScript ES modules
- Firebase Authentication (email/password)
- Cloud Firestore
- Firebase Hosting
- Firebase Security Rules

## Sebelum menjalankan
1. Buat project di https://console.firebase.google.com/.
2. Aktifkan Authentication > Sign-in method > Email/Password.
3. Buat Firestore Database (production mode).
4. Daftarkan Web App di Project settings dan salin konfigurasi ke `firebase-config.js`.
5. Ubah semua `GANTI_...` pada file konfigurasi.
6. Deploy Firestore rules dan Hosting sesuai langkah di bawah.

## Bootstrap Super Admin (wajib)
Untuk keamanan, akun pertama dibuat lewat halaman Register. Setelah akun dibuat:
1. Buka Firebase Console > Authentication > Users dan salin UID akun Anda.
2. Buka Firestore > `users` > dokumen dengan UID tersebut.
3. Ubah field `role` menjadi string `superadmin`.
4. Reload website dan login kembali.

Jangan pernah memberikan role superadmin melalui form publik. Role hanya boleh diubah oleh superadmin melalui panel atau Firebase Console. Super Admin dapat mempromosikan akun member menjadi admin dengan UID. Admin biasa tidak dapat mengubah role.

## Jalankan lokal
Hosting modul ES dari `file://` tidak didukung. Gunakan server lokal:
```bash
npm install -g firebase-tools
firebase login
firebase use --add
firebase emulators:start --only hosting
```
Atau jalankan `npx serve .` dari folder project.

## Deploy gratis ke Firebase Hosting
Firebase Hosting memiliki kuota gratis pada Spark plan sesuai batas Firebase yang berlaku. Firestore dan Authentication juga memiliki kuota gratis terbatas. Buat project dan cek kuota di Firebase Console.
```bash
npm install -g firebase-tools
firebase login
firebase use --add
firebase deploy --only firestore:rules,hosting
```
Jika CLI meminta project, pilih project Firebase Anda. File `.firebaserc` dibuat oleh `firebase use --add`; jangan commit kredensial rahasia.

## GitHub
Buat repository kosong di GitHub, lalu:
```bash
git init
git add .
git commit -m "Initial JUSTSTERLING website"
git branch -M main
git remote add origin https://github.com/USERNAME/REPOSITORY.git
git push -u origin main
```
GitHub Pages bukan host yang cocok untuk menyimpan konfigurasi rahasia atau menggantikan Firebase Auth/Firestore. Gunakan Firebase Hosting sebagai hosting aktif.

## Catatan fitur & batasan starter
- Multi-admin: role `superadmin` dan `admin`; hanya superadmin yang bisa mengubah role dari panel.
- Member: email/password tanpa OTP email; password diproses Firebase Authentication, bukan disimpan di Firestore.
- Gambar layanan memakai URL HTTPS. Firebase Storage sengaja tidak dipakai agar starter tidak mengharuskan upgrade billing. Periksa persyaratan paket Storage terbaru sebelum mengaktifkannya.
- Maintenance flag sudah tersimpan di settings, namun halaman maintenance publik terpusat dan pengecualian admin perlu ditambahkan sebelum produksi.
- Statistik member di dashboard saat ini dihitung dari data pesanan, bukan daftar lengkap akun Auth.
- Tidak ada payment gateway otomatis; status pembayaran/order dikonfirmasi admin secara manual.
- Jangan gunakan layanan boosting yang melanggar ketentuan platform, memanipulasi engagement secara tidak autentik, atau meminta password akun sosial pelanggan.
- Sebelum produksi: aktifkan App Check, pasang aturan validasi field/ukuran teks lebih ketat, audit aturan keamanan, siapkan backup Firestore, dan uji di project staging.
