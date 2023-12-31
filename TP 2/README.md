# TP2 : Environnement virtuel

![Alt text](img/R.jpg)

Dans ce TP, on remanipule toujours les mêmes concepts qu'au TP1, mais en environnement virtuel avec une posture un peu plus orientée administrateur qu'au TP1.

- [TP2 : Environnement virtuel](#tp2--environnement-virtuel)
- [I. Topologie réseau](#i-topologie-réseau)
  - [Compte-rendu](#compte-rendu)
- [II. Interlude accès internet](#ii-interlude-accès-internet)
- [III. Services réseau](#iii-services-réseau)
  - [1. DHCP](#1-dhcp)
  - [2. Web web web](#2-web-web-web)

# I. Topologie réseau

## Tableau d'adressage

| Node             | LAN1 `10.1.1.0/24` | LAN2 `10.1.2.0/24` |
| ---------------- | ------------------ | ------------------ |
| `node1.lan1.tp2` | `10.1.1.11`        | x                  |
| `node2.lan1.tp2` | `10.1.1.12`        | x                  |
| `node1.lan2.tp2` | x                  | `10.1.2.11`        |
| `node2.lan2.tp2` | x                  | `10.1.2.12`        |
| `router.tp2`     | `10.1.1.254`       | `10.1.2.254`       |

- node1.lan1.tp2

## Compte-rendu

☀️ Sur **`node1.lan1.tp2`**

- afficher ses cartes réseau
```
[c1@node1 ~]$ hostname
node1.lan1.tp2
[c1@node1 ~]$ ip a
1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN group default qlen 1000
    link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
    inet 127.0.0.1/8 scope host lo
       valid_lft forever preferred_lft forever
    inet6 ::1/128 scope host
       valid_lft forever preferred_lft forever
2: enp0s8: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc fq_codel state UP group default qlen 1000
    link/ether 08:00:27:1f:44:8c brd ff:ff:ff:ff:ff:ff
    inet 10.1.1.11/24 brd 10.1.1.255 scope global noprefixroute enp0s8
       valid_lft forever preferred_lft forever
    inet6 fe80::a00:27ff:fe1f:448c/64 scope link
       valid_lft forever preferred_lft forever
```
- afficher sa table de routage
```
[c1@node1 ~]$ ip n s
10.1.1.1 dev enp0s8 lladdr 0a:00:27:00:00:0e DELAY
10.1.1.254 dev enp0s8 lladdr 08:00:27:25:06:a3 STALE
10.1.1.12 dev enp0s8 lladdr 08:00:27:da:24:57 STALE
```
- prouvez qu'il peut joindre `node2.lan2.tp2`
```
[c1@node1 ~]$ ping 10.1.2.12
PING 10.1.2.12 (10.1.2.12) 56(84) bytes of data.
64 bytes from 10.1.2.12: icmp_seq=1 ttl=63 time=6.15 ms
64 bytes from 10.1.2.12: icmp_seq=2 ttl=63 time=2.85 ms
64 bytes from 10.1.2.12: icmp_seq=3 ttl=63 time=3.16 ms
64 bytes from 10.1.2.12: icmp_seq=4 ttl=63 time=3.56 ms
64 bytes from 10.1.2.12: icmp_seq=5 ttl=63 time=3.06 ms
^C
--- 10.1.2.12 ping statistics ---
5 packets transmitted, 5 received, 0% packet loss, time 4010ms
rtt min/avg/max/mdev = 2.853/3.757/6.150/1.218 ms
[c1@node1 ~]$ ip n s
10.1.1.1 dev enp0s8 lladdr 0a:00:27:00:00:0e REACHABLE
10.1.1.254 dev enp0s8 lladdr 08:00:27:25:06:a3 REACHABLE
```
- prouvez avec un `traceroute` que le paquet passe bien par `router.tp2`
```
[c1@node1 ~]$ traceroute 10.1.2.12
traceroute to 10.1.2.12 (10.1.2.12), 30 hops max, 60 byte packets
 1  10.1.1.254 (10.1.1.254)  1.923 ms  1.669 ms  1.575 ms
 2  10.1.2.12 (10.1.2.12)  4.745 ms !X  4.646 ms !X  4.423 ms !X
```

# II. Interlude accès internet
![Alt text](<img/R (1).jpg>)
☀️ **Sur `router.tp2`**

- prouvez que vous avez un accès internet (ping d'une IP publique)
```
[c1@router ~]$ ping 8.8.8.8
PING 8.8.8.8 (8.8.8.8) 56(84) bytes of data.
64 bytes from 8.8.8.8: icmp_seq=1 ttl=56 time=22.8 ms
64 bytes from 8.8.8.8: icmp_seq=2 ttl=56 time=36.8 ms
64 bytes from 8.8.8.8: icmp_seq=3 ttl=56 time=21.1 ms
^C
--- 8.8.8.8 ping statistics ---
3 packets transmitted, 3 received, 0% packet loss, time 2007ms
rtt min/avg/max/mdev = 21.065/26.910/36.821/7.047 ms
```
- prouvez que vous pouvez résoudre des noms publics (ping d'un nom de domaine public)
```
[c1@router ~]$ ping google.com
PING google.com (142.250.200.206) 56(84) bytes of data.
64 bytes from mrs08s17-in-f14.1e100.net (142.250.200.206): icmp_seq=1 ttl=56 time=20.2 ms
64 bytes from mrs08s17-in-f14.1e100.net (142.250.200.206): icmp_seq=2 ttl=56 time=22.5 ms
64 bytes from mrs08s17-in-f14.1e100.net (142.250.200.206): icmp_seq=3 ttl=56 time=26.1 ms
64 bytes from mrs08s17-in-f14.1e100.net (142.250.200.206): icmp_seq=4 ttl=56 time=65.7 ms
^C
--- google.com ping statistics ---
4 packets transmitted, 4 received, 0% packet loss, time 3055ms
rtt min/avg/max/mdev = 20.193/33.623/65.706/18.644 ms
```
☀️ **Accès internet LAN1 et LAN2**

- ajoutez une route par défaut sur les deux machines du LAN1
```
[c1@node2 ~]$ sudo ip route add default via 10.1.1.254 dev enp0s8
[c1@node2 ~]$ ip r s
10.1.1.0/24 dev enp0s8 proto kernel scope link src 10.1.1.12 metric 100
10.1.2.0/24 via 10.1.1.254 dev enp0s8 proto static metric 100
```
- ajoutez une route par défaut sur les deux machines du LAN2
```
[c1@node2 ~]$ sudo ip route add default via 10.1.2.254 dev enp0s8
[c1@node2 ~]$ ip r s
10.1.2.0/24 dev enp0s8 proto kernel scope link src 10.1.2.12 metric 100
10.1.1.0/24 via 10.1.2.254 dev enp0s8 proto static metric 100
```
- configurez l'adresse d'un serveur DNS que vos machines peuvent utiliser pour résoudre des noms
```
[c1@node2 ~]$ cat /etc/resolv.conf
# Generated by NetworkManager
search lan1.tp1
nameserver 1.1.1.1
```
- prouvez que `node2.lan1.tp2` a un accès internet :
  - il peut ping une IP publique
  
  ![Alt text](img/OIP.jpg)
```
  [c1@node2 ~]$ ping 1.1.1.1
PING 1.1.1.1 (1.1.1.1) 56(84) bytes of data.
64 bytes from 1.1.1.1: icmp_seq=1 ttl=55 time=12.7 ms
64 bytes from 1.1.1.1: icmp_seq=2 ttl=55 time=13.4 ms
64 bytes from 1.1.1.1: icmp_seq=3 ttl=55 time=13.5 ms
^C
--- 1.1.1.1 ping statistics ---
3 packets transmitted, 3 received, 0% packet loss, time 2004ms
rtt min/avg/max/mdev = 12.719/13.189/13.482/0.348 ms
  ```
  - il peut ping un nom de domaine public
```
  [c1@node2 ~]$ ping google.com
PING google.com (142.250.75.238) 56(84) bytes of data.
64 bytes from par10s41-in-f14.1e100.net (142.250.75.238): icmp_seq=1 ttl=114 time=18.1 ms
64 bytes from par10s41-in-f14.1e100.net (142.250.75.238): icmp_seq=2 ttl=114 time=19.2 ms
64 bytes from par10s41-in-f14.1e100.net (142.250.75.238): icmp_seq=3 ttl=114 time=18.0 ms
^C
--- google.com ping statistics ---
4 packets transmitted, 3 received, 25% packet loss, time 3007ms
rtt min/avg/max/mdev = 18.021/18.425/19.186/0.538 ms
```

# III. Services réseau
## 1. DHCP
![Alt text](img/e8fa6aefad3afa081d286b5f649007eb42433e601ab917474a57ef12993ef8d7_1.jpg)

☀️ **Sur `dhcp.lan1.tp2`**

- n'oubliez pas de renommer la machine (`node2.lan1.tp2` devient `dhcp.lan1.tp2`)
```
[c1@dhcp ~]$ hostname
dhcp.lan1.tp2
```
- changez son adresse IP en `10.1.1.253
```
[c1@dhcp ~]$ ip a
1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN group default qlen 1000
    link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
    inet 127.0.0.1/8 scope host lo
       valid_lft forever preferred_lft forever
    inet6 ::1/128 scope host
       valid_lft forever preferred_lft forever
2: enp0s8: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc fq_codel state UP group default qlen 1000
    link/ether 08:00:27:c3:0d:ad brd ff:ff:ff:ff:ff:ff
    inet 10.1.1.253/24 brd 10.1.1.255 scope global noprefixroute enp0s8
       valid_lft forever preferred_lft forever
    inet6 fe80::a00:27ff:fec3:dad/64 scope link
       valid_lft forever preferred_lft forever

```
- setup du serveur DHCP
  - commande d'installation du paquet
```
sudo dnf install dhcp-server -y
sudo nano /etc/dhcp/dhcpd.conf
sudo cat /etc/dhcp/dhcpd.conf
sudo systemctl enable --now dhcpd
sudo firewall-cmd --list-all
sudo firewall-cmd --add-service=dhcp
sudo firewall-cmd --runtime-to-permanent
sudo firewall-cmd --list-all
```
  - fichier de conf
```
[c1@dhcp ~]$ sudo cat /etc/dhcp/dhcpd.conf
#
# DHCP Server Configuration file.
#   see /usr/share/doc/dhcp-server/dhcpd.conf.example
#   see dhcpd.conf(5) man page
#
# create new
# specify domain name
option domain-name "Gregouz-pepouse";

#specify DNS server's hostname or IP address
option domain-name-servers 1.1.1.1;

#default lease time
default-lease-time 600;

#max lease time
max-lease-time 7200;

# this DHCP server to be declared valid
authoritative;

#specify network address and subnetmask
subnet 10.1.1.0 netmask 255.255.255.0 {
        #specify range of lease IP address
        range dynamic-bootp 10.1.1.100 10.1.1.200;
        #specify broadcast address
        option broadcast-address 10.1.1.255;
        #specify gateway
        option routers 10.1.1.254;
}
```
  - service actif
```
[sudo] password for c1:
● dhcpd.service - DHCPv4 Server Daemon
   Loaded: loaded (/usr/lib/systemd/system/dhcpd.service; enabled; vendor preset: disabled)
   Active: active (running) since Wed 2023-10-25 11:51:58 CEST; 1h 16min ago
     Docs: man:dhcpd(8)
           man:dhcpd.conf(5)
 Main PID: 2010 (dhcpd)
   Status: "Dispatching packets..."
    Tasks: 1 (limit: 11148)
   Memory: 8.7M
   CGroup: /system.slice/dhcpd.service
           └─2010 /usr/sbin/dhcpd -f -cf /etc/dhcp/dhcpd.conf -user dhcpd -group dhcpd --no-pid

Oct 25 12:13:06 dhcp.lan1.tp2 dhcpd[2010]: DHCPACK on 10.1.1.100 to 08:00:27:1f:44:8c (node1) via enp0s8
Oct 25 12:18:06 dhcp.lan1.tp2 dhcpd[2010]: DHCPREQUEST for 10.1.1.100 from 08:00:27:1f:44:8c (node1) via enp0s8
Oct 25 12:18:06 dhcp.lan1.tp2 dhcpd[2010]: DHCPACK on 10.1.1.100 to 08:00:27:1f:44:8c (node1) via enp0s8
Oct 25 12:23:06 dhcp.lan1.tp2 dhcpd[2010]: DHCPREQUEST for 10.1.1.100 from 08:00:27:1f:44:8c (node1) via enp0s8
Oct 25 12:23:06 dhcp.lan1.tp2 dhcpd[2010]: DHCPACK on 10.1.1.100 to 08:00:27:1f:44:8c (node1) via enp0s8
Oct 25 13:07:23 dhcp.lan1.tp2 dhcpd[2010]: DHCPDISCOVER from 08:00:27:1f:44:8c (node1) via enp0s8
Oct 25 13:07:24 dhcp.lan1.tp2 dhcpd[2010]: DHCPOFFER on 10.1.1.100 to 08:00:27:1f:44:8c (node1) via enp0s8
Oct 25 13:07:25 dhcp.lan1.tp2 dhcpd[2010]: Wrote 1 leases to leases file.
Oct 25 13:07:25 dhcp.lan1.tp2 dhcpd[2010]: DHCPREQUEST for 10.1.1.100 (10.1.1.253) from 08:00:27:1f:44:8c (node1) via enp0s8
Oct 25 13:07:25 dhcp.lan1.tp2 dhcpd[2010]: DHCPACK on 10.1.1.100 to 08:00:27:1f:44:8c (node1) via enp0s8
[c1@dhcp ~]$
```

☀️ **Sur `node1.lan1.tp2`**

- demandez une IP au serveur DHCP
```
[c1@node1 ~]$ sudo cat /etc/sysconfig/network-scripts/ifcfg-enp0s8
[sudo] password for c1:
DEVICE=enp0s8

BOOTPROTO=dhcp
ONBOOT=yes
[c1@node1 ~]$
```
- prouvez que vous avez bien récupéré une IP *via* le DHCP
```
[c1@node1 ~]$ ip a
1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN group default qlen 1000
    link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
    inet 127.0.0.1/8 scope host lo
       valid_lft forever preferred_lft forever
    inet6 ::1/128 scope host
       valid_lft forever preferred_lft forever
2: enp0s8: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc fq_codel state UP group default qlen 1000
    link/ether 08:00:27:1f:44:8c brd ff:ff:ff:ff:ff:ff
    inet 10.1.1.100/24 brd 10.1.1.255 scope global dynamic noprefixroute enp0s8
       valid_lft 495sec preferred_lft 495sec
    inet6 fe80::a00:27ff:fe1f:448c/64 scope link
       valid_lft forever preferred_lft forever
[c1@node1 ~]$
```
- prouvez que vous avez bien récupéré l'IP de la passerelle
```
[c1@dhcp ~]$ ip r s
10.1.1.0/24 dev enp0s8 proto kernel scope link src 10.1.1.253 metric 100
10.1.2.0/24 via 10.1.1.254 dev enp0s8 proto static metric 100
[c1@dhcp ~]$
```
- prouvez que vous pouvez `ping node1.lan2.tp2`
```
[c1@node1 ~]$ ping 10.1.2.11
PING 10.1.2.11 (10.1.2.11) 56(84) bytes of data.
64 bytes from 10.1.2.11: icmp_seq=1 ttl=63 time=1.48 ms
64 bytes from 10.1.2.11: icmp_seq=2 ttl=63 time=2.51 ms
64 bytes from 10.1.2.11: icmp_seq=3 ttl=63 time=3.54 ms
^C
--- 10.1.2.11 ping statistics ---
3 packets transmitted, 3 received, 0% packet loss, time 2005ms
rtt min/avg/max/mdev = 1.484/2.508/3.536/0.838 ms
[c1@node1 ~]$
```
## 2. Web web web

☀️ **Sur `web.lan2.tp2`**

- n'oubliez pas de renommer la machine (`node2.lan2.tp2` devient `web.lan2.tp2`)
```
[c1@node2 ~]$ hostname
web.lan2.tp2
[c1@node2 ~]$
```
- setup du service Web
  - installation de NGINX
```
[c1@web ~]$ sudo dnf install nginx -y
[...]
Installed:
  nginx-1:1.20.1-14.el9_2.1.x86_64 nginx-core-1:1.20.1-14.el9_2.1.x86_64 nginx-filesystem-1:1.20.1-14.el9_2.1.noarch rocky-logos-httpd-90.14-1.el9.noarch

Complete!
```
  - gestion de la racine web `/var/www/site_nul/`
  
  - configuration NGINX
```

```
  - service actif
```

```
  - ouverture du port firewall
```

```
- prouvez qu'il y a un programme NGINX qui tourne derrière le port 80 de la machine (commande `ss`)
```
[c1@web ~]$ ss -ltpu | grep http
tcp   LISTEN 0      128          0.0.0.0:http      0.0.0.0:*
tcp   LISTEN 0      128             [::]:http         [::]:*
[c1@web ~]$
```
- prouvez que le firewall est bien configuré
```

```
☀️ **Sur `node1.lan1.tp2`**

- éditez le fichier `hosts` pour que `site_nul.tp2` pointe vers l'IP de `web.lan2.tp2`
```
10.1.2.12 site_nul.tp2
```
- visitez le site nul avec une commande `curl` et en utilisant le nom `site_nul.tp2`
```
ozoux@Guillaume MINGW64 ~
$ curl site_nul.tp2
<!DOCTYPE html>
<html>
<head>
    <title>Site Nul</title>
</head>
<body>
    <h1>Bienvenue sur le Site Nul</h1>
    <p>Ceci est une page web de démonstration.</p>

    <h2>Section 1</h2>
    <p>Cette section contient un exemple de texte.</p>

    <h2>Section 2</h2>
    <ul>
        <li>Élément de liste 1</li>
        <li>Élément de liste 2</li>
        <li>Élément de liste 3</li>
    </ul>

    <p>Merci de visiter le Site Nul!</p>
</body>
</html>
```
![Alt text](img/aafb72a6810f87fe0b98b5dd93f4423069399e922c386b2f21f7d2b797d7d4d7_1.jpg)