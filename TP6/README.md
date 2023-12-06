# TP6 : STP, OSPF, bigger infra
## I. STP

🌞 **Configurer STP sur les 3 switches**
```
IOU1#show spanning-tree

VLAN0001
  Spanning tree enabled protocol ieee
  Root ID    Priority    32769
             Address     aabb.cc00.0100
             This bridge is the root
             Hello Time   2 sec  Max Age 20 sec  Forward Delay 15 sec

  Bridge ID  Priority    32769  (priority 32768 sys-id-ext 1)
             Address     aabb.cc00.0100
             Hello Time   2 sec  Max Age 20 sec  Forward Delay 15 sec
             Aging Time  15  sec

Interface           Role Sts Cost      Prio.Nbr Type
------------------- ---- --- --------- -------- --------------------------------
Et0/0               Desg FWD 100       128.1    P2p
Et0/1               Desg FWD 100       128.2    P2p
Et0/2               Desg FWD 100       128.3    P2p
Et0/3               Desg FWD 100       128.4    P2p
Et1/0               Desg FWD 100       128.5    P2p
Et1/1               Desg FWD 100       128.6    P2p
Et1/2               Desg FWD 100       128.7    P2p
Et1/3               Desg FWD 100       128.8    P2p
Et2/0               Desg FWD 100       128.9    P2p
Et2/1               Desg FWD 100       128.10   P2p
Et2/2               Desg FWD 100       128.11   P2p
Et2/3               Desg FWD 100       128.12   P2p
Et3/0               Desg FWD 100       128.13   P2p
Et3/1               Desg FWD 100       128.14   P2p
Et3/2               Desg FWD 100       128.15   P2p
Et3/3               Desg FWD 100       128.16   P2p


IOU1#
```
```
IOU2#show spanning-tree

VLAN0001
  Spanning tree enabled protocol ieee
  Root ID    Priority    32769
             Address     aabb.cc00.0100
             Cost        100
             Port        2 (Ethernet0/1)
             Hello Time   2 sec  Max Age 20 sec  Forward Delay 15 sec

  Bridge ID  Priority    32769  (priority 32768 sys-id-ext 1)
             Address     aabb.cc00.0200
             Hello Time   2 sec  Max Age 20 sec  Forward Delay 15 sec
             Aging Time  300 sec

Interface           Role Sts Cost      Prio.Nbr Type
------------------- ---- --- --------- -------- --------------------------------
Et0/0               Desg FWD 100       128.1    P2p
Et0/1               Root FWD 100       128.2    P2p
Et0/2               Desg FWD 100       128.3    P2p
Et0/3               Desg FWD 100       128.4    P2p
Et1/0               Desg FWD 100       128.5    P2p
Et1/1               Desg FWD 100       128.6    P2p
Et1/2               Desg FWD 100       128.7    P2p
Et1/3               Desg FWD 100       128.8    P2p
Et2/0               Desg FWD 100       128.9    P2p
Et2/1               Desg FWD 100       128.10   P2p
Et2/2               Desg FWD 100       128.11   P2p
Et2/3               Desg FWD 100       128.12   P2p
Et3/0               Desg FWD 100       128.13   P2p
Et3/1               Desg FWD 100       128.14   P2p
Et3/2               Desg FWD 100       128.15   P2p
Et3/3               Desg FWD 100       128.16   P2p


IOU2#
```
```
IOU3#show spanning-tree

VLAN0001
  Spanning tree enabled protocol ieee
  Root ID    Priority    32769
             Address     aabb.cc00.0100
             Cost        100
             Port        3 (Ethernet0/2)
             Hello Time   2 sec  Max Age 20 sec  Forward Delay 15 sec

  Bridge ID  Priority    32769  (priority 32768 sys-id-ext 1)
             Address     aabb.cc00.0300
             Hello Time   2 sec  Max Age 20 sec  Forward Delay 15 sec
             Aging Time  300 sec

Interface           Role Sts Cost      Prio.Nbr Type
------------------- ---- --- --------- -------- --------------------------------
Et0/0               Desg FWD 100       128.1    P2p
Et0/1               Altn BLK 100       128.2    P2p
Et0/2               Root FWD 100       128.3    P2p
Et0/3               Desg FWD 100       128.4    P2p
Et1/0               Desg FWD 100       128.5    P2p
Et1/1               Desg FWD 100       128.6    P2p
Et1/2               Desg FWD 100       128.7    P2p
Et1/3               Desg FWD 100       128.8    P2p
Et2/0               Desg FWD 100       128.9    P2p
Et2/1               Desg FWD 100       128.10   P2p
Et2/2               Desg FWD 100       128.11   P2p
Et2/3               Desg FWD 100       128.12   P2p
Et3/0               Desg FWD 100       128.13   P2p
Et3/1               Desg FWD 100       128.14   P2p
Et3/2               Desg FWD 100       128.15   P2p
Et3/3               Desg FWD 100       128.16   P2p
```
🌞 **Altérer le spanning-tree** en désactivant un port
```
IOU1(config)# interface Ethernet0/3
IOU1(config-if)#shutdown
IOU1(config-if)#exit
IOU1(config)#exit
IOU1#show spanning-tree

VLAN0001
  Spanning tree enabled protocol ieee
  Root ID    Priority    32769
             Address     aabb.cc00.0100
             This bridge is the root
             Hello Time   2 sec  Max Age 20 sec  Forward Delay 15 sec

  Bridge ID  Priority    32769  (priority 32768 sys-id-ext 1)
             Address     aabb.cc00.0100
             Hello Time   2 sec  Max Age 20 sec  Forward Delay 15 sec
             Aging Time  15  sec

Interface           Role Sts Cost      Prio.Nbr Type
------------------- ---- --- --------- -------- --------------------------------
Et0/0               Desg FWD 100       128.1    P2p
Et0/1               Desg FWD 100       128.2    P2p
Et0/2               Desg FWD 100       128.3    P2p
Et1/0               Desg FWD 100       128.5    P2p
Et1/1               Desg FWD 100       128.6    P2p
Et1/2               Desg FWD 100       128.7    P2p
Et1/3               Desg FWD 100       128.8    P2p
Et2/0               Desg FWD 100       128.9    P2p
Et2/1               Desg FWD 100       128.10   P2p
Et2/2               Desg FWD 100       128.11   P2p
Et2/3               Desg FWD 100       128.12   P2p
Et3/0               Desg FWD 100       128.13   P2p
Et3/1               Desg FWD 100       128.14   P2p
Et3/2               Desg FWD 100       128.15   P2p
Et3/3               Desg FWD 100       128.16   P2p


IOU1#show spanning-tree interface Ethernet0/3
no spanning tree info available for Ethernet0/3
```

🌞 **Altérer le spanning-tree** en modifiant le coût d'un lien
```
IOU1#configure terminal
Enter configuration commands, one per line.  End with CNTL/Z.
IOU1(config)# interface Ethernet0/3
IOU1(config-if)#spanning-tree cost 50
IOU1(config-if)#exit
IOU1(config)#exit
IOU1#
*Dec  1 14:17:33.864: %SYS-5-CONFIG_I: Configured from console by console
IOU1#show spanning-tree

VLAN0001
  Spanning tree enabled protocol ieee
  Root ID    Priority    32769
             Address     aabb.cc00.0100
             This bridge is the root
             Hello Time   2 sec  Max Age 20 sec  Forward Delay 15 sec

  Bridge ID  Priority    32769  (priority 32768 sys-id-ext 1)
             Address     aabb.cc00.0100
             Hello Time   2 sec  Max Age 20 sec  Forward Delay 15 sec
             Aging Time  300 sec

Interface           Role Sts Cost      Prio.Nbr Type
------------------- ---- --- --------- -------- --------------------------------
Et0/0               Desg FWD 100       128.1    P2p
Et0/1               Desg FWD 100       128.2    P2p
Et0/2               Desg FWD 100       128.3    P2p
Et0/3               Desg FWD 50        128.4    P2p
Et1/0               Desg FWD 100       128.5    P2p
Et1/1               Desg FWD 100       128.6    P2p
Et1/2               Desg FWD 100       128.7    P2p
Et1/3               Desg FWD 100       128.8    P2p
 --More--

```

## II. OSPF
🌞 **Montez la topologie**

- IP statiques sur tout le monde
ROUTEUR
```
R1#show ip interface br
Interface                  IP-Address      OK? Method Status                Protocol
FastEthernet0/0            10.6.21.1       YES manual up                    up  
FastEthernet0/1            10.6.13.1       YES manual up                    up  
FastEthernet1/0            10.6.41.1       YES manual up                    up  
FastEthernet2/0            10.6.3.254      YES manual up                    up  
```
```
R02#show ip interface br
Interface                  IP-Address      OK? Method Status                Protocol
FastEthernet0/0            10.6.21.2       YES manual up                    up  
FastEthernet0/1            10.6.23.2       YES manual up                    up  
FastEthernet1/0            10.6.52.2       YES manual up                    up  
FastEthernet2/0            unassigned      YES unset  administratively down down
```
```
R3#show ip int br
Interface                  IP-Address      OK? Method Status                Protocol
FastEthernet0/0            10.6.23.1       YES NVRAM  up                    up  
FastEthernet0/1            10.6.13.2       YES NVRAM  up                    up  
FastEthernet1/0            unassigned      YES NVRAM  administratively down down
FastEthernet2/0            unassigned      YES NVRAM  administratively down down
```
```
R4#show ip interface br
Interface                  IP-Address      OK? Method Status                Protocol
FastEthernet0/0            10.6.1.254      YES NVRAM  up                    up  
FastEthernet0/1            10.6.41.2       YES NVRAM  up                    up  
FastEthernet1/0            10.6.2.254      YES NVRAM  up                    up  
FastEthernet2/0            unassigned      YES NVRAM  administratively down down
```
```
R5#show ip int br
Interface                  IP-Address      OK? Method Status                Protocol
FastEthernet0/0            unassigned      YES NVRAM  administratively down down
FastEthernet0/1            10.6.52.1       YES NVRAM  up                    up  
FastEthernet1/0            unassigned      YES NVRAM  administratively down down
FastEthernet2/0            unassigned      YES NVRAM  administratively down down
NVI0                       10.6.52.1       YES unset  up                    up  
```
VPCS
```
waf.tp6.b1> show ip

NAME        : waf.tp6.b1[1]
IP/MASK     : 10.6.1.11/24
GATEWAY     : 10.6.1.254
DNS         :
MAC         : 00:50:79:66:68:09
LPORT       : 20064
RHOST:PORT  : 127.0.0.1:20065
MTU         : 1500

```
```
meo.tp6.b1> show ip

NAME        : meo.tp6.b1[1]
IP/MASK     : 10.6.2.11/24
GATEWAY     : 10.6.2.254
DNS         :
MAC         : 00:50:79:66:68:06
LPORT       : 20074
RHOST:PORT  : 127.0.0.1:20075
MTU         : 1500
```
```
john.tp6.b1> show ip

NAME        : john.tp6.b1[1]
IP/MASK     : 10.6.3.11/24
GATEWAY     : 10.6.3.254
DNS         :
MAC         : 00:50:79:66:68:08
LPORT       : 20072
RHOST:PORT  : 127.0.0.1:20073
MTU         : 1500
```

🌞 **Configurer OSPF sur tous les routeurs**

- tous les routeurs doivent partager tous les réseaux auxquels ils sont connectés
```
interface FastEthernet0/0
 ip address 10.6.21.1 255.255.255.252
 duplex auto
 speed auto
!
interface FastEthernet0/1
 ip address 10.6.3.254 255.255.255.0
 duplex auto
 speed auto
!
interface FastEthernet1/0
 ip address 10.6.41.1 255.255.255.252
 duplex auto
 speed auto
!
interface FastEthernet2/0
 ip address 10.6.13.1 255.255.255.252
 duplex auto
 speed auto
!
router ospf 1
 router-id 1.1.1.1
 log-adjacency-changes
 network 10.6.3.0 0.0.0.255 area 3
 network 10.6.13.0 0.0.0.3 area 0
 network 10.6.21.0 0.0.0.3 area 0
 network 10.6.41.0 0.0.0.3 area 1
!
----------------
R1#sh ip ospf neighbor

Neighbor ID     Pri   State           Dead Time   Address         Interface
2.2.2.2           1   FULL/DR         00:00:31    10.6.21.2       FastEthernet0/0
3.3.3.3           1   FULL/BDR        00:00:35    10.6.13.2       FastEthernet2/0
4.4.4.4           1   FULL/BDR        00:00:33    10.6.41.2       FastEthernet1/0
----------------
R1#sh ip route
[...]
     10.0.0.0/8 is variably subnetted, 8 subnets, 2 masks
C       10.6.13.0/30 is directly connected, FastEthernet2/0
O       10.6.1.0/24 [110/11] via 10.6.41.2, 00:04:12, FastEthernet1/0
O       10.6.2.0/24 [110/2] via 10.6.41.2, 00:04:02, FastEthernet1/0
C       10.6.3.0/24 is directly connected, FastEthernet0/1
C       10.6.21.0/30 is directly connected, FastEthernet0/0
O       10.6.23.0/30 [110/11] via 10.6.21.2, 00:05:50, FastEthernet0/0
                     [110/11] via 10.6.13.2, 00:04:42, FastEthernet2/0
C       10.6.41.0/30 is directly connected, FastEthernet1/0
O IA    10.6.52.0/30 [110/20] via 10.6.21.2, 00:05:52, FastEthernet0/0
```
```
interface FastEthernet0/0
 ip address 10.6.52.2 255.255.255.252
 duplex auto
 speed auto
!
interface FastEthernet0/1
 ip address 10.6.21.2 255.255.255.252
 duplex auto
 speed auto
!
interface FastEthernet1/0
 ip address 10.6.23.2 255.255.255.252
 duplex auto
 speed auto
!
interface FastEthernet2/0
 no ip address
 shutdown
 duplex auto
 speed auto
!
router ospf 1
 router-id 2.2.2.2
 log-adjacency-changes
 network 10.6.21.0 0.0.0.3 area 0
 network 10.6.23.0 0.0.0.3 area 0
 network 10.6.52.0 0.0.0.3 area 2
!
---------------------------
R2#sh ip ospf neighbor

Neighbor ID     Pri   State           Dead Time   Address         Interface
3.3.3.3           1   FULL/BDR        00:00:32    10.6.23.1       FastEthernet1/0
1.1.1.1           1   FULL/BDR        00:00:32    10.6.21.1       FastEthernet0/1
5.5.5.5           1   FULL/BDR        00:00:36    10.6.52.1       FastEthernet0/0
---------------------------
R2#sh ip route
Gateway of last resort is not set

     10.0.0.0/8 is variably subnetted, 8 subnets, 2 masks
O       10.6.13.0/30 [110/2] via 10.6.23.1, 00:07:16, FastEthernet1/0
O IA    10.6.1.0/24 [110/13] via 10.6.23.1, 00:06:46, FastEthernet1/0
O IA    10.6.2.0/24 [110/4] via 10.6.23.1, 00:06:36, FastEthernet1/0
O IA    10.6.3.0/24 [110/12] via 10.6.23.1, 00:07:16, FastEthernet1/0
C       10.6.21.0/30 is directly connected, FastEthernet0/1
C       10.6.23.0/30 is directly connected, FastEthernet1/0
O IA    10.6.41.0/30 [110/3] via 10.6.23.1, 00:07:16, FastEthernet1/0
C       10.6.52.0/30 is directly connected, FastEthernet0/0
```
```
interface FastEthernet0/0
 no ip address
 shutdown
 duplex auto
 speed auto
!
interface FastEthernet0/1
 ip address 10.6.23.1 255.255.255.252
 duplex auto
 speed auto
!
interface FastEthernet1/0
 no ip address
 shutdown
 duplex auto
 speed auto
!
interface FastEthernet2/0
 ip address 10.6.13.2 255.255.255.252
 duplex auto
 speed auto
!
router ospf 1
 router-id 3.3.3.3
 log-adjacency-changes
 network 10.6.13.0 0.0.0.3 area 0
 network 10.6.23.0 0.0.0.3 area
---------------------------
R3#sh ip ospf neighbor

Neighbor ID     Pri   State           Dead Time   Address         Interface
2.2.2.2           1   FULL/DR         00:00:35    10.6.23.2       FastEthernet0/1
1.1.1.1           1   FULL/DR         00:00:36    10.6.13.1       FastEthernet2/0
---------------------------
R3#sh ip route
Gateway of last resort is not set

     10.0.0.0/8 is variably subnetted, 8 subnets, 2 masks
C       10.6.13.0/30 is directly connected, FastEthernet2/0
O IA    10.6.1.0/24 [110/12] via 10.6.13.1, 00:07:57, FastEthernet2/0
O IA    10.6.2.0/24 [110/3] via 10.6.13.1, 00:07:47, FastEthernet2/0
O IA    10.6.3.0/24 [110/11] via 10.6.13.1, 00:08:24, FastEthernet2/0
O       10.6.21.0/30 [110/11] via 10.6.13.1, 00:08:24, FastEthernet2/0
C       10.6.23.0/30 is directly connected, FastEthernet0/1
O IA    10.6.41.0/30 [110/2] via 10.6.13.1, 00:08:25, FastEthernet2/0
O IA    10.6.52.0/30 [110/20] via 10.6.23.2, 00:08:35, FastEthernet0/1
```
```
interface FastEthernet0/0
 ip address 10.6.41.2 255.255.255.252
 duplex auto
 speed auto
!
interface FastEthernet0/1
 ip address 10.6.1.254 255.255.255.0
 duplex auto
 speed auto
!
interface FastEthernet1/0
 ip address 10.6.2.254 255.255.255.0
 duplex auto
 speed auto
!
router ospf 1
 router-id 4.4.4.4
 log-adjacency-changes
 network 10.6.1.0 0.0.0.255 area 1
 network 10.6.2.0 0.0.0.255 area 1
 network 10.6.41.0 0.0.0.3 area 1
!
------------------
R4#sh ip ospf neighbor

Neighbor ID     Pri   State           Dead Time   Address         Interface
1.1.1.1           1   FULL/DR         00:00:30    10.6.41.1       FastEthernet0/0
-------------------
R4#sh ip route
Gateway of last resort is not set

     10.0.0.0/8 is variably subnetted, 8 subnets, 2 masks
O IA    10.6.13.0/30 [110/11] via 10.6.41.1, 00:09:29, FastEthernet0/0
C       10.6.1.0/24 is directly connected, FastEthernet0/1
C       10.6.2.0/24 is directly connected, FastEthernet1/0
O IA    10.6.3.0/24 [110/20] via 10.6.41.1, 00:09:29, FastEthernet0/0
O IA    10.6.21.0/30 [110/20] via 10.6.41.1, 00:09:29, FastEthernet0/0
O IA    10.6.23.0/30 [110/21] via 10.6.41.1, 00:09:29, FastEthernet0/0
C       10.6.41.0/30 is directly connected, FastEthernet0/0
O IA    10.6.52.0/30 [110/30] via 10.6.41.1, 00:09:31, FastEthernet0/0
```
```
interface FastEthernet0/1
 ip address 10.6.52.1 255.255.255.252
 ip nat inside
 ip virtual-reassembly
 duplex auto
 speed auto
!
[...]
!
router ospf 1
 router-id 5.5.5.5
 log-adjacency-changes
 network 10.6.52.0 0.0.0.3 area 2
!
------------------
R5#sh ip ospf neighbor

Neighbor ID     Pri   State           Dead Time   Address         Interface
2.2.2.2           1   FULL/DR         00:00:31    10.6.52.2       FastEthernet0/1
-------------------
R5#sh ip route
Gateway of last resort is not set

     10.0.0.0/8 is variably subnetted, 8 subnets, 2 masks
O IA    10.6.13.0/30 [110/12] via 10.6.52.2, 00:09:59, FastEthernet0/1
O IA    10.6.1.0/24 [110/23] via 10.6.52.2, 00:09:59, FastEthernet0/1
O IA    10.6.2.0/24 [110/14] via 10.6.52.2, 00:09:59, FastEthernet0/1
O IA    10.6.3.0/24 [110/22] via 10.6.52.2, 00:09:59, FastEthernet0/1
O IA    10.6.21.0/30 [110/20] via 10.6.52.2, 00:09:59, FastEthernet0/1
O IA    10.6.23.0/30 [110/11] via 10.6.52.2, 00:09:59, FastEthernet0/1
O IA    10.6.41.0/30 [110/13] via 10.6.52.2, 00:10:00, FastEthernet0/1
C       10.6.52.0/30 is directly connected, FastEthernet0/1
```
🌞 **Test**

```
Waf> ping 10.6.1.254

10.6.1.254 icmp_seq=1 timeout
84 bytes from 10.6.1.254 icmp_seq=2 ttl=255 time=4.450 ms
84 bytes from 10.6.1.254 icmp_seq=3 ttl=255 time=3.754 ms
84 bytes from 10.6.1.254 icmp_seq=4 ttl=255 time=10.141 ms
84 bytes from 10.6.1.254 icmp_seq=5 ttl=255 time=6.350 ms
Waf> ping 10.6.52.1

84 bytes from 10.6.52.1 icmp_seq=1 ttl=251 time=57.580 ms
84 bytes from 10.6.52.1 icmp_seq=2 ttl=251 time=43.928 ms
84 bytes from 10.6.52.1 icmp_seq=3 ttl=251 time=77.516 ms
84 bytes from 10.6.52.1 icmp_seq=4 ttl=251 time=83.855 ms
```

## III. DHCP relay

🌞 **Configurer un serveur DHCP** sur `dhcp.tp6.b1`

```
[c1@dhcp ~]$ sudo cat /etc/dhcp/dhcpd.conf
#
# DHCP Server Configuration file.
#   see /usr/share/doc/dhcp-server/dhcpd.conf.example
#   see dhcpd.conf(5) man page
#

option domain-name      "tp6-domain";

option domain-name-servers      1.1.1.1;

default-lease-time      600;

max-lease-time  7200;

authoritative;

subnet 10.6.1.0 netmask 255.255.255.0 {
        range dynamic-bootp 10.6.1.100 10.6.1.200;
        option routers 10.6.1.254;

}

subnet 10.6.3.0 netmask 255.255.255.0 {
        range dynamic-bootp 10.6.3.100 10.6.3.200;
        option routers 10.6.3.254;
}
```

🌞 **Configurer un DHCP relay sur la passerelle de John**
```
R1#sh ip int br
Interface                  IP-Address      OK? Method Status                Protocol
FastEthernet0/0            10.6.21.1       YES NVRAM  up                    up
FastEthernet0/1            10.6.3.254      YES NVRAM  up                    up
FastEthernet1/0            10.6.41.1       YES NVRAM  up                    up
FastEthernet2/0            10.6.13.1       YES NVRAM  up                    up
R1#conf t
Enter configuration commands, one per line.  End with CNTL/Z.
R1(config)#interf
R1(config)#interface f
R1(config)#interface fastEthernet 0/1
R1(config-if)#ip help
R1(config-if)#ip helper-address 10.6.1.253
R1(config-if)#exit
R1(config)#exit
```
```
Dec  5 12:51:28 dhcp dhcpd[791]: DHCPDISCOVER from 00:50:79:66:68:02 via 10.6.3.254
Dec  5 12:51:29 dhcp dhcpd[791]: DHCPOFFER on 10.6.3.100 to 00:50:79:66:68:02 (john) via 10.6.3.254
Dec  5 12:51:32 dhcp dhcpd[791]: DHCPREQUEST for 10.6.3.100 (10.6.1.253) from 00:50:79:66:68:02 (john) via 10.6.3.254
Dec  5 12:51:32 dhcp dhcpd[791]: DHCPACK on 10.6.3.100 to 00:50:79:66:68:02 (john) via 10.6.3.254
```

## IV. Bonus

### 1. ACL

C'est un peu moche que les clients puissent `ping` les IPs des routeurs de l'autre côté de l'infra.

Normalement, il peut joindre sa passerelle, internet, épuicétou.

🌞 **Configurer une access-list**

- ça se fait sur les routeurs
- le but :
  - les clients peuvent ping leur passerelle
  - et internet
  - épuicétou

![it ain't one](./img/it_aint_one.jpg)