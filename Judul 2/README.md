# 🚀 Lab 10.4.4: Membangun Jaringan Switch dan Router

Dokumentasi ini mencatat penyelesaian untuk tugas praktikum **Lab 10.4.4 - Build a Switch and Router Network**.

## 🎯 Tujuan Praktikum

Tujuan utama dari lab ini adalah untuk mengkonfigurasi topologi jaringan yang terdiri dari satu router (R1), satu switch (S1), dan dua PC (PC-A & PC-B). Fokus utamanya adalah mengkonfigurasi router agar dapat me-routing paket antara dua subnet yang berbeda, sehingga **PC-A** dan **PC-B** dapat saling berkomunikasi.

## 🗺️ Skenario & Topologi

Berdasarkan modul praktikum, topologi yang digunakan memisahkan PC-A dan PC-B ke dalam dua jaringan (subnet) yang berbeda:

* **PC-A** berada di jaringan `192.168.1.0/24`.
* **PC-B** berada di jaringan `192.168.0.0/24`.

Router R1 berfungsi sebagai jembatan (gateway) untuk kedua jaringan tersebut.

### Tabel Pengalamatan (Addressing Table)

Berikut adalah konfigurasi IP kunci yang digunakan dalam praktikum ini:

| Device | Interface | IP Address | Default Gateway |
| :--- | :--- | :--- | :--- |
| R1 | G0/0/0 | `192.168.0.1/24` | N/A |
| R1 | G0/0/1 | `192.168.1.1/24` | N/A |
| PC-A | NIC | `192.168.1.3/24` | `192.168.1.1` |
| PC-B | NIC | `192.168.0.3/24` | `192.168.0.1` |

---

## 📊 Hasil Tes Konektivitas (PING)

Berikut adalah bukti screenshot *sebelum* dan *sesudah* konfigurasi router dilakukan.

### ❌ Kondisi Awal (Sebelum Konfigurasi)

Sebelum interface router (default gateway) dikonfigurasi, PC-B **tidak dapat** menjangkau PC-A. Paket PING gagal total (100% loss) karena PC-B tidak tahu rute untuk mencapai jaringan `192.168.1.0/24`.

**Ping dari PC-B (`192.168.0.3`) ke PC-A (`192.168.1.3`) - GAGAL**
![Ping Gagal 100% Loss](PC-B%20ke%20PC-A%20(1).png)

### ✅ Kondisi Akhir (Setelah Konfigurasi Berhasil)

Setelah interface router R1 (G0/0/0 dan G0/0/1) dan *default gateway* pada setiap PC diatur sesuai tabel pengalamatan, router berhasil me-routing paket antar kedua subnet.

**1. Ping dari PC-A (`192.168.1.3`) ke PC-B (`192.168.0.3`) - SUKSES**
![Ping Sukses dari PC-A ke PC-B](PC-A%20ke%20PC-B.png)

**2. Ping dari PC-B (`192.168.0.3`) ke PC-A (`192.168.1.3`) - SUKSES**
![Ping Sukses dari PC-B ke PC-A](PC-B%20ke%20PC-A%20(3).png)

---

### 💡 Analisis Tambahan: Kenapa Terjadi 25% Packet Loss di Awal?

Anda mungkin memperhatikan bahwa pada tes PING pertama setelah konfigurasi, terjadi 25% packet loss (satu paket "Request timed out").

![Ping Pertama dengan 25% Loss](PC-B%20ke%20PC-A%20(2).png)

**Ini adalah hal yang normal!**

Ini terjadi karena proses **ARP (Address Resolution Protocol)**. Ketika PC-B mengirim paket pertama ke PC-A:

1.  Paket dikirim ke *default gateway* (Router R1).
2.  Router R1 tahu harus mengirimnya ke jaringan `192.168.1.0/24`, tapi R1 belum tahu alamat MAC (hardware) dari PC-A (`192.168.1.3`).
3.  Paket PING pertama *time out* sementara R1 mengirimkan *ARP broadcast* di jaringan `192.168.1.0/24` untuk bertanya, "Siapa pemilik IP 192.168.1.3?"
4.  PC-A merespons ARP dengan alamat MAC-nya.
5.  Setelah R1 mendapatkan alamat MAC, paket PING ke-2, 3, dan 4 berhasil terkirim.

Pada tes PING selanjutnya (seperti yang terlihat pada gambar sukses 0% loss), proses ARP tidak diperlukan lagi karena informasinya sudah tersimpan di *ARP table* router.
