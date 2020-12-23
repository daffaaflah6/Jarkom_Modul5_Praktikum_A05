# Jarkom_Modul5_Praktikum_A05

- 05111840000030 MUHAMMMAD DAFFA’ AFLAH SYARIF
- 05111840000163 PUTU PUTRI NATIH DEVAYANTI

### (A) Tugas pertama kalian yaitu membuat topologi jaringan sesuai dengan rancangan yang diberikan Bibah seperti dibawah ini :

![Screen Shot 2020-12-23 at 15 38 37](https://user-images.githubusercontent.com/56763600/102973091-2454ff00-4537-11eb-9f3d-36d6c02c06bf.png)

Keterangan : SURABAYA diberikan IP TUNTAP

```
MALANG merupakan DNS Server diberikan IP DMZ
MOJOKERTO merupakan DHCP Server diberikan IP DMZ
MADIUN dan PROBOLINGGO merupakan WEB Server
Setiap Server diberikan memory sebesar 128M
Client dan Router diberikan memori sebesar 96M
Jumlah host pada subnet SIDOARJO 200 Host
Jumlah host pada subnet GRESIK 210 Host
```

- Topologi
```
# Switch
uml_switch -unix switch0 > /dev/null < /dev/null & #A01
uml_switch -unix switch1 > /dev/null < /dev/null & #A02
uml_switch -unix switch2 > /dev/null < /dev/null & #A03
uml_switch -unix switch3 > /dev/null < /dev/null & #A04
uml_switch -unix switch4 > /dev/null < /dev/null & #A05
uml_switch -unix switch5 > /dev/null < /dev/null & #A06

# Router
xterm -T SURABAYA -e linux ubd0=SURABAYA,jarkom umid=SURABAYA eth0=tuntap,,,10.151.72.25 eth1=daemon,,,switch0 eth2=daemon,,,switch3 mem=96M &
xterm -T BATU -e linux ubd0=BATU,jarkom umid=BATU eth0=daemon,,,switch3 eth1=daemon,,,switch2 eth2=daemon,,,switch5 mem=96M &
xterm -T KEDIRI -e linux ubd0=KEDIRI,jarkom umid=KEDIRI eth0=daemon,,,switch0 eth1=daemon,,,switch1 eth2=daemon,,,switch4 mem=96M &

# Server
xterm -T PROBOLINGGO -e linux ubd0=PROBOLINGGO,jarkom umid=PROBOLINGGO eth0=daemon,,,switch1 mem=128M &
xterm -T MADIUN -e linux ubd0=MADIUN,jarkom umid=MADIUN eth0=daemon,,,switch1 mem=128M &
xterm -T MALANG -e linux ubd0=MALANG,jarkom umid=MALANG eth0=daemon,,,switch2 mem=128M &
xterm -T MOJOKERTO -e linux ubd0=MOJOKERTO,jarkom umid=MOJOKERTO eth0=daemon,,,switch2 mem=128M &

# Klien 1
xterm -T GRESIK -e linux ubd0=GRESIK,jarkom umid=GRESIK eth0=daemon,,,switch4 mem=96M &
xterm -T SIDOARJO -e linux ubd0=SIDOARJO,jarkom umid=SIDOARJO eth0=daemon,,,switch5 mem=96M &
```

- Halt
```
uml_mconsole SURABAYA halt
uml_mconsole MALANG halt
uml_mconsole MOJOKERTO halt
uml_mconsole SIDOARJO halt
uml_mconsole GRESIK halt
uml_mconsole BATU halt
uml_mconsole KEDIRI halt
uml_mconsole PROBOLINGGO halt
uml_mconsole MADIUN halt
```

### (B) karena kalian telah mempelajari Subnetting dan Routing, Bibah meminta kalian untuk membuat topologi tersebut menggunakan teknik CIDR atau VLSM. 

![Screen Shot 2020-12-23 at 16 03 27](https://user-images.githubusercontent.com/56763600/102973911-91b55f80-4538-11eb-9540-267d9bd23846.png)

![Screen Shot 2020-12-23 at 16 03 33](https://user-images.githubusercontent.com/56763600/102973916-94b05000-4538-11eb-9189-19f8546e3ba3.png)

![tuntap (2)](https://user-images.githubusercontent.com/56763600/102973893-895d2480-4538-11eb-8101-682470d6f01a.png)

Setelah melakukan subnetting, 
### (C) kalian juga diharuskan melakukan routing agar setiap perangkat pada jaringan tersebut dapat terhubung.

- Membuat file dengan perintah `nano route.sh` yang isinya seperi dibawah ini dan dijalankan dengan menggunakan perintah `bash route.sh`
```
ip route add 10.151.73.48/29 via 192.168.0.2 #MALANG&MOJO via BATU
ip route add 192.168.1.0/24 via 192.168.0.2 #SIDOARJO via BATU
ip route add 192.168.2.0/24 via 192.168.0.6 #GRESIK via KEDIRI
ip route add 192.168.0.8/29 via 192.168.0.6 #PROBO&MADIUN via KEDIRI
```

- Uncomment `net.ipv4.ip_forward=1` untuk setiap router lalu jalankan perintah `sysctl -p` untuk mengetahui hasil perubahannya
- Setting IP pada setiap UML dengan mengetikkan `nano /etc/network/interfaces`

##### SURABAYA

![iface-SBY](https://user-images.githubusercontent.com/52326074/102997757-65114000-4558-11eb-97fc-5a43c6b04970.jpg)

##### BATU

![iface-BATU](https://user-images.githubusercontent.com/52326074/102997751-63477c80-4558-11eb-81f1-fc87f0239002.jpg)

##### KEDIRI

![iface-KEDIRI](https://user-images.githubusercontent.com/52326074/102997765-66426d00-4558-11eb-8665-c91649d8a79d.jpg)

##### MALANG

![iface-MLG](https://user-images.githubusercontent.com/52326074/102997717-5591f700-4558-11eb-8762-a6e47e489d8e.jpg)

##### MOJOKERTO

![iface-MOJO](https://user-images.githubusercontent.com/52326074/102997724-575bba80-4558-11eb-9a1e-3e2156b8f246.jpg)

##### MADIUN

![iface-MADIUN](https://user-images.githubusercontent.com/52326074/102997769-67739a00-4558-11eb-9dab-a7c0a5856ef2.jpg)

##### PROBOLINGGO

![iface-PROBO](https://user-images.githubusercontent.com/52326074/102997766-66db0380-4558-11eb-9e2d-cee7d8600a2a.jpg)

##### SIDOARJO

![iface-SDA](https://user-images.githubusercontent.com/52326074/102997743-5f1b5f00-4558-11eb-891f-4e7ca5b555a7.jpg)

##### GRESIK

![iface-GRESIK](https://user-images.githubusercontent.com/52326074/102997729-59257e00-4558-11eb-91df-ae2a6e4e44ee.jpg)

- Restart network dengan mengetikkan `service networking restart` pada setiap UML
- Kemudian cek perubahan dengan menjalankan perintah `ifconfig`
- Lalu pada router `SURABAYA` jalankan `iptables –t nat –A POSTROUTING –o eth0 –j MASQUERADE –s 192.168.0.0/16` agar UML dapat mengakses internet
- Selanjutnya tes di setiap UML dengan menjalankan perintah `ping its.ac.id`



### (D) Tugas berikutnya adalah memberikan ip pada subnet SIDOARJO dan GRESIK secara dinamis menggunakan bantuan DHCP SERVER (Selain subnet tersebut menggunakan ip static). 
Kemudian kalian mengingat bahwa kalian harus setting DHCP RELAY pada router yang menghubungkannya, seperti yang kalian telah pelajari di masa lalu.

- Edit file `/etc/apt/sources.list` untuk export proxy pada setiap UML dengan sintaks berikut
```
deb http://boyo.its.ac.id/debian stretch main contrib non-free
```

- Setelah itu, lakukan update pada setiap UML dengan mengetikkan `apt-get update`

Kemudian pada UML `MOJOKERTO` dilakukan perintah berikut :

- Menjalankan perintah `apt-get update`
- Kemudian jalankan perintah `apt-get install isc-dhcp-server`
- Lau  edit f ile `nano /etc/default/isc-dhcp-server`
```
INTERFACES="eth0"
```

- Selanjutnya melakukan konfigurasi DHCP Server dengan menjalankan perintah `nano /etc/dhcp/dhcpd.conf` yang didalamnya ditambahi sebagai berikut
```
subnet 10.151.73.48 netmask 255.255.255.248 {
	option routers 10.151.73.48;
	option broadcast-address 10.151.73.55;
    }

subnet 192.168.1.0 netmask 255.255.255.0 {
	range 192.168.1.2 192.168.1.213;
	option routers 192.168.1.1;
	option broadcast-address 192.168.1.255;
	option domain-name-servers 10.151.73.50;
	option domain-name-servers 202.46.129.2;
	default-lease-time 600;
	max-lease-time 7200;
}

subnet 192.168.2.0 netmask 255.255.255.0 {
    range 192.168.2.2 192.168.2.203;
    option routers 192.168.2.1;
    option broadcast-address 192.168.2.255;
    option domain-name-servers 10.151.73.50;
    option domain-name-servers 202.46.129.2;
    default-lease-time 600;
    max-lease-time 7200;
}
```

- Kemudian restart dengan menjalankan perintah `service isc-dhcp-server restart`

Kemudian pada UML `SURABAYA`, `BATU`, `KEDIRI` dilakukan perintah berikut :

- Menjalankan perintah `apt-get update`
- Kemudian jalankan perintah `apt-get install isc-dhcp-relay`
- Lau  edit f ile `nano /etc/default/isc-dhcp-relay`
```
SERVERS="10.151.73.51"
```

- Kemudian restart dengan menjalankan perintah `service isc-dhcp-relay restart`

Kemudian pada UML `SIDOARJO`, `GRESIK` dilakukan perintah berikut :

- Edit konfigurasi dengan menjalankan perintah `nano /etc/network/interfaces` sebagai berikut
```
auto eth0
iface eth0 inet dhcp
```

- Restart network dengan mengetikkan `service networking restart` pada setiap UML

### (1) Agar topologi yang kalian buat dapat mengakses keluar, kalian diminta untuk mengkonfigurasi SURABAYA menggunakan iptables, namun Bibah tidak ingin kalian menggunakan MASQUERADE.

### (2) Kalian diminta untuk mendrop semua akses SSH dari luar Topologi (UML) Kalian pada server yang memiliki ip DMZ (DHCP dan DNS SERVER) pada SURABAYA demi menjaga keamanan.

### (3) Karena tim kalian maksimal terdiri dari 3 orang, Bibah meminta kalian untuk membatasi DHCP dan DNS server hanya boleh menerima maksimal 3 koneksi ICMP secara bersamaan yang berasal dari mana saja menggunakan iptables pada masing masing server, selebihnya akan di DROP.

kemudian kalian diminta untuk membatasi akses ke MALANG yang berasal dari SUBNET SIDOARJO dan SUBNET GRESIK dengan peraturan sebagai berikut:
### ● (4) Akses dari subnet SIDOARJO hanya diperbolehkan pada pukul 07.00 - 17.00 pada hari Senin sampai Jumat.
### ● (5) Akses dari subnet GRESIK hanya diperbolehkan pada pukul 17.00 hingga pukul 07.00 setiap harinya.
Selain itu paket akan di REJECT.

Karena kita memiliki 2 buah WEB Server, 
### (6) Bibah ingin SURABAYA disetting sehingga setiap request dari client yang mengakses DNS Server akan didistribusikan secara bergantian pada PROBOLINGGO port 80 dan MADIUN port 80.

### (7) Bibah ingin agar semua paket didrop oleh firewall (dalam topologi) tercatat dalam log pada setiap UML yang memiliki aturan drop.
