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
