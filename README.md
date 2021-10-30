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
   - Alabasta
   - EniesLobby
   - Water7
   - Skypie
### 2. Membuat website utama dengan mengakses franky.yyy.com dengan alias www.franky.yyy.com pada folder kaizoku. 

### 3. Membuat subdomain super.franky.yyy.com dengan alias www.super.franky.yyy.com yang diatur DNS nya di EniesLobby dan mengarah ke Skypie.


### 4. Memreverse domain untuk domain utama

### 5. Membuat Water7 sebagai DNS Slave untuk domain utama(Supaya tetap bisa menghubungi Franky jika server EniesLobby rusak)

### 6. Membuat subdomain melalui Water7 dengan nama general.mecha.franky.yyy.com dengan alias www.general.mecha.franky.yyy.com yang mengarah ke Skypie
- Loguetown

![image](https://user-images.githubusercontent.com/68548653/139517151-bca3228d-4f28-4f71-9ad0-932a058a4079.png)

- Alabasta

![image](https://user-images.githubusercontent.com/68548653/139517170-2310d9c7-3022-4306-b846-8bf9043cf875.png)


