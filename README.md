# Jarkom-Modul-1-B07-2023
> Laporan Resmi Praktikum 1 Jaringan Komputer B07
***
## Anggota Kelompok B07
1. I Gusti Ngurah Ervan Juli Ardana (5025211295)
2. Danar Sodik Priyambodo (5025211145)

## Daftar isi
+ [SOAL 1](#soal-1)
+ [SOAL 2](#soal-2)
+ [SOAL 3](#soal-3)
+ [SOAL 4](#soal-4)
+ [SOAL 5](#soal-5)
+ [SOAL 6](#soal-6)
+ [SOAL 7](#soal-7)
+ [SOAL 8](#soal-8)
+ [SOAL 9](#soal-9)
+ [SOAL 10](#soal-10)
+ [SOAL 11](#soal-11)
+ [SOAL 12](#soal-12)
+ [SOAL 13](#soal-13)
+ [SOAL 14](#soal-14)
+ [SOAL 15](#soal-15)
+ [SOAL 16](#soal-16)
+ [SOAL 17](#soal-17)
+ [SOAL 18](#soal-18)
+ [SOAL 19](#soal-19)
+ [SOAL 20](#soal-20)

---
## SOAL 1
### Pertanyaan
Yudhistira akan digunakan sebagai DNS Master, Werkudara sebagai DNS Slave, Arjuna merupakan Load Balancer yang terdiri dari beberapa Web Server yaitu Prabakusuma, Abimanyu, dan Wisanggeni. Buatlah topologi dengan pembagian sebagai berikut. Folder topologi dapat diakses pada drive berikut 

### Solusi
Pertama tama kita hanya perlu menyalin topology yang telah diberikan pada link drive diatas, jadinya akan seperti gambar dibawah ini

<img width="950" alt="Topology" src="https://github.com/NgurahErvan/Jarkom-Modul-2-B07-2023/assets/114007640/1f87feab-3b8d-4945-9d8f-f1618e9e21a1">

Setelah berhasil menyalin topology tersebut, kita perlu mengatur konfigurasi masing masing node yang telah ada :

<img width="222" alt="konfigurasi" src="https://github.com/NgurahErvan/Jarkom-Modul-2-B07-2023/assets/114007640/c19d4562-62ce-4e56-bcc6-5f15bbe20698">
<br>
<img width="223" alt="konfigurasi 2" src="https://github.com/NgurahErvan/Jarkom-Modul-2-B07-2023/assets/114007640/1d9a3d7d-717f-4640-b698-76d488e4df1b">

Setelah berhasil memeriksa membuat konfigurasinya dan memeriksa semua ip, kami lanjut menambahkan `iptables -t nat -A POSTROUTING -o eth0 -j MASQUERADE -s 10.12.0.0/16` kepada router pandudewenata dengan meletakan pada /root/.bashrc Kemudian kita memeriksa IP DNS dari router Pandudewenata dan mencatat IP tersebut. IP yang didapatkan yaitu 192.168.122.1 Setelah itu kita perlu memasukan node ini pada masing masing node yang ada 

`iptables -t nat -A POSTROUTING -o eth0 -j MASQUERADE -s 10.12.0.0/16` 

dan

`echo nameserver 192.168.122.1 > /etc/resolv.conf`

Kemudian kami coba ping google pada masing masing node. Hasil akhirnya kami bisa melakukan ping pada masing masing node tersebut
<img width="329" alt="ping google" src="https://github.com/NgurahErvan/Jarkom-Modul-2-B07-2023/assets/114007640/c599e461-b72c-498d-a1e3-06b97cf38c0e">
![ping google](https://github.com/NgurahErvan/Jarkom-Modul-2-B07-2023/assets/114007640/33b88c7a-8176-48da-95c1-7d0cc3cd6496)

---
## SOAL 2
### Pertanyaan
Buatlah website utama pada node arjuna dengan akses ke arjuna.yyy.com dengan alias www.arjuna.yyy.com dengan yyy merupakan kode kelompok.

### Solusi
untuk membuat website utama pertama tama kita harus melakukan update dan isntall bind9 pada node yudhistira. Setelah itu kita harus mengubah named.conf.local yang ada pada direktori bind dengan command berikut `nano /etc/bind/named.conf.local` ubah agar menjadi seperti ini

<img width="264" alt="zone" src="https://github.com/NgurahErvan/Jarkom-Modul-2-B07-2023/assets/114007640/551339b0-c758-4581-9a65-71c0053588b7">

Kemudian kita membuat 1 folder bernama jarkom dengan command `mkdir /etc/bind/jarkom`. Selanjutnya kita harus salin file db.local pada path /etc/bind ke dalam folder jarkom yang baru saja dibuat dengan  `cp /etc/bind/db.local /etc/bind/jarkom/arjuna.b07.com` kemudian kami membuka file arjuna.b07.com dan mengubah beberapa bagian menjadi:

<img width="575" alt="arjuna" src="https://github.com/NgurahErvan/Jarkom-Modul-2-B07-2023/assets/114007640/f20da087-11c6-448e-a4b4-2bea40f2911e">

Kemudian kita melakukan restart bin dengan perintah : `service bind9 restart` dan merubah settingan nameserver yang ada pada nakula dan sadewa dengan mengganti name server pada  `nano /etc/resolv.conf`

<img width="398" alt="ip yudhis" src="https://github.com/NgurahErvan/Jarkom-Modul-2-B07-2023/assets/114007640/cfc962da-7f08-4f8c-ac16-c0a434a642f5">

Terakhir kami melakukan testing dengan perintah `ping arjuna.b07.com` -c 5 dan `ping www.arjuna.b07.com`

<img width="362" alt="ping arjuna" src="https://github.com/NgurahErvan/Jarkom-Modul-2-B07-2023/assets/114007640/4bb3b847-056f-4516-ba13-5972a1c3b406">

---
## SOAL 3
### Pertanyaan
Dengan cara yang sama seperti soal nomor 2, buatlah website utama dengan akses ke abimanyu.yyy.com dan alias www.abimanyu.yyy.com.

### Solusi
soal ini hampir sama dengan soal sebelumnya, pertama kita perlu mengubah named.conf.local dengan perintah `nano /etc/bind/named.conf.local`

<img width="284" alt="zone abimanyu" src="https://github.com/NgurahErvan/Jarkom-Modul-2-B07-2023/assets/114007640/7acd7abe-852b-4f3a-bfc2-d69ccd364c42">

Kemudian kami membuat 1 folder bernama abimanyu dengan : `mkdir /etc/bind/abimanyu`. Lalu, sama seperti sebelumnya dengan copy file db.local dengan `cp /etc/bind/db.local /etc/bind/abimanyu/abimanyu.b07.com` dan buka file abimanyu yang telah di copy, kemudian sesuaikan isinya

<img width="571" alt="abimanyu" src="https://github.com/NgurahErvan/Jarkom-Modul-2-B07-2023/assets/114007640/4fbbdfba-d911-46c5-bb27-6b0c44f10750">

Selanjutnya kami perlu melakukan restart bin dengan perintah : `service bind9 restart` dan mengubah settingan nameserver pada nakula dan sadewa (client) dengan mengganti name server pada  nano `/etc/resolv.conf` yang diisi dengan ip yudhistira. Terakhir, sama seperti sebelumnya kami melakukan testing dengan perintah `ping abimanyu.b07.com -c 5` dan `ping www.abimanyu.b07.com`

<img width="434" alt="ping abimanyu" src="https://github.com/NgurahErvan/Jarkom-Modul-2-B07-2023/assets/114007640/3e42e88b-a663-453c-872a-b3acb566f11c">

---
## SOAL 4
### Pertanyaan
Kemudian, karena terdapat beberapa web yang harus di-deploy, buatlah subdomain parikesit.abimanyu.yyy.com yang diatur DNS-nya di Yudhistira dan mengarah ke Abimanyu.

### Solusi
pertama tama pada yudhistira, buka `nano /etc/bind/abimanyu/abimanyu.com` Tambahkan konfigurasi seperti berikut untuk menambahkan subdomain

<img width="505" alt="parikesit" src="https://github.com/NgurahErvan/Jarkom-Modul-2-B07-2023/assets/114007640/86aa813b-2d0f-4422-8dc7-bb450d38b6b8">

dengan konfigurasi diatas, maka akan menambahkan domain dengan nama parikesit yang juga akan mengarah ke ip abimanyu. Kemudian lakukan restart bind “service bind9 restart”
dan kami melakukan pengetesan untuk melakukan ping pada subdomain

<img width="449" alt="ping parikesit" src="https://github.com/NgurahErvan/Jarkom-Modul-2-B07-2023/assets/114007640/2cbf7647-876a-45ca-9ea4-407f38e21ef3">

---
## SOAL 5
### Pertanyaan
Buat juga reverse domain untuk domain utama. (Abimanyu saja yang direverse)

### Solusi
Pertama tama, edit named.conf.local dengan membuka `nano /etc/bind/named.conf.local` kemudian kami menambahkan konfigurasi untuk melakukan reverse domain utama yaitu abimanyu, perlu diketahui bahwa ip abimanyu kelompok kita adalah 10.12.3.3 maka akan di reverse menjadi 3.3.12.10. tanpa ip keempat

<img width="301" alt="zone reverse" src="https://github.com/NgurahErvan/Jarkom-Modul-2-B07-2023/assets/114007640/ba996d16-0d95-40fd-bfa4-24bf52a9080e">

sama seperti sebelumnya dengan copy file db.local dengan `cp /etc/bind/db.local /etc/bind/abimanyu/3.12.10.in-addr.arpa` kami melakukan perubahan pada file 3.12.10.in-addr.arpa menjadi seperti gambar di bawah ini

<img width="378" alt="adr reverse" src="https://github.com/NgurahErvan/Jarkom-Modul-2-B07-2023/assets/114007640/10663451-9fb9-4d27-a55f-022857b5c56b">

kemudian kami harus melakukan restart `service bind9 restart`
untuk melakukan pengecekan konfigurasi maka pada client (nakula/sadewa) bisa dilakukan download dengan nameserver awal dulu (192.168.122.1) agar terhubung ke internet untuk melakukan apt-get update dan juga apt-get install dnsutils. Setelah proses download selesai maka ubah nameserver ke arah yudhistira untuk melakukan cek konfigurasi dengan command `host -t PTR 10.12.3.3` (ip abimanyu) dan apabila berhasil maka akan muncul output seperti ini 

<img width="340" alt="reverse" src="https://github.com/NgurahErvan/Jarkom-Modul-2-B07-2023/assets/114007640/6a25e79a-e8c5-46b6-b3d6-db28f602e2c7">


---
## SOAL 6
### Pertanyaan
Agar dapat tetap dihubungi ketika DNS Server Yudhistira bermasalah, buat juga Werkudara sebagai DNS Slave untuk domain utama.

### Solusi
kami melakukan edit pada `/etc/bind/named.conf.local` di yudhistira dan sesuaikan dengan syntax berikut

![img](image/6-1.png)
![img](image/6-2.png)

kita mengubah pada kedua dns, kemudian lakukan 
```
service bind9 restart
```
selanjutnya kita melakukan setting pada dns slave yaitu werkudara dengan mengunduh bind9 dengan cara 
```
apt-get update
``` 
kemudian
```
apt-get install bind9 -y
``` 
dan selanjutnya menambahkan pada `/etc/bind/named.conf.local` pada werkudara dengan

![img](image/6-3.png)

dengan kedua dns, karena kita mengubah pada keduanya. kemudian lakukan 
```
service bind9 restart
```
lakukan
```
service bind9 stop
```
pada yudhistira untuk melakukan cek apakah bisa mengakses melalui dns slave yang sudah dibuat. pada client (nakula/sadewa) masukkan 2 nameserver yaitu ip dari yudhistira dan juga ip dari werkudara dengan 
```
nano /etc/resolv.conf
```
![img](image/6-4.png)

kita melakukan ping ke kedua dns pada client sadewa. dan ping berhasil meskipun bind9 pada yudhistira di stop yang berarti konfigurasi dns slave berhasil

![img](image/6-5.png)
![img](image/6-6.png)

---
## SOAL 7
### Pertanyaan
Seperti yang kita tahu karena banyak sekali informasi yang harus diterima, buatlah subdomain khusus untuk perang yaitu baratayuda.abimanyu.yyy.com dengan alias www.baratayuda.abimanyu.yyy.com yang didelegasikan dari Yudhistira ke Werkudara dengan IP menuju ke Abimanyu dalam folder Baratayuda.

### Solusi
Kita melakukan konfigurasi untuk kasus ini adalah delegasi subdomain, Pada yudhistira kita melakukan edit file `/etc/bind/abimanyu/abimanyu.b07.com` dan mengubah menjadi seperti di bawah ini

![img](image/7-1.png)

keterangan ns untuk delegasi subdomain yang dimana baratayuda adalah subdomain yang akan didelegasikan. Kemudian kita melakukan edit file `/etc/bind/named.conf.options`

![img](image/7-2.png)

melakukan comment pada `dnssec-validation auto;` dan menambahkan `allow-query{any;};`
kemudian pada `/etc/bind/named.conf.local`

![img](image/7-3.png)

menambahkan allow-transfer dengan ip werkudara, namun sebelumnya sudah di set jadi tidak dilakukan perubahan karena sudah sesuai
lakukan
```
service bind9 stop
```
Pada Werkudara edit file `/etc/bind/named.conf.options` sama persis seperti poin nomor 3 dengan melakukan comment pada `dnssec-validation auto;` dan menambahkan `allow-query{any;};`

![img](image/7-4.png)

lalu kami melakukan edit file `/etc/bind/named.conf.local` pada werkudara menjadi seperti

![img](image/7-5.png)

sesuai ketentuan dengan folder baratayuda. buat directory sesuai folder 
```
mkdir /etc/bind/baratayuda
```
kemudian kita melakukan copy pada db.local dengan 
```
cp /etc/bind/db.local /etc/bind/baratayuda/baratayuda.abimanyu.b07.com
```
kemudian kita melakukan edit pada file `baratayuda.abimanyu.b07.com`

![img](image/7-6.png)

dengan menambahkan alias www pada dns tersebut dan sesuai ketentuan dengan ip mengarah ke abimanyu. lakukan
```
service bind9 stop
```
kita melakukan testing pada client (nakula/sadewa) pada delegasi subdomain yang telah dilakukan dengan dan tanpa alias (www)

![img](image/7-7.png)

---
## SOAL 8
### Pertanyaan
Untuk informasi yang lebih spesifik mengenai Ranjapan Baratayuda, buatlah subdomain melalui Werkudara dengan akses rjp.baratayuda.abimanyu.yyy.com dengan alias www.rjp.baratayuda.abimanyu.yyy.com yang mengarah ke Abimanyu.

### Solusi
pada kasus ini kita melakukan pemberian subdomain namun pada subdomain hasil delegasi sebelumnya dengan menambahkan rjp.dan alias www. kita melakukan perubahan pada file `baratayuda.abimanyu.b07.com` dengan
```
nano /etc/bind/baratayuda/baratayuda.abimanyu.b07.com
```
![img](image/8-1.png)

disini kita menambahkan subdomain sesuai ketentuan yaitu subdomain rjp dan memberikan alias www.rjp untuk CNAME yang di set menuju subdomain sebelumnya. lakukan
```
service bind9 stop
```
kita melakukan test ping dengan subdomain dan alias yang diberikan

![img](image/8-2.png)

---
## SOAL 9
### Pertanyaan
Arjuna merupakan suatu Load Balancer Nginx dengan tiga worker (yang juga menggunakan nginx sebagai webserver) yaitu Prabakusuma, Abimanyu, dan Wisanggeni. Lakukan deployment pada masing-masing worker.

### Solusi
pertama-kita akan melakukan setup pada worker, disini kita ada 3 worker yang akan digunakan, yaitu, prabukusuma, abimanyu, dan wisanggeni.

![img](image/9-1.png)

kita install lalu setup Nginx dan PHP pada setiap worker yang digunakan dengan
```
apt-get update && apt install nginx php php-fpm -y
```
buat directory yang sama pada setiap worker dengan nama yang sama agar memudahkan setup dengan nama jarkom dengan cara
```
mkdir /var/www/jarkom
```
buat file index.php dan masukkan file echo seperti pada gambar berikut pada file dengan cara
```
nano index.php
```
pastikan dijalankan pada directory tersebut.

![img](image/9-2.png)

note : bedakan nama “prabukusuma” sesuai dengan lokasi setup.

selanjutnya kita melakukan konfigurasi pada Nginx, masuk ke direktori `/etc/nginx/sites-available` lalu buat file baru dengan nama jarkom atau langsung menggunakan
```
touch /etc/nginx/sites-available/jarkom
```
jalankan
```
nano /etc/nginx/sites-available/jarkom
```
kemudian isikan seperti berikut

![img](image/9-3.png)
![img](image/9-4.png)

dengan memberikan server_name sesuai ip worker yang sedang di setup, dan root sesuai dengan directory yang telah di set sebelumnya. simpan kemudian lakukan sm link dengan cara 
```
ln -s /etc/nginx/sites-available/jarkom /etc/nginx/sites-enabled
```
disini kita mendapati error setelah mencoba yaitu muncul default page, namun kita menambahkan 1 command untuk mengatasi hal tersebut dengan cara 
```
rm -rf /etc/nginx/sites-enabled/default
```
lakukan
```
ervice php7.2-fpm start
```
dan juga lakukan
```
service nginx restart
```
kita melakukan pengecekan pada nginx dengan cara
```
nginx -t
```
dan apabila muncul seperti contoh berikut maka berhasil.

![img](image/9-5.png)

kita lakukan juga pada worker yang lain. kemudian kita melakukan setup pada load balancer yang dimana disini adalah arjuna dengan cara membuat file directory pada `/etc/nginx/sites-available` dengan nama `lb-jarkom` dengan cara
```
touch /etc/nginx/sites-available/lb-jarkom
```
kemudian kita mengisikan sebagai berikut untuk menghandle ketiga worker yang telah di setup sebelumnya

![img](image/9-6.png)

dengan server masing masing dari ip worker yang telah di setup, beri nama juga pada server name untuk akses poin. kita simpan lalu buat symlink dengan cara
```
ln -s /etc/nginx/sites-available/lb-jarkom /etc/nginx/sites-enabled
```
lakukan
```
service nginx restart
```
kita lakukan pengetesan pada client (nakula/sadewa) apakah sudah dapat diakses atau belum dengan menggunakan command
```
lynx arjuna.b07.com
```
dan akan muncul halaman seperti dibawah, yang apabila di refresh (ctrl + r) maka akan berganti ganti halaman antar worker

![img](image/9-7.png)
![img](image/9-8.png)
![img](image/9-9.png)

apabila salah satu worker melakukan stop nginx, maka halaman tidak akan muncul

---
## SOAL 10
### Pertanyaan
 Kemudian gunakan algoritma Round Robin untuk Load Balancer pada Arjuna. Gunakan server_name pada soal nomor 1. Untuk melakukan pengecekan akses alamat web tersebut kemudian pastikan worker yang digunakan untuk menangani permintaan akan berganti ganti secara acak. Untuk webserver di masing-masing worker wajib berjalan di port 8001-8003. Contoh
- Prabakusuma:8001
- Abimanyu:8002
- Wisanggeni:8003


### Solusi
pada sebelumnya, karena default kita sudah menggunakan Round Robin pada load balancer Arjuna, disini kita hanya melakukan set port masing masing sesuai ketentuan yang diberikan, akses masing masing file jarkom di worker masing masing pada site available dengan cara
```
nano /etc/nginx/sites-available/jarkom
```
kemudian ubah pada listen sesuai ketentuan dari masing masing worker.

![img](image/10-1.png)

ubah juga pada abimanyu dan wisanggeni. lakukan
```
service nginx restart
```
pada masing masing worker setelah kita lakukan perubahan, kemudian pada load balancer, buka file `lb-jarkom` pada `sites-available` dan ubah pada server dengan menambahkan port yang telah kita set

![img](image/10-2.png)

sesuaikan port dengan masing masing worker, lakukan
```
service nginx restart
```
kita lakukan lynx pada link yang sama pada client yang sudah menggunakan port, apabila muncul berarti setup berhasil

![img](image/10-3.png)

---
## SOAL 11
### Pertanyaan
Selain menggunakan Nginx, lakukan konfigurasi Apache Web Server pada worker Abimanyu dengan web server www.abimanyu.yyy.com. Pertama dibutuhkan web server dengan DocumentRoot pada /var/www/abimanyu.yyy


### Solusi

---
## SOAL 12
### Pertanyaan

### Solusi

---
## SOAL 13
### Pertanyaan

### Solusi

---
## SOAL 14
### Pertanyaan

### Solusi

---
## SOAL 15
### Pertanyaan

### Solusi

---
## SOAL 16
### Pertanyaan

### Solusi

---
## SOAL 17
### Pertanyaan

### Solusi

---
## SOAL 18
### Pertanyaan

### Solusi

---
## SOAL 19
### Pertanyaan

### Solusi

---
## SOAL 20
### Pertanyaan

### Solusi




