# Jarkom_Modul4_Lapres_T09
Anggota Kelompok:
- Donny Kurnia Ramadhani  (05311840000004)  
- Muhammad Zulfikar Fauzi (05311840000012)  
----------------------------------------------------------------------------
Catatan:
1. CLOUD diberikan IP TUNTAP  
2. Server diberikan IP DMZ  
3. Memberikan memori sebesar 64MB pada setiap UML  
----------------------------------------------------------------------------
## CIDR
Pertama kita membuat topologi seperti pada gambar, kemudian kita tentukan subnet-nya menggunakan tree di bawah ini: 
![image](https://user-images.githubusercontent.com/61267430/102006299-b5b3bc80-3d52-11eb-89f9-d2019f89e6ad.png) 
Setelah kita mengetahui pembagian subnet-nya maka kita buat topologi seperti gambar berikut:  
![image](https://user-images.githubusercontent.com/61267430/102005469-95ccca80-3d4b-11eb-919d-c5885f51f4a1.png) 
----------------------------------------------------------------------------
## VLSM
Metode VLSM kami terapkan dalam Cisco Packet Tracer, langkah pertama adalah membuat topologi sesuai dengan soal yang diberikan dan melakukan pembagian subnet seperti gambar berikut ini:
-gambartopologi-
Karena metode VLSM adalah mendistribusikan IP yang dibutuhkan sesuai dengan jumlah available IP pada masing - masing Subnet Mask maka kita tentukan dulu total IP yang duperlukan dalam topologi ini. Didapatkan hasil berikut :
- A1    : 1001 IP   /22
- A2    : 502 IP    /23
- A3    : 2 IP      /30
- A4    : 2 IP      /30
- A5    : 101 IP    /25
- A6    : 2 IP      /30
- A7    : 13 IP     /28
- A8    : 521 IP    /22
- A9    : 701 IP    /22
- A10   : 2021 IP   /21
- A11   : 2 IP      /30
- A12   : 721 IP    /22
- A13   :252 IP     /24
- Total : 5841 IP   /19
Setelah itu kita tentukan mask pada masing - masing subnet.

Jika sudah, maka kita buat tree untuk menentukan IP dan subnet yang akan digunakan:
-gambartree-

Setelah itu untuk mempermudah pengerjaan kita buat tabel seperti berikut:
-gambartabel-

Setelah didapatkan IP, maka kita masukkan IP tersebut kedalam router dan end device pada Cisco Packet Tracer. Untuk mempermudah mengingat IP yang mana milik siapa, untuk setiap NID+1 kita gunakan sebagai IP router pada subnet tersebut dan NID+2 untuk end device. Sebagai contoh NID 192.168.0.0, maka 192.168.0.1 adalah IP router pada subnet tersebut dan 192.168.0.2 adalah IP yang digunakan end device pada subnet tersebut.

### Router Surabaya
Pada router surabaya, kita gunakan modul 4E.
-gambarmodulsurabaya-
Berikut konfigurasi IP sesuai dengan jalur dan subnet yang dimiliki oleh Router Surabaya:
-gambarkonfigsby1 sampe 5-

### End Device (Client) Sidoarjo
Pada client sidoarjo, kita konfigurasikan ip dengan ketentuan sebagai berikut:
- IPv4 Address adalah IP dari Sidoarjo 
- SUbnet Mask adalah subnet mask Sidoarjo
- Default Gateway adalah Router terdekat dari sidoarjo, dalam kasus ini adalah Router Pasuruan.
- DNS Server dibiarkan default
Alhasil adalah seperti gambar berikut ini:
-gambarkonfigsidoarjo-

### Routing
Setelah semua device sudah diberikan IP, maka selanjutnya adalah routing pada router agar jalur komunikasi diketahui. Untuk routing, dilakukan kepada subnet diluar area router itu karena jarak yang terlalu jauh. Contoh pada Router Surabaya mengetahui subnet A1, A3, A6, Server Mojokerto tetapi tidak mengetahui Subnet A13. Maka kita buatkan routing dari Router Surabaya ke Subnet A13 dengan cara berikut:
- Dari Router Surabaya menuju Subnet A13 melalui Subnet A11 terlebih dahulu, makadari itu kita beri konfigurasi sebagai berikut:
-gambarroutingsurabaya_1-
Network adalah NID dari subnet A11, Mask adalah Subnet Mask A11 dan Next Hop adalah IP Router dalam subnet yang dikenali Surabaya yang dilewati untuk menuju A11 ( dalam kasus ini Router Batu). Sejauh ini surabaya bisa melakukan komunikasi dengan subnet yang dikenali oleh Router Batu (A11,A8,A2) tetapi masih belum mencapai subnet A13, makadari itu kita perlu menambahkan routing lagi di Router Batu dengan ketentuan yang sama seperti pada Router Surabaya. Berikut konfigurasi pada Router Batu:
-gambarroutingbatu-
- Sekarang Surabaya bisa melakukan komunikasi dengan subnet A13.

#### Routing keluar
Untuk konfigurasi routing menuju keluar ( semakin dekat dengan cloud/semakin dangkal subnetnya) maka cukup kita berikan konfigurasi seperti pada gambar berikut pada setiap router:
-gambarroutingkeluar-
- Network dan Mask diisi dengan 0.0.0.0
- Next Hop diisi dengan IP router yang terdekat yang menuju ke subnet yang lebih dangkal/semakin dekat dengan cloud.

### Konfigurasi pada Server
Server Mojokerto dan Malang menggunakan IP yang didapat dari DMZ, tree dibawah adalah pembagian untuk IP server.
-gambartreeserverVLSM-
Untuk konfigurasi sepenuhnya sama dengan Client Device hanya menggunakan IP yang didapat dari DMZ.

### Cloud
Cloud diberi IP tuntap. Dikonfigurasikan dalam Router Surabaya.
