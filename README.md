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
### Subnetting	
Pertama kita membuat topologi seperti pada gambar, kemudian kita tentukan jumlah IP yang dibutuhkan semua subnet menggunakan perhitungan jumlah IP seperti ini:  
| Subnet	| Jumlah IP	| Netmask |
|---------|-----------|---------|
| A1			| 1001 			| /22 		|
| A2			| 2					| /30 		|
| A3			| 2					| /30 		|	 
| A4			| 101				| /25 		|  
| A5			| 701				| /22 		| 
| A6			| 2021			| /21 		|  
| A7			| 2					| /30 		|
| A8			| 502				| /23 		|	  
| A9			| 13				| /28 		|
| A10			| 521				| /22 		| 
| A11			| 2					| /30 		|	 
| A12			| 252				| /24 		| 
| A13			| 721				| /22 		| 
|Total		|	5841 			| /19 		|
  
Lalu , lakukan pengelompokan subnet yang digunakan pada metode CIDR seperti berikut:	
| Subnet1	| Subnet2	| Netmask1	| Netmask2	| Subnet Hasil	| Netmask Hasil |  
|---------|---------|-----------|-----------|---------------|---------------|  
| A13			| A12			| /22				| /24				| B1						| /21						|  
| A8			| A9			| /23				| /28				| B2						| /22						|  
| A4			| A6			| /25				| /21				| B3						| /20						|  
| B3			| A3			| /20				| /30				| C3						| /19						|  
| C3			| A5			| /19				| /22				| D2						| /18						|  
| D2			| A2			| /18				| /30				| E2						| /17						|  
| B1			| A11			| /21				| /30				| C1						| /20						|  
| B2			| A10			| /22				| /22				| C2						| /21						|  
| C1			| C2			| /20				| /21				| D1						| /19						|  
| D1			| A7			| /19				| /30				| E1						| /18						|  
| E1			| A1			| /18				| /22				| F1						| /17						|  
| F1			| E2			| /17				| /17				| ALL						| /16						|  

Kemudian kita buat tree pembagian IP di bawah ini: 
![image](https://user-images.githubusercontent.com/61267430/102006299-b5b3bc80-3d52-11eb-89f9-d2019f89e6ad.png) 
Setelah kita mengetahui pembagian subnet-nya maka kita buat topologi seperti gambar berikut:  
![image](https://user-images.githubusercontent.com/61267430/102005469-95ccca80-3d4b-11eb-919d-c5885f51f4a1.png) 

### Setting Topologi pada UML	
Pada UML, kita buat file topologi.sh seperti berikut: 
![image](https://user-images.githubusercontent.com/61267430/102008917-7ee7a180-3d66-11eb-8d2f-c79c7f9bc293.png) 
Kita jalankan dengan command `bash topologi.sh` dan pastikan semua UML terbuka tanpa kendala. 
![image](https://user-images.githubusercontent.com/61267430/102008957-d6860d00-3d66-11eb-90a4-ef9cb2aaa6f1.png)   
Kita pastikan untuk mengedit file `/etc/sysctl.conf` seperti gambar berikut:   
![image](https://user-images.githubusercontent.com/61267430/102008983-06351500-3d67-11eb-92ac-2930b3b68f01.png)   
Kemudian cek menggunakan sysctl -p. Ulangi untuk setiap Router (Surabaya, Pasuruan, Probolinggo, Batu, Kediri, Madiun, Blitar).	
![image](https://user-images.githubusercontent.com/61267430/102009023-46949300-3d67-11eb-9cb4-f4e607df348d.png)	
Edit file `/etc/network/interfaces` dan tambahkan network interface yang sesuai dengan gambaran topologi yang sudah kita buat pada Cisco Packet Tracer.	
Contoh:		
__Router__		  
![image](https://user-images.githubusercontent.com/61267430/102009135-f1a54c80-3d67-11eb-8913-ee4ea58d43fb.png)	  
__Server__  
![image](https://user-images.githubusercontent.com/61267430/102009157-1994b000-3d68-11eb-99ba-bd4e489b9df2.png)	  
__Client__		  
![image](https://user-images.githubusercontent.com/61267430/102009168-3a5d0580-3d68-11eb-9ffe-5de51e17f868.png)	  

### Routing
Agar routing tidak hilang pada UML, kita perlu menggunakan command tambahan yaitu `post-up` dan `post-down`	
post-up route add -net 10.151.83.152 netmask 		=>	Digunakan ketika UML starting / restarting untuk menambahkan routing	
post-down route del -net 10.151.83.152 netmask	=>	Digunakan ketika UML di-halt atau dimatikan untuk menghilangkan routing (agar tidak 2 kali)	
Sedangkan routing pada masing-masing router bisa dilihat di bawah:	
__SURABAYA__   
![image](https://user-images.githubusercontent.com/61267430/102009316-3b426700-3d69-11eb-96eb-1518e91da28a.png)  
__PASURUAN__    
![image](https://user-images.githubusercontent.com/61267430/102009329-5614db80-3d69-11eb-82f6-1a132ee66011.png)  
__PROBOLINGGO__  
![image](https://user-images.githubusercontent.com/61267430/102009339-662cbb00-3d69-11eb-81f9-3db4c9fb2ce8.png)  
__BATU__  
![image](https://user-images.githubusercontent.com/61267430/102009346-75ac0400-3d69-11eb-829b-25aa4aa048b8.png)  
__KEDIRI__  	  
![image](https://user-images.githubusercontent.com/61267430/102009358-878da700-3d69-11eb-95d9-5cbb40266503.png)  
__MADIUN__    
![image](https://user-images.githubusercontent.com/61267430/102009370-97a58680-3d69-11eb-8194-437a42663da4.png)  
__BLITAR__    
![image](https://user-images.githubusercontent.com/61267430/102009388-a9872980-3d69-11eb-9d5d-3cc766396ef9.png)  

### Testing  
__Bondowoso Ping Tulungagung__  
![image](https://user-images.githubusercontent.com/61267430/102009451-14d0fb80-3d6a-11eb-84d2-09ace4b40b5a.png)	  
__Malang Ping Nganjuk__  
![image](https://user-images.githubusercontent.com/61267430/102009487-4e096b80-3d6a-11eb-81bc-d20d9bd3462c.png)   
__Mojokerto Ping Malang__  
![image](https://user-images.githubusercontent.com/61267430/102009516-76916580-3d6a-11eb-8b2d-1936cc2f657e.png)  
__Ping its.ac.id__  
Pada Surabaya, pastikan telah melakukan command `iptables –t nat –A POSTROUTING –o eth0 –j MASQUERADE –s 192.168.0.0/16`  
![image](https://user-images.githubusercontent.com/61267430/102009569-e1db3780-3d6a-11eb-8dab-e06d9396f463.png)	  
Lalu cek pada client atau server, dalam kasus ini Bojonegoro:  
![image](https://user-images.githubusercontent.com/61267430/102009597-06cfaa80-3d6b-11eb-8f83-0c9a365c5574.png)	

----------------------------------------------------------------------------

## VLSM 
Metode VLSM kami terapkan dalam Cisco Packet Tracer, langkah pertama adalah membuat topologi sesuai dengan soal yang diberikan dan melakukan pembagian subnet seperti gambar berikut ini:  
![CiscoVLSM](https://user-images.githubusercontent.com/61267430/102010351-22897f80-3d70-11eb-90b8-36472ed9c987.PNG)  

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
![treeVLSM](https://user-images.githubusercontent.com/61267430/102010279-c292d900-3d6f-11eb-958f-bfb450ea24fb.jpg)    

Setelah itu untuk mempermudah pengerjaan kita buat tabel seperti berikut:  
![tabelVLSM](https://user-images.githubusercontent.com/61267430/102010321-f66dfe80-3d6f-11eb-8f4f-86827f1a80df.jpg)  

Setelah didapatkan IP, maka kita masukkan IP tersebut kedalam router dan end device pada Cisco Packet Tracer. Untuk mempermudah mengingat IP yang mana milik siapa, untuk setiap NID+1 kita gunakan sebagai IP router pada subnet tersebut dan NID+2 untuk end device. Sebagai contoh NID 192.168.0.0, maka 192.168.0.1 adalah IP router pada subnet tersebut dan 192.168.0.2 adalah IP yang digunakan end device pada subnet tersebut.

### Router Surabaya  
Pada router surabaya, kita gunakan modul 4E.  
![modulroutersurabaya](https://user-images.githubusercontent.com/61267430/102010371-3e8d2100-3d70-11eb-90fd-cda31f67430e.PNG)  
Berikut konfigurasi IP sesuai dengan jalur dan subnet yang dimiliki oleh Router Surabaya:  
![konfigsby1](https://user-images.githubusercontent.com/61267430/102010394-5664a500-3d70-11eb-8aa6-fbc10bbba387.PNG)  
![konfigsby2](https://user-images.githubusercontent.com/61267430/102010389-54024b00-3d70-11eb-80b1-2a191fdbb2dc.PNG)  
![konfigsby3](https://user-images.githubusercontent.com/61267430/102010390-55337800-3d70-11eb-985c-4cb2b22bddfe.PNG)  
![konfigsby4](https://user-images.githubusercontent.com/61267430/102010391-55cc0e80-3d70-11eb-856a-461b898c30f3.PNG)  
![konfigsby5](https://user-images.githubusercontent.com/61267430/102010392-5664a500-3d70-11eb-8c01-931116c728b6.PNG)  

### End Device (Client) Sidoarjo  
Pada client sidoarjo, kita konfigurasikan ip dengan ketentuan sebagai berikut:  
- IPv4 Address adalah IP dari Sidoarjo 
- SUbnet Mask adalah subnet mask Sidoarjo  
- Default Gateway adalah Router terdekat dari sidoarjo, dalam kasus ini adalah Router Pasuruan.  
- DNS Server dibiarkan default  
Alhasil adalah seperti gambar berikut ini:  
![konfigsidoarjo](https://user-images.githubusercontent.com/61267430/102010412-76946400-3d70-11eb-9acb-32c289e2b001.PNG)  

### Routing
Setelah semua device sudah diberikan IP, maka selanjutnya adalah routing pada router agar jalur komunikasi diketahui. Untuk routing, dilakukan kepada subnet diluar area router itu karena jarak yang terlalu jauh. Contoh pada Router Surabaya mengetahui subnet A1, A3, A6, Server Mojokerto tetapi tidak mengetahui Subnet A13. Maka kita buatkan routing dari Router Surabaya ke Subnet A13 dengan cara berikut:  
- Dari Router Surabaya menuju Subnet A13 melalui Subnet A11 terlebih dahulu, makadari itu kita beri konfigurasi sebagai berikut:  
![routingsurabaya_1](https://user-images.githubusercontent.com/61267430/102010432-8f9d1500-3d70-11eb-9d8e-9db4548cc1a4.PNG)  
Network adalah NID dari subnet A11, Mask adalah Subnet Mask A11 dan Next Hop adalah IP Router dalam subnet yang dikenali Surabaya yang dilewati untuk menuju A11 ( dalam kasus ini Router Batu). Sejauh ini surabaya bisa melakukan komunikasi dengan subnet yang dikenali oleh Router Batu (A11,A8,A2) tetapi masih belum mencapai subnet A13, makadari itu kita perlu menambahkan routing lagi di Router Batu dengan ketentuan yang sama seperti pada Router Surabaya. Berikut konfigurasi pada Router Batu:  
![routingbatu](https://user-images.githubusercontent.com/61267430/102010439-9cba0400-3d70-11eb-9558-5c7ca59590c5.PNG)  
- Sekarang Surabaya bisa melakukan komunikasi dengan subnet A13.

#### Routing keluar
Untuk konfigurasi routing menuju keluar ( semakin dekat dengan cloud/semakin dangkal subnetnya) maka cukup kita berikan konfigurasi seperti pada gambar berikut pada setiap router:  
![routingkeluar](https://user-images.githubusercontent.com/61267430/102010463-c4a96780-3d70-11eb-94fd-9cf3a9922e63.PNG)  
- Network dan Mask diisi dengan 0.0.0.0
- Next Hop diisi dengan IP router yang terdekat yang menuju ke subnet yang lebih dangkal/semakin dekat dengan cloud.

### Konfigurasi pada Server
Server Mojokerto dan Malang menggunakan IP yang didapat dari DMZ, tree dibawah adalah pembagian untuk IP server.
![treeserverVLSM](https://user-images.githubusercontent.com/61267430/102010303-db9b8a00-3d6f-11eb-8b5a-1254af9a2979.jpg)  
Untuk konfigurasi sepenuhnya sama dengan Client Device hanya menggunakan IP yang didapat dari DMZ.

### Cloud
Cloud diberi IP tuntap. Dikonfigurasikan dalam Router Surabaya.
