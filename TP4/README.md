🌞 Adressage

définissez les IPs statiques sur toutes les machines sauf le routeur
```
PC1> ip 10.1.10.1/24
Checking for duplicate address...
PC1 : 10.1.10.1 255.255.255.0

PC1> show ip

NAME        : PC1[1]
IP/MASK     : 10.1.10.1/24
GATEWAY     : 0.0.0.0
DNS         :
MAC         : 00:50:79:66:68:00
LPORT       : 20006
RHOST:PORT  : 127.0.0.1:20007
MTU         : 1500
```
```
PC2> ip 10.1.10.2/24
Checking for duplicate address...
PC2 : 10.1.10.2 255.255.255.0

PC2> show ip

NAME        : PC2[1]
IP/MASK     : 10.1.10.2/24
GATEWAY     : 0.0.0.0
DNS         :
MAC         : 00:50:79:66:68:01
LPORT       : 20008
RHOST:PORT  : 127.0.0.1:20009
MTU         : 1500
```
```
PC3> ip 10.1.20.1/24
Checking for duplicate address...
PC3 : 10.1.20.1 255.255.255.0

PC3> show ip

NAME        : PC3[1]
IP/MASK     : 10.1.20.1/24
GATEWAY     : 0.0.0.0
DNS         :
MAC         : 00:50:79:66:68:02
LPORT       : 20010
RHOST:PORT  : 127.0.0.1:20011
MTU         : 1500
```


🌞 Configuration des VLANs
déclaration des VLANs sur le switch sw1
```

IOU1#show vlan br

VLAN Name                             Status    Ports
---- -------------------------------- --------- -------------------------------
1    default                          active    Et1/0, Et1/1, Et1/2, Et1/3
                                                Et2/0, Et2/1, Et2/2, Et2/3
                                                Et3/0, Et3/1, Et3/2, Et3/3
10   clients                          active    Et0/0, Et0/1
20   admin                            active    Et0/2
30   server                           active    Et0/3
1002 fddi-default                     act/unsup
1003 token-ring-default               act/unsup
1004 fddinet-default                  act/unsup
1005 trnet-default                    act/unsup
```

ajout des ports du switches dans le bon VLAN (voir le tableau d'adressage de la topo 2 juste au dessus)
il faudra ajouter le port qui pointe vers le routeur comme un trunk :
```
IOU1#show interface trunk

Port        Mode             Encapsulation  Status        Native vlan
Et1/0       on               802.1q         trunking      1

Port        Vlans allowed on trunk
Et1/0       1-4094

Port        Vlans allowed and active in management domain
Et1/0       1,10,20,30

Port        Vlans in spanning tree forwarding state and not pruned
Et1/0       1,10,20,30
```
🌞 Config du routeur

attribuez ses IPs au routeur
```
R1(config)#interface fastEthernet 0/0.10
R1(config-subif)#encapsulation dot1q 10
R1(config-subif)#ip addr 10.1.10.254 255.255.255.0
R1(config-subif)#exit
R1(config)#interface fastEthernet 0/0.20
R1(config-subif)#encapsulation dot1q 20
R1(config-subif)#ip addr 10.1.20.254 255.255.255.0
R1(config-subif)#exit
R1(config)#interface fastEthernet 0/0.30
R1(config-subif)#encapsulation dot1q 30
R1(config-subif)#ip addr 10.1.30.254 255.255.255.0
R1(config-subif)#exit
R1(config)#exit
```
🌞 Vérif

tout le monde doit pouvoir ping le routeur sur l'IP qui est dans son réseau
```
PC1> ping 10.1.10.254

10.1.10.254 icmp_seq=1 timeout
84 bytes from 10.1.10.254 icmp_seq=2 ttl=255 time=5.099 ms
84 bytes from 10.1.10.254 icmp_seq=3 ttl=255 time=9.239 ms
84 bytes from 10.1.10.254 icmp_seq=4 ttl=255 time=12.278 ms
84 bytes from 10.1.10.254 icmp_seq=5 ttl=255 time=12.294 ms
```
```
PC2> ping 10.1.10.254

84 bytes from 10.1.10.254 icmp_seq=1 ttl=255 time=10.948 ms
84 bytes from 10.1.10.254 icmp_seq=2 ttl=255 time=16.208 ms
84 bytes from 10.1.10.254 icmp_seq=3 ttl=255 time=6.396 ms
84 bytes from 10.1.10.254 icmp_seq=4 ttl=255 time=7.216 ms
84 bytes from 10.1.10.254 icmp_seq=5 ttl=255 time=8.994 ms
```
```
PC3> ping 10.1.20.254

84 bytes from 10.1.20.254 icmp_seq=1 ttl=255 time=10.557 ms
84 bytes from 10.1.20.254 icmp_seq=2 ttl=255 time=8.422 ms
84 bytes from 10.1.20.254 icmp_seq=3 ttl=255 time=3.782 ms
84 bytes from 10.1.20.254 icmp_seq=4 ttl=255 time=6.748 ms
84 bytes from 10.1.20.254 icmp_seq=5 ttl=255 time=9.396 ms
```
```
[c1@web1 ~]$ ping 10.1.30.254
PING 10.1.30.254 (10.1.30.254) 56(84) bytes of data.
64 bytes from 10.1.30.254: icmp_seq=1 ttl=255 time=27.8 ms
64 bytes from 10.1.30.254: icmp_seq=2 ttl=255 time=5.54 ms
64 bytes from 10.1.30.254: icmp_seq=3 ttl=255 time=8.51 ms
64 bytes from 10.1.30.254: icmp_seq=4 ttl=255 time=2.07 ms
```
ajoutez une route par défaut sur la machine virtuelle
testez des ping entre les réseaux

PC1
```
PC1> ping 10.1.20.1

10.1.20.1 icmp_seq=1 timeout
84 bytes from 10.1.20.1 icmp_seq=2 ttl=63 time=24.233 ms
84 bytes from 10.1.20.1 icmp_seq=3 ttl=63 time=22.822 ms
84 bytes from 10.1.20.1 icmp_seq=4 ttl=63 time=38.918 ms
84 bytes from 10.1.20.1 icmp_seq=5 ttl=63 time=22.445 ms
```
PC3
```
adm1> ping 10.1.30.1

84 bytes from 10.1.30.1 icmp_seq=1 ttl=63 time=12.882 ms
84 bytes from 10.1.30.1 icmp_seq=2 ttl=63 time=16.578 ms
84 bytes from 10.1.30.1 icmp_seq=3 ttl=63 time=11.573 ms
84 bytes from 10.1.30.1 icmp_seq=4 ttl=63 time=18.740 ms
```
```
[c1@web1 ~]$ ping 10.1.10.2
PING 10.1.10.2 (10.1.10.2) 56(84) bytes of data.
64 bytes from 10.1.10.2: icmp_seq=1 ttl=63 time=17.4 ms
64 bytes from 10.1.10.2: icmp_seq=2 ttl=63 time=14.2 ms
64 bytes from 10.1.10.2: icmp_seq=3 ttl=63 time=14.8 ms
64 bytes from 10.1.10.2: icmp_seq=4 ttl=63 time=17.4 ms
```
## NAT

🌞 côté routeur, il faudra récupérer un IP en DHCP

```
R1#show ip int br
Interface                  IP-Address      OK? Method Status                Protocol
FastEthernet0/0            unassigned      YES NVRAM  up                    up
FastEthernet0/0.10         10.1.10.254     YES NVRAM  up                    up
FastEthernet0/0.20         10.1.20.254     YES NVRAM  up                    up
FastEthernet0/0.30         10.1.30.254     YES NVRAM  up                    up
FastEthernet0/1            unassigned      YES DHCP   up                    up
```

🌞 vous devriez pouvoir ping 1.1.1.1
```
R1#ping 1.1.1.1

Type escape sequence to abort.
Sending 5, 100-byte ICMP Echos to 1.1.1.1, timeout is 2 seconds:
!!!!!
Success rate is 100 percent (5/5), round-trip min/avg/max = 20/22/28 ms
```
vérifiez un ping vers un nom de domaine
```

```
🌞  Vous devez me rendre le show running-config de tous les équipements
sw1 :
```
sw1#sho running-config
Building configuration...

Current configuration : 1604 bytes
!
! Last configuration change at 20:59:13 UTC Mon Nov 6 2023
!
version 15.2
service timestamps debug datetime msec
service timestamps log datetime msec
no service password-encryption
service compress-config
!
hostname sw1
!
boot-start-marker
boot-end-marker
!
!
logging discriminator EXCESS severity drops 6 msg-body drops EXCESSCOLL
logging buffered 50000
logging console discriminator EXCESS
!
no aaa new-model
!
```
sw2 :
```
sw2#sho running-config
Building configuration...

Current configuration : 1635 bytes
!
! Last configuration change at 05:45:57 UTC Tue Nov 7 2023
!
version 15.2
service timestamps debug datetime msec
service timestamps log datetime msec
no service password-encryption
service compress-config
!
hostname sw3
!
boot-start-marker
boot-end-marker
!
!
logging discriminator EXCESS severity drops 6 msg-body drops EXCESSCOLL
logging buffered 50000
logging console discriminator EXCESS
!
no aaa new-model
!
```
sw3 :
```
sw3#sho running-config
Building configuration...

Current configuration : 1635 bytes
!
! Last configuration change at 05:45:57 UTC Tue Nov 7 2023
!
version 15.2
service timestamps debug datetime msec
service timestamps log datetime msec
no service password-encryption
service compress-config
!
hostname sw3
!
boot-start-marker
boot-end-marker
!
!
logging discriminator EXCESS severity drops 6 msg-body drops EXCESSCOLL
logging buffered 50000
logging console discriminator EXCESS
!
no aaa new-model
!
```
R1 :
```
R1#show running-config
Building configuration...

Current configuration : 1599 bytes
!
version 12.4
service timestamps debug datetime msec
service timestamps log datetime msec
no service password-encryption
!
hostname R1
!
boot-start-marker
boot-end-marker
!
!
no aaa new-model
memory-size iomem 5
no ip icmp rate-limit unreachable
ip cef
!
no ip domain lookup
!
multilink bundle-name authenticated
!
archive
 log config
  hidekeys
!
ip tcp synwait-time 5
!
interface FastEthernet0/0
 no ip address
 ip nat inside
 ip virtual-reassembly
 speed 100
 full-duplex
!
interface FastEthernet0/0.10
 encapsulation dot1Q 10
 ip address 10.1.10.254 255.255.255.0
 ip nat inside
 ip virtual-reassembly
!
interface FastEthernet0/0.20
 encapsulation dot1Q 20
 ip address 10.1.20.254 255.255.255.0
 ip nat inside
 ip virtual-reassembly
!
interface FastEthernet0/0.30
 encapsulation dot1Q 30
 ip address 10.1.30.254 255.255.255.0
 ip nat inside
 ip virtual-reassembly
!
interface FastEthernet0/1
 ip address dhcp
 ip nat outside
 ip virtual-reassembly
 duplex auto
 speed auto
!
interface FastEthernet1/0
 ip address dhcp
 shutdown
 duplex auto
 speed auto
!
interface FastEthernet2/0
 no ip address
 shutdown
 duplex auto
 speed auto
!
ip forward-protocol nd
!
!
no ip http server
no ip http secure-server
ip nat inside source list 1 interface FastEthernet0/1 overload
!
access-list 1 permit any
no cdp log mismatch duplex
!
control-plane
!
line con 0
 exec-timeout 0 0
 privilege level 15
 logging synchronous
line aux 0
 exec-timeout 0 0
 privilege level 15
 logging synchronous
line vty 0 4
 login
!
!
end
```
🌞Mettre en place un serveur DHCP dans le nouveau bâtiment

serveur dhcp :
```
sudo cat /etc/dhcp/dhcpd.conf
[sudo] password for c1:
#
# DHCP Server Configuration file.
#   see /usr/share/doc/dhcp-server/dhcpd.conf.example
#   see dhcpd.conf(5) man page
#
option domain-name "GNS-server-is-back";

option domain-name-servers 1.1.1.1;

default-lease-time 600;

max-lease-time 7200;

authoritative;

subnet 10.1.10.0 netmask 255.255.255.0 {
        range dynamic-bootp 10.1.10.3 10.1.10.10;
        option routers 10.1.10.254;

}
```

🌞  Vérification
PC3 :
```

PC3> ip dhcp -r
DDORA IP 10.1.10.3/24 GW 10.1.10.254

PC3> ping 1.1.1.1

1.1.1.1 icmp_seq=1 timeout
1.1.1.1 icmp_seq=2 timeout

PC3> ping google.com
google.com resolved to 142.250.178.142

google.com icmp_seq=1 timeout
^C
PC3> ping 10.1.30.1

84 bytes from 10.1.30.1 icmp_seq=1 ttl=63 time=23.030 ms
84 bytes from 10.1.30.1 icmp_seq=2 ttl=63 time=13.298 ms
84 bytes from 10.1.30.1 icmp_seq=3 ttl=63 time=19.880 ms

PC3> ping ynov.com
ynov.com resolved to 104.26.11.233
```
PC4 :
```
PC4> ip dhcp -r
DDORA IP 10.1.10.4/24 GW 10.1.10.254

PC4> ping 10.1.30.1

84 bytes from 10.1.30.1 icmp_seq=1 ttl=63 time=21.439 ms
84 bytes from 10.1.30.1 icmp_seq=2 ttl=63 time=16.054 ms
84 bytes from 10.1.30.1 icmp_seq=3 ttl=63 time=12.233 ms
84 bytes from 10.1.30.1 icmp_seq=4 ttl=63 time=15.785 ms

PC4> ping 1.1.1.1

1.1.1.1 icmp_seq=1 timeout

PC4> ping ynov.com
ynov.com resolved to 172.67.74.226
```

PC5 :
```
PC5> ip dhcp -r
DDORA IP 10.1.10.5/24 GW 10.1.10.254

PC5> ping google.com
google.com resolved to 142.250.75.238

PC5> ping 10.1.30.1

84 bytes from 10.1.30.1 icmp_seq=1 ttl=63 time=33.649 ms
84 bytes from 10.1.30.1 icmp_seq=2 ttl=63 time=18.222 ms
84 bytes from 10.1.30.1 icmp_seq=3 ttl=63 time=14.876 ms
84 bytes from 10.1.30.1 icmp_seq=4 ttl=63 time=17.869 ms

PC5> ping ynov.com
ynov.com resolved to 172.67.74.226
```
