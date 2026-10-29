# Opsi build APK melalui GitHub Actions

Workflow `.github/workflows/build-apk.yml` sudah disertakan. Jika seluruh isi folder proyek ini dimasukkan ke sebuah repository GitHub, workflow dapat membangun APK debug secara otomatis.

1. Buat repository baru di GitHub.
2. Unggah **isi folder proyek** (bukan hanya ZIP) sehingga `settings.gradle`, `build.gradle`, folder `app`, dan `.github/workflows/build-apk.yml` berada di root repository.
3. Buka tab **Actions**, pilih **Build VYATA ON APK**, lalu tekan **Run workflow**. Jika Actions tidak muncul, pastikan Actions diizinkan untuk repository.
4. Setelah workflow berhasil, buka hasil run dan unduh artifact **VYATA-ON-debug-APK**.
5. Ekstrak artifact ZIP untuk mendapatkan `app-debug.apk`, lalu pindahkan APK ke HP untuk instalasi.

APK debug cocok untuk pengujian internal. Untuk distribusi resmi yang luas, siapkan signing key dan buat release build yang ditandatangani dengan aman.
