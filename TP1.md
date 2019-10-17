# TP1
## I.Gather informations
🌞 Récupérer une liste des cartes réseau avec leur nom, leur IP et leur adresse MAC
```
[root@localhost sysconfig]# ip a
1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN group default qlen 1000
    link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
    inet 127.0.0.1/8 scope host lo
       valid_lft forever preferred_lft forever
    inet6 ::1/128 scope host
       valid_lft forever preferred_lft forever
2: enp0s3: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc fq_codel state UP group default qlen 1000
    link/ether 08:00:27:9c:77:51 brd ff:ff:ff:ff:ff:ff
    inet 10.0.2.15/24 brd 10.0.2.255 scope global dynamic noprefixroute enp0s3
       valid_lft 84986sec preferred_lft 84986sec
    inet6 fe80::61e4:5dd3:bdd2:b6c8/64 scope link noprefixroute
       valid_lft forever preferred_lft forever
3: enp0s8: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc fq_codel state UP group default qlen 1000
    link/ether 08:00:27:5f:ff:ae brd ff:ff:ff:ff:ff:ff
    inet 192.168.56.102/24 brd 192.168.56.255 scope global dynamic noprefixroute enp0s8
       valid_lft 723sec preferred_lft 723sec
    inet6 fe80::d4b7:b928:9ceb:e81d/64 scope link noprefixroute
       valid_lft forever preferred_lft forever
 ```
🌞 Déterminer si les cartes réseaux ont récupéré une IP en DHCP ou non,  
si oui, affichez le bail DHCP utilisé par la machine    
hint : tous les baux DHCP que votre machine stocke sont dans /var/lib/NetworkManager
```
[root@localhost sysconfig]# cd /var/lib
[root@localhost lib]# cd NetworkManager/
[root@localhost NetworkManager]# ls
internal-71076d80-4c99-4159-8fc3-664c2a9de7d0-enp0s3.lease  secret_key
internal-fa8d95bb-7d79-4223-8f36-c1acc2ae6829-enp0s8.lease  timestamps
NetworkManager-intern.conf
```
hint2 : vous pouvez récupérer des infos sur le bail actuel d'une interface donnée avec :
sudo nmcli con show <INTERFACE_NAME> pour toutes les infos
sudo nmcli -f DHCP4 con show <INTERFACE_NAME> pour les infos DHCP 

```
[root@localhost NetworkManager]# sudo nmcli con show enp0s8
connection.id:                          enp0s8
connection.uuid:                        fa8d95bb-7d79-4223-8f36-c1acc2ae6829
connection.stable-id:                   --
connection.type:                        802-3-ethernet
connection.interface-name:              enp0s8
connection.autoconnect:                 no
connection.autoconnect-priority:        0
connection.autoconnect-retries:         -1 (default)
connection.multi-connect:               0 (default)
connection.auth-retries:                -1
connection.timestamp:                   1569509080
connection.read-only:                   no
connection.permissions:                 --
connection.zone:                        --
connection.master:                      --
connection.slave-type:                  --
connection.autoconnect-slaves:          -1 (default)
connection.secondaries:                 --
connection.gateway-ping-timeout:        0
connection.metered:                     unknown
connection.lldp:                        default
connection.mdns:                        -1 (default)
connection.llmnr:                       -1 (default)
802-3-ethernet.port:                    --
802-3-ethernet.speed:                   0
802-3-ethernet.duplex:                  --
802-3-ethernet.auto-negotiate:          no
802-3-ethernet.mac-address:             --
802-3-ethernet.cloned-mac-address:      --
802-3-ethernet.generate-mac-address-mask:--
802-3-ethernet.mac-address-blacklist:   --
802-3-ethernet.mtu:                     auto
802-3-ethernet.s390-subchannels:        --
802-3-ethernet.s390-nettype:            --
802-3-ethernet.s390-options:            --
802-3-ethernet.wake-on-lan:             default
802-3-ethernet.wake-on-lan-password:    --
ipv4.method:                            auto
ipv4.dns:                               --
ipv4.dns-search:                        --
ipv4.dns-options:                       ""
ipv4.dns-priority:                      0
ipv4.addresses:                         --
ipv4.gateway:                           --
ipv4.routes:                            --
ipv4.route-metric:                      -1
ipv4.route-table:                       0 (unspec)
ipv4.ignore-auto-routes:                no
ipv4.ignore-auto-dns:                   no
ipv4.dhcp-client-id:                    --
```
```
[root@localhost NetworkManager]# sudo nmcli con show enp0s3
connection.id:                          enp0s3
connection.uuid:                        71076d80-4c99-4159-8fc3-664c2a9de7d0
connection.stable-id:                   --
connection.type:                        802-3-ethernet
connection.interface-name:              enp0s3
connection.autoconnect:                 yes
connection.autoconnect-priority:        0
connection.autoconnect-retries:         -1 (default)
connection.multi-connect:               0 (default)
connection.auth-retries:                -1
connection.timestamp:                   1569509080
connection.read-only:                   no
connection.permissions:                 --
connection.zone:                        --
connection.master:                      --
connection.slave-type:                  --
connection.autoconnect-slaves:          -1 (default)
connection.secondaries:                 --
connection.gateway-ping-timeout:        0
connection.metered:                     unknown
connection.lldp:                        default
connection.mdns:                        -1 (default)
connection.llmnr:                       -1 (default)
802-3-ethernet.port:                    --
802-3-ethernet.speed:                   0
802-3-ethernet.duplex:                  --
802-3-ethernet.auto-negotiate:          no
802-3-ethernet.mac-address:             --
802-3-ethernet.cloned-mac-address:      --
802-3-ethernet.generate-mac-address-mask:--
802-3-ethernet.mac-address-blacklist:   --
802-3-ethernet.mtu:                     auto
802-3-ethernet.s390-subchannels:        --
802-3-ethernet.s390-nettype:            --
802-3-ethernet.s390-options:            --
802-3-ethernet.wake-on-lan:             default
802-3-ethernet.wake-on-lan-password:    --
ipv4.method:                            auto
ipv4.dns:                               --
ipv4.dns-search:                        --
ipv4.dns-options:                       ""
ipv4.dns-priority:                      0
ipv4.addresses:                         --
ipv4.gateway:                           --
ipv4.routes:                            --
ipv4.route-metric:                      -1
ipv4.route-table:                       0 (unspec)
ipv4.ignore-auto-routes:                no
ipv4.ignore-auto-dns:                   no
ipv4.dhcp-client-i
```
🌞 Afficher la table de routage de la machine et sa table ARP, expliquez chacune des lignes des deux tables
```
[root@localhost NetworkManager]# ip route
default via 10.0.2.2 dev enp0s3 proto dhcp metric 100
10.0.2.0/24 dev enp0s3 proto kernel scope link src 10.0.2.15 metric 100
192.168.56.0/24 dev enp0s8 proto kernel scope link src 192.168.56.102 metric 101
```
Cette route est vers le réseau 10.0.2.2 (enp0s3 + 10.0.2.0/24), elle est utilisée pour une connexion (locale), la passerelle de cette route est à l'IP 10.0.2.15 et cette IP est portée par 192.168.56.102 .
``
[root@localhost ~]# arp -a
_gateway (10.0.2.2) at 52:54:00:12:35:02 [ether] on enp0s3
? (192.168.20.1) at 0a:00:27:00:00:15 [ether] on enp0s8
``
La 1ère ligne c'est l'adresse IP de la passerelle sur le réseau 10.0.2.2/24 toujours associée à son adresse MAC.
La 2ème ligne indique c'est l'IP de mon ordianateur du réseau 192.168.20.1/24 avec son adresse MAC correspondante.

🌞 Récupérer la liste des ports en écoute (listening) sur la machine (TCP et UDP)
Trouver/déduire la liste des applications qui écoutent sur chacun des ports TCP ou UDP repérés comme étant en écoute sur la machine (au moins un serveur SSH)
``
[root@localhost ~]# sudo ss -tlpnu
Netid    State      Recv-Q     Send-Q                Local Address:Port          Peer Address:Port     
udp      UNCONN     0          0                         127.0.0.1:323                0.0.0.0:*         users:(("chronyd",pid=762,fd=6))
udp      UNCONN     0          0               192.168.30.3%enp0s9:68                 0.0.0.0:*         users:(("NetworkManager",pid=793,fd=27))
udp      UNCONN     0          0               192.168.20.3%enp0s8:68                 0.0.0.0:*         users:(("NetworkManager",pid=793,fd=22))
udp      UNCONN     0          0                  10.0.2.15%enp0s3:68                 0.0.0.0:*         users:(("NetworkManager",pid=793,fd=18))
udp      UNCONN     0          0                             [::1]:323                   [::]:*         users:(("chronyd",pid=762,fd=7))
tcp      LISTEN     0          128                         0.0.0.0:22                 0.0.0.0:*         users:(("sshd",pid=815,fd=6))
tcp      LISTEN     0          128                            [::]:22                    [::]:*         users:(("sshd",pid=815,fd=8))
``
🌞 Récupérer la liste des DNS utilisés par la machine    
``
[root@localhost ~]# cat /etc/resolv.conf
# Generated by NetworkManager
nameserver 172.20.10.1
[root@localhost ~]# dig www.reddit.com

; <<>> DiG 9.11.4-P2-RedHat-9.11.4-17.P2.el8_0.1 <<>> www.reddit.com
;; global options: +cmd
;; Got answer:
;; ->>HEADER<<- opcode: QUERY, status: NOERROR, id: 34516
;; flags: qr rd ra; QUERY: 1, ANSWER: 2, AUTHORITY: 0, ADDITIONAL: 1

;; OPT PSEUDOSECTION:
; EDNS: version: 0, flags:; udp: 4096
;; QUESTION SECTION:
;www.reddit.com.                        IN      A

;; ANSWER SECTION:
www.reddit.com.         1178    IN      CNAME   reddit.map.fastly.net.
reddit.map.fastly.net.  1114    IN      A       151.101.121.140

;; Query time: 6 msec
;; SERVER: 172.20.10.1#53(172.20.10.1)
;; WHEN: Thu Oct 17 11:10:43 CEST 2019
;; MSG SIZE  rcvd: 94
``
🌞 Afficher l'état actuel du firewall   
``
[root@localhost ~]# sudo firewall-cmd --list-all
public (active)
  target: default
  icmp-block-inversion: no
  interfaces: enp0s3 enp0s8 enp0s9
  sources:
  services: cockpit dhcpv6-client ssh
  ports:
  protocols:
  masquerade: no
  forward-ports:
  source-ports:
  icmp-blocks:
  rich rules:
  ``
  🐙 Sous CentOS8, ce n'est plus iptables qui est utilisé pour manipuler le filtrage réseau mais nftables. Jouez un peu avec nft et affichez les "vraies" règles firewall (firewalld, manipulé avec firewall-cmd n'est qu'une surcouche à nft)
 ``
 table ip filter {
        chain INPUT {
                type filter hook input priority 0; policy accept;
        }

        chain FORWARD {
                type filter hook forward priority 0; policy accept;
        }

        chain OUTPUT {
                type filter hook output priority 0; policy accept;
        }
}
table ip6 filter {
        chain INPUT {
                type filter hook input priority 0; policy accept;
        }

        chain FORWARD {
                type filter hook forward priority 0; policy accept;
        }

        chain OUTPUT {
                type filter hook output priority 0; policy accept;
        }
}
...
chain nat_POST_public_pre {
        }

        chain nat_POST_public_log {
        }

        chain nat_POST_public_deny {
        }

        chain nat_POST_public_allow {
        }

        chain nat_POST_public_post {
        }
}
``
## II.Edit configuration
### 1.Configuration cartes réseau
🌞 Modifier la configuration de la carte réseau privée
     modifier la configuration de la carte réseau privée pour avoir une nouvelle IP statique définie par vos soins

 Ajouter une nouvelle carte réseau dans un DEUXIEME réseau privé UNIQUEMENT pr      
 🌞 Dans la VM définir une IP statique pour cette nouvelle carte

  Vérifier vos changements
      Afficher les nouvelles cartes/IP
      ``
      [root@localhost ~]# ip a
1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN group default qlen 1000
    link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
    inet 127.0.0.1/8 scope host lo
       valid_lft forever preferred_lft forever
    inet6 ::1/128 scope host
       valid_lft forever preferred_lft forever
2: enp0s3: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc fq_codel state UP group default qlen 1000
    link/ether 08:00:27:9c:77:51 brd ff:ff:ff:ff:ff:ff
    inet 10.0.2.15/24 brd 10.0.2.255 scope global dynamic noprefixroute enp0s3
       valid_lft 85121sec preferred_lft 85121sec
    inet6 fe80::61e4:5dd3:bdd2:b6c8/64 scope link noprefixroute
       valid_lft forever preferred_lft forever
3: enp0s8: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc fq_codel state UP group default qlen 1000
    link/ether 08:00:27:5f:ff:ae brd ff:ff:ff:ff:ff:ff
    inet 192.168.20.3/24 brd 192.168.20.255 scope global dynamic noprefixroute enp0s8
       valid_lft 1121sec preferred_lft 1121sec
    inet6 fe80::d4b7:b928:9ceb:e81d/64 scope link noprefixroute
       valid_lft forever preferred_lft forever
4: enp0s9: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc fq_codel state UP group default qlen 1000
    link/ether 08:00:27:53:ac:31 brd ff:ff:ff:ff:ff:ff
    inet 192.168.30.3/24 brd 192.168.30.255 scope global dynamic noprefixroute enp0s9
       valid_lft 1120sec preferred_lft 1120sec
    inet6 fe80::26:a3f1:cba7:71fa/64 scope link noprefixroute
       valid_lft forever preferred_lft forever
[root@localhost ~]# nmcli d
DEVICE  TYPE      STATE      CONNECTION
enp0s3  ethernet  connected  enp0s3
enp0s8  ethernet  connected  enp0s8
enp0s9  ethernet  connected  Wired connection 1
lo      loopback  unmanaged  --
``
- Vérifier les nouvelles tables ARP/de routage
``
[root@localhost ~]# ip r s
default via 10.0.2.2 dev enp0s3 proto dhcp metric 100
10.0.2.0/24 dev enp0s3 proto kernel scope link src 10.0.2.15 metric 100
192.168.20.0/24 dev enp0s8 proto kernel scope link src 192.168.20.3 metric 101
192.168.30.0/24 dev enp0s9 proto kernel scope link src 192.168.30.3 metric 102
[root@localhost ~]# arp -e
Address                  HWtype  HWaddress           Flags Mask            Iface
_gateway                 ether   52:54:00:12:35:02   C                     enp0s3
192.168.30.2             ether   08:00:27:ae:68:92   C                     enp0s9
192.168.20.1             ether   0a:00:27:00:00:15   C                     enp0s8
192.168.20.2             ether   08:00:27:c6:2d:93   C                     enp0s8
``
### 2.Serveur SSH
???
