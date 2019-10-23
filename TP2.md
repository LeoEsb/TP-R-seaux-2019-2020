# TP2 : Network low-level, Switching

## I. Simplest setup

🌞 Mettre en place la topologie ci-dessus🦧

🌞 Faire communiquer les deux PCs🦧

- avec un ping qui fonctionne   

```
PC1> ping 10.2.1.2
84 bytes from 10.2.1.2 icmp_seq=1 ttl=64 time=0.466 ms
84 bytes from 10.2.1.2 icmp_seq=2 ttl=64 time=0.694 ms
84 bytes from 10.2.1.2 icmp_seq=3 ttl=64 time=0.611 ms
84 bytes from 10.2.1.2 icmp_seq=4 ttl=64 time=0.682 ms
84 bytes from 10.2.1.2 icmp_seq=5 ttl=64 time=0.677 ms   
```


- déterminer le protocole utilisé par ping à l'aide de Wireshark   
![wireshark](IMG/wireshark.PNG)

## II. More switches

🌞 Gaire communiquer les trois PCs🦧
```
PC1> ping 10.2.2.2
84 bytes from 10.2.2.2 icmp_seq=1 ttl=64 time=0.815 ms
^C
PC1> ping 10.2.2.3
84 bytes from 10.2.2.3 icmp_seq=1 ttl=64 time=0.712 ms
84 bytes from 10.2.2.3 icmp_seq=2 ttl=64 time=1.032 ms
```
```
PC2> ping 10.2.2.1
84 bytes from 10.2.2.1 icmp_seq=1 ttl=64 time=0.954 ms
84 bytes from 10.2.2.1 icmp_seq=2 ttl=64 time=0.992 ms
^C
PC2> ping 10.2.2.3
84 bytes from 10.2.2.3 icmp_seq=1 ttl=64 time=3.533 ms
84 bytes from 10.2.2.3 icmp_seq=2 ttl=64 time=1.355 ms
```
```
PC3> ping 10.2.2.1
84 bytes from 10.2.2.1 icmp_seq=1 ttl=64 time=0.965 ms
84 bytes from 10.2.2.1 icmp_seq=2 ttl=64 time=1.041 ms
^C
PC3> ping 10.2.2.2
84 bytes from 10.2.2.2 icmp_seq=1 ttl=64 time=1.384 ms
84 bytes from 10.2.2.2 icmp_seq=2 ttl=64 time=1.526 ms
```
🌞 Analyser la table MAC d'un switch🦧

- show mac address-table
- comprendre/expliquer chaque ligne
```
IOU1#show mac address-table
          Mac Address Table
-------------------------------------------

Vlan    Mac Address       Type        Ports
----    -----------       --------    -----
   1    0050.7966.6800    DYNAMIC     Et0/0
   1    0050.7966.6801    DYNAMIC     Et0/1
   1    0050.7966.6802    DYNAMIC     Et1/1
   1    aabb.cc00.0211    DYNAMIC     Et1/1
   1    aabb.cc00.0310    DYNAMIC     Et0/1
   1    aabb.cc00.0330    DYNAMIC     Et1/1
Total Mac Addresses for this criterion: 6
```
🐙 En lançant Wireshark sur les liens des switches, il y a des trames CDP qui circulent. Quoi qu'est-ce ?🦧
![wireshark](IMG/wireshark1.PNG)


🌞 Déterminer les informations STP🦧

- A l'aide des commandes dédiées au protocole

- Qui est le root bridge, quels sont les ports désactivés, etc
```
IOU3#show spanning-tree

VLAN0001
  Spanning tree enabled protocol rstp
  Root ID    Priority    32769
             Address     aabb.cc00.0100
             Cost        100
             Port        2 (Ethernet0/1)
             Hello Time   2 sec  Max Age 20 sec  Forward Delay 15 sec

  Bridge ID  Priority    32769  (priority 32768 sys-id-ext 1)
             Address     aabb.cc00.0300
             Hello Time   2 sec  Max Age 20 sec  Forward Delay 15 sec
             Aging Time  300 sec

Interface           Role Sts Cost      Prio.Nbr Type
------------------- ---- --- --------- -------- --------------------------------
Et0/0               Desg FWD 100       128.1    Shr
Et0/1               Root FWD 100       128.2    Shr
Et0/2               Desg FWD 100       128.3    Shr
Et0/3               Altn BLK 100       128.4    Shr
Et1/0               Desg FWD 100       128.5    Shr
Et1/1               Desg FWD 100       128.6    Shr
Et1/2               Desg FWD 100       128.7    Shr
```
Le rooot bridge est l'interface Et0/1 et le port désactivé est Et0/3.

🌞 Faire un schéma en représentant les informations STP🦧

- rôle des switches (qui est le root bridge)
- rôle de chacun des ports
```
                                                       +-------------------+
                                                       |                   |
                                                       |                   |
                                                       |       PC2         |
                                                       |                   |
                                                       +--------+----------+
                                                                |
                                                                |
                                                                |
                                                                |
                                                                |  FWD
                                                       +--------+----+
                                                       |             |  FWD
                                            +----------+    SW2      +-----------+
                                            |     ROOT |             |           |
                                            |          +-------------+           |
                                            |FWD                                 |
                                            |                                    |  BLK
                                      +-----+----+                       +-------+------+
                                  FWD |   SW1    |                  FWD  |              |
           +--------------------------+          +-----------------------+     SW3      +--------------+
           |                          +----------+  FWD                  |              |  FWD         |
           |                                                             +--------------+              |
           |                          ROUTE BRIDGE                                                     |
+----------+--- -----+                                                                                 |
|                    |                                                                         +--------------+   
|       PC1          |                                                                         |              |         
|                    |                                                                         |    PC3       |
|                    |                                                                         +--------------+            
+--------------------+                                                                                                    
                                                                                                             
```                                                                                                          
🌞 Confirmer les informations STP

- effectuer un ping d'une machine à une autre
- vérifier que les trames passent bien par le chemin attendu (Wireshark)
Ping du PC3 vers PC2 : 
```
15	14.489988	10.2.2.3	10.2.2.1	ICMP	98	Echo (ping) request  id=0xde9f, seq=1/256, ttl=64 (reply in 16)
Frame 15: 98 bytes on wire (784 bits), 98 bytes captured (784 bits) on interface 0
Ethernet II, Src: Private_66:68:00 (00:50:79:66:68:00), Dst: Private_66:68:02 (00:50:79:66:68:02)
Internet Protocol Version 4, Src: 10.2.2.3, Dst: 10.2.2.1
Internet Control Message Protocol
```
```
16	14.490836	10.2.2.1	10.2.2.3	ICMP	98	Echo (ping) reply    id=0xde9f, seq=1/256, ttl=64 (request in 15)
Frame 16: 98 bytes on wire (784 bits), 98 bytes captured (784 bits) on interface 0
Ethernet II, Src: Private_66:68:02 (00:50:79:66:68:02), Dst: Private_66:68:00 (00:50:79:66:68:00)
Internet Protocol Version 4, Src: 10.2.2.1, Dst: 10.2.2.3
Internet Control Message Protocol
```
🌞 Ainsi, déterminer quel lien a été désactivé par STP   
C'est le lien entre SW2 et SW3  

🌞 Faire un schéma qui explique le trajet d'une requête ARP lorsque PC1 ping PC3, et de sa réponse
      - représenter TOUTES les trames ARP (n'oubliez pas les broadcasts)
```
                                     +-------+
                                     |       |
                                     |  PC2  |
                                     |       |
                                     +---+---+
                                         |
                                         |
                                         |
                                         |
                                     +---+---+
                                     |       |
                             +-------+  SW2  +---------+
                         ^   |       |       |         |
                         |   |       +-------+         |
                         |   |                         |
                         |   |                         |
                             |                         |
+---------+             +----+---+                +----+---+          +---------+
|         |             |        |                |        |          |         |
|  PC1    +-------------+  SW1   +----------------+   SW3  +----------+   PC3   |
|         |             |        |                |        |          |         |
+---------+             +--------+                +--------+          +---------+

Envoie ping  ------>    Qui est l'ip   --------->  Qui est l'ip  --->  C'est mon ip
pour PC3                10.2.2.3 en                10.2.2.3 en
                        broadcast                  broadcast
```
## III. Isolation
### 1. Simple
🌞 Mettre en place la topologie ci-dessus avec des VLANs
 - voir les commandes dédiées à la manipulation de VLANs
🌞 Faire communiquer les PCs deux à deux
- Vérifier que PC2 ne peut joindre que PC3
- Vérifier que PC1 ne peut joindre personne alors qu'il est dans le même réseau (sad)
```
IOU1#show vlan

VLAN Name                             Status    Ports
---- -------------------------------- --------- -------------------------------
1    default                          active    Et0/0, Et0/1, Et0/2, Et0/3
                                                Et1/0, Et1/1, Et1/2, Et1/3
                                                Et2/0, Et2/1, Et2/2, Et2/3
                                                Et3/0, Et3/1, Et3/2, Et3/3
1002 fddi-default                     act/unsup
1003 token-ring-default               act/unsup
1004 fddinet-default                  act/unsup
1005 trnet-default                    act/unsup

VLAN Type  SAID       MTU   Parent RingNo BridgeNo Stp  BrdgMode Trans1 Trans2
---- ----- ---------- ----- ------ ------ -------- ---- -------- ------ ------
1    enet  100001     1500  -      -      -        -    -        0      0
1002 fddi  101002     1500  -      -      -        -    -        0      0
1003 tr    101003     1500  -      -      -        -    -        0      0
1004 fdnet 101004     1500  -      -      -        ieee -        0      0
1005 trnet 101005     1500  -      -      -        ibm  -        0      0

Remote SPAN VLANs
------------------------------------------------------------------------------


Primary Secondary Type              Ports
------- --------- ----------------- ------------------------------------------
```
Config :
```
IOU1(config)#vlan 10
IOU1(config-vlan)#name client-network
IOU1(config-vlan)#exit
IOU1(config)#vlan 10
IOU1(config-vlan)#name vlan10
IOU1(config-vlan)#exit
IOU1(config)#interface Ethernet 0/0
IOU1(config-if)#switchport mode access
IOU1(config-if)#switchport access vlan 10
```
J'ai fais la meme mais en changeant le vlan 20 et en changeant les ports Et0/2 et Et0/3 pour que PC2 et PC3 communiquent entre eux.
Ping de PC1 vers les deux autres qui ne marchent pas #SAD
```
PC1 : 10.2.3.1 255.255.255.0

PC1> ping 10.2.3.2
^C^[[Ahost (10.2.3.2) not reachable

PC1> ping 10.2.3.3
^Chost (10.2.3.3) not reachable
```
Ping entre PC2 et PC3 :
```
PC2> ping 10.2.3.3
84 bytes from 10.2.3.3 icmp_seq=1 ttl=64 time=0.685 ms
84 bytes from 10.2.3.3 icmp_seq=2 ttl=64 time=0.671 ms
84 bytes from 10.2.3.3 icmp_seq=3 ttl=64 time=0.679 ms
^C
```
```
PC3> ping 10.2.3.2
84 bytes from 10.2.3.2 icmp_seq=1 ttl=64 time=0.459 ms
84 bytes from 10.2.3.2 icmp_seq=2 ttl=64 time=0.662 ms
84 bytes from 10.2.3.2 icmp_seq=3 ttl=64 time=0.732 ms
^C
```
### 2. Avec trunk
🌞 Mettre en place la topologie ci-dessus (en utilisant des VLAN, vous aurez besoin de la notion de trunk)
Conf sw1 :
```
IOU1#conf t
Enter configuration commands, one per line.  End with CNTL/Z.
IOU1(config)#vlan 10
IOU1(config-vlan)#name vlan10
IOU1(config-vlan)#exit
IOU1(config)#interface Ethernet 0/0
IOU1(config-if)#switchport mode access
IOU1(config-if)#switchport access vlan 10
IOU1(config-if)#interface Ethernet 0/1
IOU1(config-if)#switchport mode access
IOU1(config-if)#switchport access vlan 20
% Access VLAN does not exist. Creating vlan 20
IOU1(config-if)#exit
IOU1(config)#vlan 20
IOU1(config-vlan)#name vlan20
IOU1(config-vlan)#exit
IOU1(config)#interface Ethernet 0/1
IOU1(config-if)#switchport mode access
IOU1(config-if)#switchport access vlan 20
IOU1(config-if)#exit
IOU1(config)#nterface Ethernet 1/1
               ^
% Invalid input detected at '^' marker.

IOU1(config)#interface Ethernet 1/1
IOU1(config-if)#switchport trunk encapsulation dot1q
IOU1(config-if)#switchport mode trunk
IOU1(config-if)#
*Oct 23 20:19:55.496: %LINEPROTO-5-UPDOWN: Line protocol on Interface Ethernet1/1, changed state to down
IOU1(config-if)#
*Oct 23 20:19:58.500: %LINEPROTO-5-UPDOWN: Line protocol on Interface Ethernet1/1, changed state to up
IOU1(config-if)#switchport trunk allowed vlan 10,20
IOU1(config-if)#exit
IOU1(config)#interface Ethernet 1/0
IOU1(config-if)#switchport trunk encapsulation dot1q
IOU1(config-if)#switchport mode trunk
IOU1(config-if)#switchport trunk allowed vlan 10,20
IOU1(config-if)#exit
```
Conf sw2:
```
IOU2#conf t
Enter configuration commands, one per line.  End with CNTL/Z.
IOU2(config)#interface Ethernet 1/2
IOU2(config-if)#switchport mode access
IOU2(config-if)#switchport access vlan 10
% Access VLAN does not exist. Creating vlan 10
IOU2(config-if)#exit
IOU2(config)#vlan 10
IOU2(config-vlan)#name vlan10
IOU2(config-vlan)#exit
IOU2(config)#interface Ethernet 1/2
IOU2(config-if)#switchport mode access
IOU2(config-if)#switchport access vlan 10
IOU2(config-if)#exit
IOU2(config)#vlan 20
IOU2(config-vlan)#name vlan20
IOU2(config-vlan)#exit
IOU2(config)#interface Ethernet 1/3
IOU2(config-if)#switchport mode access
IOU2(config-if)#switchport access vlan 20
IOU2(config-if)#exit
IOU2(config)#exit
IOU2#
*Oct 23 20:25:56.047: %SYS-5-CONFIG_I: Configured from console by console
IOU2#conf t
Enter configuration commands, one per line.  End with CNTL/Z.
IOU2(config)#interface Ethernet 1/0
IOU2(config-if)#switchport mode access
IOU2(config-if)#switchport trunk encapsulation dot1q
IOU2(config-if)#switchport mode trunk
IOU2(config-if)#
*Oct 23 20:27:41.109: %LINEPROTO-5-UPDOWN: Line protocol on Interface Ethernet1/0, changed state to down
IOU2(config-if)#
*Oct 23 20:27:44.113: %LINEPROTO-5-UPDOWN: Line protocol on Interface Ethernet1/0, changed state to up
IOU2(config-if)#switchport trunk allowed vlan 10,20
IOU2(config-if)#exit
IOU2(config)#interface Ethernet 1/1
IOU2(config-if)#switchport trunk encapsulation dot1q
IOU2(config-if)#switchport mode trunk
IOU2(config-if)#switchport trunk allowed vlan 10,20
IOU2(config-if)#exit
```
Tout marche ! PC1 et PC3 peuvent se ping seulement entre eux, idem pour PC2 et PC4 ils peuvent uniquement se ping entre eux !
Démo :
```
PC1> ping 10.2.10.2
84 bytes from 10.2.10.2 icmp_seq=1 ttl=64 time=0.745 ms
84 bytes from 10.2.10.2 icmp_seq=2 ttl=64 time=0.933 ms
84 bytes from 10.2.10.2 icmp_seq=3 ttl=64 time=0.958 ms
84 bytes from 10.2.10.2 icmp_seq=4 ttl=64 time=0.942 ms
^C
PC1> ping 10.2.20.1
No gateway found

PC1> ping 10.2.20.2
No gateway found
```
```
PC2> save
Saving startup configuration to startup.vpc
.  done

PC2> ping 10.2.10.1
No gateway found

PC2> ping 10.2.10.2
No gateway found

PC2> ping 10.2.20.2
84 bytes from 10.2.20.2 icmp_seq=1 ttl=64 time=0.851 ms
84 bytes from 10.2.20.2 icmp_seq=2 ttl=64 time=1.391 ms
84 bytes from 10.2.20.2 icmp_seq=3 ttl=64 time=0.954 ms
```
```
PC3> ping 10.2.10.1
84 bytes from 10.2.10.1 icmp_seq=1 ttl=64 time=0.782 ms
84 bytes from 10.2.10.1 icmp_seq=2 ttl=64 time=0.859 ms
84 bytes from 10.2.10.1 icmp_seq=3 ttl=64 time=1.131 ms
^C
PC3> ping 10.2.20.1
No gateway found

PC3> ping 10.2.20.2
No gateway found
```
```
PC4> ping 10.2.10.1
No gateway found

PC4> ping 10.2.20.1
84 bytes from 10.2.20.1 icmp_seq=1 ttl=64 time=0.961 ms
84 bytes from 10.2.20.1 icmp_seq=2 ttl=64 time=0.920 ms
84 bytes from 10.2.20.1 icmp_seq=3 ttl=64 time=1.059 ms
^C
PC4> ping 10.2.10.2
No gateway found
```
