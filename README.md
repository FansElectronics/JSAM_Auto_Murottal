# Jadwal Sholat Auto Murottal (JSAM) dengan ESP8266 & ESP32

Firmware pemutar audio otomatis **Jadwal Sholat Auto Murottal (JSAM)** berbasis mikrokontroler ESP8266 dan ESP32 dengan modul audio DFPlayer Mini. Project ini dirancang untuk memutar lantunan Al-Qur'an (Murottal), Tarhim, serta adzan otomatis menjelang dan saat masuk waktu sholat. Perangkat ini dapat beroperasi secara mandiri (**Stand-alone / Auto Mode**) maupun terhubung sebagai penerima wireless (**Slave Mode via ESP-NOW**) yang disinkronkan langsung dengan display [Jadwal Sholat Dot Matrix (JSDM)](https://github.com/FansElectronics/JSDM_WiFi).

Pengaturan jadwal, pemilihan surat/qori, volume, jeda relay amplifier, serta pengujian audio dapat dikonfigurasi langsung melalui aplikasi Android:
👉 **Download Aplikasi Android**: https://play.google.com/store/apps/details?id=com.fanselectronics.jsam

---

## Spesifikasi Firmware ⚙️:
- **Mikrokontroler**: ESP8266 (Wemos D1 Mini) & ESP32 (DevKitC / NodeMCU)
- **Modul Audio**: DFPlayer Mini MP3 Player (MicroSD FAT32)
- **Penyimpanan Konfigurasi**: LittleFS Flash File System
- **Real-Time Clock (RTC)**: DS3231 I2C RTC (Akurat dengan kompensasi suhu)
- **Komunikasi Display / Pengaturan**: LCD 16x2 / 20x4 I2C (untuk status & navigasi)
- **Kontrol Daya Audio**: 1 Relay Paralel (Master Power Amplifier Audio Luar & Dalam)
- **Mode Operasi Fleksibel**:
  - **Mode AUTO (Stand-alone)**: Menghitung jadwal sholat mandiri via RTC internal dan menjalankan urutan playlist otomatis.
  - **Mode SLAVE (Wireless Receiver)**: Menerima pemicu jadwal, trigger audio, dan status sholat dari master JSDM secara nirkabel via **ESP-NOW**.
- **Konektivitas Konfigurasi**: Wi-Fi Access Point (Web/WebSocket/JSON API) terintegrasi ke Aplikasi Android.
- **Proteksi Flash**: Fitur reset pabrik / wipe LittleFS cepat dengan menahan tombol saat booting.

---

## Fitur yang Tersedia ✅:
- **Penjadwalan Playlist Per Sholat**: Konfigurasi mandiri untuk 5 waktu sholat (Subuh, Dzuhur, Ashar, Maghrib, Isya').
- **Pemisahan Audio Adzan Otomatis**:
  - Pemilihan tipe alarm: Buzzer Beep atau Suara MP3 Adzan penuh.
  - Pemisahan folder otomatis internal (Folder `02` untuk Adzan Subuh, Folder `01` untuk Adzan Reguler).
- **Katalog Qori & Audio Khusus**:
  - Mendukung pembacaan surat dinamis per Qori (Juz 30 maupun 114 Surat lengkap).
  - Folder khusus untuk Sholawat Tarhim dan audio kustom lainnya.
- **Sinkronisasi Database Cloud**: Database metadata surat, qori, dan durasi audio terhubung online via GitHub RAW dan tersimpan otomatis di perangkat.
- **Pencegahan Letupan Suara Amplifier**: Dilengkapi pengaturan jeda nyala relay sebelum audio berbunyi (*Delay Play*) dan jeda mati relay setelah audio tuntas (*Delay Stop/Adzan*).
- **Tombol Multifungsi Cerdas**:
  - *Klik Cepat saat Audio Berbunyi*: Menghentikan audio Murottal/Adzan seketika jika ingin adzan atau tilawah manual tanpa merusak timer jadwal.
  - *Tahan 10 Detik di Loop*: Beralih mode operasi secara instan (Mode AUTO ⟷ Mode SLAVE).
  - *Tahan 10 Detik Saat Boot*: Format total partisi LittleFS (Factory Reset / Wipe All Data).
- **Upload Lisensi & Firmware Update**: Mendukung upload `license.json` dan flashing via WebSerial Flasher.

---

## Struktur Folder MicroSD (DFPlayer Mini) 📁:
Untuk memastikan sistem pemutar audio membaca file dengan tepat, susun berkas MP3 di dalam MicroSD dengan format folder 2 digit angka:
```text
MicroSD Root/
├── 01/             -> Adzan Reguler (Dzuhur, Ashar, Maghrib, Isya) [001.mp3, 002.mp3, ...]
├── 02/             -> Adzan Subuh [001.mp3, 002.mp3, ...]
├── 03/             -> Audio Khusus / Tarhim [001.mp3, 002.mp3, ...]
├── 04 - 10/        -> Cadangan Audio Masa Depan
└── 11+/            -> Folder Qori Al-Qur'an (11 = Qori 1, 12 = Qori 2, dst.)
```
*Catatan: Pastikan MicroSD diformat FAT32 dan file audio MP3 diunduh di [RILIS TAG MP3 FILE](https://github.com/FansElectronics/JSAM_Auto_Murottal/releases/tag/MP3).* Kami menerima sumbangan file mp3 jika ingin berkontribusi.

---

## Default Access Point (Mode Pengaturan) 📶:
```text
    SSID: JSAM Auto Murottal
    PASS: fanselectronics
```

---

## Bagaimana Sistem Lisensinya? 🔐

Sistem lisensi firmware JSAM Auto Murottal menggunakan enkripsi berbasis **ID Perangkat Keras Unik (Device ID)** pada setiap chip ESP8266 / ESP32. 

- **Mode SLAVE (Gratis / Tanpa Lisensi)**: Jika Anda menggunakan modul ini murni sebagai penerima nirkabel dari JSDM WiFi / RGB, modul dapat bekerja langsung tanpa aktivasi berbayar, dengan syarat lisensi JSDM Murottal aktif.
- **Mode AUTO (Berlisensi Stand-Alone)**: Membutuhkan lisensi aktif untuk mengaktifkan perhitungan jadwal mandiri, RTC DS3231, dan playlist otomatis tanpa bergantung pada master JSDM.

Untuk melakukan aktivasi:
1. Hubungkan aplikasi Android ke sinyal Wi-Fi modul JSAM.
2. Buka menu **Aktivasi / Lisensi**, lalu salin **Device ID** yang tampil.
3. Hubungi kontak resmi Fans Electronics untuk mendapatkan file `license.json`.
4. Unggah berkas `license.json` langsung melalui aplikasi Android atau tool web flasher.

Dana pembelian lisensi ini digunakan langsung untuk mendukung riset berkelanjutan, pemeliharaan pustaka, serta penambahan fitur-fitur firmware baru.

**Harga Lisensi JSAM Stand-Alone 🏷️:**
- Lisensi JSAM Stand-Alone Mode (ESP8266 / ESP32): **Rp. 150,000**

**Syarat dan Ketentuan Lisensi 📜:** 
- Lisensi berlaku mutlak untuk 1 (satu) chip ESP8266 atau ESP32 karena mengikat berdasarkan ID unik perangkat keras.
- Garansi lisensi berlaku apabila chip rusak secara fisik dan kirim chip rusak kepada kami sebagai bukti. Jika chip tidak dapat dikirim, wajib membeli lisensi baru.
- Garansi berlaku 1 tahun dari license diterbitkan.
- Pembelian pihak kedua/ketiga tetap dilayani sesuai syarat di atas selama ID unik terdaftar resmi di database kami.

---

## Sistem Pembayaran & Donasi 💵

- **Pemesanan Lisensi & Bantuan Teknis**: https://wa.me/6281803750001
- **Dukungan Donasi Riset**: https://saweria.co/fanselectronics

---

## Cara Flash Firmware & Upload Lisensi 🚀
Anda dapat melakukan kompilasi mandiri atau menggunakan tool browser flasher WebSerial resmi dari Fans Electronics tanpa perlu instalasi IDE yang rumit:
- **JSAM Firmware Flasher**: https://app.fanselectronics.com/flasher/JSAM/
- **License Uploader Tool**: https://app.fanselectronics.com/tools/license-uploader
- **File Audio MP3 Murottal**: https://github.com/FansElectronics/JSAM_Auto_Murottal/releases/tag/MP3

---

## Diagram Wiring & Skematik Hardware 💾

Pin default yang direkomendasikan untuk menghindari kendala boot strapping ESP8266
- **DFPlayer Mini TX/RX**: D6 (GPIO12 - TX ESP ke RX MP3) & D5 (GPIO14 - RX ESP ke TX MP3) *(Gunakan resistor 1kΩ pada RX DFPlayer)*
- **DFPlayer BUSY**: D7 (GPIO13) *(Wajib gunakan INPUT_PULLUP, hindari PIN D8)*
- **Relay Amplifier**: D1 / D2 (GPIO5 / GPIO4)
- **I2C RTC DS3231 & LCD**: D2 (SDA / GPIO4) & D1 (SCL / GPIO5)
- **Tombol Reset/Aksi**: D3 (GPIO0 / Flash Button)

File desain skematik dan layout PCB lengkap dapat diunduh pada folder `/PCB` di repository ini.

---

## Terima Kasih Kepada 🤲
- Allah Subhanahu Wa Ta'ala
- Arduino.cc
- GitHub
- Kontributor MP3 Files, dll
- Semua orang yang mentraktir saya kopi
