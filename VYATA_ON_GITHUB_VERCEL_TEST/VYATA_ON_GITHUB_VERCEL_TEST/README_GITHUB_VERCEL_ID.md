# VYATA ON — Paket uji GitHub + Vercel

Paket ini disiapkan sebagai **proyek baru yang terpisah** dari repositori lama. Jangan menimpa atau menghapus proyek lama.

## Isi paket
- `index.html`: tampilan web untuk uji awal Vercel.
- `app/src/main/assets/index.html`: salinan HTML untuk aplikasi Android WebView.
- Berkas Gradle, Java, resource Android, dan workflow GitHub Actions dari proyek Android sebelumnya.
- `vercel.json`: konfigurasi deploy frontend statis.

## Unggah ke GitHub dari HP
1. Ekstrak ZIP di HP atau komputer.
2. Buat repositori baru bernama `vyata-on-online`.
3. Unggah **isi folder** hasil ekstrak (bukan ZIP-nya) ke repositori baru. Pastikan `index.html`, `vercel.json`, `settings.gradle`, dan `app/` terlihat di tingkat teratas.
4. Di Vercel pilih **Add New → Project**, lalu impor repositori `vyata-on-online`.
5. Untuk uji tampilan statis, gunakan Framework Preset `Other`, Build Command kosong, dan Output Directory kosong/default. Deploy.
6. Buka URL deployment dan periksa tampilan, navigasi, serta fungsi frontend.

## Penting: status backend online
Versi ini **belum terhubung ke database pusat**. HTML saat ini masih menggunakan `localStorage` di perangkat/browser untuk sejumlah data. Deploy ke Vercel saja tidak membuat login, data siswa, dan laporan otomatis dibagikan antar-HP.

Untuk penggunaan online sebenarnya, langkah selanjutnya adalah menghubungkan frontend ke API backend dengan autentikasi server dan database persisten. Fondasi PHP + SQLite disiapkan terpisah untuk hosting PHP yang mendukung PDO_SQLite; jangan mencoba menjalankan file PHP tersebut sebagai penyimpanan lokal di Vercel.

## Keamanan
- Jangan unggah kata sandi hosting, API secret, token, database berisi data nyata, atau data pribadi ke repositori GitHub.
- Gunakan data uji saja pada deployment Vercel.
- Jangan gunakan untuk produksi sebelum login, hak akses server, sinkronisasi, dan keamanan data diuji.
- Proyek ini tidak mengubah website SIPANDU atau folder hosting SIPANDU.
