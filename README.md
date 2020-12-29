# Jarkom_Modul5_Praktikum_A05

- 05111840000030 MUHAMMMAD DAFFA’ AFLAH SYARIF
- 05111840000163 PUTU PUTRI NATIH DEVAYANTI

# SOAL + PEMBAHASAN
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
- Selanjutnya tes di setiap UML dengan menjalankan perintah `ping google.com`

![ping-google](https://user-images.githubusercontent.com/52326074/102999752-35643700-455c-11eb-88f0-6201720b092d.jpg)

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

![instal-dhcp-server-mojo](https://user-images.githubusercontent.com/52326074/103268642-61613b80-49e6-11eb-9cad-a80f9b9122e2.jpg)

- Lau  edit f ile `nano /etc/default/isc-dhcp-server`
```
INTERFACES="eth0"
```

![iface-eth0-mojo](https://user-images.githubusercontent.com/52326074/103268643-61f9d200-49e6-11eb-8da8-b8cb594663c4.jpg)

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

![instal-dhcp-relay-sby](https://user-images.githubusercontent.com/52326074/103268645-62926880-49e6-11eb-8f5c-5b7dcff3afbf.png)

![instal-dhcp-relay-batu](https://user-images.githubusercontent.com/52326074/103268646-632aff00-49e6-11eb-80f3-c139b3ad4a06.jpg)

![instal-dhcp-relay-kdr](https://user-images.githubusercontent.com/52326074/103268634-5e664b00-49e6-11eb-9a58-dfc480bd8696.jpg)

- Lau  edit f ile `nano /etc/default/isc-dhcp-relay`
```
SERVERS="10.151.73.51"
```

![relay-sby](https://user-images.githubusercontent.com/52326074/103268637-60300e80-49e6-11eb-9136-333de531f8e3.jpg)

![relay-kdr](https://user-images.githubusercontent.com/52326074/103268638-60c8a500-49e6-11eb-9b3c-fd1baa207ac4.jpg)

![relay-batu](https://user-images.githubusercontent.com/52326074/103268640-61613b80-49e6-11eb-9884-721d60c78258.jpg)

- Kemudian restart dengan menjalankan perintah `service isc-dhcp-relay restart`

Kemudian pada UML `SIDOARJO`, `GRESIK` dilakukan perintah berikut :

- Edit konfigurasi dengan menjalankan perintah `nano /etc/network/interfaces` sebagai berikut
```
auto eth0
iface eth0 inet dhcp
```

![Screen Shot 2020-12-29 at 19 45 31](https://user-images.githubusercontent.com/56763600/103281740-9d0ffb80-4a0e-11eb-8e24-326dfd13191a.png)
![Screen Shot 2020-12-29 at 19 45 54](https://user-images.githubusercontent.com/56763600/103281741-9e412880-4a0e-11eb-9b13-23f2825e5470.png)

- Restart network dengan mengetikkan `service networking restart` pada setiap UML

### (1) Agar topologi yang kalian buat dapat mengakses keluar, kalian diminta untuk mengkonfigurasi SURABAYA menggunakan iptables, namun Bibah tidak ingin kalian menggunakan MASQUERADE.

- nano soal1.sh di UML SURABAYA
```
iptables -t nat -A POSTROUTING -o eth0 -j SNAT --to 10.151.72.26
```

![1-sh](https://user-images.githubusercontent.com/52326074/103270122-ec900080-49e9-11eb-97af-0816a78bb8f1.jpg)

- bash soal1.sh kemudian ping pada client dan server

![1-1-testing](https://user-images.githubusercontent.com/52326074/103270176-f6b1ff00-49e9-11eb-9979-fde1bb3d135d.jpg)

![1-2-testing](https://user-images.githubusercontent.com/52326074/103270179-f74a9580-49e9-11eb-9716-8d2dfba6ba57.png)

![1-3-testing](https://user-images.githubusercontent.com/52326074/103270116-eb5ed380-49e9-11eb-85c8-6c1dd1424bab.jpg)

### (2) Kalian diminta untuk mendrop semua akses SSH dari luar Topologi (UML) Kalian pada server yang memiliki ip DMZ (DHCP dan DNS SERVER) pada SURABAYA demi menjaga keamanan.

- nano soal2.sh di UML SURABAYA
```
iptables -A FORWARD -p tcp --dport 22 -d 10.151.73.48/29 -i eth0 -j DROP
```

![2-sh](https://user-images.githubusercontent.com/52326074/103270125-ed289700-49e9-11eb-9bf0-5596ce776b1a.png)

- `bash soal2.sh`

![Screen Shot 2020-12-29 at 20 16 42](https://user-images.githubusercontent.com/56763600/103283317-35a87a80-4a13-11eb-9c0d-dd7db5f3da45.png)

- install netcat pada UML MALANG

- `nc -l -p 22` pada UML MALANG kemudian `nc 10.151.73.50 22` pada putty, ketikkan apapun

![2-testing](https://user-images.githubusercontent.com/52326074/103270170-f580d200-49e9-11eb-9dc7-6c2bfb330ce7.jpg)

- pesan di UML malang tidak tertampilkan

### (3) Karena tim kalian maksimal terdiri dari 3 orang, Bibah meminta kalian untuk membatasi DHCP dan DNS server hanya boleh menerima maksimal 3 koneksi ICMP secara bersamaan yang berasal dari mana saja menggunakan iptables pada masing masing server, selebihnya akan di DROP.

- `nano soal3.sh` di UML MALANG & MOJOKERTO
```
iptables -A INPUT -p icmp -m connlimit --connlimit-above 3 --connlimit-mask 0 -j DROP
```

![3-sh-mlgmojo](https://user-images.githubusercontent.com/52326074/103270134-ee59c400-49e9-11eb-96bd-8f479e2555f3.png)

- `bash soal3.sh` pada UML MALANG & MOJOKERTO

![3-1-sh](https://user-images.githubusercontent.com/52326074/103270136-eef25a80-49e9-11eb-9cd1-21f3796dda57.jpg)

![3-2-sh](https://user-images.githubusercontent.com/52326074/103270140-ef8af100-49e9-11eb-96fd-b07a803c241c.jpg)

- kemudian `ping 10.15.73.50`, terlihat apabila di-ping di lebih dari 3 UML maka pada UML berikut-berikutnya ping tidak bekerja

![3-testing](https://user-images.githubusercontent.com/52326074/103270159-f285e180-49e9-11eb-8376-bca216609d47.png)

kemudian kalian diminta untuk membatasi akses ke MALANG yang berasal dari SUBNET SIDOARJO dan SUBNET GRESIK dengan peraturan sebagai berikut:
### ● (4) Akses dari subnet SIDOARJO hanya diperbolehkan pada pukul 07.00 - 17.00 pada hari Senin sampai Jumat.

- `nano soal4.sh` di UML MALANG
```
iptables -A INPUT -s 192.168.1.0/24 -m time --timestart 07:00 --timestop 17:00 --weekdays Mon,Tue,Wed,Thu,Fri -j ACCEPT
iptables -A INPUT -s 192.168.1.0/24 -m time --timestart 17:01 --timestop 06:59 -j REJECT
iptables -A INPUT -s 192.168.1.0/24 -m time --timestart 07:00 --timestop 17:00 --weekdays Sat,Sun -j REJECT
```

![4-sh](https://user-images.githubusercontent.com/52326074/103270142-ef8af100-49e9-11eb-9a19-27f7ba10efc4.jpg)

- `bash soal4.sh` pada UML MALANG
- jalankan `date -s '2020-12-29 20:00:00'`
- kemudian `ping 10.15.73.50` pada UML SIDOARJO

![4-testing](https://user-images.githubusercontent.com/52326074/103270145-f0238780-49e9-11eb-98af-6b21d6c4b9ce.jpg)

### ● (5) Akses dari subnet GRESIK hanya diperbolehkan pada pukul 17.00 hingga pukul 07.00 setiap harinya.
Selain itu paket akan di REJECT.

- `nano soal5.sh` pada UML MALANG
```
iptables -A INPUT -s 192.168.2.0/24 -m time --timestart 07:01 --timestop 16:59 -j REJECT
```

- `bash soal5.sh` pada UML MALANG
- jalankan `date -s '2020-12-29 20:00:00' 

- kemudian `ping 10.151.73.50` pada UML GRESIK untuk melakukan testing.

- maka terlihat bahwa UML dapat diakses

- testing selanjutnya jalankan `date -s '2020-12-29 14:00:00' 

- kemudian `ping 10.151.73.50` pada UML GRESIK untuk melakukan testing.

![5-testing](https://user-images.githubusercontent.com/52326074/103270154-f1ed4b00-49e9-11eb-9444-f972813961d7.jpg)

- terlihat bahwa UML tidak dapat diakses

Karena kita memiliki 2 buah WEB Server, 
### (6) Bibah ingin SURABAYA disetting sehingga setiap request dari client yang mengakses DNS Server akan didistribusikan secara bergantian pada PROBOLINGGO port 80 dan MADIUN port 80.

- Lakukan perintah `nano soal6.sh` pada UML Surabaya yang berisikan

```
iptables -t nat -A PREROUTING -p tcp -d 10.151.73.50 -m statistic --mode nth --every 2 --packet 0 -j DNAT --to-destination 192.168.0.11:80
iptables -t nat -A PREROUTING -p tcp -d 10.151.73.50 -j DNAT --to-destination 192.168.0.10:80
iptables -t nat -A POSTROUTING -p tcp -d 192.168.0.11 --dport 80 -j SNAT --to-source 10.151.73.50
iptables -t nat -A POSTROUTING -p tcp -d 192.168.0.10 --dport 80 -j SNAT --to-source 10.151.73.50
```

- Lakukan perintah `bash soal6.sh`
![messageImage_1609241447742](https://user-images.githubusercontent.com/56763600/103282123-acdc0f80-4a0f-11eb-90cd-05d41907cf72.jpg)

- Di UML Madiun lakukan perintah `nc -l -p 80` dan `nc 10.151.73.50 80` pada UML Gresik, ketikkan kata-kata apapun
![Screen Shot 2020-12-29 at 19 29 25](https://user-images.githubusercontent.com/56763600/103282135-bbc2c200-4a0f-11eb-8fdc-41f194641c1e.png)

- Di UML Probolinggo lakukan perintah `nc -l -p 80` dan `nc 10.151.73.50 80` pada UML Gresik, ketikkan kata-kata apapun
![Screen Shot 2020-12-29 at 19 29 57](https://user-images.githubusercontent.com/56763600/103282138-be251c00-4a0f-11eb-80bf-2ffe9922dbfc.png)


### (7) Bibah ingin agar semua paket didrop oleh firewall (dalam topologi) tercatat dalam log pada setiap UML yang memiliki aturan drop.

- Lakukan perintah `nano soal7.sh` pada UML Malang, Mojokerto, Surabaya yang berisikan

```
iptables -N LOGGING
iptables -A FORWARD -j LOGGING
iptables -A LOGGING -m limit --limit 2/min -j LOG --log-prefix "IPTables-Dropped: " --log-level 4
iptables -A LOGGING -j DROP
```

![messageImage_1609206393916](https://user-images.githubusercontent.com/56763600/103282153-c7ae8400-4a0f-11eb-8916-7fbbd7025e60.jpg)

- Lakukan `bash soal7.sh` 

- Mengacu pada soal 2
![no 7](https://user-images.githubusercontent.com/56763600/103282188-ee6cba80-4a0f-11eb-9050-183839918947.png)

- Mengacu pada soal 3 dan melakukan `ping 10.151.72.26`
![messageImage_1609241990197](https://user-images.githubusercontent.com/56763600/103282195-f6c4f580-4a0f-11eb-88a7-3c32f3ec8383.jpg)

# TERIMA KASIH :)
