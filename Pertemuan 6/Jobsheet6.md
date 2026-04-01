# Laporan Praktikum Sistem Operasi Jobsheet 6

<h4> Nama   : Ahmad Rafid Riqkullah <h4>
<h4> NIM    : 254107020078 <h4>
<h4> Kelas  : TI-1G <h4>

# Manajemen Proses
## Praktikum 6.1 — Melihat Proses dan Thread
![](images/6.1%201.png "")
![](images/6.1%202.png "")

## Latihan 6.1
![](images/L6.1.png "")
* Jalankan ps aux dan amati outputnya:
1. Berapa total proses yang berjalan? Proses apa yang memiliki PID terkecil?
2. Jalankan pstree -p dan temukan proses bash Anda. Proses apa yang menjadi induk (PPID) dari bash tersebut?
3. Bandingkan output ps aux dan ps aux -L. Apa perbedaan yang Anda lihat?
* Jawaban Latihan 6.1
1. Total & PID Terkecil: Ada banyak proses yang berjalan. Proses dengan PID paling kecil adalah PID 1, yaitu proses 'init' (bisa dibilang ini "bapak" dari semua proses di Linux).
2. Induk proses bash: Dari gambar pstree, proses bash (terminalmu) dihidupkan oleh induk proses bernama 'Relay' atau 'login'.
3. Beda ps aux dan ps aux -L: Kalau pakai tambahan -L, Linux akan memunculkan detail LWP dan NLWP. Intinya, ini untuk melihat thread (anak cabang atau tugas-tugas kecil dari sebuah proses utama).

## Praktikum 6.2 — Mengamati Siklus Hidup Proses
![](images/6.2.png "")

## Latihan 6.2
![](images/L6.2.png6 "")
1. Jalankan sleep 120 & dan amati kolom STAT pada ps aux. Kondisi apa yang ditampilkan? Mengapa proses sleep berada di kondisi tersebut?
2. Jalankan beberapa perintah yang berhasil dan yang gagal, lalu catat exit code masing-masing. Pola apa yang Anda temukan?
* Jawaban Latihan 6.2
1. Kondisi sleep 120: Statusnya adalah S (Sleep). Artinya, proses ini sedang "tidur" atau istirahat sebentar karena sedang menunggu waktu 120 detiknya habis.
2. Pola Exit Code: Kalau perintahnya berhasil/sukses, kodenya adalah 0. Tapi kalau perintahnya gagal atau ada error, kodenya berupa angka selain nol (misalnya angka 2).

## Praktikum 6.3 — Mengatur Prioritas Proses
![](images/6.3.png "")

## Latihan 6.3
![](images/L6.3.png "")
1. Jalankan nice -n 5 sleep 200 & dan verifikasi nilai NI-nya dengan ps.
2. Ubah nilai nice menjadi 10 menggunakan renice, lalu verifikasi kembali.
3. Coba ubah nilai nice menjadi -5 tanpa sudo. Apa yang terjadi? Mengapa Linux membatasi hal ini untuk user biasa?
* Jawaban Latihan 6.3
1. Cek prioritas (nice): Nilai prioritas awal berhasil diset ke angka 5, lalu saat diubah pakai renice berhasil naik menjadi 10.
2. Kenapa ditolak saat ubah ke -5 tanpa sudo? Di Linux, mengubah nilai ke minus (-) artinya membuat program itu lebih diprioritaskan oleh CPU. Aturannya, user biasa tidak boleh melakukan ini agar tidak serakah menghabiskan kinerja CPU. Hanya Admin/Root (sudo) yang boleh menaikkan prioritas.

## Praktikum 6.4 — Mengirim Sinyal ke Proses
![](images/6.4.png "")

## Latihan 6.4
![](images/L6.4.png "")
1. Jalankan sleep 400 &, kirim SIGSTOP, dan amati perubahan kolom STAT. Kondisi apa yang muncul?
2. Kirim SIGCONT dan verifikasi proses kembali berjalan.
3. Hentikan proses dengan SIGTERM lalu verifikasi sudah tidak ada. Kapan Anda memilih SIGKILL daripada SIGTERM?
* Jawaban Latihan 6.4
1. Status dijeda & dilanjut: Setelah dikirim sinyal SIGSTOP, statusnya berubah jadi T (Stopped/Dijeda). Saat dikirim SIGCONT (dilanjutkan), statusnya kembali normal jadi S (Sleep).
2. Kapan pilih SIGKILL vs SIGTERM? Normalnya kita pakai SIGTERM untuk menutup program secara baik-baik. Tapi kalau programnya error, hang (not responding), atau bandel nggak mau ditutup, barulah kita tembak pakai SIGKILL untuk mematikannya secara paksa saat itu juga.

## Praktikum 6.5 — Manajemen Job Foreground dan Background
![](images/6.5.png "")

## Latihan 6.5
![](images/L6.5.png "")
1. Jalankan top di foreground. Apa yang terjadi di terminal?
2. Tekan Ctrl+Z dan cek statusnya dengan jobs. Kondisi apa yang ditampilkan?
3. Pindahkan ke background dengan bg. Apakah top dapat berjalan dengan baik di background? Mengapa?
4. Kembalikan ke foreground dengan fg, lalu keluar dengan q .
* Jawaban Latihan 6.5
1. Saat top jalan: Layar terminal langsung penuh menampilkan statistik performa komputer secara real-time (angka-angkanya terus bergerak).
2. Status setelah Ctrl+Z: Program top langsung terjeda sementara (statusnya berubah menjadi Stopped).
3. Apakah top bisa jalan di background? Tidak bisa. Program top itu wajib tampil di layar utama untuk memunculkan data. Kalau dipindah ke background (belakang layar), dia akan otomatis berhenti/terjeda lagi.

## Praktikum 6.6 — Pemantauan Proses
![](images/6.6.png "")
![](images/6.6%201.png "")
![](images/6.6%202.png "")

## Latihan 6.6
![](images/L6.6.png "")
![](images/L6.6%201.png "")
![](images/L6.6%202.png "")
1. Gunakan ps aux –sort=%mem untuk menemukan proses yang menggunakan memori paling banyak di VM Anda. Proses apa itu?
2. Di dalam top, tekan 1 . Apa yang berubah pada tampilan? Mengapa informasi ini berguna?
3. Di dalam htop, navigasikan ke proses sshd menggunakan tombol panah. Tekan F9 dan amati opsi sinyal yang tersedia
* Jawaban Latihan 6.6
1. Proses paling boros memori: Di komputermu, proses yang paling banyak memakan RAM adalah unattended-upgrade (ini adalah program bawaan Linux untuk melakukan update sistem secara otomatis).
2. Fungsi tekan '1' di top: Untuk melihat beban kinerja CPU secara lebih rinci. Kita jadi bisa memantau performa tiap-tiap inti (core) prosesor secara terpisah (dari Cpu0 sampai Cpu7), bukan cuma nilai rata-rata gabungannya.
3. Fungsi tekan 'F9' di htop: Untuk memunculkan daftar "Sinyal Aksi" di sisi kiri layar. Lewat daftar ini, kita bisa memilih mau mengirim perintah apa ke suatu proses dengan mudah (misalnya mengirim sinyal kill, stop, atau continue).

# 1.8 Latihan
## Latihan 6.A
## Latihan 6.B
## Latihan 6.C