# Jarkom_Modul2_Lapres_D04

## Tim D4

## Kevin Angga Wijaya - 05111840000024

## Samuel C. Y. Sinambela -05111840000036

## Persiapan

Membuat topologi.sh berdasarkan soal  
![topologi.sh](/img/topologi.jpg)

Mengedit interfaces

```
nano /etc/network/interfaces
```

- SURABAYA  
  ![SURABAYA](/img/interfaces_SURABAYA.jpg)

- MALANG  
  ![MALANG](/img/interfaces_MALANG.jpg)

- TUBAN  
  ![TUBAN](/img/interfaces_TUBAN.jpg)

- MOJOKERTO  
  ![MOJOKERTO](/img/interfaces_MOJOKERTO.jpg)

Pada soal 1, seluruh client TIDAK DIPERBOLEHKAN menggunakan konfigurasi IP Statis.

- MADIUN  
  ![MADIUN](/img/interfaces_MADIUN.jpg)

- GRESIK  
  ![GRESIK](/img/interfaces_GRESIK.jpg)

- SIDOARJO  
  ![SIDOARJO](/img/interfaces_SIDOARJO.jpg)

- BANYUWANGI  
  ![BANYUWANGI](/img/interfaces_BANYUWANGI.jpg)

Install yang dibutuhkan

- Install DNS pada MALANG

```
apt-get update
apt-get install bind9
```

- Install DHCP Server pada TUBAN

```
apt-get update
apt-get install isc-dhcp-server
```

- Install DHCP Relay pada SURABAYA

```
apt-get update
apt-get install isc-dhcp-relay
```

### 1. Seluruh client TIDAK DIPERBOLEHKAN menggunakan konfigurasi IP Statis.

Dapat dilihat pada interfaces dari tiap client di atas.

### 2. SURABAYA ditunjuk sebagai perantara (DHCP Relay) antara DHCP Server dan client.

Setelah menginstall DHCP Relay, maka buka file isc-dhcp-relay pada SURABAYA

```
nano /etc/default/isc-dhcp-relay
```

Isi INTERFACES dengan eth1, eth2, dan eth3  
![isc-dhcp-relay](/img/isc-dhcp-relay.jpg)

Lalu lakukan restart

```
service isc-dhcp-relay restart
```

### 3. Client pada subnet 1 mendapatkan range IP dari 192.168.0.10 sampai 192.168.0.100 dan 192.168.0.110 sampai 192.168.0.200.

Setting DHCP Server pada Tuban terlebih dahulu

```
nano /etc/default/isc-dhcp-server
```

Isi INTERFACES dengan eth0  
![isc-dhcp-server](/img/isc-dhcp-server.jpg)

Buka DHCP config

```
nano /etc/dhcp/dhcpd.conf
```

Lalu tambahkan Konfigurasi seperti pada gambar  
![dhcpd.conf](/img/dhcpd.conf.jpg)

Soal no 3 diselesaikan dengan membuat range pada subnet 1

### 4. Client pada subnet 3 mendapatkan range IP dari 192.168.1.50 sampai 192.168.1.70.

Sama seperti soal no 3, ditambahkan range pada subnet 3 sesuai dengan yang diminta

### 5. Client mendapatkan DNS Malang dan DNS 202.46.129.2 dari DHCP

Pada config, ditambahkan option domain-name-servers dan diisi dengan DNS MALANG dan DNS 202.46.129.2

### 6. Client di subnet 1 mendapatkan peminjaman alamat IP selama 5 menit, sedangkan client pada subnet 3 mendapatkan peminjaman IP selama 10 menit.

Pada config, ditambahkan default-lease-time yang disesuaikan pada tiap subnet

### 7. Pertama, akses ke proxy hanya bisa dilakukan oleh Anri sendiri sebagai user TA. (7) User autentikasi milik Anri memiliki format:

### - User : userta_yyy

### - Password : inipassw0rdta_yyy

### Keterangan : yyy adalah nama kelompok masing-masing. Contoh: userta_c01

Menginstall squid3 dan juga apache utils pada UML Mojokerto.

```
apt-get install squid
apt-get install apache2-utils
```

lalu membuat password dengan user: `userta_d04` dan password : `inipassw0rdta_d04` :

```
htpasswd -c /etc/squid/passwd userta_d04
```

Lalu backup file squid.conf :

```bash
mv /etc/squid/squid.conf /etc/squid/squid.conf.bak
```

Buka squid.conf yang baru:

```bash
nano /etc/squid/squid.conf
```

Lalu buat konfigurasi menjadi sebagai berikut
![no-7](/img/no-7.png)

Lalu restart dengan

```
service squid restart
```

Masukkan proxy seperti gambar berikut.
![no-7.1](/img/no-7.1.png)

Lalu buka salah satu website, sehingga akan menampilkan sebagai berikut.
![no-7.2](/img/no-7.2.jpg)

### 8 setiap hari Selasa-Rabu pukul 13.00-18.00. Bu Meguri membatasi penggunaan internet Anri hanya pada jadwal yang telah ditentukan itu saja. Maka diluar jam tersebut, Anri tidak dapat mengakses jaringan internet dengan proxy tersebut.

Buat file dengan command `nano /etc/squid/acl.conf` menjadi berikut ini.
![no-8](/img/no-8.jpg)

Dan tambahkan beberapa kode pada file squid.conf
![no-8.1](/img/no-8.1.png)
Lalu restart dengan

```
service squid restart
```

Ini adalah tampilan ketika website tidak bisa dibuka.
![no-8.2](/img/no-8.2.jpg)

### 9. Jadwal bimbingan dengan Bu Meguri adalah (9) setiap hari Selasa-Kamis pukul 21.00 - 09.00 keesokan harinya (sampai Jumat jam 09.00).

Buka file dengan command `nano /etc/squid/acl.conf` dan edit menjadi berikut ini.
![no-9](/img/no-9.png)

Lalu edit konfigurasi pada file `/etc/squid/squid.conf` menjadi berikut ini.
![no-9.1](/img/no-9.1.png)

Lalu restart dengan perintah `service squid restart`

### 10. Agar Anri bisa fokus mengerjakan TA, (10) setiap dia mengakses google.com, maka akan di redirect menuju monta.if.its.ac.id agar Anri selalu ingat untuk mengerjakan TA🙂.

Buka file `/etc/squid/squid.conf` dan edit file menjadi berikut ini.
![no-9.2](/img/no-9.2.png)

Lalu restart dengan perintah `service squid restart`

### 11. Untuk menandakan bahwa Proxy Server ini adalah Proxy yang dibuat oleh Anri, (11) Bu Meguri meminta Anri untuk mengubah error page default squid menjadi seperti berikut.

Jalankan perintah `cd /usr/share/squid/errors/English`. dan lakukan `wget 10.151.36.202/ERR_ACCESS_DENIED`.

Lalu restart dengan perintah `service squid restart`.

Ketika membuka website pada jam yang tidak diperbolehkan, maka halaman website error akan menampilkan:
![no-11](/img/no-11.jpg)

### 12. Karena Bu Meguri dan Anri adalah tipe orang pelupa, maka untuk memudahkan mereka, Anri

memiliki ide ketika menggunakan proxy cukup dengan mengetikkan domain
`janganlupa-ta.yyy.pw` dan memasukkan port `8080`.

Buka UML Malang dan jalankan perintah `apt-get install bind9 -y`. Lalu jalankan perintah `nano /etc/bind/named.conf.local` dan ubah konfigurasi menjadi seperti berikut.
![no-12](/img/no-12.jpg)

Lalu jalankan

```
mkdir /etc/bind/jarkom
nano /etc/bind/jarkom/janganlupa-ta.d04.pw
```

dan atur konfigurasi filenya menjadi berikut.
![no-12.1](/img/no-12.1.jpg)

Lalu jalankan perintah `service bind9 restart` dan atur proxy menjadi seperti berikut ini
![no-12.2](/img/no-12.2.png)

Sehingga ketika membuka suatu website akan menjadi seperti berikut ini.
![no-12.3](/img/no-12.3.png)
