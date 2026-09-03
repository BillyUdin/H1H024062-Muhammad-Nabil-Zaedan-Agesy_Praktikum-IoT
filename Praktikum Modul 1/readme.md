# 🌡️ Praktikum Internet of Things — ESP32 & DHT11

![ESP32](https://img.shields.io/badge/Microcontroller-ESP32-blue)
![DHT11](https://img.shields.io/badge/Sensor-DHT11-green)
![Arduino](https://img.shields.io/badge/Platform-Arduino-00979D)
![Language](https://img.shields.io/badge/Language-C%2FC%2B%2B-orange)

Repository ini berisi hasil **Praktikum Sistem Internet of Things (IoT)** menggunakan mikrokontroler **ESP32** dan sensor **DHT11** untuk melakukan pembacaan suhu dan kelembaban.

Pada praktikum ini terdapat dua percobaan utama:

1. 🌡️ Pembacaan suhu dan kelembaban menggunakan DHT11
2. 🔌 Pengendalian relay berdasarkan nilai suhu

---

## 👨‍💻 Identitas

| Keterangan | Data |
|---|---|
| **Nama** | Muhammad Nabil Zaedan Agesy |
| **NIM** | H1H024062 |
| **Program Studi** | Teknik Komputer |
| **Mata Kuliah** | Sistem Internet of Things |
| **Modul** | Modul 1 |

---

# 🎯 Tujuan Praktikum

Praktikum ini bertujuan untuk:

- Memahami proses akuisisi data sensor menggunakan mikrokontroler.
- Membaca nilai suhu dan kelembaban menggunakan sensor DHT11.
- Memahami penggunaan sensor sebagai sumber data pada sistem IoT.
- Mengendalikan aktuator berdasarkan data dari sensor.
- Menganalisis hubungan antara perubahan suhu dengan respons aktuator.

---

# 🧰 Komponen yang Digunakan

### Percobaan 1 — Tanpa Relay

- ESP32
- Sensor DHT11
- Breadboard
- Kabel jumper
- Kabel USB

### Percobaan 2 — Menggunakan Relay

- ESP32
- Sensor DHT11
- Modul relay
- Breadboard
- Kabel jumper
- Kabel USB
- LED/beban sebagai indikator

---

# 🔌 Konfigurasi Pin

| Komponen | ESP32 |
|---|---|
| DHT11 Data | GPIO 4 |
| Relay IN | GPIO 14 |
| DHT11 VCC | 3.3V |
| DHT11 GND | GND |

> Relay hanya digunakan pada percobaan kedua.

---

# 📸 Dokumentasi Rangkaian

## 1. Rangkaian DHT11 — Tanpa Relay

![Rangkaian DHT11](IMG_5389.jpg)

Rangkaian pertama digunakan untuk membaca **suhu dan kelembaban** dari sensor DHT11 menggunakan ESP32.

Alur sistem:

```text
DHT11
  │
  ▼
ESP32
  │
  ▼
Pembacaan Suhu & Kelembaban
  │
  ▼
Serial Monitor