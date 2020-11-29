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
