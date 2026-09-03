# 🌡️ Praktikum Sistem Internet of Things
## ESP32 — Sensor DHT11 & Kendali Relay

<p align="center">
  <img src="https://img.shields.io/badge/ESP32-Microcontroller-blue?style=for-the-badge&logo=espressif" alt="ESP32">
  <img src="https://img.shields.io/badge/DHT11-Sensor-green?style=for-the-badge" alt="DHT11">
  <img src="https://img.shields.io/badge/Arduino-C%2FC%2B%2B-00979D?style=for-the-badge&logo=arduino" alt="Arduino">
  <img src="https://img.shields.io/badge/IoT-Praktikum-orange?style=for-the-badge" alt="IoT">
</p>

<p align="center">
  <b>📚 Praktikum Modul 1 — Akuisisi Data Sensor dan Kendali Aktuator</b>
</p>

---

## 👨‍💻 Identitas Praktikan

| Keterangan | Detail |
|---|---|
| 👤 Nama | **Muhammad Nabil Zaedan Agesy** |
| 🆔 NIM | **H1H024062** |
| 🎓 Program Studi | **Teknik Komputer** |
| 📖 Mata Kuliah | **Sistem Internet of Things** |
| 📦 Modul | **Modul 1** |

---

# 🎯 Tujuan Praktikum

Praktikum ini bertujuan untuk memahami proses akuisisi data sensor dan pengendalian aktuator menggunakan ESP32.

### Tujuan:

- 🌡️ Membaca suhu menggunakan sensor DHT11.
- 💧 Membaca kelembaban menggunakan sensor DHT11.
- 📊 Menampilkan hasil pembacaan sensor melalui Serial Monitor.
- 🔌 Mengendalikan relay berdasarkan nilai suhu.
- 🧠 Memahami penggunaan nilai threshold.
- 🔄 Menganalisis hubungan antara data sensor dengan respons aktuator.

---

# 🧰 Komponen yang Digunakan

### Percobaan 1 — Tanpa Relay

- ESP32
- Sensor DHT11
- Breadboard
- Kabel jumper
- Kabel USB

### Percobaan 2 — Dengan Relay

- ESP32
- Sensor DHT11
- Modul Relay
- Breadboard
- Kabel jumper
- Kabel USB

---

# 🔌 Konfigurasi Pin

| Komponen | Pin ESP32 |
|---|---|
| 🌡️ DHT11 DATA | `GPIO 4` |
| 🔌 Relay IN | `GPIO 14` |
| 🔴 DHT11 VCC | `3.3V` |
| ⚫ DHT11 GND | `GND` |

---

# 📸 Dokumentasi Rangkaian

## 1️⃣ Rangkaian DHT11 — Tanpa Relay

<p align="center">
  <img src="./Dokumentasi/Percobaan1.jpeg" width="750" alt="Rangkaian DHT11 Tanpa Relay">
</p>

Rangkaian pertama digunakan untuk membaca suhu dan kelembaban dari sensor DHT11 menggunakan ESP32.

### 🔄 Alur Sistem

```text
┌─────────────┐
│    DHT11    │
│ Suhu & RH   │
└──────┬──────┘
       │
       ▼
┌─────────────┐
│    ESP32    │
└──────┬──────┘
       │
       ▼
┌─────────────────────┐
│ Pembacaan Sensor    │
│ Suhu & Kelembaban   │
└──────────┬──────────┘
           │
           ▼
┌─────────────────────┐
│   Serial Monitor    │
└─────────────────────┘
```

---

## 2️⃣ Rangkaian DHT11 + Relay

<p align="center">
  <img src="./Dokumentasi/Percobaan2.jpeg" width="750" alt="Rangkaian DHT11 Dengan Relay">
</p>

Pada percobaan kedua, ESP32 menggunakan hasil pembacaan suhu DHT11 untuk menentukan kondisi relay berdasarkan nilai threshold.

### 🔄 Alur Sistem

```text
┌─────────────┐
│    DHT11    │
└──────┬──────┘
       │
       ▼
┌─────────────┐
│    ESP32    │
└──────┬──────┘
       │
       ▼
┌─────────────────┐
│    Baca Suhu    │
└────────┬────────┘
         │
         ▼
┌─────────────────┐
│  Suhu > 30°C ?  │
└───────┬─────────┘
        │
   ┌────┴────┐
   │         │
  YA       TIDAK
   │         │
   ▼         ▼
┌──────┐  ┌───────┐
│ RELAY│  │ RELAY │
│  ON  │  │  OFF  │
└──────┘  └───────┘
```

---

# 📐 Skematik Rangkaian

## 🔹 Skematik Percobaan 1 — DHT11

<p align="center">
  <img src="./Skematik%20Rangkaian/percobaan1.png" width="750" alt="Skematik DHT11">
</p>

Sensor DHT11 terhubung ke ESP32 dengan pin data pada `GPIO 4`.

---

## 🔹 Skematik Percobaan 2 — DHT11 + Relay

<p align="center">
  <img src="./Skematik%20Rangkaian/percobaan2.png" width="750" alt="Skematik DHT11 dan Relay">
</p>

Pada rangkaian ini, pin kendali relay dihubungkan ke `GPIO 14` ESP32.

---

# 💻 Source Code

## 🌡️ Percobaan 1 — DHT11 Tanpa Relay

File program: `Source Code/percobaan1.ino`

```cpp
#include <DHT.h>

#define DHTPIN 4
#define DHTTYPE DHT11

DHT dht(DHTPIN, DHTTYPE);

void setup() {
  Serial.begin(115200);
  dht.begin();

  Serial.println("Memulai akuisisi data sensor DHT11...");
}

void loop() {
  // Membaca data kelembaban dan suhu
  float kelembaban = dht.readHumidity();
  float suhu = dht.readTemperature();

  // Periksa apakah pembacaan berhasil
  if (isnan(kelembaban) || isnan(suhu)) {
    Serial.println("Gagal membaca data dari sensor DHT11!");
  } else {
    Serial.print("Suhu: ");
    Serial.print(suhu);
    Serial.print(" °C, Kelembaban: ");
    Serial.print(kelembaban);
    Serial.println(" %");
  }

  delay(2000);
}
```

### 📌 Cara Kerja

1. ESP32 melakukan inisialisasi sensor DHT11.
2. Sensor membaca suhu dan kelembaban.
3. Fungsi `isnan()` digunakan untuk memeriksa validitas data.
4. Data ditampilkan melalui Serial Monitor.
5. Pembacaan dilakukan setiap 2 detik.

---

# 🔌 Percobaan 2 — DHT11 Dengan Relay

File program: `Source Code/percobaan2.ino`

```cpp
#include <DHT.h>

#define DHTPIN 4
#define DHTTYPE DHT11
#define RELAYPIN 14

DHT dht(DHTPIN, DHTTYPE);

const float suhuThreshold = 30.0;

void setup() {
  Serial.begin(115200);
  dht.begin();

  pinMode(RELAYPIN, OUTPUT);
  digitalWrite(RELAYPIN, LOW);
}

void loop() {
  // Membaca suhu dari sensor
  float suhu = dht.readTemperature();

  // Mengecek apakah pembacaan sensor berhasil
  if (isnan(suhu)) {
    Serial.println("Gagal membaca data sensor!");
  } else {
    Serial.print("Suhu: ");
    Serial.print(suhu);
    Serial.print(" °C -> ");

    // Kendali relay berdasarkan threshold suhu
    if (suhu > suhuThreshold) {
      digitalWrite(RELAYPIN, HIGH);
      Serial.println("Aktuator: ON");
    } else {
      digitalWrite(RELAYPIN, LOW);
      Serial.println("Aktuator: OFF");
    }
  }

  delay(2000);
}
```

---

# 🧠 Logika Kendali Relay

Relay dikendalikan berdasarkan nilai **threshold sebesar 30°C**.

| Kondisi Suhu | Kondisi Relay | Aktuator |
|---|---|---|
| `≤ 30°C` | `LOW` | 🔴 OFF |
| `> 30°C` | `HIGH` | 🟢 ON |

### Relay OFF

```text
Suhu = 28°C
Threshold = 30°C

28°C > 30°C ❌
        ↓
    RELAY OFF
```

### Relay ON

```text
Suhu = 30.20°C
Threshold = 30°C

30.20°C > 30°C ✅
        ↓
     RELAY ON
```

---

# 📊 Hasil Pengamatan

| No | Kondisi Sensor | Suhu | Kelembaban | Status |
|---:|---|---:|---:|---|
| 1 | Normal | 25°C | 61% | ✅ Valid |
| 2 | Didekati tangan | 29.80°C | 75% | ✅ Valid |
| 3 | Didekati AC | 19.80°C | 72% | ✅ Valid |
| 4 | Didekati api | 30.20°C | 50% | ✅ Valid |

Hasil pengamatan menunjukkan bahwa perubahan kondisi lingkungan di sekitar sensor memengaruhi nilai suhu dan kelembaban yang terbaca.

---

# 🖥️ Dokumentasi Serial Monitor

## 📊 Percobaan 1

<p align="center">
  <img src="./Dokumentasi/Serial%20MOnitor1.png" width="750" alt="Serial Monitor Percobaan 1">
</p>

---

## 📊 Percobaan 2

<p align="center">
  <img src="./Dokumentasi/Serial%20Monitor2.png" width="750" alt="Serial Monitor Percobaan 2">
</p>

---

# 🔄 Alur Keseluruhan Sistem

```text
              ┌─────────────┐
              │    DHT11    │
              │             │
              │ Suhu + RH   │
              └──────┬──────┘
                     │
                     ▼
              ┌─────────────┐
              │    ESP32    │
              └──────┬──────┘
                     │
            ┌────────┴────────┐
            │                 │
            ▼                 ▼
   ┌─────────────────┐  ┌─────────────────┐
   │ Serial Monitor  │  │ Threshold Suhu  │
   └─────────────────┘  └────────┬────────┘
                                  │
                           ┌──────▼──────┐
                           │ Suhu > 30°C │
                           │      ?      │
                           └──────┬──────┘
                                  │
                      ┌───────────┴───────────┐
                      │                       │
                     YA                     TIDAK
                      │                       │
                      ▼                       ▼
                 ┌─────────┐             ┌─────────┐
                 │  RELAY  │             │  RELAY  │
                 │   ON    │             │   OFF   │
                 └─────────┘             └─────────┘
```

---

# 📁 Struktur Repository

```text
Praktikum Modul 1/
│
├── 📄 README.md
│
├── 📂 Dokumentasi/
│   ├── 🖼️ Percobaan1.jpeg
│   ├── 🖼️ Percobaan2.jpeg
│   ├── 🖼️ Serial MOnitor1.png
│   └── 🖼️ Serial Monitor2.png
│
├── 📂 Skematik Rangkaian/
│   ├── 🖼️ percobaan1.png
│   └── 🖼️ percobaan2.png
│
└── 📂 Source Code/
    ├── 💻 percobaan1.ino
    └── 💻 percobaan2.ino
```

---

# 📝 Kesimpulan

Praktikum ini menunjukkan bahwa ESP32 dapat digunakan untuk melakukan akuisisi data dari sensor DHT11 dan mengolah hasil pembacaan tersebut untuk mengendalikan aktuator.

Pada percobaan pertama, ESP32 digunakan untuk membaca suhu dan kelembaban kemudian menampilkannya pada Serial Monitor.

Pada percobaan kedua, nilai suhu digunakan sebagai dasar pengambilan keputusan untuk mengaktifkan atau menonaktifkan relay. Relay akan aktif ketika suhu melebihi `30°C` dan akan mati ketika suhu berada pada atau di bawah threshold tersebut.

---

<p align="center">

### 🌡️ DHT11 → 🧠 ESP32 → 📊 Data → 🔌 Relay

<br>

<b>Praktikum Sistem Internet of Things — Modul 1</b>

<br><br>

Muhammad Nabil Zaedan Agesy

<br>

H1H024062

</p>
