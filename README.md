# Jarkom-Modul-2-D13-2021

### Anggota kelompok:
Anggota | NRP
------------- | -------------
Muthia Qurrota Akyun | 05111940000019
Fayha Syifa Qalbi | 05111940000185
Raihan Alifianto | 05111940000213

## Soal dan Jawaban
### 1. Membuat Topologi/Peta dengan kondisi: EniesLobby akan dijadikan sebagai DNS Master, Water7 akan dijadikan DNS Slave, dan Skypie akan digunakan sebagai Web Server. Terdapat 2 Client yaitu Loguetown, dan Alabasta. Semua node terhubung pada router Foosha, sehingga dapat mengakses internet.
![image](https://user-images.githubusercontent.com/68548653/139516863-c7d7a21c-1f58-42b6-a02f-af844349a75d.png)
1. Tambahkan switch, host, router, dan NAT yang diperlukan.
2. Kemudian setiap node saling dihubungkan menggunakan fitur `Add a link`
3. Lalu lakukan setting network pada setiap node dengan fitur `edit network configuration` seperti berikut. 
   - Foosha
   ```
   auto eth0
   iface eth0 inet dhcp
    
   auto eth1
   iface eth1 inet static
      address 192.198.1.1
    	netmask 255.255.255.0
    
   auto eth2
   iface eth2 inet static
    	address 192.198.2.1
    	netmask 255.255.255.0
   ```
   - Loguetown
   ```
   auto eth0
   iface eth0 inet static
	   address 192.198.1.2
	   netmask 255.255.255.0
	   gateway 192.198.1.1
   ```
   - Alabasta
   ```
   auto eth0
   iface eth0 inet static
      address 192.198.1.3
      netmask 255.255.255.0
      gateway 192.198.1.1
   ```
   - EniesLobby
   ```
   auto eth0
   iface eth0 inet static
      address 192.198.2.2
      netmask 255.255.255.0
      gateway 192.198.2.1
   ```
   - Water7
   ```
   auto eth0
   iface eth0 inet static
      address 192.198.2.3
      netmask 255.255.255.0
      gateway 192.198.2.1
   ```
   - Skypie
   ```
   auto eth0
   iface eth0 inet static
      address 192.198.2.4
      netmask 255.255.255.0
      gateway 192.198.2.1
   ```
4. Setelah itu restart semua node
5. Kemudian, masukkan `iptables -t nat -A POSTROUTING -o eth0 -j MASQUERADE -s 192.198.0.0/16` pada Foosha
6. Lalu masukkan `echo nameserver 192.168.122.1 > /etc/resolv.conf` pada semua console node.
7. Kemudian test ping google.com pada Alabasta dan Loguetown.
	- Alabasta
	
	![image](https://user-images.githubusercontent.com/68548653/139518312-cc2908f4-94dd-4fde-b07f-dbdaed425e16.png)

	- Loguetown 
	
	![image](https://user-images.githubusercontent.com/68548653/139518328-cba2515d-6c2b-4127-9b98-31637cf2dd56.png)

### 2. Membuat website utama dengan mengakses franky.yyy.com dengan alias www.franky.yyy.com pada folder kaizoku. 
Karen EniesLobby merupakan DNS Server Master, maka pada Enies Lobby jalankan perintah `apt-get update` untuk mengupdate package list. Setelah itu, install aplikasi bind9 dengan perintah `apt-get install bind9 -y`
Untuk membuat domain **franky.D13.com** lalukan perintah `nano /etc/bind/named.conf.local` lalu isi konfigurasi domain tersebut dengan sintaks berikut.
```
zone "franky.D13.com" {
    type master;
    file "/etc/bind/kaizoku/franky.D13.com";
};
```

![image](https://user-images.githubusercontent.com/68548653/139520002-79447e34-60d3-42e5-89cb-c3b2b666b7b6.png)

Kemudian buat folder kaizoku pada /etc/bind
```
mkdir /etc/bind/kaizoku
```
Pada file franky.D13.com tambahkan konfigurasi berikut.
```
  @ IN  NS  franky.D13.com
  @ IN  A   192.198.2.2
```
Lalu restart bind9 dengan perintah `service bind9 restart`
Pada file **/etc/resolv.conf** untuk node Alabasta dan Loguetown diganti menjadi IP EniesLobby
```
nameserver 192.198.2.2 ;IP EniesLobby
```
Kemudian test ping 

<img src = "https://github.com/muthiaqrrta/Jarkom-Modul-2-D13-2021/blob/main/screenshot/no2.jpeg">

### 3. Membuat subdomain super.franky.yyy.com dengan alias www.super.franky.yyy.com yang diatur DNS nya di EniesLobby dan mengarah ke Skypie.
Edit file **/etc/bind/kaizoku/franky.D13.com** dengan menambahkan subdomain untuk franky.D13.com yang mengarah ke IP Skypie seperti berikut.

<img src = "https://github.com/muthiaqrrta/Jarkom-Modul-2-D13-2021/blob/main/screenshot/no3%20script.sh.jpeg">

Restart service bind menggunakan `service bind9 restart`
Kemudian lalukan ping untuk mengecek 

<img src = "https://github.com/muthiaqrrta/Jarkom-Modul-2-D13-2021/blob/main/screenshot/no3.jpeg">

### 4. Memreverse domain untuk domain utama
Buka file **named.conf.local** pada EniesLobby dengan perintah `nano /etc/bind/named.conf.local`
Kemudian tambahkan konfigurasi berikut ke dalam file tersebut. Tambahkan reverse dari 3 byte awal dari IP yang ingin dilakukan Reverse DNS.
```
zone "2.198.192.in-addr.arpa" {
   type master;
   file "/etc/bind/kaizoku/2.198.192.in-addr.arpa";
};
```
Lalu pada file **2.198.192.in-addr.arpa** tambahkan konfigurasi berikut.
```
2.198.192.in-addr.arpa.   IN	NS	franky.D13.com.
2                         IN	PTR	franky.D13.com.
```
Lalu restart bind9 
```
service bind9 restart
```
Berikut merupakan hasil konfigurasinya.

<img src = "https://github.com/muthiaqrrta/Jarkom-Modul-2-D13-2021/blob/main/screenshot/no4.jpeg">

### 5. Membuat Water7 sebagai DNS Slave untuk domain utama(Supaya tetap bisa menghubungi Franky jika server EniesLobby rusak)
Edit file **/etc/bind/named.conf.local** pada EniesLobby sebagai berikut.
```
zone "franky.D13.com" {
   type master;
   notify yes;
   also-notify { 192.198.2.3; }; 
   allow-transfer { 192.198.2.3; }; 
   file "/etc/bind/kaizoku/franky.D13.com";
};
```
Kemudian restart bind9 dengan perintah `service bind9 restart`
Lalu pada Water7 lakukan update dan install bind9
```
apt-get update
apt-get install bind9 -y
```
Buka file **/etc/bind/named.conf.local** pada Water7 kemudian tambahkan syntax berikut.
```
zone "franky.D13.com" {
   type slave;
   masters { 192.198.2.2; }; 
   file "/var/lib/bind/franky.D13.com";
};
```
Kemudian restart bind9 dengan perintah `service bind9 restart`
Lakukan testing 
- Matikan service bind9 pada EniesLobby dengan perintah `service bind9 stop`
- Buka console Alabasta dan Loguetown. kemudian tambahkan nameserver Water7 pada file **/etc/resolv.conf**
```
nameserver 192.198.2.2
nameserver 192.198.2.3
```
- Kemudian `ping franky.D13.com`

<img src = "https://github.com/muthiaqrrta/Jarkom-Modul-2-D13-2021/blob/main/screenshot/no5.jpeg">

### 6. Membuat subdomain mecha.franky.yyy.com dengan alias www.mecha.franky.yyy.com yang didelegasikan dari EniesLobby ke Water7 dengan IP menuju ke Skypie dalam folder sunnygo
Edit file **/etc/bind/kaizoku/franky.D13.com** pada EniesLobby dengan menambahkan konfigurasi berikut.
```
 ns1	  IN	A   192.198.2.3
 mecha    IN	NS  ns1

```
Lalu edit file **/etc/bind/named.conf.local** dengan menambahkan konfigurasi berikut.
```
zone "mecha.franky.D13.com" {
    type master;
    file "/etc/bind/sunnygo/mecha.franky.D13.com";
    allow-transfer{ any; };
  };
```
Kemudian pada file **mecha.franky.D13.com** tambahkan
```
  @	IN	NS	mecha.franky.D13.com.
  @	IN	A 	192.198.2.4
```
Lalu restart bind9 dengan perintah `service bind9 restart`

Lakukan testing 

<img src = "https://github.com/muthiaqrrta/Jarkom-Modul-2-D13-2021/blob/main/screenshot/no6.jpeg">
<img src = "https://github.com/muthiaqrrta/Jarkom-Modul-2-D13-2021/blob/main/screenshot/no6-2.jpeg">

### 7. Membuat subdomain melalui Water7 dengan nama general.mecha.franky.yyy.com dengan alias www.general.mecha.franky.yyy.com yang mengarah ke Skypie
Edit file **/etc/bind/sunnygo/mecha.franky.D13.com** pada Water7, lalu tambahkan subdomain untuk mecha.franky.D13.com yang mengarah ke IP Skypie.
```
 general	IN	A	192.198.2.4
```
Kemudian restart service bind pada Water7 dan EniesLobby menggunakan perintah `service bind9 restart`
Kemudian lakukan testing
- Loguetown

![image](https://user-images.githubusercontent.com/68548653/139517151-bca3228d-4f28-4f71-9ad0-932a058a4079.png)

- Alabasta

![image](https://user-images.githubusercontent.com/68548653/139517170-2310d9c7-3022-4306-b846-8bf9043cf875.png)

## Kendala

