# 3. Serveur DHCP avec dnsmasq

**Objectif :** installer un serveur DHCP sur une VM Rocky Linux (`dnsmasq`) pour que les clients récupèrent automatiquement leur adresse IP.

Plage distribuée : `10.1.1.10` → `10.1.1.50`, bail de 12 h.

## Démarrage du service

```
[aki69@localhost ~]$ sudo systemctl start dnsmasq
[aki69@localhost ~]$ sudo systemctl status dnsmasq
● dnsmasq.service - DNS caching server.
     Loaded: loaded (/usr/lib/systemd/system/dnsmasq.service; disabled; preset: disabled)
     Active: active (running) since Tue 2026-01-13 15:14:51 CET; 8s ago
   Main PID: 1586 (dnsmasq)

Jan 13 15:14:51 localhost.localdomain dnsmasq[1586]: started, version 2.90 cachesize 150
Jan 13 15:14:51 localhost.localdomain dnsmasq-dhcp[1586]: DHCP, IP range 10.1.1.10 -- 10.1.1.50, lease time 12h
Jan 13 15:14:51 localhost.localdomain systemd[1]: Started dnsmasq.service - DNS caching server..
```

## Les trois clients récupèrent une IP automatiquement

L'échange DHCP suit le schéma **DORA** : *Discover → Offer → Request → Ack*.

```
VPCS> ip dhcp
DDORA IP 10.1.1.47/24 GW 10.1.1.253

VPCS> ip dhcp
DDORA IP 10.1.1.46/24 GW 10.1.1.253

VPCS> ip dhcp
DDORA IP 10.1.1.45/24 GW 10.1.1.253
```

📁 Capture : [`captures/tp1-03-dhcp-dora.pcapng`](../captures/tp1-03-dhcp-dora.pcapng) — les 4 messages DORA, puis l'ARP de vérification du client.

## Baux DHCP côté serveur

Le serveur garde une trace de chaque client : date d'expiration, MAC, IP attribuée et nom d'hôte.

```
[aki69@localhost dnsmasq]$ cat dnsmasq.leases
1768359428 00:50:79:66:68:01 10.1.1.46 * 01:00:50:79:66:68:01
1768359384 00:50:79:66:68:02 10.1.1.47 * 01:00:50:79:66:68:02
1768359547 00:50:79:66:68:00 10.1.1.45 VPCS1 01:00:50:79:66:68:00
```

Filtrer le bail d'un client précis :

```
[aki69@localhost dnsmasq]$ cat dnsmasq.leases | grep VPCS1
1768359547 00:50:79:66:68:00 10.1.1.45 VPCS1 01:00:50:79:66:68:00
```
