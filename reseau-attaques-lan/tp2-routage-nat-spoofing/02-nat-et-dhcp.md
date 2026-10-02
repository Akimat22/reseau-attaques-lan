# 2. Accès internet (NAT) et DHCP

**Objectif :** donner accès à internet aux clients grâce au **NAT** configuré sur le routeur, puis automatiser leur configuration réseau avec un serveur **DHCP**.

## Le routeur a accès à internet, mais pas les clients

```
r1.tp2.efrei#ping 8.8.8.8
Sending 5, 100-byte ICMP Echos to 8.8.8.8, timeout is 2 seconds:
!!!!!
Success rate is 100 percent (5/5), round-trip min/avg/max = 20/29/44 ms
```

```
node1> ping 8.8.8.8
8.8.8.8 icmp_seq=1 timeout
8.8.8.8 icmp_seq=2 timeout
8.8.8.8 icmp_seq=3 timeout
```

## Après la mise en place du NAT

Le routeur traduit les adresses privées des clients en son adresse publique :

```
node1> ping 8.8.8.8
84 bytes from 8.8.8.8 icmp_seq=1 ttl=253 time=42.818 ms
84 bytes from 8.8.8.8 icmp_seq=2 ttl=253 time=34.760 ms
84 bytes from 8.8.8.8 icmp_seq=3 ttl=253 time=27.409 ms

node4> ping 8.8.8.8
84 bytes from 8.8.8.8 icmp_seq=1 ttl=253 time=32.862 ms
84 bytes from 8.8.8.8 icmp_seq=2 ttl=253 time=58.929 ms
84 bytes from 8.8.8.8 icmp_seq=3 ttl=253 time=27.992 ms
```

## Résolution de noms

Avec un serveur DNS configuré (`1.1.1.1`), les clients peuvent joindre un site par son nom :

```
node3> ping efrei.fr
efrei.fr resolved to 51.210.229.203
84 bytes from 51.210.229.203 icmp_seq=1 ttl=253 time=38.888 ms
84 bytes from 51.210.229.203 icmp_seq=2 ttl=253 time=48.201 ms
```

## Serveur DHCP dans le LAN1

Configurer IP, passerelle et DNS à la main sur chaque client n'est pas réaliste. On ajoute une machine Rocky Linux `dhcp.tp2.efrei` (`10.2.1.253`) qui distribue ces trois informations.

Test avec un nouveau client, node5 :

```
node5> ip dhcp
DORA IP 10.2.1.100/24 GW 10.2.1.254

node5> show ip

NAME        : node5[1]
IP/MASK     : 10.2.1.100/24
GATEWAY     : 10.2.1.254
DNS         : 1.1.1.1
DHCP SERVER : 10.2.1.253
DHCP LEASE  : 598, 600/300/525
MAC         : 00:50:79:66:68:04

node5> ping efrei.fr
efrei.fr resolved to 51.210.229.203
84 bytes from 51.210.229.203 icmp_seq=1 ttl=53 time=29.065 ms
84 bytes from 51.210.229.203 icmp_seq=2 ttl=53 time=43.649 ms
```

Le client a reçu automatiquement **son IP, la passerelle (`r1`) et le DNS**, et accède directement à internet.
