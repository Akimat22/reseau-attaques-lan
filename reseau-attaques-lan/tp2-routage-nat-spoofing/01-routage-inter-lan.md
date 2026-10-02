# 1. Routage entre deux LAN

**Objectif :** relier deux réseaux (`LAN1 = 10.2.1.0/24` et `LAN2 = 10.2.2.0/24`) grâce à un routeur Cisco (`r1.tp2.efrei`), puis vérifier que toutes les machines se joignent.

| Machine | Réseau | IP | Passerelle |
|---|---|---|---|
| node1 | LAN1 | `10.2.1.11` | `10.2.1.254` |
| node2 | LAN1 | `10.2.1.12` | `10.2.1.254` |
| node3 | LAN2 | `10.2.2.11` | `10.2.2.254` |
| node4 | LAN2 | `10.2.2.12` | `10.2.2.254` |

## Ping dans le même LAN

```
node1> ping 10.2.1.12
84 bytes from 10.2.1.12 icmp_seq=1 ttl=64 time=0.758 ms
84 bytes from 10.2.1.12 icmp_seq=2 ttl=64 time=1.006 ms
84 bytes from 10.2.1.12 icmp_seq=3 ttl=64 time=1.190 ms
```

## Ping d'un LAN à l'autre

Configuration de node4 avec sa passerelle :

```
node4> ip 10.2.2.12 255.255.255.0 10.2.2.254
Checking for duplicate address...
node4 : 10.2.2.12 255.255.255.0 gateway 10.2.2.254

node4> ping 10.2.1.12
84 bytes from 10.2.1.12 icmp_seq=1 ttl=63 time=93.861 ms
84 bytes from 10.2.1.12 icmp_seq=2 ttl=63 time=13.396 ms
84 bytes from 10.2.1.12 icmp_seq=3 ttl=63 time=20.431 ms
```

```
node1> ping 10.2.2.11
84 bytes from 10.2.2.11 icmp_seq=1 ttl=63 time=32.546 ms
84 bytes from 10.2.2.11 icmp_seq=2 ttl=63 time=21.289 ms
84 bytes from 10.2.2.11 icmp_seq=3 ttl=63 time=24.677 ms
```

Le **TTL passe de 64 à 63** : le paquet a traversé un routeur, qui a décrémenté le TTL de 1. C'est la preuve que le trafic est bien routé par `r1`.

📁 Capture : [`captures/tp2-01-ping-routage-inter-lan.pcapng`](../captures/tp2-01-ping-routage-inter-lan.pcapng) — capturée sur le câble entre le routeur et le switch.
