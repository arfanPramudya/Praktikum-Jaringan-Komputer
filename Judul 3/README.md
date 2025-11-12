# Tugas Akhir Judul 3 - Praktikum Jaringan Komputer

Ini adalah dokumentasi proyek untuk lab jaringan yang dibuat menggunakan Cisco Packet Tracer. Proyek ini mencakup topologi, konfigurasi, dan hasil verifikasi konektivitas antar perangkat.

## Topologi Jaringan

Berikut adalah topologi jaringan yang digunakan dalam lab ini:

<img width="491" height="336" alt="Topologi Jaringan Tugas Akhir Judul 3" src="https://github.com/user-attachments/assets/3a04161f-3454-48b3-8363-eb4007e4aeb6" />


## Video Penjelasan

Untuk penjelasan lengkap dan tutorial langkah demi langkah mengenai konfigurasi lab ini, silakan tonton video YouTube berikut:

[**Tonton Video Konfigurasi Lab di YouTube**](https://youtu.be/MpKyZrV2BPc)

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



  Tentu, ini adalah lanjutan untuk file `README.md` Anda, yang mencakup **Part 2**.

Anda bisa menambahkan teks Markdown di bawah ini langsung ke file `README.md` Anda, tepat di bawah `Part 1`.

-----

## Part 2: Membuat VLAN dan Menetapkan Port Switch

Pada bagian ini, konfigurasi VLAN diterapkan. Beberapa VLAN (Operations, Parking\_Lot, Management, dan Native) dibuat di kedua switch. Selanjutnya, port untuk PC dan interface manajemen switch dipindahkan ke VLAN yang sesuai.

### Hasil Pengecekan Konektivitas

Setelah memindahkan port PC dan interface manajemen ke VLAN masing-masing, pengecekan konektivitas dilakukan kembali.

**Penting:** Kegagalan ping pada tahap ini **sudah diharapkan**. Ini terjadi karena port yang menghubungkan S1 dan S2 (`Fa0/1`) masih merupakan port *access* dan belum dikonfigurasi sebagai **trunk** untuk membawa trafik dari VLAN 10 dan VLAN 99.

#### 1\. Ping PC-A ke PC-B (Gagal)
<img width="868" height="351" alt="Part 2 Ping PC-A ke PC-B" src="https://github.com/user-attachments/assets/7e1a3c7f-3436-4b42-9f2d-7b671b8578af" />

  * **Hasil:** Gagal (100% loss).
  * **Analisis:** Meskipun PC-A dan PC-B sekarang berada di VLAN yang sama (VLAN 10), trafik antar-switch untuk VLAN 10 diblokir karena tidak ada *trunk link* yang mengizinkannya.

#### 2\. Ping Switch 1 ke Switch 2 (Gagal)
<img width="858" height="106" alt="Part 2 Ping Switch 1 ke Switch 2" src="https://github.com/user-attachments/assets/55f89daa-7fdd-4011-a850-d2b78aa8359f" />

  * **Hasil:** Gagal (0% success).
  * **Analisis:** Sama seperti trafik PC, trafik manajemen (VLAN 99) juga tidak dapat melewati port antar-switch karena port tersebut belum dikonfigurasi sebagai *trunk*.

Tentu, ini adalah draf untuk **Part 4** yang bisa Anda tambahkan ke file `README.md` Anda.

-----

## Part 4: Konfigurasi 802.1Q Trunk Antar Switch

Pada bagian ini, link antar switch (interface `F0/1`) dikonfigurasi sebagai *trunk* untuk mengizinkan trafik dari beberapa VLAN (VLAN 10 dan VLAN 99) melewatinya.

Konfigurasi ini awalnya menggunakan *Dynamic Trunking Protocol* (DTP). Interface `F0/1` pada S1 diatur ke mode `dynamic desirable`, yang menyebabkannya berhasil bernegosiasi menjadi *trunk* dengan S2 (yang berada dalam mode default `dynamic auto`).

### Hasil Pengecekan Konektivitas (Setelah Trunk Aktif)

Setelah link *trunk* terbentuk, konektivitas yang sebelumnya gagal di Part 2 sekarang diuji kembali.

#### 1\. Ping PC-A (VLAN 10) ke PC-B (VLAN 10)
<img width="858" height="385" alt="Part 4 Ping PC-A ke PC-B" src="https://github.com/user-attachments/assets/d887a068-d897-4cc9-80f0-fd0f86df3a7d" />

  * **Hasil:** Sukses (0% loss).
  * **Analisis:** Link trunk sekarang mengizinkan trafik untuk **VLAN 10** (Operations) lewat. Karena kedua PC berada di VLAN yang sama, mereka sekarang dapat berkomunikasi melintasi kedua switch.

#### 2\. Ping Switch 1 (VLAN 99) ke Switch 2 (VLAN 99)
<img width="841" height="220" alt="Part 4 Ping Switch 1 ke Switch 2" src="https://github.com/user-attachments/assets/3d9f2be5-f929-4295-b5b9-76e8c755965d" />

  * **Hasil:** Sukses (100% success).
  * **Analisis:** Link trunk juga mengizinkan trafik untuk **VLAN 99** (Management). Ini memungkinkan interface manajemen S1 dan S2 untuk saling berkomunikasi.

#### 3\. Ping PC-B (VLAN 10) ke Switch 2 (VLAN 99)
<img width="817" height="267" alt="Part 4 Ping PC-B ke Switch 2" src="https://github.com/user-attachments/assets/161a4a18-28e3-429f-8d16-363be886b20b" />

  * **Hasil:** Gagal (100% loss).
  * **Analisis:** Ping ini **gagal sesuai desain (inter-VLAN)**. PC-B berada di `VLAN 10`, sedangkan interface manajemen S2 berada di `VLAN 99`. Switch (Layer 2) tidak akan meneruskan trafik antara VLAN yang berbeda tanpa adanya router (Layer 3).

#### 4\. Ping PC-C (VLAN 99) ke Switch 1 & 2 (VLAN 99)
<img width="895" height="562" alt="Part 4 Ping PC-C ke Switch 1 dan Switch 2" src="https://github.com/user-attachments/assets/0fbbe047-6fcd-4e3f-a1e8-76bbbd63379e" />

  * **Hasil:** Sukses (dengan 25% loss untuk paket pertama/ARP).
  * **Analisis:** PC-C (yang tampaknya dikonfigurasi di **VLAN 99**) dapat berhasil melakukan ping ke interface manajemen S1 dan S2. Ini mengkonfirmasi bahwa PC-C berada di VLAN manajemen dan memiliki konektivitas yang benar.
