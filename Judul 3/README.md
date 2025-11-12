# Tugas Akhir Judul 3 - Praktikum Jaringan Komputer

Ini adalah dokumentasi proyek untuk lab jaringan yang dibuat menggunakan Cisco Packet Tracer. Proyek ini mencakup topologi, konfigurasi, dan hasil verifikasi konektivitas antar perangkat.

## Topologi Jaringan

Berikut adalah topologi jaringan yang digunakan dalam lab ini:

## Video Penjelasan

Untuk penjelasan lengkap dan tutorial langkah demi langkah mengenai konfigurasi lab ini, silakan tonton video YouTube berikut:

[**Tonton Tutorial Konfigurasi di YouTube**](https://youtu.be/MpKyZrV2BPc)

-----

## Part 1: Pengecekan Konektivitas (Ping)

Bagian ini berisi hasil-hasil pengecekan konektivitas awal menggunakan perintah `ping` untuk memverifikasi komunikasi antar perangkat.

### 1\. Ping PC-A ke PC-B

Pengecekan konektivitas antara dua host pada switch yang berbeda (atau sama, tergantung topologi yang lebih detail).
<img width="858" height="385" alt="Part 1 Ping PC-A ke PC-B" src="https://github.com/user-attachments/assets/428223c0-b927-447e-a6cc-8d3cf4e2187b" />

  * **Hasil:** Sukses (0% loss).
  * **Analisis:** PC-A (IP tidak terlihat) dan PC-B (192.168.10.4) dapat berkomunikasi dengan sukses.

### 2\. Ping PC-A ke Switch 1 (S1)

Pengecekan konektivitas dari PC-A ke interface manajemen Switch 1 (S1).
<img width="799" height="247" alt="Part 1 Ping PC-A ke Switch 1" src="https://github.com/user-attachments/assets/350b2cb8-37e0-42e9-8b01-4c5488a09f9d" />
  * **Hasil:** Gagal (100% loss).
  * **Analisis:** PC-A tidak dapat menjangkau interface manajemen S1 di 192.168.1.11. Ini bisa disebabkan oleh kesalahan konfigurasi IP pada PC-A, S1, atau masalah VLAN.

### 3\. Ping PC-B ke Switch 2 (S2)

Pengecekan konektivitas dari PC-B ke interface manajemen Switch 2 (S2).
<img width="813" height="378" alt="Part 1 Ping PC-B ke Switch 2" src="https://github.com/user-attachments/assets/186835bc-1b08-438d-b9a7-af86a5fb068c" />
  * **Hasil:** Gagal (100% loss).
  * **Analisis:** Sama seperti kasus PC-A, PC-B tidak dapat menjangkau interface manajemen S2 di 192.168.1.12.

### 4\. Ping PC-C ke Semua Switch

Pengecekan konektivitas dari PC-C ke interface manajemen S1 (192.168.1.11) dan S2 (192.168.1.12).
<img width="864" height="602" alt="Part 1 Ping PC-C ke Semua Switch" src="https://github.com/user-attachments/assets/593fd94e-2bfc-44e1-8f7b-6de720e9edf1" />
  * **Hasil:** Sukses dengan 25% loss (paket pertama *timeout*).
  * **Analisis:** PC-C berhasil terhubung ke kedua switch karena tidak ada default gateaway lain halnya dengan PC-A dan PC-B. Kehilangan paket pertama (`Request timed out`) adalah hal yang wajar dan biasanya disebabkan oleh proses ARP (Address Resolution Protocol) yang sedang bekerja untuk menemukan alamat MAC switch.

### 5\. Ping Switch 1 (S1) ke Switch 2 (S2)

Pengecekan konektivitas antar switch, kemungkinan melalui link *trunk*.
<img width="826" height="217" alt="Part 1 Ping Switch 1 ke Switch 2" src="https://github.com/user-attachments/assets/8b29c311-5e4e-40fc-b18a-798a138d3659" />
  * **Hasil:** Percobaan pertama 60% sukses, percobaan kedua 100% sukses.
  * **Analisis:** Koneksi antar switch berfungsi. Kehilangan beberapa paket pada percobaan pertama (ditandai dengan `.!!!!`) bisa jadi karena proses seperti Spanning Tree Protocol (STP) yang sedang *converging* atau ARP. Percobaan kedua yang sukses 100% (`!!!!!`) mengkonfirmasi bahwa link sudah stabil.
