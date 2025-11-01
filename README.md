# 🛡️ Android Security Bypass Generator (ASBG)
## Alat Otomatisasi Skrip Frida untuk Melewati Deteksi Keamanan

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)
[![Python Version](https://img.shields.io/badge/Python-3.8%2B-blue)](https://www.python.org/)
[![Frida Supported](https://img.shields.io/badge/Frida-Supported-green)](https://frida.re/)

## 📝 Deskripsi Proyek

**Android Security Bypass Generator (ASBG)** adalah *tool* berbasis Python yang dirancang untuk menganalisis kode sumber atau APK aplikasi Android dan secara otomatis menghasilkan (generate) skrip JavaScript untuk **Frida**.

Skrip Frida yang dihasilkan bertujuan untuk menonaktifkan atau melewati (bypass) berbagai mekanisme pemeriksaan keamanan yang sering diimplementasikan dalam aplikasi seluler, terutama yang terkait dengan:

* **Deteksi Root/Jailbreak:** Melewati pemeriksaan keberadaan file biner `su`, properti sistem yang mencurigakan, atau aplikasi manajer *root*.
* **SSL Pinning (Certificate Verification):** Menonaktifkan validasi sertifikat SSL/TLS, memungkinkan *proxy* seperti Burp Suite untuk mencegat lalu lintas jaringan terenkripsi aplikasi.
* **Deteksi Debugger/Emulator:** Melewati pemeriksaan apakah aplikasi sedang dijalankan dalam lingkungan yang mencurigakan.

### ⚙️ Bagaimana Cara Kerjanya? (Prinsip Kerja)

1.  **Analisis Input:** Alat ini memproses aplikasi (`.apk`) untuk mencari pola-pola pemanggilan fungsi Java yang diketahui digunakan untuk pemeriksaan keamanan (disebut **detections**).
2.  **Pengelompokan:** Deteksi dikelompokkan berdasarkan Kelas Java, Metodenya, dan *Signature* (tipe parameter) metodenya.
3.  **Generasi Skrip:** Berdasarkan setiap deteksi, fungsi Python (`generate_frida_script`) akan membuat kode JavaScript yang:
    * Memuat kelas Java target (`Java.use("com.target.Class")`).
    * Mengaitkan (hook) metode spesifik dengan *overload* yang benar (`.overload().implementation`).
    * Menyuntikkan kode bypass, seperti `return false;` atau mengizinkan koneksi SSL.
4.  **Eksekusi:** Skrip Frida yang dihasilkan kemudian dapat dieksekusi menggunakan *tool* `frida-cli` untuk secara dinamis memodifikasi perilaku aplikasi saat berjalan di perangkat Android (atau emulator) yang telah terinstal `frida-server`.

---

## 🚀 Persyaratan Sistem

Pastikan Anda memiliki alat dan dependensi berikut terinstal di lingkungan Anda:

1.  **Python 3.8+**
2.  **Java Runtime Environment (JRE)**
3.  **Frida Tools**: Untuk menjalankan skrip.
4.  **ADB (Android Debug Bridge)**: Untuk berkomunikasi dengan perangkat Android.
5.  **APKTool**: **PENTING!** Diperlukan untuk proses dekompilasi dan analisis.

---

## 📥 Panduan Instalasi (Langkah demi Langkah)

### Langkah 1: Instalasi Prasyarat

#### A. Menginstal Frida Tools

Gunakan pip untuk menginstal *client* Frida di komputer Anda:

```bash
pip install frida-tools
