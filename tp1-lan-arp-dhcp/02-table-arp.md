# 2. Trois machines et table ARP

**Objectif :** ajouter une troisième machine au LAN, vérifier que tout le monde se joint, puis observer la table ARP.

| Machine | IP | MAC |
|---|---|---|
| node1 | `10.1.1.1/24` | `00:50:79:66:68:00` |
| node2 | `10.1.1.2/24` | `00:50:79:66:68:01` |
| node3 | `10.1.1.3/24` | `00:50:79:66:68:02` |

## Tests de connectivité

**node1 → node2**
```
VPCS> ping 10.1.1.2
84 bytes from 10.1.1.2 icmp_seq=1 ttl=64 time=1.984 ms
84 bytes from 10.1.1.2 icmp_seq=2 ttl=64 time=2.425 ms
84 bytes from 10.1.1.2 icmp_seq=3 ttl=64 time=1.512 ms
```

**node2 → node3**
```
VPCS> ping 10.1.1.3
84 bytes from 10.1.1.3 icmp_seq=1 ttl=64 time=0.951 ms
84 bytes from 10.1.1.3 icmp_seq=2 ttl=64 time=1.266 ms
84 bytes from 10.1.1.3 icmp_seq=3 ttl=64 time=1.304 ms
```

**node1 → node3**
```
VPCS> ping 10.1.1.3
84 bytes from 10.1.1.3 icmp_seq=1 ttl=64 time=2.259 ms
84 bytes from 10.1.1.3 icmp_seq=2 ttl=64 time=2.286 ms
84 bytes from 10.1.1.3 icmp_seq=3 ttl=64 time=2.037 ms
```

## Table ARP de node1

Après les pings, node1 a appris l'adresse MAC de ses deux voisins :

```
VPCS> arp

00:50:79:66:68:01  10.1.1.2 expires in 95 seconds
00:50:79:66:68:02  10.1.1.3 expires in 113 seconds
```

📁 Captures :
- [`captures/tp1-02-arp-node2.pcapng`](../captures/tp1-02-arp-node2.pcapng) — requête ARP *who-has 10.1.1.2* et sa réponse *is-at*
- [`captures/tp1-02-arp-ping-node3.pcapng`](../captures/tp1-02-arp-ping-node3.pcapng) — échange ARP suivi des pings vers node3
