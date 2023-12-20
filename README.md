# Jarkom-Modul-5-D29-2023

| Nama           | NRP            |
| ---------------| ---------------|
| Christian Kevin Emor      | 5025211153      |
| Ferza Noveri     | 5025211097      |

Setelah pandai mengatur jalur-jalur khusus, kalian diminta untuk membantu North Area menjaga wilayah mereka dan kalian dengan senang hati membantunya karena ini merupakan tugas terakhir.

![image](https://github.com/Chrstnkevin/Jarkom-Modul-5-D29-2023/assets/97864068/386b0bd4-271f-4ae9-a49a-81cd4b69373a)

Untuk menghitung rute-rute yang diperlukan, gunakan perhitungan dengan metode VLSM. Buat juga pohonnya, dan lingkari subnet yang dilewati.

![image](https://github.com/Chrstnkevin/Jarkom-Modul-5-D29-2023/assets/97864068/2e0c9e4e-f5e1-451e-8046-e1f926b012cd)

Pohon VLSM:
![image](https://github.com/Chrstnkevin/Jarkom-Modul-5-D29-2023/assets/97864068/14bddd46-3378-4532-8279-b6e7aefc2399)

Rute:
![image](https://github.com/Chrstnkevin/Jarkom-Modul-5-D29-2023/assets/97864068/927bf089-68eb-4dc8-9007-0f282089186a)

Pembagian IP:
![image](https://github.com/Chrstnkevin/Jarkom-Modul-5-D29-2023/assets/97864068/a65e8fd5-803b-4389-b15b-befab768f5e4)

## Routing
- Aura
````
    # ke arah Frieren
 `  # gateway menggunakan eth0 dari Frieren
`   # Routing for subnets A10, A9, A8, A7, A6, A5
    route add -net 10.36.14.128 netmask 255.255.255.252 gw 10.36.14.146
    route add -net 10.36.14.132 netmask 255.255.255.252 gw 10.36.14.146
    route add -net 10.36.14.0 netmask 255.255.255.128 gw 10.36.14.146
    route add -net 10.36.12.0 netmask 255.255.254.0 gw 10.36.14.146
    route add -net 10.36.14.136 netmask 255.255.255.252 gw 10.36.14.146
    route add -net 10.36.14.140 netmask 255.255.255.252 gw 10.36.14.146
    
    # ke arah Heiter
    # gateway menggunakan eth0 dari Heiter
    # Routing for subnets A9, A10 
    route add -net 10.36.0.0 netmask 255.255.248.0 gw 10.36.14.150
    route add -net 10.36.8.0 netmask 255.255.252.0 gw 10.36.14.150
````

- Heiter
````
 route add -net 0.0.0.0 netmask 0.0.0.0 gw 10.36.14.149
````

- Frieren
````
 # ke arah Himmel
 # gateway menggunakan eth0 dari Himmel
 # Routing for subnets A10, A9, A8, A7
 route add -net 10.36.14.128 netmask 255.255.255.252 gw 10.36.14.138
 route add -net 10.36.14.132 netmask 255.255.255.252 gw 10.36.14.138
 route add -net 10.36.14.0 netmask 255.255.255.128 gw 10.36.14.138
 route add -net 10.36.12.0 netmask 255.255.255.0 gw 10.36.14.138
 route add -net 0.0.0.0 netmask 0.0.0.0 gw 10.10.36.145
````

- Himmel
````
 # ke arah Fern
 # gateway menggunakan eth0 dari Fern
 # Routing for subnets A10, A9
 route add -net 10.36.14.128 netmask 255.255.255.252 gw 10.36.14.2
 route add -net 10.36.14.132 netmask 255.255.255.252 gw 10.36.14.2
 route add -net 0.0.0.0 netmask 0.0.0.0 gw 10.36.14.137
````

- Fern
````
 route add -net 0.0.0.0 netmask 0.0.0.0 gw 10.36.14.1
````

## Konfigurasi
### DHCP Server (Revolte)
````
echo 'nameserver 192.168.122.1' > /etc/resolv.conf
apt-get update -y
apt-get install isc-dhcp-server -y

 rm /var/run/dhcpd.pid

 echo 'INTERFACESv4="eth0"' > /etc/default/isc-dhcp-server

 echo '
 #A10
 subnet 10.36.14.128 netmask 255.255.255.252 {
 }
 #A9
 subnet 10.36.14.132 netmask 255.255.255.252 {
 }
 #A8
 subnet 10.36.14.0 netmask 255.255.255.128 {
 	range 10.36.14.3 10.36.14.126;
 		option routers 10.36.14.1;
 		option broadcast-address 10.36.14.127;
 		option domain-name-servers 10.36.14.134;
 		default-lease-time 3600;
 		max-lease-time 5760;
 }
 #A7
 subnet 10.36.12.0 netmask 255.255.254.0 {
 	range 10.36.12.3 10.36.13.254;
 		option routers 10.36.12.1;
 		option broadcast-address 10.36.13.255;
 		option domain-name-servers 10.36.14.134;
 		default-lease-time 3600;
 		max-lease-time 5760;
 }
 #A6
 subnet 10.36.14.136 netmask 255.255.255.252 {
 }
 #A5
 subnet 10.36.14.140 netmask 255.255.255.252 {
 }
 #A2
 subnet 10.36.0.0 netmask 255.255.248.0 {
 	range 10.36.0.2 10.36.7.254;
 		option routers 10.36.0.1;
 		option broadcast-address 10.36.7.255;
 		option domain-name-servers 10.36.14.134;
 		default-lease-time 3600;
 		max-lease-time 5760;
 }
 #A10
 subnet 10.36.8.0 netmask 255.255.252.0 {
 	range 10.36.8.3 10.36.11.254;
 		option routers 10.36.8.1;
 		option broadcast-address 10.36.11.255;
 		option domain-name-servers 10.36.14.134;
 		default-lease-time 3600;
 		max-lease-time 5760;

 }
 ' > /etc/dhcp/dhcpd.conf

 service isc-dhcp-server stop
 service isc-dhcp-server start
````

## DHCP Relay (Fern - Heiter - Himmel - Aura - Frieren)
````
echo 'nameserver 192.168.122.1' > /etc/resolv.conf

apt-get update
apt-get install isc-dhcp-relay -y

echo '
SERVERS="10.36.14.130"
INTERFACES="eth0 eth1 eth2"
OPTIONS=
' > /etc/default/isc-dhcp-relay

echo 'net.ipv4.ip_forward=1' > /etc/sysctl.conf

service isc-dhcp-relay restart
````

## DNS Server (Richter)
````
echo 'nameserver 192.168.122.1' > /etc/resolv.conf

apt-get update
apt-get install isc-dhcp-relay -y

 echo '
 options {
 	directory "/var/cache/bind";
 	forwarders {
 		192.168.122.1;
 	};
 	// dnssec-validation auto;
 	allow-query{any;};
 	auth-nxdomain no;
 	listen-on-v6 { any; };
 };
 ' > /etc/bind/named.conf.options

 service bind9 start
````

# No 1
> Agar topologi yang kalian buat dapat mengakses keluar, kalian diminta untuk mengkonfigurasi Aura menggunakan iptables, tetapi tidak ingin menggunakan MASQUERADE.
kita akan memasukkan codingan ini di node `Aura`
````
IPETH0="$(ip -br a | grep eth0 | awk '{print $NF}' | cut -d'/' -f1)"
iptables -t nat -A POSTROUTING -o eth0 -j SNAT --to-source "$IPETH0" -s 10.36.0.0/20
````
menggunakan SNAT --to-source yang mengarah pada NID dari router yang berhubungan dengan NAT, yaitu 10.36.0.0/20. 
Dengan kata lain, IP tesebut adalah IP terluar / terjauh yang mencakup seluruh IP yang kita peroleh sebelumnya.

Sebelumnya juga perlu didefinisikan interface mana yang terkoneksi dengan NAT. Pada kasus ini adalah Aura, interface yang berhubungan adalah eth0. Definisi tersebut dapat dimasukkan ke dalam sebuah variabel. Di sini, digunakan variabel bernama IPETH0.
dan akan mendapatkan hasil seperti berikut
- Router
![image](https://github.com/Chrstnkevin/Jarkom-Modul-5-D29-2023/assets/97864068/33591045-7622-4159-954d-dcb51d97d796)

- Server
![image](https://github.com/Chrstnkevin/Jarkom-Modul-5-D29-2023/assets/97864068/6e31f898-8c1b-4583-ba70-c04788a1e348)

-Client
![image](https://github.com/Chrstnkevin/Jarkom-Modul-5-D29-2023/assets/97864068/36b5aa10-6076-42fc-9883-3fe8a7d75878)

# No 2
> Kalian diminta untuk melakukan drop semua TCP dan UDP kecuali port 8080 pada TCP.

````
apt install netcat
````

pada sender masukkan
````
nc <ip receiver> 8080
````

pada receiver masukkan
````
iptables -A INPUT -p tcp --dport 8080 -j ACCEPT
iptables -A INPUT -p tcp -j DROP
iptables -A INPUT -p udp -j DROP

nc -l -p 8080
````
Penjelasan

-A INPUT: Menambahkan aturan ke chain INPUT (rantai yang digunakan untuk lalu lintas yang menuju ke sistem).
-p tcp: Menentukan protokol yang digunakan, dalam hal ini TCP. --dport 8080: Menentukan port tujuan, dalam hal ini port 8080.
-j ACCEPT: Menentukan tindakan yang diambil jika paket memenuhi kriteria aturan, dalam hal ini menerima paket.
-j DROP: Menentukan tindakan yang diambil jika paket memenuhi kriteria aturan, dalam hal ini menolak (DROP) paket.''

# No 3
> Kepala Suku North Area meminta kalian untuk membatasi DHCP dan DNS Server hanya dapat dilakukan ping oleh maksimal 3 device secara bersamaan, selebihnya akan di drop

````
iptables -I INPUT -p icmp -m connlimit --connlimit-above 3 --connlimit-mask 0 -j DROP
iptables -I INPUT -m state --state ESTABLISHED,RELATED -j ACCEPT
````
dengan memanfaatkan port icmp, dilakukan limit koneksi dengan --connlimit-above menggunakan parameter 3. Tidak lupa, gunakan mask 0 yang berarti semua akses akan masuk ke dalam filtering dari --connlimit. Jika telah terdapat 3 koneksi, maka koneksi selanjutnya akan di-drop.

Penjelasan
-I INPUT: Menyisipkan aturan ke awal chain INPUT.
-p icmp: Menentukan protokol yang digunakan, dalam hal ini ICMP (Internet Control Message Protocol), yang sering digunakan untuk ping dan pesan kontrol jaringan.
-m connlimit: Menggunakan modul connlimit untuk membatasi jumlah koneksi.
--connlimit-above 3: Menentukan batas atas jumlah koneksi yang diizinkan. Dalam hal ini, aturan ini akan mencoba membatasi jumlah koneksi ICMP di atas 3.
--connlimit-mask 0: Menetapkan mask untuk mengidentifikasi koneksi. Dengan nilai 0, aturan ini akan membatasi jumlah koneksi berdasarkan alamat IP sumber.
--state ESTABLISHED,RELATED: Menentukan bahwa aturan ini akan diterapkan pada paket yang terkait dengan koneksi yang sudah didirikan (ESTABLISHED) atau terkait dengan koneksi yang ada (RELATED), misalnya, paket tanggapan terkait permintaan koneksi.
-m state: Menggunakan modul state untuk mengelola status koneksi.
-j DROP: Menentukan tindakan yang diambil jika batasan koneksi terlampaui, dalam hal ini menolak (DROP) paket.

Hasil
- Ping 10.36.14.130 di Stark
![image](https://github.com/Chrstnkevin/Jarkom-Modul-5-D29-2023/assets/97864068/00255a46-9c6a-460c-9701-2646e941a5d5)

- Ping 10.36.14.130 di heiter
![image](https://github.com/Chrstnkevin/Jarkom-Modul-5-D29-2023/assets/97864068/183539b5-b866-462e-bb9f-480f3aac3c10)

- Ping 10.36.14.130 di Frieren
![image](https://github.com/Chrstnkevin/Jarkom-Modul-5-D29-2023/assets/97864068/c40e7ab0-26dd-45fb-877e-f5c69ab22eee)

- Ketika ingin ping selanjutnya di node berbeda maka akan terhenti dan tidak akan diproses
![image](https://github.com/Chrstnkevin/Jarkom-Modul-5-D29-2023/assets/97864068/42702bfc-2d85-4e01-8027-ddeed5fb8006)

# No 4
> Lakukan pembatasan sehingga koneksi SSH pada Web Server hanya dapat dilakukan oleh masyarakat yang berada pada GrobeForest.

masuk ke `webserver`
````
iptables -A INPUT -p tcp --dport 22 -s 10.10.8.0/22 -j ACCEPT

iptables -A INPUT -p tcp --dport 22 -j REJECT
````
- testing `nmap 10.36.14.142 -p 22` pada GrobeForest, bisa terlihat dia open untuk port 22.
![image](https://github.com/Chrstnkevin/Jarkom-Modul-5-D29-2023/assets/97864068/4d3d0842-87b0-48f3-9df2-52b01069e4bc)

- testing `nmap 10.36.14.142 -p 22` pada TurkRegion, bisa terlihat dia closed untuk port 22.
![image](https://github.com/Chrstnkevin/Jarkom-Modul-5-D29-2023/assets/97864068/f237b302-9916-4456-84ac-d72864f83168)

# No 5
> Selain itu, akses menuju WebServer hanya diperbolehkan saat jam kerja yaitu Senin-Jumat pada pukul 08.00-16.00.

masuk ke `webserver`
````
iptables -A INPUT -m time --timestart 08:00 --timestop 16:00 --weekdays Mon,Tue,Wed,Thu,Fri -j ACCEPT

iptables -A INPUT -j REJECT
````

set pada web server jam yang diinginkan `date --set="2023-12-13 14:00:00"`
![image](https://github.com/Chrstnkevin/Jarkom-Modul-5-D29-2023/assets/97864068/1fe93e9d-3883-48bf-b63c-60b7cae4e6ec)

maka akan didapatkan hasil berhasil dan gagal
![image](https://github.com/Chrstnkevin/Jarkom-Modul-5-D29-2023/assets/97864068/7f388b55-4782-42b7-9303-191c0d2282dd)

# No 6
> Lalu, karena ternyata terdapat beberapa waktu di mana network administrator dari WebServer tidak bisa stand by, sehingga perlu ditambahkan rule bahwa akses pada hari Senin - Kamis pada jam 12.00 - 13.00 dilarang (istirahat maksi cuy) dan akses di hari Jumat pada jam 11.00 - 13.00 juga dilarang (maklum, Jumatan rek).

Di Web Server `(Sein dan Stark)`

`
iptables -A INPUT -p tcp --dport 80 -m time --timestart 13:01 --timestop 10:59 --weekdays Mon,Tue,Wed,Thu,Fri -j ACCEPT
iptables -A INPUT -p tcp --dport 80 -m time --timestart 12:00 --timestop 13:00 --weekdays Mon,Tue,Wed,Thu -j DROP
iptables -A INPUT -p tcp --dport 80 -m time --timestart 11:00 --timestop 13:00 --weekdays Fri -j DROP
iptables -A INPUT -p tcp --dport 80 -j DROP
`

Lalu, lakukan testing di client (saya run di TurkRegion) dengan mengubah tanggalnya terlebih dahulu

`
nmap 10.36.8.2
nmap 10.36.14.142
`

# No 7

> Karena terdapat 2 WebServer, kalian diminta agar setiap client yang mengakses Sein dengan Port 80 akan didistribusikan secara bergantian pada Sein dan Stark secara berurutan dan request dari client yang mengakses Stark dengan port 443 akan didistribusikan secara bergantian pada Sein dan Stark secara berurutan.

Di Router yang menempel dengan Web Server (Heiter dan Frieren)

`
iptables -A PREROUTING -t nat -p tcp --dport 80 -d 10.36.8.2 -m statistic --mode nth --every 2 --packet 0 -j DNAT --to-destination 10.36.14.142

iptables -A PREROUTING -t nat -p tcp --dport 80 -d 10.36.8.2 -j DNAT --to-destination 10.36.14.142

iptables -A PREROUTING -t nat -p tcp --dport 443 -d 10.36.14.142 -m statistic --mode nth --every 2 --packet 0 -j DNAT --to-destination 10.36.8.2

iptables -A PREROUTING -t nat -p tcp --dport 443 -d 10.36.14.142 -j DNAT --to-destination 10.36.8.2
`
lalu, lakukan testing di client (saya run di TurkRegion) deengan meengubah tanggalnya terlebih dahulu

Di Sein, jalankan
`while true; do nc -l -p 80 -c 'echo "ini sein bro"'; done`

Di Stark, jalankan
`while true; do nc -l -p 80 -c 'echo "ini stark bro"'; done`

Lalu, di client TurkRegion, jalankan
`
nc 10.36.8.2 80
nc 10.36.14.142 80
`

# No 8
Karena berbeda koalisi politik, maka subnet dengan masyarakat yang berada pada Revolte dilarang keras mengakses WebServer hingga masa pencoblosan pemilu kepala suku 2024 berakhir. Masa pemilu (hingga pemungutan dan penghitungan suara selesai) kepala suku bersamaan dengan masa pemilu Presiden dan Wakil Presiden Indonesia 2024.

Di web server (Sein dan Stark), lakukan seperti di bawah ini
`
iptables -A INPUT -s 10.36.14.130 -p tcp --dport 80 -m time --datestart 2023-12-14 --datestop 2024-06-26 -j DROP
`

Lalu, lakukan testing di Revolte dengan mengganti date teerlebih dahulu dan memasukkan syntax berikut
`
nmap 10.36.8.2 80
nmap 10.36.14.142 80
`

# No 9
Lakukan setting di Web Server (sein dan stark) dan masukkan configurasi sebagai berikut
`
iptables -N scan_port

iptables -A INPUT -m recent --name scan_port --update --seconds 600 --hitcount 20 -j DROP

iptables -A FORWARD -m recent --name scan_port --update --seconds 600 --hitcount 20 -j DROP

iptables -A INPUT -m recent --name scan_port --set -j ACCEPT

iptables -A FORWARD -m recent --name scan_port --set -j ACCEPT
`

Lalu, lakukan setting dengan menjalankan syntax berikut di GrobeForest
`
ping 10.36.8.2
ping 10.36.14.142
`

# No 10
Masukkan syntax berikut ke setiap node server (DNS, DHCP, Web) dan setiap router

`
iptables -A INPUT  -j LOG --log-level debug --log-prefix 'Dropped Packet' -m limit --limit 1/second --limit-burst 10
`





















