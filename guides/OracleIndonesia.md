## Prasyarat

* Kartu kredit/kartu debit yang mendukung transaksi online *tanpa* 3-D Secure (hampir semua kartu kredit berlogo Visa & MasterCard, kartu debit Jenius)

## Biaya

* Gratis selamanya, total traffic keluar (traffic masuk gratis) maksimum 10 TB tiap bulan.

## Membuat akun Oracle Cloud

Masuk ke https://www.oracle.com/cloud/free/ dan klik **Start for free**. Oracle akan membuat dua test charge, yang pertama sekitar 1 USD, yang kedua sekitar 10 USD beberapa hari kemudian, keduanya akan dibatalkan setelahnya. Kecuali [upgrade](https://www.oracle.com/cloud/free/faq.html), Oracle *tidak akan* menarik biaya walaupun kredit gratis 200 dollar sudah habis atau 30 hari telah lewat. VM free tier tetap akan berjalan selamanya.

Free tier tidak bisa mengganti/menambah region setelah pembayaran. Untuk pengguna Indonesia gunakan region Seoul/Tokyo.

Jika menemui masalah tentang token SMS, kontak customer service chat dari situs Oracle. Setelah memasukkan informasi kartu pembayaran, proses dari Oracle bisa memakan waktu beberapa jam sebelum akun diaktifkan dan bisa membuat VM. Selama menunggu email dari Oracle yang memberikan Cloud Account name, install dulu Bitvise client

## Setup Bitvise client

Download dan jalankan installer Bitvise client dari https://www.bitvise.com/ssh-client-download

## Membuat VM

Masuk ke https://www.oracle.com/cloud/sign-in.html dan gunakan cloud account name dari email Oracle, lalu login dengan username (email) dan password yang sudah diset sebelumnya.

Setelah masuk di console, klik Create a VM instance

<br><img src="../images/oracle1.png" width="323">

Di bagian Add SSH keys, klik Save Private Key. Buka Bitvise, klik Client key manager, Import. Ganti filetype dari Bitvise Keypair Files ke All Files, lalu pilih file yang baru didownload. Klik Import dan pastikan ada entry baru. Tutup dialog, kembali ke halaman Oracle dan klik Create. Tunggu sampai status instance berubah dari Provisioning menjadi Running. Cari field Public IP Address, klik Copy

Kembali ke Bitvise, paste IP address ke Host. Ubah Username ke ubuntu, pastikan Initial Method diset ke publickey. Set Client Key ke Auto, lalu klik Login. Di dialog Host Key Verification, klik Accept and Save. Setelah koneksi sukses, klik New Terminal Console

Di jendela baru yang terbuka, ketik `sudo su` dan tekan **Enter**

<br><img src="../images/oracle4.png" width="408">

Copy paste baris berikut (untuk paste, klik di jendela, lalu klik kanan), kemudian tekan Enter

`wget git.io/nenengce -O nenengce.sh && chmod +x nenengce.sh && ./nenengce.sh`

Catat path yang ditampilkan setelah "the configuration file is available". Setelah ini harus buat beberapa client profile, karena profile yang sama tidak bisa mengakses VPN bersamaan.

<br><img src="../images/oracle5.png" width="621">

Copy paste baris berikut

`bash openvpn-install.sh`

<br><img src="../images/gcege26.png" width="916">

Ikuti instruksi add new user (1). Sebaiknya tiap perangkat memiliki profile sendiri, karena misal ponsel anda menggunakan profile yang sama dengan tablet, koneksi di ponsel akan terputus saat tablet memulai koneksi, dan sebaliknya.

Ulangi langkah sebelumnya untuk membuat tambahan user untuk tiap perangkat yang diperlukan (untuk mengulangi perintah, tekan panah atas dan enter). Server ini secara teori bisa mendukung setidaknya lusinan koneksi bersamaan. Setelah selesai, profile-profile harus didownload.

Di Bitvise, klik New SFTP Window, pilih semua profile (file .ovpn) di kanan dan Download. Pindahkan masing-masing profile ke perangkat yang diperlukan

## Membuka Akses Jaringan

Di terminal console bitvise, jalankan dua baris berikut

    sudo iptables -I INPUT 6 -m state --state NEW -p tcp --dport 443 -j ACCEPT
    
    sudo netfilter-persistent save

Di halaman Oracle, di bagian  Primary VNIC, klik Subnet. Di halaman yang terbuka, klik Default Security List, lalu klik Add Ingress Rules, dan sesuaikan isinya seperti dibawah ini lalu klik Add Ingress Rules.

<br><img src="../images/oracle6.png" width="645">

## Memasang OpenVPN di perangkat

Di masing-masing perangkat, install OpenVPN client. [Windows](https://openvpn.net/client-connect-vpn-for-windows/), [Mac OS](https://openvpn.net/client-connect-vpn-for-mac-os/), [Linux](https://community.openvpn.net/openvpn/wiki/OpenvpnSoftwareRepos), [Android](https://play.google.com/store/apps/details?id=net.openvpn.openvpn) dan [iOS](https://apps.apple.com/us/app/openvpn-connect/id590379981) didukung secara resmi.

Buka aplikasi OpenVPN di perangkat, lalu import file profile sebelum penggunaan pertama kali. 

<br><img src="../images/gcege30.png" width="781">

Nama profile bisa diubah, berguna kalau ingin menggunakan beberapa server.

<br><img src="../images/gcege31.png" width="779">

Daftar profile akan menunjukkan status tiap profile. Konek atau diskonek dengan toggle switch.

<br><img src="../images/gcege32.png" width="770">

Pastikan dengan googling "What is my IP" atau browsing situs yang sebelumnya diblokir (jika masih diblokir, coba restart browser)
