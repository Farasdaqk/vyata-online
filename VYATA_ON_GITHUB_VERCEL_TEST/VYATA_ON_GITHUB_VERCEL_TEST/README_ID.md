# VYATA ON + Running Text Pendidikan — Proyek APK Android

Proyek Android WebView yang membungkus file HTML VYATA ON yang diberikan pengguna. HTML disimpan di `app/src/main/assets/index.html`, jadi konten utama ikut masuk ke APK. Aplikasi membutuhkan internet untuk pustaka eksternal yang masih dipanggil dari CDN (misalnya PDF/Excel), dan untuk layanan online lain yang memang digunakan oleh HTML.

## Yang sudah disiapkan
- Nama aplikasi: **VYATA ON**
- Package ID: `id.co.secaba.vyataon`
- WebView dengan JavaScript dan DOM storage aktif
- HTML VYATA ON + Running Text disertakan sebagai asset aplikasi
- Izin internet
- Dukungan pemilih berkas untuk impor Excel/CSV dan pilih foto
- Dukungan kamera langsung melalui aplikasi kamera Android untuk input `lap-camera-input`
- Penyimpanan WebView/LocalStorage tetap pada aplikasi di perangkat
- Ikon aplikasi sederhana

## Membuat APK melalui Android Studio (komputer/laptop)
1. Ekstrak ZIP ini ke sebuah folder.
2. Buka Android Studio, pilih **Open** lalu pilih folder `VYATAONAndroid` (folder yang berisi `settings.gradle`).
3. Pastikan Android Studio memakai JDK 17 dan sinkronkan Gradle. Jika diminta, izinkan unduhan komponen Gradle/Android SDK.
4. Tunggu proses Sync selesai.
5. Pilih **Build > Build APK(s)**.
6. APK debug biasanya berada di `app/build/outputs/apk/debug/app-debug.apk`.
7. Pindahkan APK ke HP Android, buka file tersebut, lalu izinkan pemasangan aplikasi dari sumber yang digunakan jika Android memintanya.

## Menguji di HP
- Buka aplikasi dan pastikan halaman utama tampil.
- Coba tambah laporan, pilih gambar dari galeri, dan kamera langsung.
- Coba impor/ekspor Excel serta cetak/ekspor PDF dengan internet aktif.
- Tutup lalu buka kembali aplikasi untuk memeriksa apakah data lokal masih tersimpan.

## Catatan penting
- Ini **proyek sumber Android**, bukan file APK yang sudah dibangun. Lingkungan ini tidak memiliki Android SDK/Gradle untuk menghasilkan dan menguji APK final.
- Kamera langsung memakai aplikasi kamera bawaan Android. Pengalaman dapat sedikit berbeda antarperangkat.
- PDF dan Excel menggunakan pustaka yang dipanggil dari internet dalam HTML. Jika koneksi internet mati atau CDN tidak tersedia, fitur yang bergantung pada pustaka tersebut mungkin tidak berjalan.
- Data yang disimpan di LocalStorage berada di perangkat itu. Jangan menganggap data otomatis tersinkron ke perangkat lain atau server kecuali kode aplikasi memiliki backend yang memang melakukan sinkronisasi.
- Sebelum dipakai untuk data resmi, uji dulu alur laporan, foto, ekspor, impor, serta pemulihan data pada perangkat uji.
