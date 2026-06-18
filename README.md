# ASEP-STM32: Adaptive Safety-Embedded Programming Framework

> Proyek ETS Pemrograman Kontroler Kelas 4B — STM32F401VE Bare-Metal Safety System

---

## 📁 Struktur Repository

```
ASEP-STM32/
├── Data_Analysis/       # Grafik GNUPlot (2D & 3D), Diagram Blok, Flowchart
├── Firmware/            # Source code Keil uVision (Core, Drivers, .ioc)
├── Report/              # Laporan LaTeX lengkap (main.tex) + gambar pendukung
├── Simulation/          # File simulasi Proteus 8 (.rar)
└── README.md
```

---

## 📖 Deskripsi Proyek

ASEP-STM32 adalah framework bare-metal ringan untuk mikrokontroler **STM32F401VE** yang mengintegrasikan:
- Kontrol **Adaptive PID-lite** berbasis pembacaan ADC secara polling
- Mekanisme **Safety Shutdown** di level hardware (PWM → 0%) secara otonom
- Pemantauan **Stack Sentinel** dan **Independent Watchdog (IWDG)**
- Telemetri real-time via **UART dengan CRC-32** untuk validasi integritas data

Proyek ini merupakan sintesis dari **15 jurnal ilmiah** (Scopus/WoS, 2021–2026) yang menjawab 5 pilar *future work* di bidang sistem tertanam yang aman dan efisien.

---

## 🏗️ Arsitektur & Alur Kerja

Sistem beroperasi dalam **3 Mode** yang ditentukan secara langsung dari posisi sensor (RV1/ADC):

| Mode | Posisi RV1 | Nilai ADC | PWM Output | LED |
|------|-----------|-----------|------------|-----|
| **INFO (Normal)** | 0% – 40% | 0 – 1638 | Stabil ~50% | Menyala terus |
| **WARN (Warning)** | 41% – 70% | 1639 – 2867 | Adaptif (PID-lite) | Berkedip lambat |
| **CRIT (Critical)** | 71% – 100% | 2868 – 4095 | **0% (Hard-Stop)** | **Mati total** |

![Diagram Blok](Data_Analysis/Diagram%20blokk.jpg)

---

## 🚀 Cara Menjalankan Simulasi

### Langkah 1: Compile Firmware
1. Buka folder `Firmware/MDK-ARM/` menggunakan **Keil uVision**.
2. Tekan **F7** (Rebuild All) untuk mengompilasi project.
3. Pastikan tidak ada error. File `.hex` akan terbuat otomatis di dalam folder `MDK-ARM/`.

### Langkah 2: Jalankan Simulasi Proteus
1. Ekstrak file `.rar` di dalam folder `Simulation/`.
2. Buka file skematik Proteus 8 (`.pdsprj`).
3. Double-click komponen **STM32F401VE** dan arahkan ke file `.hex` hasil compile tadi.
4. Tekan tombol **Play (▶)** untuk menjalankan simulasi.

### Langkah 3: Interaksi & Monitoring
- **Putar RV1** untuk mensimulasikan perubahan kondisi sensor.
- Pantau **Virtual Terminal** untuk melihat log UART (`[INFO]`, `[WARN]`, `[CRIT]`).
- Pantau **Oscilloscope** untuk melihat perubahan gelombang PWM secara real-time.

---

## 📊 Hasil Simulasi & Analisis

### Grafik 2D: Respons Transien Sistem
Grafik berikut menunjukkan perubahan Duty Cycle PWM terhadap perubahan tegangan sensor seiring waktu:

![Grafik 2D](Data_Analysis/grafik_asep.png)

### Grafik 3D: Permukaan Kendali Adaptif
Grafik 3D berikut memvisualisasikan hubungan antara tegangan sensor, tingkat variansi, dan respons PWM secara holistik. Terlihat jelas permukaan bidang kendali yang "runtuh" ke 0% saat memasuki zona Critical:

![Grafik 3D](Data_Analysis/grafik_3D_ASEP.png)

---

## 💡 Keunggulan ASEP-STM32

| Aspek | Metode Konvensional (RTOS/TinyML) | ASEP-STM32 |
|-------|-----------------------------------|------------|
| **Memori RAM** | > 20 KB | **< 2 KB** |
| **Latensi Respons** | Bervariasi (jitter) | **Deterministik O(1)** |
| **Keselamatan** | Berbasis cloud/analisis eksternal | **Hardware Hard-Stop otonom** |
| **Portabilitas** | Bergantung platform | **Agnostik STM32 HAL C/C++** |

---

## 📚 Referensi Jurnal

Proyek ini dibangun berdasarkan sintesis **15 jurnal ilmiah** dari basis data Scopus dan Web of Science (2021–2026). Daftar lengkap terdapat di file `Report/main.tex`.
