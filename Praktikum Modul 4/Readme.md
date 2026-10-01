# Praktikum Internet of Things - Modul 4
## Komunikasi Pertukaran Data Dua Arah (MQTT, JSON, ESP8266)

| | |
|---|---|
| **Program Studi** | Teknik Komputer UNSOED |
| **Mata Kuliah** | Praktikum Internet of Thing (TK245002) |
| **Tahun / Semester** | 2026 / 5 |
| **Modul** | 4 - Komunikasi Pertukaran Data Dua Arah |
| **Asisten** | Athallah Tsany Satryaji |
| **Praktikan** | Muhammad Nabil Zaedan Agesy (H1H024062) |

---

## Daftar Isi
1. [Tujuan Praktikum](#1-tujuan-praktikum)
2. [Alat dan Bahan](#2-alat-dan-bahan)
3. [Struktur Folder](#3-struktur-folder)
4. [Percobaan 4A - Subscribe dan Deserialisasi JSON](#4-percobaan-4a---subscribe-dan-deserialisasi-json)
5. [Percobaan 4B - Pertukaran Data Dua Arah](#5-percobaan-4b---pertukaran-data-dua-arah)
6. [Flowchart callback()](#6-flowchart-callback)
7. [Kesimpulan](#7-kesimpulan)
8. [Kendala](#8-kendala)

---

## 1. Tujuan Praktikum
1. Memahami konsep pertukaran data dua arah (bidirectional) pada sistem IoT.
2. Memahami cara kerja *subscribe* dan proses deserialisasi data JSON pada ESP8266.
3. Menerapkan penerimaan perintah kendali lewat MQTT untuk menggerakkan aktuator secara *real-time*.
4. Membangun sistem IoT yang dapat mengirim (*publish*) data sensor sekaligus menerima perintah kendali pada waktu yang sama (*full duplex*).
5. Menganalisis keseluruhan mekanisme pertukaran data IoT pada sistem yang saling terhubung.

## 2. Alat dan Bahan
- Board ESP8266 DevKit (1 buah)
- Sensor DHT11 (1 buah)
- LED (1 buah) sebagai simulasi aktuator, beserta resistor 220 Ohm (1 buah)
- Breadboard dan kabel jumper secukupnya
- Kabel USB (Micro-USB/USB-C sesuai jenis board)
- Laptop/PC dengan Arduino IDE (board manager ESP8266, library DHT sensor, PubSubClient, dan ArduinoJson)
- Jaringan WiFi dengan akses internet
- Aplikasi klien MQTT (MQTT Explorer / HiveMQ WebSocket Client)
- Broker MQTT publik (port 1883)

**Dokumentasi alat dan bahan:**

![Alat dan Bahan](Dokumentasi%20Praktikum/Alat%20dan%20Bahan.jpg)

## 3. Struktur Folder
```text
Praktikum Modul 4/
├── Dokumentasi Praktikum/
│   ├── Alat dan Bahan.jpg
│   ├── Percobaan4A.jpg
│   ├── Percobaan4B.jpg
│   └── Flowchart_callback.png
├── Source Code/
│   ├── percobaan1.ino      # Percobaan 4A
│   └── percobaan2.ino      # Percobaan 4B
└── README.md
```

---

## 4. Percobaan 4A - Subscribe dan Deserialisasi JSON

### Penjelasan
Pada percobaan ini ESP8266 berperan sebagai *subscriber* yang menerima perintah lewat MQTT. Broker MQTT menjadi perantara antara pengirim pesan (MQTT Explorer) dan ESP8266. ESP8266 mendaftar pada topic tertentu, dan setiap ada pesan baru fungsi `callback()` otomatis dijalankan. Pesan berformat JSON kemudian dideserialisasi dengan `deserializeJson()` agar nilai `perintah` dapat dibaca dan dipakai untuk mengendalikan LED. Fungsi `client.loop()` harus terus dipanggil agar komunikasi MQTT dan pesan masuk tetap terproses.

Source code: [`Source Code/percobaan1.ino`](Source%20Code/percobaan1.ino)

### Dokumentasi
![Percobaan 4A](Dokumentasi%20Praktikum/Percobaan4A.jpg)

### Hasil Pengamatan

| No. | Perintah JSON | Pesan Diterima | Hasil Parsing | Status LED | Keterangan |
|:---:|---|---|:---:|:---:|---|
| 1 | `{"perintah":"ON"}` | `{"perintah":"ON"}` | ON | Menyala | LED menyala setelah menerima perintah "ON" |
| 2 | `{"perintah":"OFF"}` | `{"perintah":"OFF"}` | OFF | Mati | LED mati setelah menerima perintah "OFF" |
| 3 | `{"perintah":"ON"}` | `{"perintah":"ON"}` | ON | Menyala | LED menyala setelah menerima perintah "ON" |
| 4 | `{"perintah":"OFF"}` | `{"perintah":"OFF"}` | OFF | Mati | LED mati setelah menerima perintah "OFF" |

### Analisis
Perintah ON membuat LED menyala dan perintah OFF membuatnya mati. Ini menunjukkan data yang dikirim lewat MQTT berhasil diterima dan diolah ESP8266 hingga menghasilkan perubahan pada aktuator, dengan pola publish-subscribe di mana ESP8266 bertindak sebagai subscriber.

---

## 5. Percobaan 4B - Pertukaran Data Dua Arah

### Penjelasan
Pada percobaan ini komunikasi dikembangkan menjadi dua arah: ESP8266 tetap menerima perintah (*subscribe*) sekaligus mengirim data suhu (*publish*). Agar keduanya berjalan bersamaan, program tidak boleh tertahan lama oleh `delay()`, sehingga digunakan pendekatan non-blocking dengan `millis()` untuk mengatur interval publish, sementara `client.loop()` tetap berjalan untuk menangani pesan masuk.

Source code: [`Source Code/percobaan2.ino`](Source%20Code/percobaan2.ino)

### Dokumentasi
![Percobaan 4B](Dokumentasi%20Praktikum/Percobaan4B.jpg)

### Hasil Pengamatan

| No. | Waktu Pengiriman (s) | Data Suhu Dipublish | Perintah Diterima | Status LED | Data Diterima Subscriber | Keterangan |
|:---:|:---:|---|:---:|:---:|---|:---:|
| 1 | 0 | `{"suhu":25.3}` | - | Mati | - | Berhasil |
| 2 | 5 | `{"suhu":25.3}` | ON | Menyala | `{"perintah":"ON"}` | Berhasil |
| 3 | 10 | `{"suhu":25.3}` | - | Menyala | - | Berhasil |
| 4 | 15 | `{"suhu":25.3}` | ON | Menyala | `{"perintah":"ON"}` | Berhasil |
| 5 | 20 | `{"suhu":25.8}` | - | Menyala | - | Berhasil |
| 6 | 25 | `{"suhu":25.8}` | ON | Menyala | `{"perintah":"ON"}` | Berhasil |
| 7 | 30 | `{"suhu":25.8}` | - | Menyala | - | Berhasil |
| 8 | 35 | `{"suhu":25.8}` | ON | Menyala | `{"perintah":"ON"}` | Berhasil |
| 9 | 40 | `{"suhu":25.8}` | - | Menyala | - | Berhasil |
| 10 | 45 | `{"suhu":25.8}` | OFF | Mati | `{"perintah":"OFF"}` | Berhasil |

### Analisis
Komunikasi dua arah memungkinkan perangkat menerima perintah untuk mengendalikan aktuator sambil mengirim data lewat MQTT. Hal ini merupakan penerapan publish dan subscribe secara simultan, dengan broker sebagai perantara antara perangkat dan aplikasi pemantau/pengendali.

---

## 6. Flowchart callback()
Alur penerimaan dan pemrosesan pesan MQTT pada fungsi `callback()`:

![Flowchart callback()](Dokumentasi%20Praktikum/Flowchart_callback.png)

### Jawaban Singkat Pertanyaan Praktikum

**4A**
- **Jika pesan bukan JSON valid?** `deserializeJson()` menghasilkan error, program menampilkan pesan gagal parsing di Serial Monitor lalu `return`, sehingga perintah tidak diteruskan ke LED.
- **Mengapa `client.subscribe()` ada di `hubungkanMQTT()`, bukan `setup()`?** Subscribe hanya bisa dilakukan setelah terhubung ke broker, dan perlu diulang setiap kali terjadi reconnect. Jika di `setup()`, subscribe hanya dijalankan sekali saat perangkat pertama menyala.

**4B**
- **Mengapa `delay()` yang lama dihindari?** Selama `delay()`, program berhenti sehingga `client.loop()` tidak berjalan dan perintah dari broker diterima terlambat.
- **Mekanisme non-blocking `millis()`:** selisih `millis()` dengan `waktuTerakhirPublish` dicek di dalam `loop()`. Data baru dipublish bila selisihnya melebihi `intervalPublish` (5000 ms), sementara `client.loop()` terus berjalan.
- **Jika `client.loop()` hanya dipanggil tiap 10 detik?** Pesan masuk terlambat diproses, perintah LED tertunda, dan koneksi dengan broker bisa menjadi tidak stabil.

---

## 7. Kesimpulan
MQTT dapat dipakai untuk komunikasi dua arah pada sistem IoT: perangkat menerima perintah sekaligus mengirim data. Percobaan ini menunjukkan keterkaitan antara MQTT, JSON, callback, dan aktuator dalam proses pertukaran data IoT.

## 8. Kendala
Board ESP8266 pertama yang digunakan tidak dapat mendeteksi SSID WiFi sehingga koneksi gagal. Masalah teratasi setelah board diganti dengan board ESP8266 lain yang kondisinya lebih baik.
