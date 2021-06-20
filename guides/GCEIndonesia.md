## Prasyarat

* Akun Google
* Kartu kredit/kartu debit yang mendukung transaksi online *tanpa* 3-D Secure (hampir semua kartu kredit berlogo Visa & MasterCard, kartu debit Jenius)

## Biaya

* Gratis untuk 90 hari, jika total traffic dibawah ~1.9 TB, dengan hanya *satu* VM aktif

* Jika kredit gratis habis, semua VM akan dimatikan. Tidak ada biaya tambahan sampai upgrade akun ke akun berbayar (bisa terkena biaya)

* Dengan akun berbayar, *satu* VM dalam daerah tertentu di AS bisa [dijalankan selamanya](https://cloud.google.com/free/docs/gcp-free-tier#always-free-usage-limits) gratis, namun bandwidth gratis hanya 1 GB per bulan, tidak termasuk traffic ke RRT dan Australia (tidak ada bandwidth gratis ke RRT dan Australia), dengan biaya sampai 0.12 USD per GB setelahnya.

## Membuat akun Cloud Engine

Masuk ke https://cloud.google.com dan klik **Get Started For Free**. Jika sudah punya akun Cloud Engine, langsung masuk ke [Membuat VM](#membuat-vm).

<br><img src="../images/gcege1.png" width="274">

Pilih negara, *kemungkinan* harus sesuai dengan kartu yang digunakan untuk pembayaran

<br><img src="../images/gcege2.png" width="989">

Sesuaikan tipe akun dan update informasi pajak.

<br><img src="../images/gcege3.png" width="1065">

Kecuali sudah pernah menambahkan kartu pembayaran ke Google, masukkan nomor kartu dan mulai trial.

<br><img src="../images/gcege4.png" width="1062">

Google akan membuat test charge (dibawah 1 USD) yang akan dibatalkan setelahnya. Kecuali [upgrade](https://cloud.google.com/free/docs/gcp-free-tier#how-to-upgrade), Google *tidak akan* menarik biaya walaupun kredit gratis 300 dollar sudah habis atau 1 tahun telah lewat. VM akan mati begitu saja, tanpa biaya tambahan.

## Membuat VM

Masuk ke https://console.cloud.google.com/compute/instances

Jika belum punya project, ikuti panduan untuk membuat project petama. Setelah project dibuat, klik Create

<br><img src="../images/gcege10.png" width="866">

Buka http://www.gcping.com/ di tab baru, biarkan berjalan sampai selesai (icon berubah ke segitiga). Catat region teratas, kecuali global

<br><img src="../images/gcege11.png" width="1220">

Di pemilihan region VM, pilih region berdasar hasil GCPing. Ganti machine type ke **f1-micro**

<br><img src="../images/gcege12.png" width="947">

Pastikan untuk centang **Allow HTTPS Traffic**. 

<br><img src="../images/gcege13.png" width="1001">

Buka **Management, security, disks, networking, sole tenancy**

<br><img src="../images/gcege14.png" width="732">

Pindah ke tab **Networking** tab dan klik icon pensil di bawah **Network interfaces**

<br><img src="../images/gcege15.png" width="928">

Dibawah **External IP**, klik **Ephemeral**, dan ganti ke **Create IP Address** 

<br><img src="../images/gcege16a.png" width="915"><img src="../images/gcege16.png" width="928">

Namai alamat dan **Reserve**.

<br><img src="../images/gcege17.png" width="1012">

Ganti **IP Forwarding** ke **On**, dan klik **Done**

<br><img src="../images/gcege18.png" width="940">

Klik **Create** di bawah

<br><img src="../images/gcege19.png" width="940">

## Install OpenVPN

Cek daftar VM di https://console.cloud.google.com/compute/instances. Catat IP *tepat* disebelah tombol **SSH**,lalu klik tombol **SSH**

<br><img src="../images/gcege20.png" width="681">

Di jendela baru yang terbuka, ketik `sudo su` dan tekan **Enter**

<br><img src="../images/gcege21.png" width="681">

Copy paste baris berikut (untuk paste, klik di prompt, lalu Ctrl-V), kemudian tekan Enter

`wget git.io/nenengce -O nenengce.sh && chmod +x nenengce.sh && ./nenengce.sh`

Catat path yang ditampilkan setelah "the configuration file is available". Setelah ini harus buat beberapa client profile, karena profile yang sama tidak bisa mengakses VPN bersamaan.

<br><img src="../images/gcege24.png" width="1310">

Copy paste baris berikut

`bash openvpn-install.sh`

<br><img src="../images/gcege26.png" width="916">

Ikuti instruksi add new user (1). Sebaiknya tiap perangkat memiliki profile sendiri, karena misal ponsel anda menggunakan profile yang sama dengan tablet, koneksi di ponsel akan terputus saat tablet memulai koneksi, dan sebaliknya.

<br><img src="../images/gcege27.png" width="1074">

Ulangi langkah sebelumnya untuk membuat tambahan user untuk tiap perangkat yang diperlukan. Server ini secara teori bisa mendukung setidaknya lusinan koneksi bersamaan. Setelah selesai, profile-profile harus didownload.

## Memasang OpenVPN di perangkat

Klik icon roda gigi di pojok kanan atas, dan pilih Download file 

<br><img src="../images/gcege28.png" width="583">

Masukkan path tiap profile yang diperlukan, dan download

<br><img src="../images/gcege29.png" width="977">

Kirimkan/copy file .ovpn ke tiap perangkat. Di masing-masing perangkat, install OpenVPN client. [Windows](https://openvpn.net/client-connect-vpn-for-windows/), [Mac OS](https://openvpn.net/client-connect-vpn-for-mac-os/), [Linux](https://community.openvpn.net/openvpn/wiki/OpenvpnSoftwareRepos), [Android](https://play.google.com/store/apps/details?id=net.openvpn.openvpn) dan [iOS](https://apps.apple.com/us/app/openvpn-connect/id590379981) didukung secara resmi.

Buka aplikasi OpenVPN di perangkat, lalu import file profile sebelum penggunaan pertama kali. 

<br><img src="../images/gcege30.png" width="781">

Nama profile bisa diubah, berguna kalau ingin menggunakan beberapa server.

<br><img src="../images/gcege31.png" width="779">

Daftar profile akan menunjukkan status tiap profile. Konek atau diskonek dengan toggle switch.

<br><img src="../images/gcege32.png" width="770">

Pastikan dengan googling "What is my IP" atau browsing situs yang sebelumnya diblokir (jika masih diblokir, coba restart browser)
