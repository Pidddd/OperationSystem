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

Jawab :<br>
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

## Praktikum 2.3 — Identifikasi Storage dan Filesystem
1. Lihat daftar disk/partisi:
```
lsblk -f
```
![Disk](Images/2.3%20Disk.png "Disk/Partisi")

2. Tampilkan UUID dan tipe filesystem:
```
sudo blkid
```
![UUID](Images/2.3%20UUID.png "UUID")

3. Lihat mount point untuk root filesystem:
```
findmnt /
```
![root filesystem](Images/2.3%20root%20filesystem.png "root filesystem")

## Praktikum 2.4 — Melihat Modul Aktif dan Informasinya
1. Cek versi kernel:
```
uname -r
```
![Kernel](Images/2.4%20Kernel.png "")

2. Tampilkan daftar modul aktif:
```
lsmod | head
```
![Modul](Images/2.4%20Modul%20Aktif.png "")

3. Pilih salah satu modul (contoh aman: loop) dan lihat detailnya:
```
modinfo loop
```
![Modul1](Images/2.4%20Satu%20Modul.png "")

4. Muat modul (jika belum aktif), lalu verifikasi:
```
1 sudo modprobe loop
2 lsmod | grep -i loop
```
![MuatModul](Images/2.4%20Muat%20Modul.png "")

5. (Opsional) lihat pesan kernel terbaru:
```
dmesg -T | tail -n 20
```
![Opsional](Images/2.4%20Opsi%20Kernel.png "")

## Praktikum 2.5 — Konfigurasi Auto-load dan Blacklist
1. Buat file auto-load:
```
echo " loop " | sudo tee / etc / modules - load . d / loop . conf
```
![AutoLoad](Images/2.5%20AutoLoad.png "")

2. Simulasikan verifikasi (tanpa reboot) dengan memastikan modul sudah aktif:
```
lsmod | grep -i loop
```
![Verif](Images/2.5%20Verifikasi.png "")

3. (Opsional, konsep) blacklist modul:
```
1 # echo " blacklist loop " | sudo tee / etc/ modprobe .d/blacklist - loop . conf
```
![Blacklist](Images/2.5%20Opsi%20Blacklist.png "")

## Praktikum 2.6 — Mengenali Block vs Character Device
1 Manajemen Perangkat Keras & Perintah Dasar Sistem Operasi
```
 ls -l / dev / sda
 ```
 ![Hardware](Images/2.6%20Manajemen.png "")

 2. Lihat detail device terminal:
```
ls -l / dev / tty
```
![Terminal Device](Images/2.6%20Devicw%20Terminal.png "")

3. Lihat disk dan partisi untuk mengaitkan dengan /dev:
```
lsblk
```
![Disk and Partisi](Images/2.6%20disk.png "")

### Latihan 2.3
Dari output ls -l, jelaskan perbedaan penanda file untuk block device dan character device. (Hint: karakter pertama pada permission string)

Jawab:
* Block Device (Penanda 'b'): Terlihat pada Gambar percobaan pertama untuk perangkat /dev/sda, di mana karakter paling awal dari string permission adalah huruf b. Ini menunjukkan perangkat yang mengelola data dalam bentuk blok besar, seperti harddisk atau SSD.

* Character Device (Penanda 'c'): Terlihat pada Gambar percobaann kedua untuk perangkat /dev/tty, di mana karakter paling awal dari string permission adalah huruf c. Ini menunjukkan perangkat yang mengelola data karakter per karakter secara berurutan, seperti terminal atau keyboard.

