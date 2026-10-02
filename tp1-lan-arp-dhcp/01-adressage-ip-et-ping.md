# 1. Adressage IP et premier ping

**Objectif :** relier deux machines (VPCS sous GNS3) dans le même LAN `10.1.1.0/24`, leur attribuer une IP statique et vérifier qu'elles communiquent.

| Machine | IP | MAC |
|---|---|---|
| node1 | `10.1.1.1/24` | `00:50:79:66:68:00` |
| node2 | `10.1.1.2/24` | `00:50:79:66:68:01` |

## Adresses MAC des deux machines

```
VPCS> show

NAME   IP/MASK              GATEWAY           MAC                LPORT  RHOST:PORT
VPCS1  10.1.1.2/24          0.0.0.0           00:50:79:66:68:01  10002  127.0.0.1:10003
       fe80::250:79ff:fe66:6801/64

VPCS> show

NAME   IP/MASK              GATEWAY           MAC                LPORT  RHOST:PORT
VPCS1  10.1.1.1/24          0.0.0.0           00:50:79:66:68:00  10004  127.0.0.1:10005
       fe80::250:79ff:fe66:6800/64
```

## Attribution d'une IP statique

```
VPCS> ip 10.1.1.1
Checking for duplicate address...
PC1 : 10.1.1.1 255.255.255.0

VPCS> ip 10.1.1.2
Checking for duplicate address...
PC1 : 10.1.1.2 255.255.255.0
```

## Test de connectivité

```
VPCS> ping 10.1.1.2
84 bytes from 10.1.1.2 icmp_seq=1 ttl=64 time=0.816 ms
84 bytes from 10.1.1.2 icmp_seq=2 ttl=64 time=0.894 ms
84 bytes from 10.1.1.2 icmp_seq=3 ttl=64 time=1.105 ms
84 bytes from 10.1.1.2 icmp_seq=4 ttl=64 time=0.863 ms
```

## Quel protocole utilise `ping` ?

`ping` utilise le protocole **ICMP** (messages *echo-request* / *echo-reply*).
Avant le premier ping, la machine utilise **ARP** pour découvrir l'adresse MAC de la destination.

📁 Capture : [`captures/tp1-01-ping-icmp.pcapng`](../captures/tp1-01-ping-icmp.pcapng) — 4 allers-retours ICMP entre `10.1.1.2` et `10.1.1.1`.
