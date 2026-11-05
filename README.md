# Catatan Kegiatan Teknisi WiFi

Aplikasi web gratis (Firebase Spark + GitHub Pages) untuk teknisi **Abdul Mufid** dan **Wawan**.

## Fitur
- Entry kegiatan **Maintenance**: masalah + solusi final + foto
- Entry kegiatan **PSB**: panjang kabel (meter), rasio/splitter, beli atau tidak, router, status Done / Sudah ON, foto
- Tanggal & jam entry otomatis (jam server Firebase)
- Riwayat realtime, filter per teknisi dan jenis, foto bisa diperbesar
- Foto dikompres otomatis (~100 KB) dan disimpan di Firestore, jadi **tidak perlu Firebase Storage** (Storage kini butuh paket berbayar)

## Langkah 1 — Buat project Firebase (gratis)
1. Buka https://console.firebase.google.com → **Add project** (matikan Google Analytics).
2. **Build → Firestore Database → Create database** → pilih lokasi `asia-southeast2 (Jakarta)` → mode production.
3. **Build → Authentication → Get started → Sign-in method → Anonymous → Enable**.
4. Tab **Rules** di Firestore → tempel isi `firestore.rules` → **Publish**.
5. **Project settings (ikon gear) → Your apps → ikon `</>` (Web)** → daftarkan app → salin `firebaseConfig`.
6. Buka `index.html`, ganti blok `firebaseConfig` dengan milik Anda.

## Langkah 1b — Buat akun Admin
1. **Authentication → Sign-in method → Email/Password → Enable**.
2. **Authentication → Users → Add user** → isi email & password admin Anda.
3. Ganti `admin@contoh.com` dengan email admin di **dua tempat**: `ADMIN_EMAIL` di `index.html` dan fungsi `isAdmin()` di `firestore.rules` (lalu Publish ulang rules).
4. Di aplikasi, tekan tombol **🔐 Admin** (pojok kanan atas) → login.

Hak akses:
| | Teknisi (tanpa login) | Admin |
|---|---|---|
| Isi entry kegiatan | ✅ | ✅ |
| Lihat riwayat & foto | ✅ | ✅ |
| Hapus catatan | ❌ | ✅ |
| Export CSV | ❌ | ✅ |

Catatan: siapa pun yang membuka alamat web aplikasi bisa melihat riwayat (termasuk nama pelanggan, alamat, dan foto). Jangan bagikan alamat webnya ke orang di luar tim.

## Foto terhapus otomatis (60 hari)
- Setiap kegiatan baru diberi tanggal hapus foto (`hapusFotoPada`) = 60 hari sejak entry.
- Saat aplikasi dibuka (teknisi atau admin), aplikasi membersihkan foto kegiatan yang sudah lewat batas. Teks catatan tetap tersimpan.
- Ini berjalan di sisi aplikasi (paket Spark tidak punya Cloud Functions terjadwal), jadi pembersihan terjadi saat ada yang membuka aplikasi.
- Kegiatan yang dibuat sebelum fitur ini tidak punya tanggal hapus, jadi fotonya tidak ikut terhapus otomatis.

## Langkah 2 — Upload ke GitHub & aktifkan Pages
1. Buat repository baru di GitHub (mis. `catatan-teknisi`).
2. Upload `index.html`, `firestore.rules`, `README.md`.
3. **Settings → Pages → Source: Deploy from a branch → `main` / root → Save**.
4. Alamat aplikasi: `https://USERNAME.github.io/catatan-teknisi/`
5. Di Firebase: **Authentication → Settings → Authorized domains → Add domain** → `USERNAME.github.io`.

## Langkah 3 — Pasang di HP teknisi
Buka alamat di Chrome HP → menu ⋮ → **Add to Home screen**.

## Catatan
- Batas gratis Firestore: 1 GB simpan, 50 ribu baca/hari, 20 ribu tulis/hari — cukup untuk usaha WiFi kecil.
- `apiKey` Firebase di web memang publik; keamanan diatur oleh Rules. Untuk keamanan lebih, aktifkan **App Check** atau ganti ke login email/password.
- Catatan tidak bisa diedit/dihapus lewat aplikasi (lihat rules). Hapus manual lewat Firebase Console bila perlu.
