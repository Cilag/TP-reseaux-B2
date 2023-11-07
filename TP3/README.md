# TP3 INFRA : Premiers pas GNS, Cisco et VLAN
## I. Dumb switch
🌞 Commençons simple

définissez les IPs statiques sur les deux VPCS :

```
PC1> ip 10.3.1.1 255.255.255.0
Checking for duplicate address...
PC1 : 10.3.1.1 255.255.255.0

PC1> show ip

NAME        : PC1[1]
IP/MASK     : 10.3.1.1/24
GATEWAY     : 255.255.255.0
DNS         :
MAC         : 00:50:79:66:68:00
LPORT       : 20004
RHOST:PORT  : 127.0.0.1:20005
MTU         : 1500
```
```
PC2> ip 10.3.1.2 255.255.255.0
Checking for duplicate address...
PC2 : 10.3.1.2 255.255.255.0

PC2> show ip

NAME        : PC2[1]
IP/MASK     : 10.3.1.2/24
GATEWAY     : 255.255.255.0
DNS         :
MAC         : 00:50:79:66:68:01
LPORT       : 20006
RHOST:PORT  : 127.0.0.1:20007
MTU         : 1500
```
ping un VPCS depuis l'autre :
```
PC2> ping 10.3.1.1

84 bytes from 10.3.1.1 icmp_seq=1 ttl=64 time=2.217 ms
84 bytes from 10.3.1.1 icmp_seq=2 ttl=64 time=4.868 ms
84 bytes from 10.3.1.1 icmp_seq=3 ttl=64 time=5.539 ms
84 bytes from 10.3.1.1 icmp_seq=4 ttl=64 time=5.085 ms
84 bytes from 10.3.1.1 icmp_seq=5 ttl=64 time=4.496 ms
```
```
PC1> ping 10.3.1.2

84 bytes from 10.3.1.2 icmp_seq=1 ttl=64 time=21.155 ms
84 bytes from 10.3.1.2 icmp_seq=2 ttl=64 time=5.707 ms
84 bytes from 10.3.1.2 icmp_seq=3 ttl=64 time=5.425 ms
84 bytes from 10.3.1.2 icmp_seq=4 ttl=64 time=15.858 ms
84 bytes from 10.3.1.2 icmp_seq=5 ttl=64 time=9.253 ms
```
afficher la CAM table du switch et vérifier les MAC qui s'y trouvent :
```
IOU1#show mac address-table
          Mac Address Table
-------------------------------------------

Vlan    Mac Address       Type        Ports
----    -----------       --------    -----
   1    0050.7966.6800    DYNAMIC     Et0/1
   1    0050.7966.6801    DYNAMIC     Et0/2
Total Mac Addresses for this criterion: 2
```
## II. VLAN

🌞 Adressage

définissez les IPs statiques sur tous les VPCS
```
PC1> show ip

NAME        : PC1[1]
IP/MASK     : 10.3.1.1/24
GATEWAY     : 255.255.255.0
DNS         :
MAC         : 00:50:79:66:68:00
LPORT       : 20004
RHOST:PORT  : 127.0.0.1:20005
MTU         : 1500

```
```
PC2> show ip

NAME        : PC2[1]
IP/MASK     : 10.3.1.2/24
GATEWAY     : 255.255.255.0
DNS         :
MAC         : 00:50:79:66:68:01
LPORT       : 20006
RHOST:PORT  : 127.0.0.1:20007
MTU         : 1500


```
```
PC3> show ip

NAME        : PC3[1]
IP/MASK     : 10.3.1.3/24
GATEWAY     : 0.0.0.0
DNS         :
MAC         : 00:50:79:66:68:02
LPORT       : 20010
RHOST:PORT  : 127.0.0.1:20011
MTU         : 1500

```

vérifiez avec des ping que tout le monde se ping

```
PC3> ping 10.3.1.1

84 bytes from 10.3.1.1 icmp_seq=1 ttl=64 time=3.091 ms
84 bytes from 10.3.1.1 icmp_seq=2 ttl=64 time=5.394 ms
84 bytes from 10.3.1.1 icmp_seq=3 ttl=64 time=5.910 ms
84 bytes from 10.3.1.1 icmp_seq=4 ttl=64 time=7.771 ms
84 bytes from 10.3.1.1 icmp_seq=5 ttl=64 time=7.811 ms
```
```
PC3> ping 10.3.1.2

84 bytes from 10.3.1.2 icmp_seq=1 ttl=64 time=11.967 ms
84 bytes from 10.3.1.2 icmp_seq=2 ttl=64 time=2.701 ms
84 bytes from 10.3.1.2 icmp_seq=3 ttl=64 time=2.469 ms
84 bytes from 10.3.1.2 icmp_seq=4 ttl=64 time=8.774 ms
84 bytes from 10.3.1.2 icmp_seq=5 ttl=64 time=3.300 ms
```
```
PC2> ping 10.3.1.1

84 bytes from 10.3.1.1 icmp_seq=1 ttl=64 time=1.629 ms
84 bytes from 10.3.1.1 icmp_seq=2 ttl=64 time=5.982 ms
84 bytes from 10.3.1.1 icmp_seq=3 ttl=64 time=8.222 ms
84 bytes from 10.3.1.1 icmp_seq=4 ttl=64 time=18.006 ms
84 bytes from 10.3.1.1 icmp_seq=5 ttl=64 time=1.240 ms
```

🌞 Configuration des VLANs

déclaration des VLANs sur le switch sw1 :

```
IOU1(config)#vlan 10
IOU1(config-vlan)#name client
IOU1(config-vlan)#vlan 20
IOU1(config-vlan)#name admin
IOU1(config-vlan)#exit
IOU1(config)#exit
```
```
IOU1#show vlan

VLAN Name                             Status    Ports
---- -------------------------------- --------- -------------------------------
1    default                          active    Et0/0, Et0/1, Et0/2, Et0/3
                                                Et1/0, Et1/1, Et1/2, Et1/3
                                                Et2/0, Et2/1, Et2/2, Et2/3
                                                Et3/0, Et3/1, Et3/2, Et3/3
10   client                           active
20   admins                           active
1002 fddi-default                     act/unsup
1003 token-ring-default               act/unsup
1004 fddinet-default                  act/unsup
1005 trnet-default                    act/unsup
```
ajout des ports du switches dans le bon VLAN :

```
IOU1#conf t
Enter configuration commands, one per line.  End with CNTL/Z.
IOU1(config)#interface Ethernet0/0
IOU1(config-if)#switchport mode access
IOU1(config-if)#switchport access vlan 10
IOU1(config-if)#exit
IOU1(config)#interface Ethernet0/1
IOU1(config-if)#switchport mode access
IOU1(config-if)#interface Ethernet0/1
IOU1(config-if)#switchport mode access
IOU1(config-if)#switchport access vlan 10
IOU1(config-if)#exit
IOU1(config)#interface Ethernet0/2
IOU1(config-if)#switchport mode access
IOU1(config-if)#switchport access vlan 20
IOU1(config-if)#exit
```
```
IOU1#show vlan

VLAN Name                             Status    Ports
---- -------------------------------- --------- -------------------------------
1    default                          active    Et0/0, Et1/0, Et1/1, Et1/2
                                                Et1/3, Et2/0, Et2/1, Et2/2
                                                Et2/3, Et3/0, Et3/1, Et3/2
                                                Et3/3
10   client                           active    Et0/1, Et0/2
20   admins                           active    Et0/3
1002 fddi-default                     act/unsup
1003 token-ring-default               act/unsup
1004 fddinet-default                  act/unsup
1005 trnet-default                    act/unsup
```


🌞 Vérif


pc1 et pc2 doivent toujours pouvoir se ping
pc3 ne ping plus personne :

- PC1 -> PC2
```
PC1> ping 10.3.1.2

84 bytes from 10.3.1.2 icmp_seq=1 ttl=64 time=5.553 ms
84 bytes from 10.3.1.2 icmp_seq=2 ttl=64 time=3.834 ms
84 bytes from 10.3.1.2 icmp_seq=3 ttl=64 time=4.897 ms
84 bytes from 10.3.1.2 icmp_seq=4 ttl=64 time=5.263 ms
84 bytes from 10.3.1.2 icmp_seq=5 ttl=64 time=4.099 ms
```
- PC2 -> PC1
```
PC2> ping 10.3.1.1

84 bytes from 10.3.1.1 icmp_seq=1 ttl=64 time=4.098 ms
84 bytes from 10.3.1.1 icmp_seq=2 ttl=64 time=3.629 ms
84 bytes from 10.3.1.1 icmp_seq=3 ttl=64 time=21.119 ms
84 bytes from 10.3.1.1 icmp_seq=4 ttl=64 time=2.405 ms
84 bytes from 10.3.1.1 icmp_seq=5 ttl=64 time=8.477 ms
```
- PC3 -> PC2
```
PC3> ping 10.3.1.2

host (10.3.1.2) not reachable
```
![Alt text](img/vlan.png)

# III. Ptite VM DHCP
 - VM dhcp.tp3.b2
 
 installez un serveur DHCP
```
[c1@dhcp ~]$ sudo dnf install dhcp-server -y
```
modif du fichier conf :
```
[c1@dhcp ~]$ sudo nano /etc/dhcp/dhcpd.conf
```
```
[c1@dhcp ~]$ sudo cat /etc/dhcp/dhcpd.conf
#
# DHCP Server Configuration file.
#   see /usr/share/doc/dhcp-server/dhcpd.conf .example
#   see dhcpd.conf(5) man page
#
#
option domain-name "GNS-server";

option domain-name-servers 1.1.1.1;

default-lease-time 600;

max-lease-time 7200;

authoritative;

subnet 10.3.1.0 netmask 255.255.255.0 {
        range dynamic-bootp 10.3.1.100 10.3.1.200;
}
```

il doit délivrer des IPs entre 10.3.1.100 et 10.3.1.200

```
IOU1#show vlan br

VLAN Name                             Status    Ports
---- -------------------------------- --------- -------------------------------
1    default                          active    Et1/2, Et1/3, Et2/0, Et2/1
                                                Et2/2, Et2/3, Et3/0, Et3/1
                                                Et3/2, Et3/3
10   client                           active    Et0/0, Et0/1, Et1/0
20   admin                            active    Et0/2, Et0/3, Et1/1
1002 fddi-default                     act/unsup
1003 token-ring-default               act/unsup
1004 fddinet-default                  act/unsup
1005 trnet-default                    act/unsup

```
vérifier avec le pc4 que vous pouvez récupérer une IP en DHCP :
```
PC4> ip dhcp -r
DDORA
PC4> show ip IP 10.3.1.100/24

NAME        : PC4[1]
IP/MASK     : 10.3.1.100/24
GATEWAY     : 0.0.0.0
DNS         : 1.1.1.1
DHCP SERVER : 10.3.1.254
DHCP LEASE  : 590, 600/300/525
DOMAIN NAME : GNS-server
MAC         : 00:50:79:66:68:03
LPORT       : 20028
RHOST:PORT  : 127.0.0.1:20029
MTU         : 1500

```
![Alt text](<img/Capture d'écran 2023-11-07 202957.png>)

vérifier avec le pc5 que vous ne pouvez PAS récupérer une IP en DHCP :
```
PC5> ip dhcp -r
DDD
Can't find dhcp server

```
![Alt text](<img/Capture d'écran 2023-11-07 203308.png>)