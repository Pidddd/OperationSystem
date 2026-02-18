# Laporan Praktikum Sistem Operasi Jobsheet 1

<h4> Nama   : Ahmad Rafid Riqkullah <h4>
<h4> NIM    : 254107020078 <h4>
<h4> Kelas  : TI-1G <h4>

## Praktikum 2.1 - Identifikasi CPU dan Memory
1. Tampilkan Informasi CPU:
```    
lscpu
```
![CPU Information](Images/2.1%20CPU.png "cpu")

2. Tampilkan Ringkasan Memory
```
free -h
```
![Memory Information](Images/2.1%20Memory.png "Memory")

3. (Opsional) cek informasi hardware dari DMI/BIOS (butuh sudo):
```
sudo dmidecode -t system
```
![DMI / BIOS](Images/2.1%20DMIorBIOS.png "DMI or BIOS")

### Latihan 2.1
Catat: (1) jumlah CPU(s), core/thread, (2) total RAM, (3) total swap. Jelaskan perbedaan RAM vs swap dalam 2–3 kalimat.

Jawab : 
* Informasi CPU
Jumlah CPU (Logical cores): 8
Core per socket: 4 Core fisik.
Thread per core: 2 (Artinya setiap core fisik bisa mengerjakan 2 tugas sekaligus).

* Informasi Memori
Total RAM: 3.7 GiB (Sekitar 4 GB).
Total Swap: 1.0 GiB.

* Apa bedanya RAM vs Swap?
RAM adalah memori utama yang sangat cepat untuk memproses data aplikasi yang sedang kamu buka. Sementara itu, Swap berfungsi sebagai area perluasan di dalam hard disk atau SSD yang digunakan sistem hanya saat RAM fisik sudah hampir penuh, guna mencegah komputer hang atau aplikasi tertutup tiba-tiba.

## Praktikum 2.2 - Identifikasi Perangkat PCI/USB dan Driver
1. Lihat daftar perangkat PCI:
```
lspci
```
![PCI](Images/2.2%20PCI.png "PCI")

2. Lihat perangkat PCI beserta driver kernel yang digunakan:
```
lspci - nnk
```
![Driver PCI](Images/2.2%20Driver%20PCI.png "Driver PCI")

3. Fokus pada NIC (Ethernet) untuk mencari modul driver:
```
lspci - nnk | grep - A3 -i ethernet
```
![NIC](Images/2.2%20NIC&Driver.png "NIC and Driver")

4. Lihat perangkat USB:
```
lsusb
```
![USB](Images/2.2%20USB.png "USB")
* <font color="red"> Perintah lsusb tidak menampilkan hasil apa pun karena perangkat USB fisik belum di-mounting atau dihubungkan dari sistem utama (Windows) ke dalam sistem virtual (WSL). Akibatnya, sistem Linux tidak dapat mendeteksi adanya perangkat keras USB yang terpasang. <font>

5. Lihat topologi USB (tree):
```
lsusb -t
```
![TopologiUSB](Images/2.2%20Topologi%20USB.png "TopologiUSB")

### Latihan 2.2
Temukan 1 perangkat PCI (misal NIC) dan tuliskan: Vendor:Device ID (angka
heksadesimal), nama driver/modul kernel, dan deskripsi singkat fungsinya.

Jawab   :
* Vendor:Device ID: 1414:008e
(Penjelasan: Angka heksadesimal ini ditemukan di dalam kurung kotak di baris pertama perangkat tersebut).
* Nama Driver/Modul Kernel: dxgkrnl
(Penjelasan: Terlihat pada baris "Kernel driver in use").
* Deskripsi Singkat Fungsinya:  Perangkat ini berfungsi sebagai pengontrol grafis 3D virtual yang memungkinkan sistem Linux kamu (WSL) menggunakan tenaga kartu grafis (GPU) dari Windows untuk menjalankan aplikasi yang butuh performa visual tinggi atau perhitungan berat.