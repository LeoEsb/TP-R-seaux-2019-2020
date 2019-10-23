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
